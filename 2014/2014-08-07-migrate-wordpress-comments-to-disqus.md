# 迁移WordPress评论到Disqus

今年年初的时候，将博客整体从WordPress迁移到了新的Ghost平台，当初对原来的WordPress的评论的迁移没能做好功课，以至于在大半年的这段时间里都一直没能完整迁移。一开始以为Disqus的默认账户系统无法兼容WordPress的邮件帐号，现在看来我实在太傻太想当然了，这么大的市场，Disqus又怎么会放弃？所以，**将评论从WordPress迁移到Disqus是可行的**，只不过步骤可能会有点繁琐。

大概的操作步骤是：

1. 在原来的wordpress上安装disqus的插件，通过插件管理页面上的“导出到disqus”功能，将wordpress的评论同步导出至Disqus的系统里。
2. Disqus会将数据导入到其系统中，对于没有对应Disqus帐号的邮件地址，在Disqus里对应的链接就是空的，但不影响评论的显示。导入的过程会持续一段时间，完成后系统将会有邮件通知。
3. 如果URL的规则已经有所更改，那么就需要将旧的URL映射到新的URL上并合并重复的评论内容。这需要使用Disqus的URL Mapper功能，通过上传一个CSV的URL映射表，将所有相同URL的评论合并起来。

对于我这种已经迁移至Ghost的用户，为了不影响线上正在运行的博客，还得先用子域名和数据库备份恢复原来的wordpress，然后再从Disqus导出URL做整体的URL映射。

经过了一系列折腾的步骤之后，我总算是找回了原来博客的900多条评论。对于原打算放弃的这近千条评论，却在短短几小时里失而复得，还是感觉很开心:) 

把以前的留言板的评论定向到了About页面，看到上一条评论时间是7个月前，果然很是怀念啊～