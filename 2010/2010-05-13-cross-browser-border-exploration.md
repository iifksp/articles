# 跨浏览器边框探索

虽然起了一个看似很牛逼的题目，但本文可以说完全是蛋疼的人的一种消遣~通常开发人员都有自己的放松方式。写文章用不了太久，倒是图材准备了老半天。谨以此文，让我们来消遣下各个浏览器对于边框的理解方式。

参与此次测试的浏览器包括windows下的几乎全部：ie6，ie7，ie8，ie9preview，chrome，firefox，safari，opera，seamonkey。各版本皆为网上下载的最新版。并且由于这次的测试里，**IE678的表现一致，firefox和seamonkey又是裙带，所以合并作IE8和firefox**。下图就是这次浏览器的截图：

![browser border icon](https://swordair.com/content/images/2013/Dec/browser__border_icon.jpg)

**上面的这种排列顺序是故意的**。下面的测试里就会显示出其原因，截图也都是按照这个顺序排列的。

首先看下面这张图，六种浏览器里显示了一个20X20的DIV，其边框为`top:2px`，其余1px。似乎没有什么不同。这是由于1px的细微让我们没有在意。(事实上这整个问题我们本来就不需要在意~)

![](https://swordair.com/content/images/2013/Dec/browser_border_0.gif)

先把问题简单化，单边2px，三边1px，并将结果放大5倍，我们能清楚的看到这种差异——opera和safari相反，ie8缺胳膊少腿，下面三个家伙一致~这里要注意到的是第二行3者在交汇处都有渐进效果。

![](https://swordair.com/content/images/2013/Dec/browser_border_1.gif)

也许我们该让这种效果变得更加明显。当仅仅加粗单边之后(10px)，IE8的怪癖显露无疑。opera和safari依旧相对。下面3个统一战线的渐进变的更加明显。

![](https://swordair.com/content/images/2013/Dec/browser_border_2.gif)

假如颠倒下会是怎样？3边加厚，结果是有些出乎意料的。opera的表现很诡异，换句话说，它顶上的那条线，在上面的例子里“优先”了只是一种巧合。IE表现的更加像opera，而不是它缺胳膊少腿的一贯形象。safari贯彻始终，并且，下面三个家伙的结果也是意料之内的。

![](https://swordair.com/content/images/2013/Dec/browser_border_3.gif)

回到最开始第一幅图的地方。用4种颜色分别画4条边。这里仍旧是放大5倍，现在就能更加清楚的看到IE8的做法。

![](https://swordair.com/content/images/2013/Dec/browser_border_4.gif)

在边框宽度都相同的情况下，每个浏览器都表现的挺好。对于边框斜角的应用我最早在《Eric Meyer on CSS 1》里看到。这种45度斜角使用上没有危险性，但是其他角度就未必是这样了，特别是用在双线的时候。
这里留意下面三个截图，FF和chrome，ie9的表现是不同的。可以看到渐进的处理方式不相同。chrome和ie9交汇处有白边，但是FF没有。

![](https://swordair.com/content/images/2013/Dec/browser_border_5.gif)

下面就是双线了，这又出了一个问题，就是ie9的双线是糊的...由于用的是4条等宽边框，其它看起来似乎都不错。

![](https://swordair.com/content/images/2013/Dec/browser_border_6.gif)

于是，换用一种糟糕点的角度呢？ie8首先不成框样，居然用空隙...感觉上非常粗糙。另外ie9糊的真水...

![](https://swordair.com/content/images/2013/Dec/browser_border_7.gif)

当我再尝试一些角度时，Opera也出问题了，它的边框显得多余很难看，简直和ie8有的一拼...

![](https://swordair.com/content/images/2013/Dec/browser_border_8.gif)

当然，本来浏览器对于线型的解释就不同，所以实线之外的，就没有什么参考价值了，这里仅做娱乐~~
dotted之后，尽显衣钵本色。

![](https://swordair.com/content/images/2013/Dec/browser_border_9.gif)

dashed又如何？同是webkit，表现却并不像刚才的dotted那么相同。ie8依旧很诡异，只有4个点，上下的线消失了...事实上，之所以这里把DIV加长到20x30，是因为，20x20，不够ie8显示这么宽的dashed线型，那4个点也会消失的~

![](https://swordair.com/content/images/2013/Dec/browser_border_10.gif)

下面就是最有趣的地方了，当DIV的尺寸变成20x40，线宽如图所示，结果，ie8和ie9是可以合体的！！！它们的合体可以组成一个完整的独一无二的solid框！一像素都不差~

![](https://swordair.com/content/images/2013/Dec/browser_border_11.gif)

当将DIV的尺寸加到的89x39的时候，ie8才完全显示出dashed框，并且此时chrome会突然变得和safari不同。

![](https://swordair.com/content/images/2013/Dec/browser_border_12.gif)

好了，也算的上是一轮浏览器的探索了。或者说消遣，再或者说无用工。因为没有结论，只有现象，而且还是无用的现象。但仍然希望这能勉强算的上是块砖头，期待高见~

![](https://swordair.com/content/images/2013/Dec/browser_border_dotted.gif)