众所周知WordPress会对编辑器里的内容再格式化一遍，比如自动分段。但有些时候这些添加的格式反而也会让人很头疼。最近帮助一个客户处理类似的问题，遇到了 WordPress 在 `input` 和 `select` 前会插入额外的换行即 `<br>`，从而彻底破坏了页面样式。

随后，我用自己博客做了相同的Test，没有发现相同问题，可见格式化和版本有直接关系。客户使用的是WP3.3.2，而我始终更新 WordPress 到最新版本(虽然WP越来越臃肿，但软件更新还是必须的)。然后我查了一下，看到是一个 `wpautop` 函数在起作用，于是立刻查看了对应的代码，以及将新旧版本做了一个比对。这个函数位于 wp-includes 中的 formatting.php 这个文件里：
```
function wpautop($pee, $br = true) {
	//......
}
```
不同版本有细微区别，在3.5中的 `$br = true` ，在3.3.2里，则是 `$br = 1`。但作用确是相同的，设置为 false 或者 0 就可以阻止 WordPress 添加额外的换行。但这么做未免有些小题大作，很多时候我们甚至非常需要WP的自动格式，原因是并非所有的WP站长或者作者都是前端开发人员，他们不可能用纯HTML代码来编写网站内容。所以，为了解决客户遇到的 `input` 和 `select` 前的额外换行并将其移除，其实有更温和的办法。

依然是在这个 wpautop 函数里，两个版本都有一句对 $allblocks 变量的定义，对比之后，它们的细微差别正好就是我所遇到的版本格式化差异。查看代码就会发现，这段代码涉及了一连串的正则替换，其中就有换行的添加。

WordPress 3.3.2
```
$allblocks = '(?:table|thead|tfoot|caption|col|colgroup|tbody|tr|td|th|div|dl|dd|dt|ul|ol|li|pre|select|option|form|map|area|blockquote|address|math|style|input|p|h[1-6]|hr|fieldset|legend|section|article|aside|hgroup|header|footer|nav|figure|figcaption|details|menu|summary)';
```

WordPress 3.5
```
$allblocks = '(?:table|thead|tfoot|caption|col|colgroup|tbody|tr|td|th|div|dl|dd|dt|ul|ol|li|pre|select|option|form|map|area|blockquote|address|math|style|p|h[1-6]|hr|fieldset|noscript|samp|legend|section|article|aside|hgroup|header|footer|nav|figure|figcaption|details|menu|summary)';
```

删除这个定义里对应标签就能避免这个标签被 WordPress 额外格式化，也就避免了额外的换行。

到这里，客户的问题就完整解决了。改动也非常小，而且等到他下次从3.3.2更新到3.5的时候，也不需要重置这个问题。因为随着版本的更新，他可能再也不会遇到这个问题了:)

顺便提上一句，其实从 WP 3.5 开始的那个媒体管理器非常好用，虽然WP越来越肥是众所周知，不过这次的更新我还是要顶它的！