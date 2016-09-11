# Ghost更新为0.5.10

前段时间一直很忙碌，Ghost的多次更新都没能及时补上。虽然更新只需花几分钟时间，但坐定的机会却并不常有。

下载了ghost-latest.zip，由于每次都忘记了命令，还是得跟着更新文档来一遍。官方文档也是不断再改进的，对以前通用的更新步骤再做了平台的细化，看起来真的方便多了。更新后显示版本为0.5.10。

由于跳过了好几个版本更新，好几个新功能如今才刚刚用上。最重要的莫过于Tag管理器和导航了。

## Tag管理器

Ghost很容易不小心输错而生成垃圾标签，现在终于可以用管理器来删除这些无用标签而不必跑进数据库折腾了。管理器还提供了对tag页面的管理，可以自定义封面图和meta信息。

![Ghsot标签管理器](https://swordair.com/content/images/2015/03/ghost-tag-manager.png)

虽然用起来还是有点问题，特别是tag非常多的情况下，但ghost终于可以管理标签了，喜极而泣。

## 导航构建器和 `{{navigation}}`

Ghost一直都没有页面的导航控制，主题作者必须将代码直接写进模板，这点相当蛋疼。Ghost走到现在总体步伐相当稳健，对每一个特性的审视都很严格，若非是博客必须的功能可能永远不会加入线路图里，以防止Ghost变成WordPress那样的怪物。

![Ghost导航构建器](https://swordair.com/content/images/2015/03/ghost-navigation-manager.png)

好用的导航控制，支持拖放排序！足够简单和好用。更新后我第一件事就是把自己主题里的导航控制交给{{navigation}}来输出：
```
<!--<nav class="site-nav" id="site-nav">
    <li><a href="/">Home</a></li>
    <li><a href="/design/">Design</a></li>
    <li><a href="/paint-works/">Paint</a></li>
    <li><a href="/links/">Links</a></li>
    <li><a href="/about/">About</a></li>
</nav>-->
{{navigation}}
```
一下子清爽多了:)


## 其他新功能

- RSS图片输出
- 用户帮助菜单
- 对于Node v0.12以及io.js-v1.2的支持

现在最为期待的是Ghost可以再更新一个标签云和文章存档，再加上page的模板选择，如此一来，这个博客系统单就博客的基本功能就完全赶上臃肿的WordPress了。

虽然仅仅只是添加了些基础功能，但毫无疑问，这些功能都是你想要的，这也是为什么每次更新Ghost都特别欣喜，一路看着Ghost成长成你想要的那个样子，打心底里高兴:)