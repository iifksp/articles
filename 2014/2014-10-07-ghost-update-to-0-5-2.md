# Ghost更新为0.5.2

收到了Ghost发来的邮件，Ghost可以升级到0.5.2，开始支持文章图片和自定义标题、描述。因为忙，没有第一时间升级，所以乘着国庆假期最后的几个小时，简单对比查看了新旧版本的区别。

我是从0.5.0升到0.5.2，版本越进虽小，但改变依旧明显。因为自定义内容多了很多，所以后台对于文章的属性设置，由一开始的弹出泡，移到了右侧抽屉的侧边栏里，操作也依旧十分直观方便。

![ghost 0.5.2 editor](https://swordair.com/content/images/2014/10/ghost-0-5-2-editor.png)

新版支持文章图片(Post Image)，Ghost终于可以在首页为文章配图。对此只需要在主题中添加post image的支持即可：
```
{{#if image}}<img class="post-image" src="{{image}}" alt="{{{title}}}" />{{/if}}
```
之前，我们却只能使用`{{content words="20"}} `这样的代码强行将内容里的图片输出在首页上。而现在，主页的代码终于可以换回概要输出`{{excerpt words="20"}}`，并指定想要的封面图片。但是，在图片指定时似乎也有些不友好，如果重复内容里的已存在的图片，上传后会形成两个地址。目前暂时可以直接将已存在的图片URL写入数据库避免重复上传。

上传图片的过程中，我发现Ghost的上传路径悄然改变了。虽然目录依旧按照日期组织，但之前的英文月份目录名已经被更为直观月份数字所取代。

另一个新功能是自定义文章标题和描述，Ghost会输出在 `header` 的 `meta` 标签里。而这对于SEO大有好处。后台界面贴心的使用了Google搜索结果的事实效果展示标题和描述，一目了然。对应数据库中的 `meta_title` 以及 `meta_description`，默认为空时，Ghost输出文章标题作为meta标题，但描述的输出为空。

每次更新我都还需要对core的中间件做一些改动，这是因为需要兼容部分古老的静态内容。中间件的代码改变也很大，这段在之前多个版本中使用的代码：
```
expressServer.use(subdir + '/tools', express['static'](...));
```
需要修改为：
```
blogApp.use('/tools', express['static'](...));
```

最后是一个更新的小插曲：因为google字体加载问题，每次更新都需要更改后台模板去掉google字体的引用。也许因为字体变化，导致后台的发布按钮处有些许错位。为此，还需要稍稍修改后台样式ghost.min.css。

对于这样一个简单的博客系统而言，更新着实麻烦了些。但每次更新的进步都看在眼里，就好象当年的WordPress的早期版本一样。看着Ghost长大，希望它能继续简洁下去，而不要去走WP的老路。