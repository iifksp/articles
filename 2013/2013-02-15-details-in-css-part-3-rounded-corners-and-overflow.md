# 那些CSS的细节问题(3) —— 圆角边框和overflow

本篇是上篇《[那些CSS的细节问题(2) —— 圆角响应区](https://swordair.com/details-in-css-part-2-rounded-response-area/)》的一个简短的延续，因为在上篇的篇幅里实在已经容不下更多混乱的部分。


尽管现在圆角边框的教材已经烂了大街到处都是，并且这个属性已经是诸多CSS3属性里应用最为广泛的特性之一，但却丝毫无法掩盖这个属性一路走来的坎坷。时至今日，现代浏览器对圆角边框的解释仍然存在诸多问题，比如上篇[那些CSS的细节问题(2) —— 圆角响应区](https://swordair.com/details-in-css-part-2-rounded-response-area/) 讲到的对“响应区”影响的问题，实则也只是这个属性一路而来的问题之一而已。因为，单看一个属性确实很明了——无论是定义还是实际实现起来，然而，一旦这个属性和其他属性作用在一起，事情就没这么简单了。

下文中的内容，**皆以如下版本的浏览器为基准**：Chrome-24，IE-9，Firefox-18，Opera-12.12，safari-5.1.7。



回到标题，这里主要讨论的就是圆角边框 `border-radius` 和 `overflow` 作用在一起时产生的一些问题。为此，我做了一个简单的 [DEMO](http://www.swordair.com/demos/border-radius-and-overflow-scrollbar/)。你可以用不同的浏览器访问查看显示的效果，并查看源代码直接获知这个DEMO究竟在表达什么——虽然说起来也不复杂：就是在一个圆角`div`上添加各种不同的 `overflow` 值，内容和边界会表现成什么样？如果你懒得打开浏览器逐个查看，下面就是各种结果的截图(好吧，让我们忽略Safari吧~单单Chrome已经够我们受的了)：

![border radius and overflow](https://swordair.com/content/images/2014/Jan/border_radius_and_overflow.png)

Opera是可悲的，它无法藏主被超出的内容。其实早期不止是Opera存在这样的问题，`border-radius` 对 `overflow:hidden;` 无效的bug([459144](https://bugzilla.mozilla.org/show_bug.cgi?id=459144))以前也一样存在于 Firefox 里，不过现在已经被修复，只是Opera的动作似乎稍慢了。

整张图里，一眼望去最为奇葩的就是Firefox了，当`border-radius`和`overflow`的滚动条同时出现的时候，它居然是这样的一个形状！而这次行为一直的居然是Chrome 和 IE... 如果是你，你觉得当滚动条出现的时候应该是怎么样的？我觉得吧，应该是类似Chrome和IE9的那样，但由于没能考证到什么定义依据，所以也不敢确定。请确切知道的朋友告诉我一声～

最后一列 `overflow:hidden;` + `position:relative`; 似乎显得有些多余，其实不然。在我草拟这篇文章的时候，webkit存在一个bug（[50072](https://bugs.webkit.org/show_bug.cgi?id=50072)），就是设置 `position` 为非 `static` 值以及 `overflow:hidden;` 时，浏览器无法正确隐藏超界的内容。如果你使用低版本Chrome就能在DEMO页看到这个现象。当然现在，似乎这个bug已经被修复了，这也从侧面说明了我这篇东西其实拖了很久...

下一个问题，所有的浏览器在实现 `border-radius` 和 `overflow:hidden;` 的时候，和常规的矩形box的并不相同，我个人认为这是实现上的错误。众所周知，在矩形`overflow:hidden;` 的情况下，我们无法用鼠标选取被隐藏的超出部分的内容文本，但是，如果是 `border-radius` 曲面隐藏的话，情况就不同了。在白色边角上用鼠标拖动，都可以选中那些看不见的文字。但是，当你保持所有属性不变，单单去掉border-radius的瞬间，所有的这些看不见的文字回复到了正常的 `overflow:hidden;` 的状态——它们无法被选中。换句话说：**所有的浏览器，在实现`border-radius` 和 `overflow:hidden;` 共同作用的时候，都只是视觉上隐藏了文本，文本仍然有交互，即使那是曲面外的部分——理应没有任何的交互和响应**。

这篇就差不多完了，文中如有任何错误，欢迎各路吐槽。下篇《那些CSS的细节问题》，还是接着说圆角边框...有空就抓紧写，没空的话还得在磨叽一段时间...

#### 参考资料

1. [overflow:hidden, border-radius and position:absolute](http://tech.bluesmoon.info/2011/04/overflowhidden-border-radius-and.html) by Philip Tellis
2. [Rounded corners fail to cut off content in webkit browsers if position:relative](http://stackoverflow.com/questions/6144398/rounded-corners-fail-to-cut-off-content-in-webkit-browsers-if-positionrelative)