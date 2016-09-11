# Ghost更新为0.7.4

今年初使用的Ghost，到现在正好1年时间。Ghost版本从0.3.3到当前的0.7.4，有了不小的进步。短短一年，我就已经忘记了wordpress的后台长什么样子了。以前，每次Ghost发布新版本我总是第一时间更新的，不过自从迁到阿里云之后，国内的网络总是给安装带来一些众所周知的特定的麻烦。不过既然在墙内生存，却仍唯有适应一途。

由于这年生活的异常忙碌，捣鼓各种配置的时间是被压缩的最多的，结果就是很多命令都记得模模糊糊。在经历了两次更新失败之后，总算还是找齐了所有的原因并成功完成了所有的更新。

首先是阿里云的镜像系统默认的源list有问题，需要先删除所有source list然后`apt-get update`一下，否则使用nodesource更新node会出错。如今node的版本已经更新到了4.x，自己还在用0.10.x，确实有更新的必要。

```
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install -y build-essential
npm install npm -g
npm install --production
npm install pm2 -g
NODE_ENV=production pm2 start index.js --name "ghost"
```

大体就是这些命令。这里就要继续吐槽阿里云了，`npm install --production`一直会失败，即便我是用淘宝的npm镜像也不行，一些内容还是需要访问源服务器和亚马逊云，所以总是报网络错误。无奈我只能找另一台linode的机器，在上面用同样的方式先下载编译好所有的模块，然后打包丢给阿里云。在加上一些老的php的内容和静态资源，以及要做好过去几年的wordpress文章映射，整个一搞还是花掉了2个多小时。

Ghost 0.7.4的新后台焕然一新，新的导航更方便好用，从设计的角度来说，也是为了应对Ghost日益增多的功能。如今界面的更新着眼于消除暗色调区块，纤细和精致，使整体上更为明亮，而Ghost界面更新的套路也没能例外，总体好评。查看了一下系统占用，用pm2运行的情况下相比0.6.x版本，多占用约25%的内存，感觉勉强可以接受。

尽管Ghost也正在变的复杂的路上，不过当前而言，Ghost团队还是很好的控制住了这个博客系统的膨胀。而我似乎也是时候来更新一下主题了，毕竟新的一年2016就在眼前。

BGM @ Time Will Tell - X-Ray Dog
