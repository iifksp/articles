# Ghost更新为0.8.0

抽空更新了一下Ghost的版本，现在最新版本是0.8.0，测试版则是0.9。看了一下更新列表，感觉依旧只是例行升级。我已经有好几个版本没有更新，因为在阿里云下载一些依赖包是非常吃力的一件事，但这次更新还是能目测发现到一些新内容。比如后台多了Apps的设置，集成了Slack。比如修复了之前版本导航无法排序的bug等等。不过这几次的更新对于单人博主来说都是些无关痛痒的功能，大多是对多个作者协同工作的优化，即Ghost虽为专注博客的系统，却也绝非是小巧到专注于个人博客。

更新有些警告和提示，比如邮件功能。Ghost使用的是常用的nodemailer，和PHP便捷的系统邮件调用还有不小的差距。目前是配置邮件服务商来提供邮件的代发。

```
WARNING: Ghost is attempting to use a direct method to send email.
It is recommended that you explicitly configure an email service.
Help and documentation can be found at http://support.ghost.org/mail.
```

更新数据库结构变化。除了一些新增的字段外，还有一张用于记录订阅者邮件的表被新建。Ghost没有内建评论系统，却要内建订阅有点意外。总之依旧是一个对个人博主不是太重要的功能。

```
Migrations: Database upgrade required from version 004 to 005
Migrations: Creating database backup
Migrations: Database backup written to: ***
Migrations: Running migrations
Migrations: Updating database to 005
Migrations: Removing column: tags.hidden
Migrations: Adding column: posts.visibility
Migrations: Adding column: tags.visibility
Migrations: Adding column: users.visibility
Migrations: Adding column: posts.mobiledoc
Migrations: Adding column: users.facebook
Migrations: Adding column: users.twitter
Migrations: Creating table: subscribers
Migrations: Ensuring default settings
Migrations: Running fixture updates
Migrations: Updating fixtures to 005
Migrations: Add ghost-scheduler client fixture
Migrations: Adding permissions fixtures for clients
Migrations: Adding permissions_roles fixtures for clients
Migrations: Adding permissions fixtures for subscribers
Migrations: Adding permissions_roles fixtures for subscribers
```

关于主题，主要多了订阅页面，需要配置邮件，官方推荐mailgun，不过国内很多地方无法注册会一直报错，因为其使用了google的机器验证。折腾了一会确实没有什么可以更新，所以还是继续等后面的版本吧。而且主题作者项里多出来的facebook和twitter属性，在国内也很悲剧。对于Ghost来说，链接的Apps越多，对于国内恐怕就有越多的鸡肋。

怕是真的已经习惯了吧，墙内的生活...

