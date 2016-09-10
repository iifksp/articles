# 调试样式：debug.css

之前写过一篇 [开发过程中的调试样式](/debug-stylesheet-in-development/)，讲述了著名的37signals团队如何用样式视觉化代码的缺陷。但那只是浅尝辄止地提及了一下，现在我整理了一份相对完整的样式表。

由于是一个工具，主要是以用为主。所以如果你不想看下面的详细解释和说明，你可以直接将下面的链接拖动到书签栏：

<a href="javascript:(function(){var elm = document.createElement('link');elm.setAttribute('rel','stylesheet');elm.setAttribute('type','text/css');elm.setAttribute('href','http://www.swordair.com/css/debug.css');if(typeof elm != undefined){document.getElementsByTagName('head')[0].appendChild(elm);}})();">Debug CSS by 葵中剑</a>

这是一个 js 的 bookmarklet，它的作用只是简单地在页面注入一段样式。在把它放到书签栏之后，打开你喜欢的网站，然后点击这个书签，看看发生了什么？:)

RSS可能看不到，你也可以复制下面的代码并保存为一个书签，或者直接复制到浏览器地址栏后回车，效果是一样的：

```
javascript:(function(){var elm = document.createElement('link');elm.setAttribute('rel','stylesheet');elm.setAttribute('type','text/css');elm.setAttribute('href','http://www.swordair.com/css/debug.css');if(typeof elm != undefined){document.getElementsByTagName('head')[0].appendChild(elm);}})();
```

然后我再开始详细说明这个样式都干了些什么——你自己也可以根据自己需要编写符合自身使用的调试样式，但通常包含我在下面讨论的几个要素。

## 什么是调试样式？
调试样式就是利用CSS选择器和特殊的样式，视觉化代码缺陷的手段。最常用的样式可能如下所示：

```
img:not([width]):not([height]) {
	border: 2px solid red !important;
}
```

这段代码将页面里的没有设置高宽的图片全部添加2px的红色边框，使得我们能从页面上一下子看到识别它。这类样式通常包含这几个特点：


- 因为能检查的大部分是属性，所以大量的使用属性选择器
- 有 !important 用于提升特殊性
- 大多通过鲜艳的背影和边框凸显问题元素


前两条没什么可说的，最后一条，在当前的浏览器背景下，边框已经可以有很多种替代方式，并且这些方式更好。调试样式应该尽可能避免与页面样式的冲突，所以用 `outline` 替代 `boder` 可以避免调试样式对布局的影响，因为 `outline` 并不像 `border` 那样占据空间。而另一个替代的边框标注的方案是 `box-shadow` 模拟的边框，因为其同样不占据空间。并且，我们调试使用的浏览器都支持CSS3。

也许你已经可以想到，还有哪些问题需要被调试样式标出。根据检验的严格程度，标出的内容也不尽相同，但一个相对严格的样式应该包含下面这些项：

- 行内样式：通常这可以容忍
- 缺失必要属性：典型例子就是 `img` 标签的 `alt`
- 属性为空值或常用占位值：典型例子就是 `a` 标签的 `href`
- 元素内容为空：很常见，但能避免
- 废弃的属性：应尽量避免
- 废弃的元素：应尽量避免


这个列表自上而下是有顺序的，这样做是基于样式复写的考虑。当然也是个人习惯，我并不喜欢在调试样式里添加太多颜色和样式区分，线型和线粗就已经能很好的反应问题的严重程度。实际使用的时候配合3种颜色足以表示所有的情况。

本来打算把上面列出来的项逐个说明，不过现在觉得那样的话这篇东西就太长了。所以就请直接点击查看最终的代码吧:

[debug-strict.css](http://www.swordair.com/css/debug-strict.css) 以及 [debug.css](http://www.swordair.com/css/debug.css)

我提供了两个版本，前者在实际使用时效果太强，所以轻微修改了一下，两者相比有如下不同：


1. `img` 没有 `title` 非常常见，所以从debug-strict.css里移除了
2. 不设高宽的 `img` 也很常见，所以单独拿出来用蓝色线标注


完成这个样式表的时候也同时参考了 Eric Myers toolkit，不过那个 toolkit 已经严重过期。比如 `u`，`s`，`menu` 等元素在HTML5中，被赋予了新的使命，已经不再是废弃元素。相反，`table` 的 `summary` 等属性反而被新标准废弃。这样的情况还有很多，但毕竟**标准还是不断在更新完善中**。

这个样式表过期属性和元素参照的是最新版的 [HTML5 differences from HTML4](http://dev.w3.org/html5/html4-differences/)，当然还包括了一些这份草案没有列出的项。其实，这份样式表本来就是我翻译这篇文档的附带产物:)~

## 实际使用分析

前面讲了这么多，无非是纸上谈兵，下面就来点实际截图。

### AliExpress.com

正巧老东家AliExpress新首页上线，正好可以拿来做实验品~ 新首页很给力，风格清淡大气，个人感觉混合着亚马逊新版和ebay的味道，这页面一出就给人一种走C一路到底的感觉。

![aliexpress home](https://swordair.com/content/images/2014/Jan/aliexpress_home_compare.png)

图中，黄色部分是空标签，红色点框是缺少 `alt` 的图片，蓝色点框则是没有设置高宽的图片——实际上，大部分网站都多少会有一些这种“算不上问题的问题”。但通过注入一段简单的样式，就能很轻易的看出页面的特色。

比如，截图里的旧版，主banner下面只有第二块小banner没有 `alt`，应该不至于是文案不到位吧，难道亚历克苏斯同学你又调皮了......另外，旧版其它两个缺少 `alt` 的图片也显而易见了。新版缺少的 `alt` 则更多，不知是否刻意为之。但撇开微量的行内样式，无论新版还是旧版，整个页面在 debug.css 面前都极为出色。

### Taobao.com

惯例看一下淘宝的首页，不过页面截图太长，而且淘宝改版的频率还是挺高的，所以就不放截图了。

看起来相对是比较清爽的。从红色的1px点线可以看出，虽然类目里的样式相同，但有些却是行内样式，多少可以看出增改类目时留下的痕迹。

### amazon.com & ebay.com
amazon 和 ebay的结果可能出乎意料，大量的黄色块说明有很多的空标签；隐约的绿色边框表示其使用了废弃的属性——这不难理解，最常见的废弃属性就是 `table` 的一些老属性，哪里有 `table`，哪里废弃属性就越可能多。

![amazon ebay](https://swordair.com/content/images/2014/Jan/amazon_ebay_css_debug.png)

### Baidu & Google
百度和 Google 的对比也是很有趣，特别是 Google，用了如 `center` 这样的元素（实际上百度在早几年前也一样是用 `center` 的）。Google是我所知最喜欢用空标签的了，圆角/箭头/阴影等等，我都曾看到其用过空标签来实现过。所以黄成这样其实也在意料之内。

![baidu google](https://swordair.com/content/images/2014/Jan/baidu_google_css_debug.png)

### 360buy & 51buy & dangdang & amazon
除了亚马逊，其实国内的各B2C都相对比较清爽。当当你的广告还能在多一点吗？我擦，截个图要关你丫多少广告啊！！

![360buy 51buy amazon dangdang](https://swordair.com/content/images/2014/Jan/360buy_51buy_amazon_dangdang_css_debug.png)

## 总结
从上面的对比来说，没有什么绝对的结果，用什么代码，遗漏了多少东西似乎构不成足够的理由。有趣的是，在debug.css面前，明显是国内的网站比较干净——那并不是说国内的代码水平有多高，但似乎重视程度有差距。

对于动态加载的图片和链接，`alt` 之类的可能就不重要了，所以**单单看看样式的结果也并不尽准确**。闲来无事的时候，用这个样式测了下 w3c 的主页，真的几乎可以说无懈可击...

虽然我尽可能保证代码的准确，但毕竟一人为之，有错漏之处，欢迎指出。

最后我想说，好久不写这么长了，好累...果然是老了么...
