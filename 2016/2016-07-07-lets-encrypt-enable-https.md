![let's encrypt banner](https://swordair.com/content/images/2016/07/banner-lets-encrypt.jpg)

如今的网站，HTTPS也已经逐渐成为了标配了。[Let's Encrypt](https://letsencrypt.org/) 作为个人用户最好的免费证书的选择，也关注了一段时间了，这次就抽空将网站强制到HTTPS。这里主要记录一下过程和遇到的问题。

[Let's Encrypt](https://letsencrypt.org/)作为新的证书颁发机构，免费，自动，开放，这三点对于个人用户来说尤为重要。免费自不用说，自动更是免去了很多过程和麻烦。官网目前推荐的获取和安装方式是[certbot](https://certbot.eff.org/)，只需要简单运行一些命令并作一些配置即可。我的环境是nginx以及ubuntu 14.04。

下载certbot-auto并设置为可执行
```
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
```
这里我遇到了python安装卡住不动的问题，等了很长的时间，貌似一直到超时为止。原因似乎是某些文件下载失败。这也是国内服务器的可悲之处。我使用的是阿里云主机，参考 [no response "Installing Python packages" ](https://github.com/certbot/certbot/issues/2516)，最终是通过修改pip.conf解决：
```
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```
在安装python时依旧卡了小会，但最终成功，提示信息：Installation succeeded.

之后运行certbot-auto
```
$ ./certbot-auto
```
出现交互界面，填写邮箱，域名，并验证域名所有权。这里需要填写包含子域名，比如swordair.com, www.swordair.com，空格分隔。为了验证域名，其会在网站根目录生成一个文件并访问。由于我的主要博客内容由ghost驱动，用nginx做反向代理，所以事先需要做好配置不然会验证失败。报错信息类似：
```
Failed authorization procedure. swordair.com (http-01): urn:acme:error:unauthorized :: The client lacks sufficient authorization :: Invalid response from http://swordair.com/.well-known/acme-challenge/...: 
```
如果验证通过则会显示恭喜的信息：
```
Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/swordair.com/fullchain.pem. Your cert will
   expire on 2016-10-05. To obtain a new or tweaked version of this
   certificate in the future, simply run certbot-auto again. To
   non-interactively renew *all* of your certificates, run
   "certbot-auto renew"
```
信息提示成功，并说明有效期为3个月，之后需要手动续约或者配置自动脚本或任务来自动续约。

最后将配置添加到nginx的配置文件中：
```
ssl_certificate /etc/letsencrypt/live/swordair.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/swordair.com/privkey.pem;
```

HTTPS使用的是端口是443而非80，由于我强制使用HTTPS，所以还需要对80端口的HTTP做跳转。并且我prefer的是无www的域名，所以还不能忘了对HTTPS额外做无www的跳转。做完这些后验证一下跳转和证书状态，在手机上也试了下，证书的兼容性还是非常不错的。

对于站点来说，转向HTTPS如果内容都能HTTPS自然最好，不过可能总难免出现偶然的混合。这点Ghost的支持还算是不错，只需要修改主配置文件config.js即可（其他配置参考[How to setup SSL for self-hosted Ghost](http://support.ghost.org/setup-ssl-self-hosted-ghost/)），需要注意到只是连接的外部资源文件。比如我在about页面引用了一张boinc分布式计算排名图，好在其同时支持HTTP和HTTPS。最后，不能忘了要用disqus的迁移工具将所有HTTP的url映射到HTTPS，不然评论就全丢了～

很多大型网站目前都是HTTP和HTTPS并存的形式，而对于个人博客而言，保留一种足矣。