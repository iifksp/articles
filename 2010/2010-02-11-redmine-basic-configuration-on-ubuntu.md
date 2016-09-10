# 在ubuntu上简单配置redmine

这是一份完整详细的配置文档(*08/03/2009 created by iifksp*)，关于如何在ubuntu上配置运行项目管理工具redmine。

发布本文时redmine最新稳定版本已升至0.9.2，rails的版本也已升至2.3.5，并发布了3.0 beta，配置过程可能不同。

redmine官方网站：http://www.redmine.org
redmine下载：http://rubyforge.org/frs/?group_id=1850
ubuntu官方网站：http://www.ubuntu.com

此次配置使用 ubuntu-9.04-server-i386 / redmine-0.8.4

## 1. 配置运行环境
安装ruby解释器：
```
apt-get install ruby rubygems
```
安装sqlite3数据库：(或者安装mysql数据库，可参看在ubuntu上配置LAMP)
```
apt-get install sqlite sqlite3 libsqlite3-ruby
```

## 2. 配置redmine
获取redmine：
```
wget http://rubyforge.org/frs/download.php/56909/redmine-0.8.4.tar.gz
```
移动到工作目录：
```
mv redmine-0.8.4.tar.gz /usr/local/
```
解压文件：
```
tar zxvf redmine-0.8.4.tar.gz
```
进入目录，复制数据库配置范例：
```
cp config/database.yml.example config/database.yml
```
修改数据库配置文件：
```
vim config/database.yml
```
将production部分配置成如下所示：
```
production:
adapter: sqlite3
dbfile: /usr/local/redmine-0.8.4/db/redmine.db
```
到db目录下建立数据库文件：
```
sqlite3 redmine.db
chmod a+x redmine.db
```
如果是mysql数据库的配置则类似于如下：
```
production:
  adapter: mysql
  database: redmine
  host: localhost
  username: root
  password: root
  encoding: utf8
```
安装rake和libopenssl-ruby
```
apt-get install rake libopenssl-ruby
```
rake迁移数据库：
```
rake db:migrate RAILS_ENV="production"
```
读取默认值：
```
rake redmine:load_default_data RAILS_ENV=`production`
```
选择中文zh。完成后，数据库和相关表被自动创建，redmine被设置为默认状态。

邮件的配置：
```
cd /usr/local/redmine-0.8.4/config
cp email.yml.example email.yml
vim email.yml
```
按需修改，例如下：
```
production:
  delivery_method: :smtp
  smtp_settings:
    address: mail.swordair.com
    port: 25
    domain: swordair.com
    authentication: :login
    user_name: redmine
    password: redmine
```
更多有关redmine邮件的配置参看：http://www.redmine.org/wiki/redmine/Email_Configuration

## 3. 启动redmine
在redmine主目录运行：
```
ruby script/server -e production
```
如果使用的是mysql数据库，可能会报错：/tmp/mysql.sock找不到。因为在/tmp确实没有mysql.sock文件，此文件位于 /var/run/mysqld/mysqld.sock，所以做一个软链接即可：
```
ln -s /var/run/mysqld/mysqld.sock /tmp/mysql.sock
```
redmine默认使用3000端口，浏览器访问`http://localhost:3000`
用户名admin，密码admin。

## 4. 备份redmine
只需备份redmine的主目录即可。如果数据库是MySQL，还需备份库。

## 5. 补丁和其他
### 邮件发送的504错误
Redmine Email测试时出现504 5.7.4 Unrecognized authentication type，也是由于使用redmine发送邮件时目标服务器不需要任何的验证。为使redmine邮件工作正常，需要删除有关验证的配置信息。找到config/email.yml文件将其内容由
```
production:
  delivery_method: :smtp
  smtp_settings:
    address: mail.swordair.com
    port: 25
    domain: swordair.com
    authentication: :login      #delete this line
    user_name: redmine       #delete this line
    password: redmine        #delete this line
```
改为
```
production:
  delivery_method: :smtp
  smtp_settings:
    address: mail.swordair.com
    port: 25
    domain: swordair.com
```
### 发送邮件标题乱码问题
问题表现为主题<32字符时显示正确，超过显示乱码。
需要为redmine打补丁，补丁位置：http://www.redmine.org/attachments/2290/mailer-subject-base64.patch

实测打完补丁后，主题字符数上限大大提高，基本解决乱码问题。
更多情况参看：http://www.redmine.org/issues/3592 以及 http://www.redmine.org/issues/3601