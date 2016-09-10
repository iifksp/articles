# 在windows上配置LMS ilias-3.9.x

这是一份完整详细的配置文档，关于如何在windows上的ilias-3.9.x以及ilias-3.10.x的配置。

发布本文时ilias最新稳定版本已升至4.0.3，已自带大部分依赖工具，安装已经不再如文中所述这么繁复。*created by iifksp 2009/08/03*

对于ilias3-3.9.x来说，在windows上的安装过程比起在linux上的要复杂一些。

[ILIAS官方网站](http://www.ilias.de/) | [官方windows安装文档](http://www.ilias.de/docu/goto.php?target=pg_29381_367&client_id=docu)

环境：windows xp sp3 / xampp-1.7.1

## 1. 安装xampp
PHP的运行环境选择官方推荐的XAMPP集成环境的windows版本。ilias 3.X 被证明在XAMPP运行良好。

XAMPP包含了最新版本的Apache、MySQL、PHP以及Perl。在XAMPP的官方可以下载到最新版本的XAMPP：http://www.apachefriends.org/en/xampp-windows.html
当前版本为1.7.1。

安装之前，创建XAMPP的安装目录。由于在默认Vista的设置下缺少c:program files文件夹足够的写入权限，所以XAMPP被推荐安装在c:myfolderxampp。

- 新建文件夹C:\xampp (XAMPP将被安装在这里)
- 新建文件夹C:\xampp\ILIAS_DATA (ILIAS的数据被存储在这里，但这不是存储课程数据的位置)
- 新建文件C:\xampp\ILIAS_DATA\ILIAS3_LOG.txt (ILIAS的日志文件)。配置完上面后，运行xampp的安装，选择安装路径(e.g C:xampp)，选择需要的选择完成安装。

## 2. 配置PHP环境
按照官方要求配置PHP的运行，主要配置文件如下
```
apache C:\xampp\xampp\apche2\inphp.ini
php5 C:\xampp\xampp\php\php.ini
php4 C:\xampp\xampp\php\php4\php.ini
```
文件php5.ini和php4.ini是备份拷贝。修改php.ini的条目：解注
```
;error_log = xamppxamppapachelogsphperror.log
```
将下面条目修改为100M：
```
post_max_size = 16M
```
将下面条目修改为80M：
```
upload_max_filesize = 16M
```
将下面条目修改为300：
```
mysql.connect_timeout = 60
```
为了能使用sendmail作为邮件系统对外发送邮件，还需要修改下面这些设置：
将
```
SMTP=localhost
```
修改为所使用SMTP服务器的IP。
解注
```
;sendmail_from = me@example.com
```
并更改为合适值。

在安装过程中PHP的安全模式必须被关闭：
```
safe_mode = off
```
安装完成后应该开启安全模式：
```
safe_mode = on
```
通过访问`http://localhost/` 验证XAMPP的安装。通过访问`http://localhost/security/xamppsecurity.php`配置XAMPP的安全设置。

## 3. 安装ILIAS依赖软件
需要安装ILIAS依赖的其他附加软件。

ILIAS需要zip和unzip程序压缩和解压文件，开源程序zip-win32.exe和unzip-w32.exe被证明工作良好。

可以在http://www.info-zip.org 找到需要的zip和unzip程序文件。
我最后使用的是原生Info-ZIP unzip.exe(版本5.52)和zip(版本2.32)的拷贝，参考：http://stahlforce.com/dev/index.php?tool=zipunzip

两者最终被证明运行良好。将两者拷贝到XAMPP的目录中待用。

ILIAS还需要ImageMagick来创建和显示图像和略缩图，所以需从其官网：http://www.imagemagick.org/script/binary-releases.php 下载ImageMagick。

安装时可以选择在XMAPP目录里新建文件夹(e.g. C:/xampp/imagemagick)，安装完成后需确认其安装路径下存在文件convert.exe。

最后，需要安装Java的运行环境JDK: http://www.java.com/en/download/download_the_latest.jsp 安装过程中注意其安装路径，之后需要使用(e.g. C:/programs/java)。

然而，到这里，官方安装文档仍然不完整。直接安装配置会遇到错误。还缺少两个PEAR组件：HTML_Template_IT和MDB2#mysql。
可以到官网下载这两个组件：http://pear.php.net/
两者链接分别是：http://pear.php.net/package/HTML_Template_IT 和 http://pear.php.net/search.php?q=MDB2&in=packages 。

这里推荐的做法是下载已有的PEAR包文件替换XAMPP下的PEAR文件夹。参看：http://www.ilias.de/iosbb/viewtopic.php?f=24&t=5602

所需文件的链接：http://www.schmitt-informatik.ch/download/ilias_pear.tar.gz

下载后解压文件，用其中的文件替换XAMPP下的PEAR文件夹中的全部内容。至此，ILIAS所需安装条件满足。

## 4. 安装配置ILIAS
从官方网站下载ILIAS包，解压后放在C:\xampp\htdocs文件夹中。确定解压后的文件夹中没有.htaccess文件，有则删除之。

使用浏览器访问`http://localhost/ilias3/` 就可以开始安装配置ILIAS了。在安装基本设置页面里，需要键入如下路径：

1. Path to data directory：这是数据路径，选择C:/xampp/ILIAS_DATA。这个路径必须存在(不能选择创建)
2. Path to logfile：这里需要日志文件的路径和全文件名，输入C:/xampp/ILIAS_DATA/ILIAS3_LOG.txt。这个文件必须事先存在
3. Path to Convert:需要ImageMagick的convert.exe文件，选择C:/xampp/ImageMagick/convert.exe
4. Path to Zip：zip程序路径(e.g.C:/xampp/zip/zip.exe)
5. Path to Unzip: unzip程序路径(e.g.C:/xampp/unzip/unzip.exe)
6. Path to Java：需要Java的执行路径(e.g. C:/program/java/java.exe)
7. 其他选项可空
8. 选择一个ILIAS 3.x 的密码用作安装管理
9. 点击save

然后跟着向导完成安装。最后登录`http://localhost/ilias3/`使用root账户，密码homer，登录ilias使用和验证ilias的安装。