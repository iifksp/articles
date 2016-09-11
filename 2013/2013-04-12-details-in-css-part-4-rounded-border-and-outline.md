# 那些CSS的细节问题(4) —— 圆角边框和outline

能写文章，说明时间略有空余，这应该算好事吧～看了下草稿箱，果断还是选这篇先写完，因为和前面的几篇有关联。在之前的 [圆角响应区](https://swordair.com/details-in-css-part-2-rounded-response-area/) 以及 [圆角边框和overflow](https://swordair.com/details-in-css-part-3-rounded-corners-and-overflow/) 中，已经讨论过圆角边框的一些问题，本篇如题，是关于圆角边框和 `outline` 的，这里也正好回答了一丝在[圆角响应区](https://swordair.com/details-in-css-part-2-rounded-response-area/)里提出的“为什么不在做DEMO的时候用 `outline` 描画边缘？”的问题。在写那篇之前做DEMO的时候，我是用了 `outline` 的，但注意到 `outline` 的一些问题之后，又把它从DEMO里移除了，并起草了这篇文章的标题，然后，一放就是3个月之久...

进入正题，**如果同时使用圆角边框 `border-radius` 以及 `outline` ，那么 `outline` 的形状应该是怎么样的？也该是圆角吗？**

这个问题看起来简单，本质却复杂。在讨论这个问题前，抛开标准，作为使用这两个属性的人，自己使用之后的期望是怎样的？如果是我，根据以前使用的经验：圆角的圆弧外没有交互区域，既然边框界定了元素，而 `outline` 又是元素的勾勒，那么 `outline` 应该跟随边框的圆角。但这未必是正确和准确的，所以这之后我做了部分考证。

先看浏览器(Chrome-24,IE-9,Opera-12,FF20)的表现：所有的 `outline` 都显示为矩形，无一例外，也就是说没有跟随边框的圆角。这是一个和预料相矛盾的初步印象，也是我刨根问底的原因。下面就从头说起了，期间，会遇到很多连带的问题，既然这一系列文章讨论的是“蛋疼的细节”，所以我还是会花些篇幅探讨它们。

如果对 `outline` 的定义如果有什么模糊的地方，可以先参考阅读标准 [CSS2.1 outline](http://www.w3.org/TR/CSS21/ui.html#dynamic-outlines) 以及 [CSS3 outline](http://www.w3.org/TR/css3-ui/#outline-properties)。定义的内容很明确，但是也可以肯定，当前通篇没有任何的  `outline`  和 `border-radius` 的相互作用的描述。

于是，我简单做了一个[DEMO](http://www.swordair.com/demos/border-radius-outline/)用来描述 `outline` 的本质。如果你不打算打开多个浏览器查看[DEMO](http://www.swordair.com/demos/border-radius-outline/)，可以看下面这张截图。就结论而言，即使是单独讲 `outline` ，其在当前浏览器的表现也并不一致：

![](https://swordair.com/content/images/2014/Jul/border-radius-outline.png)

上图左边是firefox，右边是Chrome，IE和Opera。之所以没有标注浏览器，是因为虽然大体表现成上面两种类型，但细节上还是有差别。比如Chrome在加粗行内元素的 `outline` 的时候(图中蓝色线)，不同行的衔接处会有断裂。撇去这些细节不说，标准中说“如果元素跨越多行，`outline` 应当是包围所有元素的**最小轮廓**(minimum outline)”，所以firefox的实现方式明显是与标准相悖的。

这个例子的另一个目的是为了展示，当内容超出容器之后，容器的 `outline`  是否应该包括超出的部分？图中红色的框代表容器，定高定宽，所以内容顶出容器可见。这时，容器的 `outline` (绿色)也分为两种情况，firefox展示为包含超出内容，其他浏览器则不包含。当然，firefox虽然包含，也并非是期待中最小包含。

这里存在浏览器存在分歧其实也并非什么问题，因为 `outline`毕竟定义于 CSS User Interface Module，部分定义的模糊也是故意为之，这给了UA更多的在界面上的自由。可以说 `outline` 这个东西本身就存在不确定性，所以，一旦和圆角边框作用在一起，还能有是非对错嘛？

就如同定义没有给出 `outline` 的层叠关系一样，定义同样没有给出圆角边框下 `outline` 应该如何表现，区别是，前者明确指出“不定义”，后者完全是没有提及。但这不妨碍浏览器的自作主张，比如firefox存在私有属性 [`-moz-outline-radius`](https://developer.mozilla.org/en-US/docs/CSS/-moz-outline-radius)，可以直接手动控制outline的圆角值。不过，这个值的支持在后续版本可能被移除，跟着这个 2年多前的[Bug 593717](https://bugzilla.mozilla.org/show_bug.cgi?id=593717)(- remove -moz-outline-radius and make outlines follow border-radius) 找到的当时w3c的讨论：[css3-background: border-radius and outlines](http://lists.w3.org/Archives/Public/www-style/2010Apr/thread.html#msg165)。有趣的是，读完这个讨论可以得知，4年前就有人在webkit提议实现firefox已经实现的 `outline-radius` ([Bug 25293](https://bugs.webkit.org/show_bug.cgi?id=25293))。

回到最初的问题，圆角边框下 `outline` 应该如何？因为没有明确定义的关系，正确与否并不重要，重要是的在使用任何一种东西的时候，使用者期望它是怎么样的。标准本身是一个描述，一个游戏规则，同时，也是一个多数人的期望的集合。于我而言，我仍然希望 `outline` 跟着边框绘制，用来勾勒一个实际元素的轮廓，而不是一个box。同时也认为 `outline-radius` 是没有必要的。

更实际的问题，当前 `outline` 被绘制成box的情况下，大多数情况下是可以使用 `box-shadow` 代替的，至少目前为止 `box-shadow` 跟随 `border-radius` 比较完美。

### 结束语

`border-radius` 话题到这里就差不多结束了，最后顺便一提，还记得 rogerjohansson 写过一篇 [Fieldset, legend, border-radius and box-shadow](http://www.456bereastreet.com/archive/201302/fieldset_legend_border-radius_and_box-shadow/)，讲述了一些有趣问题，没看过的朋友不妨瞅瞅。 另外，这里哪里写的不好的，欢迎吐槽。

`border-radius` 从刚诞生时连背景都包不住的窘境，到现在被广泛使用，这种跃进虽然发生的如此之慢，但它确实实实在在地发生着，作为一个技术观众，我能做的就是记录下它蛋疼的成长，并在十年之后回想起时，付之一笑:)