# CSS3 box-shadow 详解(2)

人一旦被一些繁杂的事消耗掉精力的话，即使有时间静下心来写篇东西也变得那么的不易啊，呵呵。题外话就不多说了，真没想到这篇早就构思好的东西写了这么久...

~~阅读本文请使用chrome，Opera或者FireFox浏览~~(23 December 2013: 由于迁移到ghost，其早期版本对HTML的支持不够理想，所以迁移过程中将所有HTML例子切成了图片)

在上一篇的[CSS3 box-shadow 详解(1)](/details-on-css3-box-shadow-part-1/)里，我从标准的角度出发，详细地写到了box-shadow的标准的方方面面。但毕竟是游戏规则似的细节，读起来恐怕还是没什么味道，按照上次的安排，这篇里不再牵扯标准的任何细节性的东西，主要就是讲实际应用中，box-shadow能做些什么。

## 设计中的阴影

对于设计来说，阴影的重要性和颜色等同。视觉要表达的是“光”的美的规则，任何视觉归结到底是光的艺术。当然，有光就有影。要表现空间和层次，通常的做法无非就是这3种：**阴影**、**渐变**和**透视图**。这三者是让网页脱离平面化而表现出层次的常用手法。

现在流行的设计里总是使用了大量的阴影，看看Vista、win7里夸张的box阴影，mac里的阴影比比皆是。box-shadow赋予了我们阴影样式，使得我们可以不再总是依赖于图片的使用。当然box-shadow最为常用的做法就是“当做阴影”来使用，但它的用法却远不止如此，甚至还能“管些其他样式的闲事”。虽然我不推荐box-shadow做太多越权的事，但在当前有些属性还不完善的情况下，box-shadow的良好支持不失为一种解决途径。

## 阴影
当然它本来就是阴影:) 。我们可以为网页元素中的任意的box添加视觉上的阴影表现，这正是box-shadow本来要做的。

![](https://swordair.com/content/images/2013/Dec/box_shadow_detail_2_001.png)

虽然基于标题“详解”二字我很想写的更全面些，但是对于box-shadow阴影层面上的使用，我还是决定略过。这方面的使用可以参考css3.info上的 [Box-shadow, CSS3中最好的新特性之一](http://www.css3.info/preview/box-shadow/)(可能有时被墙)。其它国内的关于box-shadow的阴影使用的文章也已经相当的多了，所以这里就不再多说了。 因为这是box-shadow最基本使用方法，并没有什么特殊之处。

## 边框
除了把阴影用作阴影以外，我们还能拿它做什么？**由于box-shadow拥有扩展半径这个值，使得阴影具备了充当边框的能力**。看看下面的阴影模拟的边框：

![](https://swordair.com/content/images/2013/Dec/box_shadow_detail_2_002.png)

实际上，用阴影充当边框，其效果是似边框而非边框，阴影充当边框和真正的CSS边框有着本质的不同。如果你的眼力够强悍，应该是可以看出上面两者的不同的——虽然他们的尺寸完全相同外表也毫无新差别，但是左边的box要比右边的低那么一点点，是的，一个像素。

当我们加粗边框之后，这种表现就变得异常明显：

![](https://swordair.com/content/images/2013/Dec/box_shadow_detail_2_003.png)

这和我们之前讲的阴影不影响布局的结论不谋而合——虽然边框被计算了宽度，但浏览器忽略阴影。因为这个特点，阴影所模拟的边框可以更加自由的使用，当然必须注意层级关系和覆盖。并且恰恰是因为可以覆盖，所以在没有绝对和相对定位的情况下，在普通流中，都可以将边框叠加！

事实上，阴影创造出的边框远不止如此。我在前篇多次提到的“多阴影”，在实现边框上也是大有作用。

![](https://swordair.com/content/images/2013/Dec/box_shadow_detail_2_004.png)

多阴影不仅仅使得阴影可以模拟多多层边框的情况，更重要的是，配合box-shadow的扩展半径，我们更是能造出单层的多色边框。或者在边框上加上什么修饰也变得异常简单。

![](https://swordair.com/content/images/2013/Dec/box_shadow_detail_2_005.png)

所以借助于box-shadow，我们可以模拟出许多种曾经让人皱眉头的边框，当然这是有限定的，并且适用面其实并不大。但是当我们恰当的使用的时候，仍然有突出的效果。同样，使用box-shadow作边框有很多要注意的地方。**注意使用时的层级关系**（前篇有详细讲过），**防止阴影出乎意料外的覆盖（毕竟其不影响布局）**，最后就是**要考虑到阴影的大小始终是跟着box的高宽的**，所以box宽高宽变化必须被考虑在内。实际上box-shadow的边框同样有相当好的自适应效果，但同时可能自适应出来的效果并不是设计者所希望的那样。

好了，关于伪造边框的使用就写到这里。其实box-shadow对于边框发挥余地还是相当大的，配合模糊、扩展半径和多阴影这几个关键要素，我相信还能够伪造出更加有趣的边框。为了引出下面的一个标题，我们看看下面这个多阴影的例子：

![](https://swordair.com/content/images/2013/Dec/box_shadow_detail_2_006.png)

将一个多阴影边框模糊后的对比效果如上面的左右所示——模糊了30px。模糊意味着边界的不明显，这其实和一个概念很相近——渐变。如果你看到了边框各个边框因为模糊而融合在一起，那么这正是下面所要讲的内容。

## 渐变
如果你是一位设计师，一定不难理解模糊和渐变的裙带关系，这就是我们最后要利用模糊的原因。box-shadow的模糊值，我们可以利用这个值伪造渐变效果。

也许你要问为什么不使用CSS3的渐变呢？这有两个原因：其一，各个浏览器对于渐变的支持的使用方法上，差别较大。其二，伪造渐变和伪造边框一样，在特定场景下是有用的。所以用box-shadow伪造渐变是有价值的。

不幸的是，**现代浏览器对于模糊的支持上仍然是有所不同的**。由于W3C虽然定义了box-shadow的各个值，但是却并没有限定box-shadow的模糊所使用的算法，这就直接导致了各浏览器之间的差别。在这点上，Firefox显然要高明些，它的模糊算法非常平滑，是完全可以当做渐变来使用的。然后是Opera，表现的一样不错，至少从Opera11（因为在我的印象里Opera10的表现也很一般，改天再看下）开始它的模糊算法也比较平滑。Chrome以及Safari相对来说，表现的都非常一般，在模糊的边缘是有些生硬的，甚至都能看到棱角...囧。(**更新**：Chrome 15 已经解决了模糊算法的问题)

下面是一个直观的比较，用的就是刚才上面的那个效果，差异是显而易见的：

![](https://swordair.com/content/images/2013/Dec/box_shadow_chrome_8.jpg)

![](https://swordair.com/content/images/2013/Dec/box_shadow_ff_3_6_13.jpg)

![](https://swordair.com/content/images/2013/Dec/box_shadow_opera11.jpg)

然而差异如此，把box-shadow用在表现简单渐变上也已经是足够的了。多阴影、内阴影、扩展半径以及模糊这四个点，已经使box-shadow具备了渐变的基本能力。

下面就是两个最简单的例子：

![](https://swordair.com/content/images/2013/Dec/box_shadow_detail_2_007.png)

当然，限于模糊算法，模拟的渐变跨度并不能太大。左右渐变已经非常明显的变得很“不线性”，并且这种情况在chrome下会变得更加的明显。换言之，这种模拟的渐变可能比较适合用在按钮、tab标签等小件的上下渐变上。

其实我博客的自己的主题里是大量使用了box-shadow的(现在的主题已经换过了，已经移除了大部分阴影)(23 December 2013: 迁移到Ghost之后暂时用默认主题)。但大部分都是非常浅的#ccc甚至是#eee这种非常弱的灰色调。因为本来自己的主题就是各种色调的灰线和非常淡的灰色块。举个box-shadow使用上的例子，我觉得自己的导航上的tab标签会比较适合：

![box shadow tab](https://swordair.com/content/images/2013/Dec/box_shadow_tab.png)

单单一个tab标签上，我就用了4个阴影，一个用来投射阴影，一个用来制造底色渐变，一个用做白色的勾线渐变，最后的字体的白色阴影则是制造雕刻的立体感。不过，一切都是那么的淡，所以几乎是看不出来的。但是我仍然相信，在灰度表现较好的显示器上，这点想法还是多少能够表现出来的。我不敢说它有多美观，甚至甚少有人能看出用了多少阴影，但我觉得这么淡足够了，毕竟这是细节，丰富的它的时候拘泥其中，但是一旦作为一个主体的一小部分，是可以完全忽略的。

```
#top-nav ul li{
	float:left;
	margin:0 0.5em 0 0;padding:0;
	-webkit-box-shadow:0px 0px 6px #eee;
	-moz-box-shadow:0px 0px 6px #eee;
	box-shadow:0px 0px 6px #eee;
}
#top-nav ul li a{
	display:block;
	background-color:#f9f9f9;
	padding:0.2em 0.8em;
	text-shadow: #fff 0px 1px 0px;
	border-top:1px solid #999;
	border-left:1px solid #999;
	border-right:1px solid #999;
	-webkit-box-shadow:inset 0px -7px 15px -7px #dfdfdf, inset 0px 0px 0px 1px #fff;
	-moz-box-shadow:inset 0px -7px 15px -7px #dfdfdf, inset 0px 0px 0px 1px #fff;
	box-shadow:inset 0px -7px 15px -7px #dfdfdf, inset 0px 0px 0px 1px #fff;
}
```

虽然大费周章的用CSS做这些PS只需瞬间完成的事确实非常蛋疼，就像CSS3一开始人们热衷于用圆角和渐变绘制各种图标一样显得有些毫无意义。这些都是事实。但仅仅因为如此而放弃对于新特性的探索，也绝对是蛋疼的。

总结就不写了，只是没想到这篇文章写这么久(笑)。匆匆成文，写错的地方还请各位随意指出。

## 扩展阅读
- http://www.w3.org/TR/css3-background/#the-box-shadow