# CSS3 box-shadow 详解(1)

这段时间确实比较繁忙，所以博客一直都没什么产出:) 

前段时间看到一篇《9个你现在可以使用的CSS3属性( [9 CSS3 Properties You Can Use Now](http://www.elliotswan.com/2009/07/27/9-css3-properties-you-can-use-now/))》，描述了当前可以渐进使用的CSS3的新的属性。但实际上由于种种原因，当前能使用的其实远达不到9个这么多。

本文讨论的就是其中之一，box-shadow，而且是从比较细节的角度。既然是详解就必然要写的详尽，于是，写到一半的时候才发觉内容太多，所以就分成了2个章节。这个章节里讨论box-shadow标准的描述，所以你能知道一些**非常细节的东西**，当然这些东西都没法使用，所以**如果你只是想了解怎么使用box-shadow，请跳过这一章**，直接阅读我写的《[CSS3 box-shadow 详解(2)](https://swordair.com/details-on-css3-box-shadow-part-2/)》，那里我会写记录些常用的或者是有趣的使用方法。

和往常一样，我先是查找了国内已经有人写过的内容避免自己写的和他们的有所冲突。和我之前写 [CSS3 Media Query](details-on-css3-media-queries) 时的没有多少好文的情况不同，关于box-shadow已经涌现出了很大的一批内容，所以后面的描述中我将援引他们。

正如标题所示，这里讨论的是CSS3的box-shadow，所以并没有打算讨论过时浏览器的模拟。关于模拟的这方面内容可以参看张鑫旭的《[CSS实现跨浏览器兼容性的盒阴影效果](http://www.zhangxinxu.com/wordpress/?p=711)》和《[CSS实现跨浏览器的box-shadow盒阴影效果(2)](http://www.zhangxinxu.com/wordpress/?p=976)》，或者是小鱼的《[跨浏览器实现投影(box-shadow)那点事](http://www.happinesz.cn/archives/1426/)》。

下面，确保你的浏览器是非IE，然后就开始正题吧:)

## 从标准开始

W3C标准里关于[box-shadow的定义](http://www.w3.org/TR/css3-background/#the-box-shadow)内容其实并不多，花上一小块时间就能看完。

<table>
	<tr><th>Name:</th><th>box-shadow</th></tr>
	<tr><td>Value:</td><td>none | <shadow> [ , <shadow> ]*</td></tr>
	<tr><td>Initial:</td><td>none</td></tr>
	<tr><td>Applies to:</td><td>all elements</td></tr>
	<tr><td>Inherited:</td><td>no</td></tr>
	<tr><td>Percentages:</td><td>N/A</td></tr>
	<tr><td>Media:</td><td>visual</td></tr>
	<tr><td>Computed value:</td><td>any <length> made absolute; any color computed; otherwise as specified</td></tr>
</table>

> The ‘box-shadow’ property attaches one or more drop-shadows to the box. The property is a comma-separated list of shadows, each specified by 2-4 length values, an optional color, and an optional ‘inset’ keyword. Omitted lengths are 0; omitted colors are a UA-chosen color.

这里没有多少值得说的地方，唯一需要关注的是box-shadow可以使用 **1个 或 多个**投影，类型可以是默认的投影，以及可选的阴影内嵌，这点在后面的应用中可以利用。表达式如下：
```
<shadow> = inset? && [ <length>{2,4} && <color>? ]
```
表达式非常直观，但不是经常看的人可能不是很习惯。这个属性定义了**2-4个长度值**，以及两个可选值**inset**和**color**。同时定义里也描述了默认值，长度默认为0，颜色默认为UA所选的颜色（通常是浏览器默认值黑色，但浏览器是有差异的）。或许mozilla给出的定义更为直观些：
```
box-shadow:  none | <shadow> [,<shadow>]*
where <shadow> is defined as:
inset? && [ <offset-x> <offset-y> <blur-radius>? <spread-radius>? <color>? ]
```
mozilla的这个定义应该更加完整，不仅表现出了多阴影的特性，就连值是什么都写的很清楚。

- 每一个box-shadow属性由一个或一组逗号分隔的&lt;shadow&gt;给出
- 每个&lt;shadow&gt;被定义为下面这些值的组合：
	- **inset**关键字，将外部投影转变为内部阴影。
	- 第1个长度**offset-x**代表阴影x轴向的偏移，正值向右，负值向左。
	- 第2个长度**offset-y**代表阴影y轴向的偏移，正值向下，负值向上。
	- 第3个长度**blur-radius**代表阴影模糊半径，不允许负值。
	- 第4个长度**spread-radius**代表阴影扩展半径，正值放大，负值缩小。
	- **color**代表投影的颜色。

所以通常最简单的调用是这样的：
```
box-shadow: 5px 5px;
```
当然这里不讨论浏览器前缀的问题，比如-webkit和-moz。对应到相应浏览器就必须加上各自的前缀。但之所以称box-shadow的支持度高，是基于标准上的完善度和浏览器的广泛度。

- firefox从Gecko 1.9.1（Firefox 3.5）引入此属性的支持，并在<a rel="nofollow" href="http://hacks.mozilla.org/2010/09/firefox-4-recent-changes-in-firefox/">FF4最近的更新</a>里去掉了-moz的前缀。
- Opera 10.5 pre-alpha开始支持此标准属性，并且属性前是不需要前缀的。
- Webkit核心是最早实现box-shadow的引擎，所以Safari 3开始就支持了box-shadow。虽然最初的实现上有些bug，不过很快就修复了。
- 同样是Webkit核心的chrome，自然也是支持的。webkit在最初支持的时候并没有附带前缀，但有趣的是，2009年10月，<a 
href="http://www.w3.org/blog/CSS/2009/10/01/resolutions_79">W3c突然将box-shadow 从 CSS Backgrounds and Borders Level 3 里移除了</a>，导致webkit重新引入了这个属性的前缀。所以当前，chrome使用box-shadow是需要前缀的。(**更新：最新版的Chrome 10已经移除了前缀**)
- 我很欣地看到IE9支持box-shadow......

developer mozilla有一份[box-shadow的浏览器支持列表](https://developer.mozilla.org/en/CSS/-moz-box-shadow)。所以这里讨论时说到 "box-shadow: 5px 5px;" 就是指这样的一种序列：
```
-moz-box-shadow: 5px 5px;
-webkit-box-shadow: 5px 5px;
box-shadow: 5px 5px;
```
回到这个最简的例子，实际上 "box-shadow: 5px 5px;" 在chrome里不会有效果，尽管标准定义里颜色是可选的，但是chrome在没有给出颜色的时候和firefox表现不同。也就是说这条规则不是没有生效，只是chrome似乎是表现成透明了。而firefox则会选择使用黑色来生成阴影。

基于这样的原因，我们还是应该直接给定box-shadow的颜色值。下面就是一个最简单的例子：

![box shadow example 1](https://swordair.com/content/images/2013/Dec/box_shadow_example_1.png)

在不给定颜色时chrome给人的感觉没有任何反应其实还有另外一个原因，那就是一个是否将阴影计算为内容的问题。举个例子，一个宽度100%的div设置偏移x轴50px的阴影，父容器overflow会将这多出来的50px怎样？如果把这个div放在body里结果又会是怎样？答案是**chrome完全忽略阴影，而firefox则撑开父元素出现了滚动条**。但到底谁的实现更为正确呢？下面会有详细说到。

标准里有一张非常直观的图，描述了box-shadow的工作方式。这张图直观到只要仔细看懂就几乎能明白box-shadow方方面面。

![box shadow spread blur](https://swordair.com/content/images/2013/Dec/spread_blur.png)

图中可以得到的信息很多。比如圆角、阴影扩展、模糊、内阴影以及padding值是如何影响阴影的。下面是另外的一些细节。

非零值的border-radius将会以相同的作用影响阴影的外形，不过同样是CSS3属性，**border-image不影响阴影的外形**。

就如同box模型的层次一样，阴影也有属于自己的层次。外阴影绘制在背景之下，内阴影绘制在边框(包括图片边框)之下背景图片之上。众所周知，背景图片的层级在背景颜色之上，所以整个层级关系就是：**边框 &gt; 内阴影 &gt; 背景图片 &gt; 背景颜色 &gt; 外阴影** 。

根据定义，**阴影不会触发滚动条，也不会增加可滚动区的大小**。刚才说到chrome完全无视阴影而firefox撑出了滚动条(**更新**：新Firefox已经修正了这个问题)，现在就这一点而言，似乎webkit显得更为标准。实际上无视阴影也是完全合理的，本来我们就不应该因为阴影样式而造成伪内容影响到布局。

多个阴影以逗号分隔列出的时候，他们从前到后依次被覆盖，**定义越前面的阴影有越高的层级，会覆盖在后面定义的阴影之上**。如上所述，阴影不影响布局(layout)，并依照层次覆盖其他box及其阴影之上，如下所示：

![box shadow example 2](https://swordair.com/content/images/2013/Dec/box_shadow_example_2.png)

关于box-shadow，当前就先讨论这么多。还有一些关于标准的东西尚比较零散，等我重新组织后再更新此文。然后下一篇《[CSS3 box-shadow 详解(2)](https://swordair.com/details-on-css3-box-shadow-part-2/)》我会详细讲述其应用。





