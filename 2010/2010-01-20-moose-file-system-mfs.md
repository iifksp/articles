# Moose File System(MFS)

FMS是一个网络分布式文件系统。它将物理的本地化（Server）数据以单个的资源呈现给用户。对于标准文件操作来说，MFS表现的和其他类Unix文件系统一样。它有层状的结构 （目录树）， 存储了文件的属性（权限，最后访问和修改时间），并且能够创建特别的文件（块以及字符设备，管道和套接字）， 符号链接（文件名指向其他本地的可访问的文件，不是MFS必须的）和硬链接（一个文件的不同名称指向MFS上的同一数据）。

上面的一段是我原封不动翻译自MFS官网的简介。

![mfs architecture](https://swordair.com/content/images/2013/Dec/mfs_architecture.png)

在这整个架构里，MFS由这3种机器组成：

**MFS MASTER**：一个单一的管理服务器，管理整个文件系统。它存储了每个文件的元数据，这些信息包括了文件的大小、属性、位置等，其还包括那些非常规的文件信息，比如目录结构、套接字、管道和设备。作为整个系统的关键，这台主机被建议配备冗余的电力供应、ECC校验内存、RAID1/RAID5/RAID10磁盘阵列、UPS等。

**CHUNKSERVERS**：提供数据服务的任意数量的服务器，用来存储文件数据并自我拷贝（如果一个文件应该存在不止一个副本）。数据服务器可以通过配置、启动其chunkserver进程，动态的连接到MFS上以满足需求。MFS所报告的空闲空间是所有的连接多MFS的磁盘空闲空间的总和。

**CLIENT**：任意数量的启动有mfsmount进程的客户端，其通过与MASTER的通信，来接受和修改文件信息，并通过与CHUNKSERVERS的通信来改名实际的文件数据。

最后的挂载通过mfsmount命令，它基于FUSE(Filesystem in USErspace)机制，所以MFS在任何的实现了FUSE的操作系统上可用（Linux, FreeBSD, MacOS X...）

在仔细观察了它的架构图之后，我开始搭建这个分布式的文件系统。
当前发行的稳定版是[mfs-1.6.11.tar.gz](http://www.moosefs.com/files/mfs-1.6.11.tar.gz)，但是1.6相对于1.5做了很多改变，而官方的关于1.6的文档又十分匮乏，所以很多都只好先行摸索。这次使用的是1.6.11。

官方1.5参考：http://www.moosefs.com/pages/userguides.html

这次的配置环境：
```
MASTER: Ubuntu_9.10_server_i386 node IP:192.168.195.133
CHUNKSERVERS: Ubuntu_9.10_server_i386 CLC IP:192.168.195.132
CLIENT1: Ubuntu_9.04_desktop_i386 IP:192.168.195.137
CLIENT2: Ubuntu_9.10_desktop_i386 IP:192.168.195.130
```

## 一. 统一配置

安装必要的编译环境，安装MFS依赖的库，添加MFS运行时用户和组：
```
apt-get install build-essential
apt-get install libfuse-dev
useradd mfs
```
## 二. 配置Master
```
tar zxvf mfs-1.6.11.tar.gz
cd mfs-1.6.11
./configure --prefix=/usr/local/mfs --disable-mfsmount  --disable-mfschunkserver --enable-mfsmaster --with-default-user=mfs --with-default-group=mfs
make
make install
cd /usr/local/mfs/etc
cp mfsexports.cfg.dist mfsexports.cfg
cp mfsmaster.cfg.dist mfsmaster.cfg
cp mfsmetalogger.cfg.dist mfsmetalogger.cfg
vim mfsmaster.cfg
```
对于配置文件mfsmaster.cfg，移除所有的注释并配置相应值：
```
WORKING_USER = mfs
WORKING_GROUP = mfs
SYSLOG_IDENT = mfsmaster
LOCK_MEMORY = 0
NICE_LEVEL = -19

EXPORTS_FILENAME = /usr/local/mfs/etc/mfsexports.cfg

DATA_PATH = /usr/local/mfs/var/mfs

BACK_LOGS = 50

REPLICATIONS_DELAY_INIT = 300
REPLICATIONS_DELAY_DISCONNECT = 3600

MATOML_LISTEN_HOST = *
MATOML_LISTEN_PORT = 9419

MATOCS_LISTEN_HOST = *
MATOCS_LISTEN_PORT = 9420

MATOCU_LISTEN_HOST = *
MATOCU_LISTEN_PORT = 9421

CHUNKS_LOOP_TIME = 300
CHUNKS_DEL_LIMIT = 100
CHUNKS_WRITE_REP_LIMIT = 1
CHUNKS_READ_REP_LIMIT = 5

REJECT_OLD_CLIENTS = 0

# deprecated, to be removed in MooseFS 1.7
# LOCK_FILE = /var/run/mfs/mfsmaster.lock
```
创建运行目录，修改所有权：
```
mkdir -p /var/run/mfs
sudo chown mfs.mfs /var/run/mfs
cd /usr/local/mfs/var/mfs
cp metadata.mfs.empty metadata.mfs
```
最后启动MFS Master：
```
sudo /usr/local/mfs/sbin/mfsmaster start
```
启动成功，如下：
```
working directory: /usr/local/mfs/var/mfs
lockfile created and locked
initializing mfsmaster modules ...
loading sessions ... ok
sessions file has been loaded
exports file has been loaded
loading metadata ...
loading objects (files,directories,etc.) ... ok
loading names ... ok
loading deletion timestamps ... ok
checking filesystem consistency ... ok
loading chunks data ... ok
connecting files and chunks ... ok
all inodes: 12
directory inodes: 1
file inodes: 11
chunks: 4
metadata file has been loaded
stats file has been loaded
master <-> metaloggers module: listen on *:9419
master <-> chunkservers module: listen on *:9420
main master server module: listen on *:9421
mfsmaster daemon initialized properly
```
主机Master配置完成。

**安装总结：**相对1.5多出了mfsexports.cfg的配置，用于权限的控制。如果这个文件不存在，将导致后续的客户机挂载MFS的请求被拒绝。即发生mfsmaster register error: Permission denied的错误。但是如果在客户机上安装1.5的client工具，则会直接绕过这个文件。

## 三. 配置CHUNKSERVERS
与master的配置类似，在配置参数上略有不同。
```
tar zxvf mfs-1.6.11.tar.gz
cd mfs-1.6.11
./configure --prefix=/usr/local/mfs --disable-mfsmount  --enable-mfschunkserver --disable-mfsmaster --with-default-user=mfs --with-default-group=mfs
make
make install
cd /usr/local/mfs/etc
cp mfschunkserver.cfg.dist mfschunkserver.cfg
cp mfshdd.cfg.dist mfshdd.cfg
vim mfschunkserver.cfg
```
修改如下
```
WORKING_USER = mfs
WORKING_GROUP = mfs
SYSLOG_IDENT = mfschunkserver
LOCK_MEMORY = 0
NICE_LEVEL = -19

DATA_PATH = /usr/local/mfs/var/mfs

MASTER_RECONNECTION_DELAY = 5

# modify to master host ip
MASTER_HOST = 192.168.195.133
MASTER_PORT = 9420

MASTER_TIMEOUT = 60

CSSERV_LISTEN_HOST = *
CSSERV_LISTEN_PORT = 9422
CSSERV_TIMEOUT = 5

HDD_CONF_FILENAME = /usr/local/mfs/etc/mfshdd.cfg
HDD_TEST_FREQ = 10

# deprecated, to be removed in MooseFS 1.7
# LOCK_FILE = /var/run/mfs/mfschunkserver.lock
# BACK_LOGS = 50
```
然后修改mfshdd.cfg，这个文件制定作为存储的路径：
```
vim mfshdd.cfg
```
根据需要修改，这里改为
```
/mnt/data
```
建立存储目录、工作目录，分配所有权：
```
mkdir -p /mnt/data
sudo chown mfs.mfs /mnt/data
mkdir -p /var/run/mfs
sudo chown mfs.mfs /var/run/mfs
```
最后启动CHUNKSERVER：
```
sudo /usr/local/mfs/sbin/mfschunkserver start
```
成功启动并初始化：
```
working directory: /usr/local/mfs/var/mfs
lockfile created and locked
initializing mfschunkserver modules ...
scanning folder /mnt/data/ ...
/mnt/data/: 4 chunks found
scanning complete
main server module: listen on *:9422
stats file has been loaded
mfschunkserver daemon initialized properly
```
主机CHUNKSERVER配置完成，增加CHUNKSERVER方法相同。

## 四. 配置CLIENT
```
tar zxvf mfs-1.6.11.tar.gz
cd mfs-1.6.11
./configure --prefix=/usr/local/mfs --enable-mfsmount  --disable-mfschunkserver --disable-mfsmaster --with-default-user=mfs --with-default-group=mfs
make
make install
```
安装完成后就可以挂载MFS了。使用mfsmount命令挂载刚才配置的文件系统：
```
mkdir -p /mnt/mfs
/usr/local/mfs/bin/mfsmount -H 192.168.195.133 /mnt/mfs
```
Client也配置完成。

其他的客户机的配置类似，挂载后的文件系统对客户机来说是单独的数据源。任何客户机的对于文件的操作都会影响到其他系统。至此，整个配置完成。关于MFS的维护和管理，以及备份和恢复，我会另起一文。