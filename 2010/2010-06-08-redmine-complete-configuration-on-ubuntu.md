# Redmine+Apache+SVN+Postfix完整配置指南

对于这篇配置，我维护了一个[文档版本](http://www.swordair.com/docs/redmine-complete-configuration-on-ubuntu/)并会尽力保持更新。(**22 December 2013 update:** 实际上，现在作为一个设计师，已经很久没有维护文档了...)

如果你是一个项目管理者，可能听说过redmine。它是一个项目管理系统的后起之秀，具备了广泛的项目管理平台特点同时，还提供了诸多的独有的特性。包括了内建的wiki、BUG问题跟踪、SVN集成等。本文将从头开始，详细地构建起整个项目管理的系统。如果你的团队正需要一个这样的平台，希望此文可以作为你的参考:)

你可以从本文中了解到如何配置好一个redmine系统，可能这中间会遇到些问题，但它们会被解决，然后让redmine跑在apache上。如何配置一个svn库，然后集成到redmine中去。以及如何配置redmine的邮件通知。
如果你不打算亲历亲为体验这种繁复的安装过程，你也完全可以使用BitNami的[一体化安装包](http://bitnami.org/stack/redmine)，这会使得安装部署redmine像安装xampp一样简单。

自ubuntu-10.04-LTS推出也已经有一个月的时间，所以这次的系统就用它了~对Ubuntu我是很有偏爱啊~虽说是一个完整的配置，但是涉及到的SVN以及Postfix只是略微讲述，仅仅满足于这个配置，目的是不让此文变成长篇大论，这两者的讨论远远超出了本文的范围。不过，我会给出足够多的扩展阅读，在那些扩展里可以找到你需要的内容。

然后让我们开始吧~

## 安装ubuntu-10.04-server
这里配置的是Ubuntu server最新的10.04。关于系统安装就不多说什么了，塞进光盘然后一路next~

![Ubuntu server 10.04](https://swordair.com/content/images/2013/Dec/001_install_ubuntu1004_menu.png)

系统初始配置信息:

- IP：192.168.242.130
- 主机名：redmine
- 预装选择：LAMP server，Mail server，OpenSSH server

如果不需要redmine的邮件通知，或者不想自己搭建邮件服务器而使用已存在或者其他SMTP邮件服务来发送邮件的话，可以不用安装Mail server，即postfix。如果是这样，那么下面的这步也可以省去。

![install postfix](https://swordair.com/content/images/2013/Dec/005_install_postfix.png)

完成安装后，既然是新系统，就先更新到最新吧:)
```
apt-get update
apt-get upgrade
```
然后我们来开始配置redmine。

## 获取redmine及相关信息
redmine基于ROR，所以对于ROR的开发人员来说部署这个系统要比不了解ROR的人容易的多。这里假设你对ROR是有一定的了解的。如果不了解，照着步骤做即可。

先找到redmine的下载，[redmine的官方网站](http://www.redmine.org/)上有很多参考信息，[下载列表](http://rubyforge.org/frs/?group_id=1850)则是在rubyforge.org上。

当前最新版本是0.9.4。redmine官方的[安装和配置文档](http://www.redmine.org/wiki/redmine/RedmineInstall)包含了linux和windows的配置，包括对系统需求。但可能文档不尽详尽，安装中会遇到很多问题。如果仅仅只是想体验下安装过程，并想知道怎么解决具体遇到的问题，可以参看我之前写的[redmine-0.9.x配置过程](https://swordair.com/redmine-0-9-x-configuration-on-ubuntu)。

我将redmine放在/usr/local/里：
```
cd /usr/local/
```
获取当前版本并解压：
```
wget http://rubyforge.org/frs/download.php/70486/redmine-0.9.4.tar.gz
tar zxvf redmine-0.9.4.tar.gz
mv redmine-0.9.4 redmine
```

## 配置mysql数据库
数据库是mysql，为redmine建立库，库名redmine。同时创建redmine用户，把库的权限分配给这个用户。最后设置用户的密码为'redminePASSWORD'。当然这里的库名、用户名和密码，可以按实际情况替换。
```
mysql -u root -p
mysql> create database redmine character set utf8;
mysql> grant select,insert,delete,update,create,drop,alter,index on redmine.* to redmine;
mysql> SET PASSWORD FOR 'redmine' = PASSWORD('redminePASSWORD');
mysql> flush privileges;
mysql> exit;
```

## 安装与配置
进入redmine的主目录，开始配置数据文件，把配置指向刚才建立的库。
```
cd /usr/local/redmine
cp config/database.yml.example config/database.yml
vim config/database.yml
```
配置production部分成如下所示。其中的database，username，password按实际情况替换。
```
production:
  adapter: mysql
  database: redmine
  host: localhost
  username: redmine
  password: redminePASSWORD
  encoding: utf8
```

当前版本的需求是ruby 1.8.6, 1.8.7  Rails 2.3.5  Rack 1.0.1。为此首先安装需要的包。
```
apt-get install rake rubygems libopenssl-ruby libmysql-ruby
```
可以看到这些包被安装：
```
irb1.8 libmysql-ruby libmysql-ruby1.8 libopenssl-ruby
libopenssl-ruby1.8 libreadline-ruby1.8 libreadline5 libruby1.8 rake
rdoc1.8 ruby ruby1.8 rubygems rubygems1.8 unzip zip
```
rake的安装则必须指定版本：
```
gem install rack --version=1.0.1
```
上面这条命令在我以前写配置的时候一直有效，但是在编写此文时，gem的在线安装有些问题。不知是出于网络的问题还是其他原因，我得到了下面的错误：
```
WARNING:  RubyGems 1.2+ index not found for:
        http://gems.rubyforge.org/

RubyGems will revert to legacy indexes degrading performance.
Bulk updating Gem source index for: http://gems.rubyforge.org/
ERROR:  While executing gem ... (Gem::RemoteSourceException)
    Error fetching remote gem cache: SocketError: getaddrinfo: Temporary failure in name resolution (http://gems.rubyforge.org/yaml)
```
google之后也没能找到什么结果，所以只好本地安装rack了。
下载对应的rack-1.0.1.gem到本地后安装：
```
wget http://rubyforge.org/frs/download.php/65736/rack-1.0.1.gem
gem install --local rack-1.0.1.gem
</pre>
继续下面的步骤，生成会话存储密钥：
<pre lang="bash">
RAILS_ENV=production rake config/initializers/session_store.rb
```
注：r3055之后的版本移除了config/initializers/session_store.rb，使用下面的命令替代。
```
rake generate_session_store
```
然后开始创建数据库表结构，在redmine的根目录下运行：
```
RAILS_ENV=production rake db:migrate
```
读取默认配置数据，当遇到选择语言(Select language)时，选择zh：
```
RAILS_ENV=production rake redmine:load_default_data
```
```
Select language: bg, bs, ca, cs, da, de, el, en, es, fi, fr, gl, he, hr, hu, id, it, ja, ko, lt, nl, no, pl, pt, pt-BR, ro, ru, sk, sl, sr, sv, th, tr, uk, vi, zh, zh-TW [en]zh
====================================
Default configuration data loaded.
```
至此，redmine简单的配置就完成了。使用其自带的webrick来运行redmine，来检查下redmine的配置吧:)
```
ruby script/server webrick -e production
```
默认的管理员用户名和密码都是admin，进入系统后就可以开始熟悉下了。可以为每个人定义语言环境。下图是管理页面。

![redmine management page](https://swordair.com/content/images/2013/Dec/002_redmine_management.png)

### 在apache上部署
其自带的webrick可能不能满足使用需求，那么就把它配置到apache上。
apache运行ROR有多种方式，这里使用passenger。
```
apt-get install build-essential
apt-get install apache2-prefork-dev libaprutil1-dev libapr1-dev ruby1.8-dev
```
然后安装 passenger
```
gem install passenger
passenger-install-apache2-module
```
如果报passenger-install-apache2-module这条命令找不到的话，那么通过下面的命令查看执行路径：
```
gem environment
```
```
RubyGems Environment:
  - RUBYGEMS VERSION: 1.3.5
  - RUBY VERSION: 1.8.7 (2010-01-10 patchlevel 249) [i486-linux]
  - INSTALLATION DIRECTORY: /var/lib/gems/1.8
  - RUBY EXECUTABLE: /usr/bin/ruby1.8
  - EXECUTABLE DIRECTORY: /var/lib/gems/1.8/bin
  - RUBYGEMS PLATFORMS:
    - ruby
    - x86-linux
  - GEM PATHS:
     - /var/lib/gems/1.8
     - /root/.gem/ruby/1.8
  - GEM CONFIGURATION:
     - :update_sources => true
     - :verbose => true
     - :benchmark => false
     - :backtrace => false
     - :bulk_threshold => 1000
  - REMOTE SOURCES:
     - http://gems.rubyforge.org/
```
其中，EXECUTABLE DIRECTORY就是命令的全路径，所以对于我例子里的情况执行
```
/var/lib/gems/1.8/bin/passenger-install-apache2-module
```
根据提示安装和部署。passenger会在本机编译并成为apache的一个模块。安装过程中会遇到下面的提示信息(根据版本的不同，信息也会稍有变化)：
```
Welcome to the Phusion Passenger Apache 2 module installer, v2.2.13.

This installer will guide you through the entire installation process. It
shouldn't take more than 3 minutes in total.

Here's what you can expect from the installation process:

 1. The Apache 2 module will be installed for you.
 2. You'll learn how to configure Apache.
 3. You'll learn how to deploy a Ruby on Rails application.

Don't worry if anything goes wrong. This installer will advise you on how to
solve any problems.
```
```

The Apache 2 module was successfully installed.

Please edit your Apache configuration file, and add these lines:

   LoadModule passenger_module /var/lib/gems/1.8/gems/passenger-2.2.13/ext/apache2/mod_passenger.so
   PassengerRoot /var/lib/gems/1.8/gems/passenger-2.2.13
   PassengerRuby /usr/bin/ruby1.8

After you restart Apache, you are ready to deploy any number of Ruby on Rails
applications on Apache, without any further Ruby on Rails-specific
configuration!
```
```
Deploying a Ruby on Rails application: an example

Suppose you have a Rails application in /somewhere. Add a virtual host to your
Apache configuration file and set its DocumentRoot to /somewhere/public:

   <VirtualHost *:80>
      ServerName www.yourhost.com
      DocumentRoot /somewhere/public    # <-- be sure to point to 'public'!
      <Directory /somewhere/public>
         AllowOverride all              # <-- relax Apache security settings
         Options -MultiViews            # <-- MultiViews must be turned off
      </Directory>
   </VirtualHost>

And that's it! You may also want to check the Users Guide for security and
optimization tips, troubleshooting and other useful information:

  /var/lib/gems/1.8/gems/passenger-2.2.13/doc/Users guide Apache.html

Enjoy Phusion Passenger, a product of Phusion (www.phusion.nl) :-)
http://www.modrails.com/

Phusion Passenger is a trademark of Hongli Lai & Ninh Bui.
```
根据提示信息部署，我这里的步骤稍有不同。
首先，编辑apache的配置文件并添加下面的信息：
```
vim /etc/apache2/apache2.conf
```
```
LoadModule passenger_module /var/lib/gems/1.8/gems/passenger-2.2.13/ext/apache2/mod_passenger.so
   PassengerRoot /var/lib/gems/1.8/gems/passenger-2.2.13
   PassengerRuby /usr/bin/ruby1.8
```
然后在/etc/apache2/sites-available添加一个站点：
```
vim redmine
```
并添加如下内容：
```
RailsBaseURI /redmine
```
在web根目录建立redmine主目录的符号链接，并设置权限：
```
ln -s /usr/local/redmine/public /var/www/redmine 
chown -R www-data:www-data /var/www
```
启用redmine站点：
```
a2ensite redmine
```
最后重启apache：
```
/etc/init.d/apache2 restart
```
或，重新加载配置：
```
/etc/init.d/apache2 reload
```
打开浏览器，如果你能够访问到redmine，那么恭喜你，redmine已经在apache上运行良好！

![redmine on apache](https://swordair.com/content/images/2013/Dec/003_redmine_apache.png)

## 建立SVN版本库
版本控制svn可以参考《subversion 权威指南》，网上也有很多下载。不过我不太喜欢这本书，因为看起来会比较无聊:) 下面简单地安装svn并建立一个测试用库。

首先，安装subversion版本控制：
```
apt-get install subversion
```
创建SVN的根目录，这里我建在/var。然后建立一个演示用的库。
```
cd /var
mkdir svn
cd svn
svnadmin create demo
```
然后配置demo库：
```
cd demo/conf
ls -l
```
conf目录里是authz，passwd和svnserve.conf这三个文件，分别用于配置用户权限、用户密码和配置此版本库(demo)。
```
-rw-r--r-- 1 root root 1089 2010-06-04 14:45 authz
-rw-r--r-- 1 root root  335 2010-06-04 14:44 passwd
-rw-r--r-- 1 root root 2265 2010-06-04 14:44 svnserve.conf
```
首先配置svnserve.conf的内容：

- anon-access 匿名访问默认权限，默认为read。
- auth-access 授权访问默认权限，默认为write。
- password-db 用户密码文件，默认为与svnserve.conf同目录的passwd文件。
- authz-db 用户授权文件，默认为与svnserve.conf同目录的authz文件。
- realm 显示库名

需要注意的是，每行开头不能留空格。
```
[general]
anon-access = read
auth-access = write
password-db = passwd
authz-db = authz
realm = Demo Repository
```
passwd文件里存储的是用户名和密码，一行一条记录。
```
[users]
redmine = redminePASSWORD
```
authz是授权文件，配置着每个用户和组的权利，下面是把redmine用户放到redmine_group组里并赋予redmine_group组demo库的读写权限。
```
[groups]
dev = redmine
[demo:/]
@dev = rw
```
简单配置完后，启动svnserve：
```
svnserve -d -r /var/svn
```
最后将SVN服务加入自启动：
```
cd /etc/rc2.d
vim S88svnserve
```
并在文件S88svnserve中添加上面的启动命令
```
svnserve -d -r /var/svn
```
最后还不能忘了加上执行权限：
```
chmod +x S88svnserve
```
然后，在redmine中对应项目的配置里，指向对应的版本库。

![svn on redmine](https://swordair.com/content/images/2013/Dec/004_redmine_svn.png)

## 邮件配置
这里让redmine用默认配置的Postfix来发送邮件。如果在安装ubuntu的时候没有安装邮件服务器，这里也可以通过下面的命令来安装：
```
apt-get install postfix
```
这里之所以要特意配置个邮件服务器，完全是为了使整个redmine系统完整。完全可以使用其他邮件服务。

默认配置的Postfix已经能够满足当前的发信情况。关于邮件系统和Postfix的讨论严重超出了本文范围，对于不熟悉Postfix的人，我推荐阅读[Postfix基础配置](https://help.ubuntu.com/community/PostfixBasicSetupHowto)，如果想了解更多，可以阅读[Postfix虚拟邮件系统完全配置](https://help.ubuntu.com/community/PostfixCompleteVirtualMailSystemHowto)(尽管此文还未完全完成)。

如果想要知道邮件系统的来龙去脉，邮件服务器如何处理邮件，那么我强烈建议阅读《Postfix权威指南》一书，这本书对于邮件系统的讲解深入浅出，是本好书。

redmine邮件的配置文件同样在主目录的config里：
```
cp email.yml.example email.yml
vim email.yml
```
移动到末尾可以看到默认的配置，将production改为如下所示。其中的域名等信息按实际情况替换。
```
production:
  delivery_method: :smtp
  smtp_settings:
    address: localhost
    port: 25
    domain: swordair.com
    authentication: :none
```
更多邮件信息参考：[官方邮件配置参考](http://www.redmine.org/wiki/redmine/Email_Configuration)。里面包括了如何来写验证信息等内容。

至此，你就获得了一个敏捷运行的redmine项目管理系统。谢谢阅读，如有什么问题，请留言给我，我会尽力解决:)