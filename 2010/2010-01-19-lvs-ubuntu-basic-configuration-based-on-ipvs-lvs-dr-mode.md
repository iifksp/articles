# LVS:Ubuntu基于IPVS LVS-DR模式的简单配置

Linux Virtual Server (LVS)，此文是我学习LVS之后一个初步的简单配置的过程。

这真是一个相当复杂的集群系统，至少在我看来是这样，初次接触的话还要补充繁多的网络术语和以前一直一知半解的名词。其长达数百页的全英文的HOWTO，比起qmail更加让人头疼。qmail况且有中文的《life with qmail》可以用来入门，至少解决了许多的特殊的名词。

这次的配置使用环境如下：
```
Director：ubuntu-9.10-desktop-i386
IP：192.168.195.130
VIP：192.168.195.200

realserver1：Hercules
IP：192.168.195.134
VIP：192.168.195.200

realserver2：Hercules
IP：192.168.195.135
VIP：192.168.195.200
```

Hercules是一个小巧的附带thttpd的OS(kernel 2.6.17-7)的一个虚拟应用，这里仅仅只是用来测试负载均衡。

## 配置Realserver

1.[下载Hercul](http://prdownloads.sourceforge.net/istanbul/Hercules-1.3-esx3.zip?download)

2.解压Hercules并用VMware打开。

3.启动Hercules，默认情况下它的IP是由DHCP分配的。

4.远程登录Hercules，默认用户名和密码都是admin。

5.在其web根目录/chroot/htdocs/创建测试用的test.html，内容如下：
```
<html>
	<head>
		<title>This is fake WWW server 1</title>
	</head>
	<body>
		This is fake WWW server 1
	</body>
</html>
```
6.查看是否能够访问到http://[IP]/test.html。

7.新建/etc/init.d/S91lvs，chmod +x，内容如下：

```
#!/bin/sh

# VIP installation for LVS-DR
# Ian Gibbs flash666 at yahoo dot com
# reproduced and modified by iifksp 2010/1/19

IPEND=200
VIP=192.168.195.$IPEND
NETWORK_INIT_SCRIPT="/etc/init.d/S40network"

service=LVS

case "$1" in

	start)
		echo -n "Starting $service: "
# Down the network
$NETWORK_INIT_SCRIPT stop
echo ""

# Alter ARP behaviour
echo "Modifying kernel ARP params..."
echo 1 > /proc/sys/net/ipv4/conf/eth0/arp_ignore
echo 2 > /proc/sys/net/ipv4/conf/eth0/arp_announce
echo 1 > /proc/sys/net/ipv4/conf/all/arp_ignore
echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce

# Bring the interface back up
# (doing it this way prevents ARP broadcasts you don't want)
$NETWORK_INIT_SCRIPT start

#install_realserver_vip
/sbin/ifconfig lo:$IPEND $VIP broadcast $VIP netmask 255.255.255.255 up
echo "Added VIP locally:"
/sbin/ifconfig lo:$IPEND

# installing route for VIP $VIP on device lo:$IPEND
/sbin/route add -host $VIP dev lo:$IPEND
echo "Modified routing table:"
/bin/netstat -rn
		;;

	stop)
		echo -n "Stopping $service: "
		/sbin/ifconfig lo$IPEND down
		;;
esac
```

8.关闭Hercules的虚拟机并复制一份作为realserver2。

9.相应修改realserver2的test.html，将其中 server1 改为 server2 。

10.同时启动这两个Hercules虚拟机。

## 配置Director
1. 安装Ubuntu，附带安装OpenSSH server。
2. 更新核心并重启。

		apt-get update
		apt-get upgrade

3. 安装ipvsadm。

		apt-get install ipvsadm

4. 保存下面的脚本安装配置LVS，并以root身份运行。/root/lvs-setup.sh:
```
#!/bin/sh
IPEND=200
VIP=192.168.195.$IPEND
RIP1=192.168.195.134
RIP2=192.168.195.135

#director is not gw for realservers: leave icmp redirects on
echo 'setting icmp redirects (1 on, 0 off) '
echo "1" > /proc/sys/net/ipv4/conf/all/send_redirects
echo "1" > /proc/sys/net/ipv4/conf/default/send_redirects
echo "1" > /proc/sys/net/ipv4/conf/eth0/send_redirects

#add ethernet device and routing for VIP $VIP
/sbin/ifconfig eth0:$IPEND $VIP broadcast $VIP netmask 255.255.255.255
/sbin/route add -host $VIP dev eth0:$IPEND
#listing ifconfig info for VIP $VIP
/sbin/ifconfig eth0:$IPEND

#check VIP $VIP is reachable from self (director)
/bin/ping -c 1 $VIP
#listing routing info for VIP $VIP
/bin/netstat -rn

#setup_ipvsadm_table
#clear ipvsadm table
/sbin/ipvsadm -C
#installing LVS services with ipvsadm
#add http to VIP with round robin scheduling
/sbin/ipvsadm -A -t $VIP:http -s rr

#forward http to realserver using direct routing with weight 1
/sbin/ipvsadm -a -t $VIP:http -r $RIP1 -g -w 1
#check realserver reachable from director
ping -c 1 $RIP1

#forward http to realserver using direct routing with weight 1
/sbin/ipvsadm -a -t $VIP:http -r $RIP2 -g -w 1
#check realserver reachable from director
ping -c 1 $RIP2

#displaying ipvsadm settings
/sbin/ipvsadm
```

完成后，一个基本的轮询的负载均衡就配置好了。

```
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
TCP  ubuntu.local:www rr
  -> 192.168.195.135:www          Route   1      0          0
  -> 192.168.195.134:www          Route   1      0          0
```

## 添加故障转移

IPVS无法自己做到检测realserver的状态，无法避免将请求报文发送到已经挂掉的服务器。这需要另一个程序，那就是ldirectord。

### 1. 安装ldirectord。
```
apt-get install ldirectord
```
ldirectord 将会接管ipvs的配置，所以在ldirectord的配置会覆盖原来的ipvs配置信息。

### 2. 创建配置文件

创建ldirectord的配置文件/etc/ha.d/ldirectord.cf

```
checktimeout=3
checkinterval=5
autoreload=yes
logfile="/var/log/ldirectord.log"
quiescent=yes
virtual=192.168.195.200:80
#	fallback=127.0.0.1:80
	real=192.168.195.134:80 gate
	real=192.168.195.135:80 gate
	service=http
	request="test.html"
	receive="fake"
	scheduler=wlc
	protocol=tcp
	checktype=negotiate
```
必须特别小心的是virtual后的每行必须以一个tab开始。

### 3. 重启或者启动ldirectord的脚本
```
/etc/init.d/ldirectord start
```

这样就完成了简单故障转移(failover)。

运行ipvsadm，可以得到如下的信息：
```
iifksp@ubuntu:~$ sudo ipvsadm
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
TCP  ubuntu.local:www wlc
  -> 192.168.195.134:www          Route   1      0          0
  -> 192.168.195.135:www          Route   1      0          0
```

这表示，当前的调度算法是wlc (加权最少链接Weighted Least Connections)，“具有较高权值的服务器将承受较大比例的活动连接负载，调度器可以自动问询真实服务器的负载情况，并动态地调整其权值”。

当一台realserver挂掉后，ldirectord将会检测到，并将其Weight值设置为0：

```
iifksp@ubuntu:~$ sudo ipvsadm
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
TCP  ubuntu.local:www wlc
  -> 192.168.195.134:www          Route   1      0          0
  -> 192.168.195.135:www          Route   0      0          0
```

从而避免了将报文发送给已经故障的服务器。

另外的配置请参考：[基于Linux的可扩展服务器架构方案](http://www.yaozhifeng.net/internet/ubuntu_lvs_keepalived/)

### 参考链接：

* [LVS HOWTO](http://www.austintek.com/LVS/LVS-HOWTO/HOWTO/)
* [LVS Ubuntu](http://surrey.lug.org.uk/node/303)
* [The Perfect Load-Balanced & High-Availability Web Cluster With 2 Servers Running Xen On Ubuntu 8.04 Hardy Heron](http://www.howtoforge.com/the-perfect-load-balanced-and-high-availability-web-cluster-with-2-servers-running-xen-on-ubuntu-8.04-hardy-heron)