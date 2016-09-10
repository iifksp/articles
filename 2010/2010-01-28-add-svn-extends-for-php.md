# 为PHP增加SVN扩展

这次涉及到一个网上资料很少的问题，PHP的SVN扩展。

windows平台下，最简单的php服务器搭建无疑是xampp。但是当前版本的xampp(1.7.3)并不带有svn的扩展，即php_svn.dll。从版本更新中得知2009.1.23的xampp做了如下的变化：
```
23. Jan 2009 1.7.0 pl1 beta3 
Delete php\ext\php_svn.dll 
Delete php\php5.ini 
Delete mysql-gui-tools-noinstall-5.0-r15 
New build xampp-control.exe 
Patching phpMyAdmin\main.php (305-318)
```
也是在这个版本开始，xampp移除了对svn扩展的支持。

在PECL的网站上仍然可以下载到最新的SVN扩展源码:

http://pecl.php.net/package/svn

当前的版本是0.5.1，发布于2009.9.23，状态仍然是beta。查看其在php.net上的文档:

http://www.php.net/manual/en/book.svn.php

但是由于文档不全仍然让人一头雾水。起初是想为我的windows下添加以下svn扩展的，但是却看到了这样一句 'A DLL for this PECL extension is currently unavailable' ，又是一盆冷水。跳转链接指向如何在windows下编译PHP。于是我装Visual C++ 2008 Express Edition，装了Windows Platform SDK，捣鼓了一阵，还是没能成功。

随后我又去网上直接下了php_svn.dll文件，把他丢进ext目录里，在php.ini里添加extension = php_svn.dll，重启，还是失败...最终我还是放弃了windows的扩展搭建，把目标转向了Linux。

我一直都很喜欢Ubuntu，这次用的照旧还是Ubuntu。习惯性的，在Ubuntu server-9.04上已经安装了LAMP，所以我先是安装了一个svn，快速建了一个库：
```
apt-get install subversion
mkdir /var/svn
cd /var/svn
svnadmin create MyCategory
svnserve -d -r /var/svn
```
subversion的安装不是必须的。似乎现在还没有基于Debian的发布版有这个扩展的二进制包。所以要在ubuntu上使用这个扩展就必须手动安装

然后安装必要的组件：
```
apt-get install build-essential
apt-get install php-pear php5-dev libsvn-dev
```
build-essential是必要的编译环境，通常这个都会预先安好。接下去就是三个依赖，php-pear php5-dev libsvn-dev 。如果用下面的命令搜索
```
apt-cache search pecl
```
那么结果会是这样的
```
dh-make-php - Creates Debian source packages for PHP PEAR and PECL extensions
php-auth - PHP PEAR modules for creating an authentication system
php5-radius - PECL radius module for PHP 5
php5-remctl - PECL module for Kerberos-authenticated command execution
php-pear - PEAR - PHP Extension and Application Repository
```
出现php-pear的原因，是PEAR和PECL共享了相同的包和分发机制。适当PHP开发包也是必要的,比如说php5-dev。

最后，就是Subversion的头文件，即libsvn-dev，被用来在没有安装Subversion的环境中编译svn扩展。

下一步，安装扩展：
```
sudo pecl install svn
```
由于当前版本是0.5.1，所以命令变更如下：
```
sudo pecl install svn-0.5.1
```
这一步会下载svn扩展的源代码，编译成svn.so文件然后再把它安装到PHP能找到它的地方。但是，编译到一半，问题就来了:
```
/usr/bin/ld: cannot find -lsasl2
collect2: ld returned 1 exit status
make: *** [svn.la] Error 1
```
出现这个问题，我一时没能google到什么信息。直到我在google第6页找到了Ubuntu论坛里一位朋友的解决方法:

**搜索sasl2而不是lsasl2**，然后安装。
```
apt-cache search sasl2
sudo apt-get install libsasl2-dev libsasl2-modules-ldap
```
之后继续刚才安装svn-0.5.1的命令，报错的内容变化了：
```
/usr/bin/ld: cannot find -lneon-gnutls
```
需要继续搜索和安装缺失的包
```
apt-cache search neon gnutls
sudo apt-get install libneon27-gnutls-dev
```
至此，终于通过编译，并在完成的最后提示在php.ini中增加一行：
```
extension=svn.so
```
重启Apache
```
/etc/init.d/apache2 reload
```
通过查看phpinfo()，可以看到多出了svn一项。

<table border="0" cellpadding="3" width="600">
<tr style="background-color:#9999cc; font-weight: bold; color: #000000;"><th style="border: 1px solid #000000;vertical-align: baseline;background-color:#9999cc;color:#000000">svn support</th><th style="border: 1px solid #000000;vertical-align: baseline;background-color:#9999cc;color:#000000">enabled</th></tr>
<tr><td style="background-color: #ccccff; font-weight: bold; color: #000000;border: 1px solid #000000; vertical-align: baseline;">svn client version </td><td style="background-color: #cccccc; border: 1px solid #000000; vertical-align: baseline;">1.5.4 </td></tr>
<tr><td style="background-color: #ccccff; font-weight: bold; color: #000000;border: 1px solid #000000; vertical-align: baseline;">svn extension version </td><td style="background-color: #cccccc; border: 1px solid #000000; vertical-align: baseline;">0.5.1 </td></tr>
</table>

写一个脚本测试一下，成功。
```
<?php
svn_checkout('svn://localhost/MyCategory', dirname(__FILE__) . '/MyCategory');
?> 
```

#### 参考资料：
* http://ubuntuforums.org/showthread.php?t=1158439