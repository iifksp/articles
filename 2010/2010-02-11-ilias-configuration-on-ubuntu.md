# 在ubuntu上配置LMS ilias-3.9.x

这是一份完整详细的配置文档，关于如何在ubuntu上的ilias-3.9.x以及ilias-3.10.x的配置。

发布本文时ilias最新稳定版本已升至4.0.3，已自带大部分依赖工具，安装已经不再如文中所述这么繁复。*created by iifksp 2009/08/03*

环境：ubuntu-9.04-server-i386

## 1. 系统准备
安装如下软件包：apache2,php5,php5-gd,php5-mysql,php5-xsl,php-pear,htmldoc,imagemagick,mysql-client-5.0,mysql-server-5.0,sendmail,sun-java5-jre 运行命令：
```
apt-get install apache2 php5 php5-gd php5-mysql php5-xsl php-pear htmldoc
apt-get install imagemagick mysql-client-5.0 mysql-server-5.0 sendmail sun-java5-jre
```
安装所有的建议的相关的软件包。

## 2. 配置PHP
配置php.ini文件：
```
vim /etc/php5/apache2/php.ini
```
根据官方推荐配置参数设置如下：
```
max_execution_time = 600
memory_limit = 128M

error_reporting = E_ALL & ~E_NOTICE
display_errors = On

post_max_size = 60M
upload_max_filesize = 40M

session.gc_probability = 1
session.gc_divisor = 100

session.gc_maxlifetime = 3600
session.hash_function = 0
```
根据需要修改配置文件。如果一切工作良好，可以考虑将display_errors设置为Off，并且将PHP的错误写入一个日志文件。
## 3. PEAR组件
ilias需求PEAR包Auth，DB以及HTML_Template_IT。ILIAS 3.10.0和更高版本需要PEAR包MDB2而不是包DB。
```
pear install Auth
pear install HTML_Template_IT
```
对于ILIAS 3.9.x版本：
```
pear install DB
```
对于ILIAS 3.10.x 和更高版本：
```
pear install MDB2
pear install MDB2#mysql
```
需要Spreadsheet_Excel_Writer模块，用来导出Microsoft Excel文件格式。但由于Spreadsheet_Excel_Writer还是beta测试阶段，所以需要安装的话要先将stable状态切换到beta：
```
pear config-set preferred_state beta
```
然后就可以安装Spreadsheet_Excel_Writer模块：
```
pear install --alldeps Spreadsheet_Excel_Writer
```
通过使用--alldeps参数，这同时安装了依赖的OLE模块。将状态设回到stable输入：
```
pear config-set preferred_state stable
```
## 4. 安装ilias
在 http://www.ilias.de/docu/">http://www.ilias.de/docu/ 上下载最新的ilias版本，或直接使用命令下载其当前稳定版3.10.8：
```
wget http://downloads.sourceforge.net/ilias/ilias-3.10.8.tar.gz
```
运行如下命令：
```
cp ilias-3.x.x.tar.gz /var/www
tar xzvf /var/www/ilias-3.x.x.tar.gz
chown -R www-data:www-data /var/www/ilias3
```
创建ILIAS工作的数据目录：(官方值)
```
mkdir /opt/iliasdata
chown www-data:www-data /opt/iliasdata
```
打开浏览器，输入`http://localhost/ilias3/setup/setup.php`后，跟着向导，输入下面的路径和禁用convert、htmldoc以及LaTeX检测：(官方值)
```
- Path to data directory: /opt/iliasdata
- Path to log file: /opt/iliasdata/ilias3.log
- Path convert: /usr/bin/convert
- Path zip: /usr/bin/zip
- Path unzip: /usr/bin/unzip
- Path java: /usr/bin/java
- Path htmldoc: /usr/bin/htmldoc
```
完成安装后，即可登陆ILIAS，使用管理员账号root，密码homer，登录ilias使用和验证ilias的安装。