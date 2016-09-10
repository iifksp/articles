# 再谈inline-block

我们对 inline-block 并不陌生，在工作中经常会碰到类似的应用，比如 list 的居中，比如各种 gallery  的列表展示，细小到可能只是一个段落里的一个元素，大到也许是整个的布局，都可以见到。

引用怿飞的 [display:inline-block的深入理解](http://www.planabc.net/2007/03/11/display_inline-block/) ，对于其中已经提到的这里不再冗叙。这篇文章里，我试图谈论的是网络上对于 inline-block 的一些误解，以及个人对于 inline-block 理解上的一些补充。

看看标准是怎么说 [inline-block](http://www.w3.org/TR/2010/WD-CSS2-20101207/visuren.html#value-def-inline-block) 的：

> This value causes an element to generate an inline-level block container. The inside of an inline-block is formatted as a block box, and the element itself is formatted as an atomic inline-level box.

翻译下的意思，就是说，inline-block 使得一个元素创建一个行级的块容器，inline-block 内部被格式化成一个块元素，而元素本身则被格式化成一个行元素。

某种意义上换句话说，一旦一个元素被设置成 inline-block，那么对于它的父元素来说，它是一个行元素；但对于它的子元素来说，它是一个块元素。感觉有点像绕口令，如果还没有绕晕的话，我推荐 [Robert Nyman的说法](http://robertnyman.com/2010/02/24/css-display-inline-block-why-it-rocks-and-why-it-sucks/)：

> Basically, it’s a way to make elements inline, but preserving their block capabilities such as setting width and height, top and bottom margins and paddings etc.

> 它是一种使元素成为行元素的方式，但同时保留了块的设置高宽、上下margin、padding等的特性。

一言蔽之，一个**拥有块能力的行元素**，真是杰作。展开以后可以有这么多定义，但实则，inline-block 如其名所示，就是**行中块**。

如果回顾一下 inline-block 的使用历史，几年前，代码应该是这个样子：
```
display:-moz-inline-stack;
display:inline-block;
*display:inline;
zoom:1;
```
代码分别用于 firefox2-、现代浏览器、最后两行 IE6/7的hack。但现在主流浏览器早已完全支持 inline-block，需要额外考虑的只有 IE6/7 而已。但国内的情况是，恐怕在未来的很长的一段时间内，我们仍然不可避免地需要保留触发 IE6/7 haslayout 的样式并以此模仿出 inline-block。那么现在，其实代码只是减少了一行而已。
```
display:inline-block;
*display:inline;
zoom:1;
```

人们对于 inline-block 最大的误解可能是将其用作 galley 并代替 float 时，相邻两个元素间出现的**4px间隙**，并写了相应的hack，可能是下面这个样子：
```
ul{
	letter-spacing:-4px;
	word-spacing:-1em;
}
li{
	display:inline-block;*display:inline;zoom:1;
	letter-spacing:0;
	word-spacing:normal;
	vertical-align:top;
}
```

在父元素上将 letter-spacing 设为 -4px，并在子元素上设回 0。word-spacing 的设置并不是必须的，不过如果不设置，我的印象里 Opera 的表现会有问题。这段 hack 在所有的浏览器里表现良好，不用修改html，看起来也并没有什么问题。

但那个 4px 的间隙究竟是什么？其实稍微想想就能明白，那其实是一个空白符——由于inline-block表现成行元素，那么所有的空白符都会被浏览器计算在内并忽略到只剩一个，无论是回车、tab，还是空格。如果把列表结构写成下面这样：
```
<ul>
	<li>item-1</li>
	<li>item-2</li>
	<li>item-3</li>
	<li>item-4</li>
</ul>
```
每一项中间的空白符都是无法避免的，这也必然在两个 inline-block 间产生空隙——实际上那是个空白符，并且宽度受到字体类型以及字体大小的影响，对于 12px 的 Arial 的空白符，其宽度正好为 4px，4px 的 hack 由来仅此而已。因此，即使不打算改动 html 勉强使用 hack，4px 值也是不准确的，它会因为很多情况发生变化——**这个值应该用 0.34em**，但不论如何，这都是个糟糕的方式，虽然很多情况下它可以工作。

另一种hack的方法是在父元素ul上设置 `font-size:0;` 并在子元素上设置回正常大小，同样可以避免这个问题，不过代码一样显得凌乱和没有逻辑性。

如果改动 html 移除所有空白符的话，问题自然迎刃而解。问题在于代码洁癖和习惯可能使我们无法接受这种形式：
```
<ul>
	<li>item-1</li><li>item-2</li><li>item-3</li><li>item-4</li>
</ul>
```
或者这样：
```
<ul>
	<li>item-1</li
	><li>item-2</li
	><li>item-3</li
	><li>item-4</li>
</ul>
```
甚至是这样：
```
<ul>
	<li>item-1</li><!--
	--><li>item-2</li><!--
	--><li>item-3</li><!--
	--><li>item-4</li>
</ul>
```
如果是后台输出的话第一种其实也可以接受，前台必须准确要求后台不能输出多余的空白。至于其他两种，就有点那个啥......

基于这类原因，应该避免使用 inline-block 造成代码的混乱。但面对每个项都高度不确定的gallery形式，我们可能不得不选择使用inline-block，然后用  `vertical-align:top;` 等样式对齐其中一边。（inline-block 的 vertical-align 与 table-cell 的 vertical-align 容易产生混淆，inline-block 的 vertical-align 其实叫做 baseline-align 也许更合适）

一些老的注意事项这里不再罗列，因为相关浏览器已经不再有这类问题。如果有兴趣可以参考 [Cross Browser Support for inline-block Styling](http://foohack.com/2007/11/cross-browser-support-for-inline-block-styling/) 一文，里面有详细说到旧版Mozilla使用 inline-block 遇到的问题。


inline-block 往往表现的很神奇，原因在于它加夹在 inline 和 block 之间，两者的特性皆有，因而在一些 inline 和 block 无法解决的问题出现时，一想到 inline-block 就能突然化腐朽为神奇。当然，随着其使用的越来越普遍，开发者对其也越来越熟悉——一个格式化为行内元素的块容器(block container)，可视化格式模型里的精巧的部件。
