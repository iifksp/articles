# CSS定位机制之一：普通流

内容比较长，就分为了3篇，这是第一篇。下面是整个列表：

- **CSS定位机制之一：普通流**
- CSS定位机制之二：浮动(已坑)
- CSS定位机制之三：绝对定位(已坑)

虽然以前也时不时会翻阅CSS2.1的文档，不过这么认真的逐字逐句地看，却还是第一次。也许正像画漫画一样，和技术以及时间都没多大关系，缺少的可能只是一种心境。

由于没有搜到任何自己认为完整的关于文档流、浮动和绝对定位的中文文章，于是鼓起勇气决定自己来写篇。为此啃掉了CSS2.1里的 [8 Box model](http://www.w3.org/TR/CSS2/box.html) 以及 [9 Visual formatting model](http://www.w3.org/TR/CSS2/visuren.html) 。实话说，还真是看得有点头大，呵呵~

**文档流**，其实标准里根本就没有这个词。如果把文档流直译为英文就是 document flow ，但标准里只有另一个词，叫做**普通流**( [normal flow](http://www.w3.org/TR/CSS2/visuren.html#normal-flow) )，或者称为常规流。但似乎大家更习惯文档流的称呼，因为很多中文翻译的书就是这么来的。比如《CSS Mastery》，英文原书中至始至终都只有普通流 normal flow 这一词，从来没出现过文档流 document flow 。但是中文译本 “普通流” 和 “文档流” 却是交替出现的。**也正是这点，造成了现在网络上这两词的严重混淆。**

什么是普通流？简单说就是元素按照其在 HTML 中的位置顺序决定排布的过程。并且这种过程遵循标准的描述。

为了从不同角度说明，我采集了一些可能冗长、具体或者晦涩的其他人给出的定义：

- 将窗体自上而下分成一行行，并在每行中按从左至右的顺序排放元素，即为文档流。
- [Jennifer.Kyrnin@About.com](http://webdesign.about.com/od/cssglossary/g/bldefnormalflow.htm): 普通流是元素在多数情况下呈现在 web 页面上的方式。所有 HTML 都在块框( block boxes，块级元素 )或者行内框( inline boxes，行内元素 )中。
- [Rainbo Design](http://www.rainbodesign.com/pub/css/css-positioning-2.html): 当浏览器开始渲染 HTML 文档，它从窗口的顶端开始，经过整个文档内容的过程中，分配元素需要的空间。除非文档的尺寸被 CSS 特别的限定，否则浏览器垂直扩展文档来容纳全部的内容。每个新的块级元素渲染为新行。行内元素则按照顺序被水平渲染直到当前行遇到了边界，然后换到下一行垂直渲染。这个过程被成为普通文档流。

可见，把流( flow )理解为流程，完全说的通。普通流即是**通常情况**下的元素排布和定位流程。但其实在CSS2.1RC里，普通流的本质是三种定位机制( [Positioning schemes](http://www.w3.org/TR/CSS2/visuren.html#positioning-scheme) )之一，被定义为：

> Normal flow. In CSS 2.1, normal flow includes block formatting of block boxes, inline formatting of inline boxes, relative positioning of block or inline boxes, and positioning of run-in boxes.

这个过程包括了块格式化( [block formatting](http://www.w3.org/TR/CSS2/visuren.html#block-formatting) )，行内格式化( [inline formatting](http://www.w3.org/TR/CSS2/visuren.html#inline-formatting) )，相对定位( [relative positioning](http://www.w3.org/TR/CSS2/visuren.html#relative-positioning) )，以及 [run-in boxes](http://www.w3.org/TR/CSS2/visuren.html#run-in)的定位。似乎和上面那些迥然不同，但是把这些分解开来，仍然是一致的。

另外，9.4 [Normal flow](http://www.w3.org/TR/CSS2/visuren.html#normal-flow)下还有一段：

> Boxes in the normal flow belong to a formatting context, which may be block or inline, but not both simultaneously. Block boxes participate in a block formatting context. Inline boxes participate in an inline formatting context.

这是段描述，不是定义。在普通流中的 Box(框) 属于一种 formatting context(格式化上下文) ，类型可以是 block ，或者是 inline ，但不能同时属于这两者。并且， Block boxes(块框) 在 block formatting context(块格式化上下文) 里格式化， Inline boxes(块内框) 则在 inline formatting context(行内格式化上下文) 里格式化。

我们知道，任何被渲染的元素都属于一个 box ，并且不是 block ，就是 inline 。即使是未被任何元素包裹的文本，根据不同的情况，也会属于匿名的 block boxes 或者 inline boxes。所以上面的描述，即是把所有的元素划分到对应的 formatting context 里。

组合上面的定义，并且姑且先把 formatting context 看做是一种范围限定，那么具体讲，普通流就是这样的过程：

1. 在对应的 block formatting context 中，块级元素按照其在HTML中的顺序，在其容器框里从左上角开始，从上到下垂直地依次分配空间、堆砌( stack )，并独占一行，边界紧贴父容器。两相邻元素间的距离由 margin 属性决定，在同一个 block formatting context 中的垂直边界将被重叠( <a href="http://www.w3.org/TR/CSS2/box.html#collapsing-margins">collapse</a> )。并且，除非创建一个新的 block formatting context ，否则块级元素的宽度不受浮动元素的影响(这就是浮动元素能盖在块级元素上面的原因)。 
2. 在对应的 inline formatting context 中，行内元素从容器的顶端开始，一个接一个地水平排布。水平填充、边框和边距对行内元素有效。但垂直的填充、边框和空白边不影响其高度。一个水平行中的所有 inline box 组成了名为 line box 的矩形区域。 line box 的高度始终容下所有的 inline box ，并只与行高有关。 line box 的宽度受到父容器和浮动元素存在的影响(这就是文本环绕浮动元素)。如果 line box 的宽度小于容器， line box 的水平排布就取决于 text-align 。如果 line box 的宽度大于容器，则截断 line box 并换行在新的 line box 中重新排布元素(截断处不应用 padding 和 margin 值)。如果 line box 无法截断，如单词过长或者指定不换行，则会溢出容器。 
3. 对这些 block box 和 inline box 进行相对定位，即相对于已排布的位置进行偏移。元素在其中保留原来所占用的空间。 

说了一堆东西，其实就只是在说如何排布元素而已。那些都非常容易理解，除了一个概念—— formatting context 。

什么是 **formatting context** ？ context 总是解释为上下文环境，那么格式化上下文就应该是指格式化时的前后关系。然而对此，标准里没有更多的定义和解释。

虽然 [mozilla developer center](https://developer.mozilla.org/en/CSS/block_formatting_context) 上没有关于 inline formatting context 的资料，但是却有关于 block formatting context 的描述：一个 **block formatting context** 是web页面可视化CSS渲染的一个部分，是一块 block boxes 排布以及 float 元素相互作用的区域。

用自己的话简言之，那是一个作用范围。可以把它理解成是一个独立的容器，并且这个容器的里box的布局，与这个容器外的毫不相干。

下面的这些情况，都会创建一个新的 block formatting context：

- 根元素
- 浮动或绝对定位的元素
- display 值为 inline-blocks ， table-cell 或 table-caption
- overflow 值为非 visible

虽然标准里没有提到根元素会创建新的 block formatting context ，但是mozilla提到了，并且这也解释了初始的一个上下文环境的建立。

这里有个建立( establishes )的概念，这个概念和建立容器块( [containing block](http://www.w3.org/TR/CSS2/visuren.html#containing-block) )的概念类似。比如，A是B父元素，当B被渲染时其位置和大小会参照一个容器块，这个容器块是由其父元素A建立的。是的，有点简单问题复杂化。虽然实质上父元素就是子元素的容器，但是过程中间却有个建立( establishes )的概念。并且这个创建的概念被应用于其他作用范围，包括 block formatting context 。

想想我们平常在做的事情。当一个父元素因为子元素浮动而导致高度为0的时候，也许我们会习惯的加上这样的规则：`overflow:hidden;zoom:1;` 。

`overflow:hidden` 不正是创建了一个新的 block formatting context 吗？那么 zoom:1 是怎么回事？这不得不提到 IE 私有的 hasLayout ，一个和 block formatting context 行为相仿的IE特产。对于 hasLayout ，本文不做讨论。可以阅读那篇有名的 [On having layout](http://www.satzansatz.de/cssd/onhavinglayout.html) ，[中文版](http://old9.blogsome.com/2006/04/11/onhavinglayout/) 由 [old9](http://old9.blogsome.com/) 翻译过，但是链接似乎暂时挂掉了，所以可以看看[蓝色理想上转载的版本](http://bbs.blueidea.com/viewthread.php?tid=2636904)。

这就是为什么浮动元素总是容纳浮动元素的原因——浮动元素创建了新的 block formatting context，其内部的布局不在和外部有关。比如内部的浮动清除不会再影响到外部，并且内部的浮动对于外部而言也是不可见的。这也是为什么《精通CSS》会说 “应用值为 hidden 或 auto 的 overflow 属性会自动地清理任何浮动元素” 的原因。同时，这也是为什么有的时候必须用清除浮动而不是设置overflow来使父容器容纳浮动元素，因为 “设置框的overflow属性会影响它的表现”。

举个实际当中的常见例子，比如<strong>垂直边距叠加问题</strong>：
```
<html>
	<head></head>
	<body>
		<div id="A" style="background:gray;margin-top:20px;">
			<div id="B" style="margin-top:10px;">
				我有10px m-t
			</div>
		</div>
	</body>
</html>
```
如果在现代的浏览器里运行，那么 B 的10px上边距将会和父元素 A 的20px上边距重叠起来，从而看起来就好像 B 没有上边距一样。就像前面说的那样，**同一个 block formatting context 中的垂直边界将被重叠**。那么不同 block formatting context 呢？

这种情况，如果不想加边框(` border:1px solid transparent;` )解决的话，那么就需要 A 建立一个新的 block formatting context 将 B 包裹起来，那么 A 的上边距和 B 的上边距就毫不相干了。可以用浮动，也可以加 overflow ，这要看具体情况。
```
<html>
	<head></head>
	<body>
		<div id="A" style="background:gray;margin-top:20px;overflow:auto;">
			<div id="B" style="margin-top:10px;">
				我有10px m-t
			</div>
		</div>
	</body>
</html>
```

因为一位朋友对上面的代码提了个问题，可能是我说的不够清楚的缘故，所以今天(2011/01/08)我补上一张图：

![block formatting context](https://swordair.com/content/images/2013/Dec/block_formatting_context.png)

再一个例子，回头看下上面过程里这样的一句：**除非创建一个新的 block formatting context ，否则块级元素的布局不受浮动元素的影响**。这是造成元素重叠的原因。比如下面的代码：
```
<html>
	<head></head>
	<body>
		<div id="A" style="background:gray;overflow:auto;">
			<div id="B" style="background:green;margin:10px 10px 0 0;float:right;">
				浮动元素m_10_10_0_0
			</div>
			<div id="C" style="background:red;">
				普通流块
			</div>
		</div>
	</body>
</html>
```
普通流块 C 在容器 A 里排布的时候，并不受浮动元素的影响，甚至当浮动元素 B 不存在。但是如果 C 创建了新的 block formatting context ，那么，普通流块c就会像 line box 一样受到浮动元素存在的影响而缩小。
```
<html>
	<head></head>
	<body>
		<div id="A" style="background:gray;overflow:auto;">
			<div id="B" style="background:green;margin:10px 10px 0 0;float:right;">
				浮动元素m_10_10_0_0
			</div>
			<div id="C" style="background:red;overflow:auto;">
				普通流块
			</div>
		</div>
	</body>
</html>
```
关于浮动和 block formatting context ，brunildo.org 上有一份不错的[参考](http://www.brunildo.org/test/FloatMarginOverflow.html)。

说了这么多了，其实概念仍然可以很简单。普通流仅仅只是一种定位的机制。而flow本身在标准里也可以作为一个动词，就如同按顺序一个个的拿出HTML元素然后放到页面上一般。只是要注意一下 formatting context 的概念，特别是 block formatting context ，因为其影响更大(包括边距重叠、浮动清除、元素重叠等)。

关于更多 block formatting context 以及例子，可以参看 [CSS 101: Block Formatting Contexts](http://www.yuiblog.com/blog/2010/05/19/css-101-block-formatting-contexts/) 或者 [Control Block Formatting Context](http://www.communitymx.com/content/article.cfm?page=1&cid=6BC9D)。

写到这里也就差不多了，有空的时候补几张图出来，所谓一图胜千言。由于技术所限，如文中有不当之处，请留言给我，非常感谢。
下次空段时间出来就要写浮动那货了: )

**Update 2012-11-04：**一晃两年多时间过去了，我居然还没写整章浮动，这是有多懒啊～这篇文章在网上的传播已经非常广泛，不过作为出处，这里只受到了google的照顾。百度搜索“普通流”一词确实就是自己写的东西，不过出现的是各种无节操转载，原作者已经彻底消失了...直到最近有网友问起一个问题，我才想起多年以前居然还写了这么篇文章，吐槽了一下《精通CSS》的翻译。时间，当真过得飞快啊，呵呵。



