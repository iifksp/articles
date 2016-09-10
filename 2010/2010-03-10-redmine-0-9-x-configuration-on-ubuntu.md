# redmine-0.9.x配置过程

项目管理系统redmine对我来说，有一种特别的感情。它使我去接触了ROR，使我重新认识了ubuntu，并且也是它，让我得到了第一份工作。值此3月，距redmine-0.8.4发布已经过去了10个月，现在的最新稳定版已经更新到了0.9.3，并且rails的版本也已升至2.3.5，而且发布了3.0 beta。虽然ruby的黄金时期已经过去了几个年头，但是它的发展势头还是不减。

![](/content/images/2013/Dec/redmine_header_en.gif)

于是当初我写的关于[redmine-0.8.x配置文档](http://swordair.com/redmine-basic-configuration-on-ubuntu/)已经略显过时了。

今天看到了redmine-0.9.3，于是就像当年一样看着官方wiki里的指导走了一遍过程。中间遇到了不少问题，所以本文不是一个标准的配置文档，而是我自己的整个配置过程——包含着遇到的各种问题，以及解决方案。

官方网站：http://www.redmine.org/ 有很多好的资料，下载wiki：http://www.redmine.org/wiki/redmine/Download 也能带来很多帮助，最后，redmine在rubyforge上的下载地址：http://rubyforge.org/frs/?group_id=1850

从0.9.x开始，redmine的需求变成了ruby-1.8.6,1.8.7  Rails-2.3.5  Rack-1.0.1，而Rails-2.3.5已经包含在了vender目录里了。这次的配置平台式ubuntu-9.10-server-i386，并预装了LAMPserver。

进入工作目录，这里选择主目录。
```
cd -
```
下载redmine-0.9.3
```
wget http://rubyforge.org/frs/download.php/69449/redmine-0.9.3.tar.gz
```
解压
```
tar zxvf redmine-0.9.3.tar.gz
```
进入解压后的redmine根目录
```
cd redmine-0.9.3
```
MySQL数据库设置
```
mysql -u root -p
mysql> create database redmine character set utf8;
mysql> grant select,insert,delete,update,create,drop,alter,index on redmine.* to redmine
mysql> SET PASSWORD FOR 'redmine' = PASSWORD('redminePASSWORD');
mysql> flush privileges;
mysql> exit;
```
配置redmine数据库配置文件
```
cp config/database.yml.example config/database.yml
vim config/database.yml
```
```
production:
  adapter: mysql
  database: redmine
  host: localhost
  username: redmine
  password: redminePASSWORD
```
如果数据库不是使用标准的端口(3306)，使用port指定端口号：
```
production:
  adapter: mysql
  database: redmine
  host: localhost
  port: 3307
  username: redmine
  password: redminePASSWORD
```
生成会话存储密码
```
RAILS_ENV=production rake config/initializers/session_store.rb
```
这会报出一个rake尚未安装的错误，使用下面的命令安装rake。
```
apt-get install rake
```
同时附带安装了
```
libruby1.8 ruby ruby1.8 unzip zip
```
再次运行rake，仍然报错
```
rake aborted!
no such file to load -- rubygems
```
跟着报错信息继续安装缺失的包
```
apt-get install rubygems
```
同时附带安装了
```
irb1.8 libreadline-ruby1.8 rdoc1.8 rubygems1.8
```
再次运行rake，成功执行。
注：r3055之后的版本移除了config/initializers/session_store.rb，使用下面的命令替代。
```
rake generate_session_store
```
完成了会话存储密码生成后，就可以开始创建数据库表，在redmine的根目录下运行
```
RAILS_ENV=production rake db:migrate
```
然而又报错了
```
rake aborted!
Could not find RubyGem rack (~> 1.0.1)
```
如果这个时候安装rack，必须指定版本，因为redmine-0.9.3需求的事rack-1.0.1，如果直接
```
gem install rack
```
这将会安装rack-1.1.0，这种情况下运行rake的报错信息会说明这点
```
rake aborted!
RubyGem version error: rack(1.1.0 not ~> 1.0.1)
```
所以必须指定版本安装rack，用--version参数
```
gem install rack --version=1.0.1
```
再次运行rake，发觉报错信息变成了
```
rake aborted!
no such file to load -- net/https
```
这时联想到https，可能是缺少了SSL的某些文件，对于ruby，执行
```
apt-get install libopenssl-ruby
```
再次运行rake，报错信息再次变化
```
rake aborted!
no such file to load -- mysql
```
对此，执行
```
apt-get install libmysql-ruby
```
运行rake后执行成功，数据库表被创建。
然后插入默认配置数据到数据库里

```
RAILS_ENV=production rake redmine:load_default_data
```

```
Select language: bg, bs, ca, cs, da, de, el, en, es, fi, fr, gl, he, hr, hu, id, it, ja, ko, lt, nl, no, pl, pt, pt-BR, ro, ru, sk, sl, sr, sv, th, tr, uk, vi, zh, zh-TW [en]zh
====================================
Default configuration data loaded.
```

运行redmine的用户必须可以读写files, log, tmp这三个目录，假设由redmine这个用户运行，就需要执行下面的命令
```
mkdir tmp public/plugin_assets
sudo chown -R redmine:redmine files log tmp public/plugin_assets
sudo chmod -R 755 files log tmp public/plugin_assets
```
最后在redmine的根目录运行下面的命令，启动redmine。
```
ruby script/server webrick -e production
```
这个WEBrick是一个轻量的web服务器，一般总是用作开发和调试。再验证了redmine安装后，就可以考虑把它迁移到apache上去了。
验证`http://localhost:3000/`来验证redmine的配置。用户名admin，密码admin。

关于SMTP邮件服务器的配置，官方也有说明。因为0.9.x和0.8.x相比这部分没有变化，所以可以参看我以前写的关于0.8.x文章。

数据备份方面，文档真的很贴心。之前0.8.x版本还不曾有的备份步骤，现在也提供的比较完善了。甚至直接提供了命令：
```
# Database
/usr/bin/mysqldump -u <username> -p<password> <redmine_database> | gzip > /path/to/backup/db/redmine_`date +%y_%m_%d`.gz

# Attachments
rsync -a /path/to/redmine/files /path/to/backup/files
```
将命令里的/path/to/redmine和/path/to/backup换成实际情况里的目录。这两条命令很明确的指出了redmine的备份，只需要备份数据库以及file文件夹。

至此，redmine已经能很好地工作了。如果打算把它部署到apache上，可以参看我之后写的文章。
