# 换用pm2运行Ghost

从WordPress换到Ghost以来也快半年了。相对于前辈WordPress，Ghost还是非常年轻的，所以很多功能都还不完善。2个月前的0.4.2版本才刚刚支持到tag，而静态页面的支持也只是在年初。即便年轻，Ghost也没有甩开大步往前冲，其更新频率其实并不高。

之前一直使用官方提供的自启动脚本运行Ghost，好处是服务器重启后无需关心应用的启动，对于我这种不时折腾服务器的人来说还是非常实用。不过却有一个致命的缺点，就是如果服务器上同时运行数个node应用，官方脚本service ghost restart会将其他应用一并停掉，这点就相当烦人了，尽管对于单独运行一个Ghost实例的博客来说其实并没有什么影响。

我的服务器上一直用pm2运行了好几个node应用，好几次重启Ghost后忘记重启其他应用，所以最后决定让pm2一起接管Ghost。

## 使用pm2 ##

[pm2](https://github.com/unitech/pm2)是一个内建负载平衡的Node应用的进程管理器。它有很多Forever不具备的功能，所以现在使用的人越来越多。

使用pm2运行Ghost和运行其他应用没什么不同：

```
NODE_ENV=production pm2 start index.js --name swordair
```
由于是运行于生产环境，所以指定了`NODE_ENV=production`。应用上线后就可以通过list/monit等各种命令查看运行状态，非常简单直观。

## 启动脚本 ##

pm2可以自己生成自启动脚本，这点甩Forever几条街。使用起来也异常简单：

```
sudo pm2 startup
```
当然，可能会遇到下面这样的错误(至少我遇到了)，这是因为需要指定运行对应的平台。

```
error: missing required argument ‘platform'
```
我用的是ubuntu，所以在命令后附加参数`ubuntu`即可。其他还可以选择`centos`或者是`redhat`等。

```
# sudo pm2 startup ubuntu
PM2 Generating system V init script in /etc/init.d/pm2-init.sh
PM2 Making script booting at startup...
PM2 -ubuntu- Using the command chmod +x /etc/init.d/pm2-init.sh; update-rc.d pm2-init.sh defaults
 Adding system startup for /etc/init.d/pm2-init.sh ...
   /etc/rc0.d/K20pm2-init.sh -> ../init.d/pm2-init.sh
   /etc/rc1.d/K20pm2-init.sh -> ../init.d/pm2-init.sh
   /etc/rc6.d/K20pm2-init.sh -> ../init.d/pm2-init.sh
   /etc/rc2.d/S20pm2-init.sh -> ../init.d/pm2-init.sh
   /etc/rc3.d/S20pm2-init.sh -> ../init.d/pm2-init.sh
   /etc/rc4.d/S20pm2-init.sh -> ../init.d/pm2-init.sh
   /etc/rc5.d/S20pm2-init.sh -> ../init.d/pm2-init.sh

PM2 Done.
```
这样，Ghost就能在系统重启后也自动启动。在/etc/init.d/pm2-init.sh中添加`export NODE_ENV="production"`可以确保应用运行在生产环境：

```
export HOME="/root"
export NODE_ENV="production"
```

最后的效果还是不错的，我所有的node应用都由pm2接管，统一管理可以大大提高效率。观察了一小段时间，只是发现资源占用比之前稍高一些。

#### 参考资料 ####

1. [使用 Ghost](http://docs.ghost.org/zh/usage/configuration/)
2. [安装Ghost & 开始尝试 - 初始化脚本](http://docs.ghost.org/zh/installation/deploy/)
3. [How To Use PM2 to Setup a Node.js Production Environment On An Ubuntu VPS](https://www.digitalocean.com/community/articles/how-to-use-pm2-to-setup-a-node-js-production-environment-on-an-ubuntu-vps)
