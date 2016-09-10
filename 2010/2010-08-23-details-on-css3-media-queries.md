# CSS3 Media Queries 详解

说起CSS3的新特性，就不得不提到 Media Queries 。最近 Max Design 更新的一个[泛读列表](http://www.maxdesign.com.au/2010/08/20/some-links-293/)里，赫然就有关于 Media Queries 的文章。同时位列其中的也有前天我刚刚翻译的 [IE9, Opacity 和 Alpha](/ie9-opacity-and-alpha-translation/) 。

虽然标题相同，但本文并不是列表上 [CSS3 Media Queries](http://www.webdesignerwall.com/tutorials/css3-media-queries/) 的译文，因为原版有Demo有例子有图片，全文不长而且不难看懂，所以我也就不翻译了。基于自己已经了解到一定程度，所以就打算自己写。

CSS2中有已经定义了 [Media](http://www.w3.org/TR/CSS2/media.html) 的部分，包括类型、组别和规则等。CSS并非为了显示器而创造，而是应用于各种各样的媒体，比如常见的显示器，越来愈多的手持设备，可能略显过时的电视机等等。

而 Media Queries 的引入，其作用就是允许添加表达式用以确定媒体的环境情况，以此来应用不同的样式表。换句话说，其允许我们在不改变内容的情况下，改变页面的布局以精确适应不同的设备。

引用[CSS3 Media Queries](http://www.webdesignerwall.com/tutorials/css3-media-queries/)里的直观的 [DEMO](http://www.webdesignerwall.com/demo/media-queries/)，当浏览器宽度改变时，应用的CSS发生变化。而这一切，原本需要 JavaScript 的控制才能做到。

Web移动化的趋势越加明显，各种手持设备的发展现在完全引领着整个互联网的发展。虽然国内受到很多制约，但是这种浪潮却无法阻挡。前段时间jQuery宣布mobile计划，也加速了这种变化。Media Queries 不久的将来应该就会被广泛使用，以更好的支持新兴设备比如iPad。事实上， jQuery 甚至有 [Media Queries的插件](http://plugins.jquery.com/project/MediaQueries)。

下面来看看 Media Queries 做了什么。

为了展示我拙劣的PS技术，以及为了不让我的伪技术博客过于沉闷，我画了下面这张图：

![media queries](https://swordair.com/content/images/2013/Dec/media_queries_ps.png)

一个三栏布局，在屏幕变窄的情况下，变成1栏布局，甚至是消除多余两栏而只留下通常的内容的第2栏。Media Queries是如何工作的？

先看看 link 标签的写法：
```
<link rel="stylesheet" type="text/css" href="swordair.css" media="screen and (min-width: 400px)">
```
在media属性里：

- **screen** 是媒体类型里的一种，CSS2.1定义了[10种媒体类型](http://www.w3.org/TR/CSS2/media.html#media-types)
- **and** 被称为关键字，其他关键字还包括 **not**(排除某种设备)，**only**(限定某种设备)
- **(min-width: 400px)** 就是媒体特性，其被放置在一对圆括号中。完整的特性参看 [相关的Media features部分](http://www.w3.org/TR/css3-mediaqueries/#media1)

媒体特性共13种，可以说是一个类似CSS属性的集合。但和CSS属性不同的是，媒体特性只接受单个的逻辑表达式作为其值，或者没有值。并且其中的大部分接受 min/max 的前缀，用来表示 大于等于/小于等于 的逻辑，以此避免使用 `<` 和 `>` 这些字符。

根据文档，我还是列张表吧：

<table>
	<tr><th>Media features</th><th>Value</th><th>Applies to</th><th>Accepts min/max</th></tr>
	<tr><th>width</th><td>length</td><td>visual and tactile media types</td><td>yes</td></tr>
	<tr><th>height</th><td>length</td><td>visual and tactile media types</td><td>yes</td></tr>
	<tr><th>device-width</th><td>length</td><td>visual and tactile media types</td><td>yes</td></tr>
	<tr><th>device-height</th><td>length</td><td>visual and tactile media types</td><td>yes</td></tr>
	<tr><th>orientation</th><td>portrait | landscape</td><td>bitmap media types</td><td>no</td></tr>
	<tr><th>aspect-ratio</th><td>ratio</td><td>bitmap media types</td><td>yes</td></tr>
	<tr><th>device-aspect-ratio</th><td>ratio</td><td>bitmap media types</td><td>yes</td></tr>
	<tr><th>color</th><td>integer</td><td>visual media types</td><td>yes</td></tr>
	<tr><th>color-index</th><td>integer</td><td>visual media types</td><td>yes</td></tr>
	<tr><th>monochrome</th><td>integer</td><td>visual media types</td><td>yes</td></tr>
	<tr><th>resolution</th><td>resolution</td><td>bitmap media types</td><td>yes</td></tr>
	<tr><th>scan</th><td>progressive | interlace</td><td>"tv" media types</td><td>no</td></tr>
	<tr><th>grid</th><td>integer</td><td>visual and tactile media types</td><td>no</td></tr>
</table>

那么，回到刚才的那条 Media Query，media="screen and (min-width: 400px)" 的意思就是当屏幕的宽度大于等于400px的时候，应用此条CSS。

一个 Media Query 包含一种媒体类型，如果媒体类型没有指定，那么就是默认类型all，比如：
```
<link rel="stylesheet" type="text/css" href="example.css" 
	media="(max-width: 600px)">
```

一个 Media Query 包含0到多个表达式，表达式又包含0到多个关键字，以及一种媒体特性，比如：
```
<link rel="stylesheet" type="text/css" href="example.css" 
	media="handheld and (min-width:20em) and (max-width:50em)">
```

**逗号(,)**被用来表示并列，表示或者。比如下面的例子表示此CSS被应用于宽度小于20em的手持，或者宽度小于30em的屏幕：
```
<link rel="stylesheet" type="text/css" href="example.css" 
	media="handheld and (max-width:20em), screen and (max-width:30em)">
```

**not** 关键字用来排除符合表达式的设备，比如：
```
<link rel="stylesheet" type="text/css" href="example.css" 
	media="not screen and (color)">
```

再看些其他例子(不准确，只是用来说明)：
```
<link rel="stylesheet" type="text/css" href="styleA.css" 
	media="screen and (min-width: 800px)">

<link rel="stylesheet" type="text/css" href="styleB.css" 
	media="screen and (min-width: 600px) and (max-width: 800px)">

<link rel="stylesheet" type="text/css" href="styleC.css" 
	media="screen and (max-width: 600px)">
```
上面将设备分成3种，分别是宽度大于800px时，应用styleA，宽度在600px到800px之间时应用styleB，以及宽度小于600px时应用styleC。这其实是一个CSS覆盖的问题，所以当宽度正好等于800px时该应用那个样式？答案是styleB，因为前两条表达式都成立，后者覆盖了前者。

所以说上面的例子虽然能工作，但是不准确。这个例子正常情况应该这样写：
```
<link rel="stylesheet" type="text/css" href="styleA.css" 
	media="screen">

<link rel="stylesheet" type="text/css" href="styleB.css" 
	media="screen and (max-width: 800px)">

<link rel="stylesheet" type="text/css" href="styleC.css" 
	media="screen and (max-width: 600px)">
```

并非所有的浏览器都支持Media Queries，那么这些浏览器怎么看待Media Queries？

Media Queries是CSS3对于Media Type的一个扩展，所以不支持Media Queries的浏览器，应该仍然要识别Media Type，但是IE只是简单的忽略了样式。**only** 关键字可能显得有些多余，对支持Media Queries的浏览器来说确实是这样，因为加不加 only 没有影响。only的作用，很多时候是用来对那些不支持Media Queries但是却读取Media Type的设备隐藏样式表的。比如：
```
<link rel="stylesheet" type="text/css" href="example.css" 
	media="only screen and (color)">
```

- 支持Media Queries的设备，正确应用样式，就仿佛only不存在
- 不支持Media Queries但正确读取Media Type的设备，由于先读取到only而不是screen，将忽略这个样式
- 不支持Media Queries的IE不论是否有only，都忽略样式



最后再来看看 Media Queries 的支持情况。不出意外的，IE678全部出局，但是IE9幸免。根据IEBlog上的这篇 [HTML5 and Same Markup](http://blogs.msdn.com/b/ie/archive/2010/05/05/html5-and-same-markup-second-ie9-platform-preview-available-for-developers.aspx) 来看，IE9支持Media Queries。至于其他浏览器，同样不出意外的，全部支持Media Queries。

**完整的支持情况罗列成如下表：**

<table>
	<tr><th>IE6</th><td><span style="color:red;">不支持</span></td></tr>
	<tr><th>IE7</th><td><span style="color:red;">不支持</span></td></tr>
	<tr><th>IE8</th><td><span style="color:red;">不支持</span></td></tr>
	<tr><th>IE9</th><td><span style="color:green;">支持</span></td></tr>
	<tr><th>Chrome5</th><td><span style="color:green;">支持</span></td></tr>
	<tr><th>Opera10</th><td><span style="color:green;">支持</span></td></tr>
	<tr><th>Firefox3.6</th><td><span style="color:green;">支持</span></td></tr>
	<tr><th>Safari4</th><td><span style="color:green;">支持</span></td></tr>
</table>

其他定义media的方法比如@media等，应用Media Queries的方法相同，所以不在重复叙述。本文如有任何问题请留言告诉我，谢谢:)