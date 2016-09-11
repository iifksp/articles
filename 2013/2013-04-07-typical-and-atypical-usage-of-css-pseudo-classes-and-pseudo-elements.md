# CSS伪类与CSS伪元素的典型与非典型应用

在上一篇《[CSS伪类与CSS伪元素的区别及由来](https://swordair.com/origin-and-difference-between-css-pseudo-classes-and-pseudo-elements/)》里，已经详细区分了伪类和伪元素。但在这篇讲应用的文章里，其实并不需要这么明晰的概念区分。本文也不会再重提单冒号和双冒号之间的区别，本文更关注如何更好的使用伪元素和伪类——通过罗列一些典型甚至是非典型的用法，讲述伪类和伪元素的一些使用上的小技巧。在这个过程中，**始终忽略浏览器的支持情况**，任何一个例子无法正确运行的话，请先检查一下你当前的浏览器，是否赶上了这个时代的浪潮～

当然，一开始都是一些比较常规的内容，如果你对这些内容已经了然，可以直接跳到后面“非典”的部分:)

## 列表处理

伪类与伪元素很大部分常规用途是在处理列表的。特别是 `:nth-child()` 这类结构伪类。

### 页脚链接

页脚链接可能是一个最为合适的例子，因为例子里伪元素和伪类并用。很多网站的页脚链接是用竖线分隔的罗列`<a>`标签形式。在这种页脚链接里使用伪类和伪元素的组合，就可以让链接的列表的HTML代码变得非常干净整洁：

```
ul.list{list-style:none;}
ul.list li{float:left;}
ul.list li:after{content:"\00a0|\00a0";}
ul.list li:last-child::after{content:"";}
```

但当前，放眼望去几乎不会有人这么做。绝大部分依然使用旧的方式。除了一些维护上的考量，习惯也是一个主要原因。熟优熟略这里不讨论，这里只是作为最典型的例子展示它们的应用。

### 最后项，奇偶项，倍数项

另一个广为人知的例子是：去掉列表最后一个元素的边框或者边距。在这种情况下，我们似乎已经习惯了给最后一个元素加上一个类似`last-item`的类名，然后单独写一条CSS规则。

```
ul.list li{margin-right:10px;}
ul.list li.last-item{margin-right:0;}
```

实际上，我自己在写的时候也会偏向一个早就习惯了的做法。在时间限定的情况下，不会考虑激进的方案，也不会考虑类似负边距或者超界显示等等优化方案，而只是一味的寻求最为保险和熟悉的习惯上的东西。习惯是可怕的顽固，所以可能今后还是会有很长一段时间不会写成下面这样：
```
#ul.list li{margin-right:10px;}
#ul.list li.last-child{margin-right:0;}
```

隔行背景色可能是奇偶项使用最多的情况，但现在又有多少人已经**完全**使用`odd`和`even`来替代额外添加的类名？

```
tr:nth-child(odd){background-color:#f4f4f4;}
tr:nth-child(even){background-color:#f9f9f9;}
```

还有诸如 Gallery 的特定项的边距，伪类的处理也不错不是嘛？
```
li:nth-child(3n) {margin-right:0;}
```

不过眼下，似乎更多人用负边距或者`inline-block`来处理，甚至直接让透明边距顶出容器，优化的手法已经很多，而且在维护性上甚至更有优势。

### 彩色
这其实还是上面的命题，但却单独拿出来说。如今的网页越来越多彩，也就不免单独指定一些亮丽的颜色。还是有很多人将特定的颜色用特定的类名标注。然而，如果颜色的映射顺序不怎么重要的话的，也许只是要达到多彩的简单目的，完全可以这么做：
```
li:nth-child(1) a{color:#f30;}
li:nth-child(2) a{color:#c0f;}
li:nth-child(3) a{color:#90f;}
li:nth-child(4) a{color:#06f;}
li:nth-child(5) a{color:#0cf;}
```

应对各种区块的色系调整，伪类的使用可以大幅减少HTML里的单一用途的类名。顺着这个例子推荐一个网站：[xycss.com](http://xycss.com/)。这是一个由 [Jeff Starr](http://perishablepress.com/) 建立的关于响应栅格布局的网站，他撰写的框架 xy.css 也有非常多的可以学习的地方。当然在这里举其作为例子还是因为这个网站的首页和这个例子相符——伪类实现的多彩的色块。

## 清除浮动

这是一个被讲烂的命题，不过我必须将其列在伪元素的典型应用的列表里，因为一般而言，伪元素(:after)在这里总是最为常见。之前我写的《<a href="http://www.swordair.com/blog/2011/10/716/">再谈清除浮动</a>》一文已经详细阐述过，所以这里不再冗述。唯一想顺带提一下的是，虽然清除浮动方法各异，但我自己其实挺偏向空标签的，原因嘛，还是习惯吧...

## 非典型应用

所谓的非典型应用其实无非就是“不干正经事”的意思，与当年的`table`多少有些神似。这部分里有一些已经淘汰的手法，但为了纪念它们存在的痕迹，还是将它们例举和提及。

### 阴影
如今的浏览器大部分都已经支持`box-shadow`这个方便的阴影制造器，也正因为此，`box-shadow`在如今的使用中颇为频繁。关于`box-shadow`的文章我也至少写过4篇：

<ul>
	<li><a href="http://www.swordair.com/blog/2010/11/492">CSS3 box-shadow 详解(1)</a></li>
	<li><a href="http://www.swordair.com/blog/2010/12/510/">CSS3 box-shadow 详解(2)</a></li>
	<li><a href="http://www.swordair.com/blog/2012/04/881/">再谈box-shadow</a></li>
	<li><a href="http://www.swordair.com/blog/2012/11/950/#s3">那些CSS的细节问题(1)中第三部分有关“多重阴影数量的限制”的讨论和测试</a></li>
</ul>

虽然如今愈发觉得这个属性存在被滥用的危险，但是实际使用时，制造阴影往往需要和伪元素一起使用，特别是当页面中的某些布局需要额外元素制造阴影的时候。由于凡是支持`box-shadow`的浏览器都支持伪元素，所以几乎任何时候要添加`box-shadow`，伪元素都是考虑范围之一。比如，给全局网页加上一个顶部的阴影渐变：

```
body:before{content:"";position:fixed;top:-15px;left:0;width:100%;height:15px;box-shadow:0px 0px 15px rgba(150,150,150,.8);}
```
你可以直接在我的网页顶部看到效果(2013年下半年改扁平化之后应该会移除)。

### CSS调试

之前写的[调试样式：debug.css](/debugging-style-debug-css/)，部分内容是有关 `:empty` 伪类的应用。
```
div:empty,
span:empty,
li:empty,
p:empty,
td:empty,
th:empty{padding:1em;background-color:#ff0 !important;}
```
`:empty`可以在开发过程中以可视化的方式过滤空标签。

### 额外的提示

比如给相应的链接增加相应的内容提示:
```
.example-block:before {
   content: "Example:";
}
```
在我的印象里，早期的W3c标准文档里就是类似这么干的。这种额外信息的提示源自伪元素和CSS Content的配合使用，更多例子推荐阅读 Chris Coyier写的 [CSS Content](http://css-tricks.com/css-content/)

### 内描边和背景透明

这两个算是浏览器在CSS2的大时代背景里夹缝里生存的trick，如今看来早已经过时。现在的内描边可以用`box-shadow`轻而易举的完成，透明背景则需要用到CSS的RGBA以及IE滤镜。但当时，不知道是谁想出用四角绝对定位一个伪元素，然后画边框来实现内描边，应用opacity和IE透明滤镜来实现背景透明。虽然过时，但每每提起这种陈年旧事总是感觉好怀念啊～

### 最后

最后的最后，说起伪元素的应用，不得不提的是一个相当妖孽的网站：[One Div](http://one-div.com/)，用一个DIV拯救世界吧，少年！查看代码就会发现，其中充斥了伪元素和`box-shadow`，这么多把阴影当画笔的图标，当真是，妖孽极了...

## 结语
这篇写了很久，思路断断续续的，写的不好，请见谅。水平有限写的不到位的，欢迎吐槽。

这期间浏览器翻天覆地的变化，让我觉得写些东西出来过不多久就要过时似的...Opera放弃内核，Chrome和Webkit分道扬镳，现在无法判断这些事是好是坏，总而言之，不学习一直停留着的话，真是要被洪流给吞了的...加了个油！
