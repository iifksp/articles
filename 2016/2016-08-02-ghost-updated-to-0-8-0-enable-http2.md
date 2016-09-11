# Ghost更新至0.9.0并启用HTTP/2

![Ghost 0.9.0 and http2](https://swordair.com/content/images/2016/08/banner-ghost-0-9-0-http2.png)

Ghost在0.8.0之后不久便更新了新版本0.9.0，本来想等HTTP2一起弄，不过这段时间阿里云还是迟迟不上Ubuntu 16.04 LTS，实在有些等不及，所以还是决定直接从14.04升级到16.04。因为早些年吃过很多更新系统的亏，所以一直对系统核心升级颇为不喜。好在这次比较顺利，更新到16.04之后，获得了OpenSSL 1.0.2g也就具备了启用HTTP/2的条件，所以顺带也将Ghost更新到最新版本。

续[简述HTTP/2的特性及其对前端的影响](/http2-features-and-implications-for-front-end-develop/)一文的部署部分，系统更新参阅[How To Upgrade to Ubuntu 16.04 LTS](https://www.digitalocean.com/community/tutorials/how-to-upgrade-to-ubuntu-16-04-lts)，之后卸载之前安装的Nginx后重新安装，新的Nginx包会使用OpenSSL 1.0.2g对ALPN提供支持。用Chrome的调试工具调出协议一栏就能验证协议已经由http 1.1变成h2:

![](/content/images/2016/08/http2-protocol-in-chrome.png)

之后更新Ghost，阿里云依旧虐心，好依赖几个文件死活下不来，半小时后放弃。于是还是老样子再开个Linode过渡一下node_modules，只用了2分钟...

浏览了一下依赖文件，[Ghost 0.9.0](https://dev.ghost.org/ghost-0-9-0/)这版改动算是比较大的，**依赖文件足足比前一个版本多出一倍**，而且还撤换了好多个原来使用的包。不过就功能来说并没有多大越进，最实用的新功能应该是“定时发布”，为此还需要更新数据库，主要是调整时间格式。最近几个更新的功能似乎都是针对多用户博客的，我作为中低频个人用户比较无感。倒是Disqus被墙了好一段时间了，这时候才越发想起内建评论系统的好处...可惜Ghost未来也难有自建评论。

更新完HTTP/2之后速度的提升还是很明显的，特别是图片，我几乎没有在HTTP/2下看到以前常见的加载闪烁，总之效果很赞，确实值得花些功夫。