# 为Ghost博客添加Disqus评论

由于Ghost平台没有内建评论功能，所以自从博客整体从WordPress迁移至Ghost后，最不习惯的还是没有了评论吐槽，安静得令人发指...总感觉缺少些什么。

也不知道今天吃错了什么药，心血来潮的去注册了Disqus然后将其评论功能安在了Ghost上，简单测试了一下就这么用吧～。虽然Disqus的服务在国内时常要抽风，但聊胜于无，总比没有强些...具体的步骤参见 [Enable Comments On Ghost With Disqus](http://blog.christophvoigt.com/enable-comments-on-ghost-with-disqus/)，已经写得异常清楚了。

感觉Disqus和Ghost还是很相配的，除开要抽风的问题，其实确实是评论功能的最佳替代品——不用担心垃圾评论的问题，而博客系统也可以更专注于内容的发布上。

最后记录一个使用Disqus过程中遇到的问题，就是在Disqus的头像似乎对png24带半透明的支持上有不足，比如我的头像会全显示成黑的。解决办法是对disqus单独上传头像，并避免png24半透明。