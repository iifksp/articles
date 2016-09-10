# 移除Textarea周围额外的空白

以前，`textarea` 100%宽度撑满父容器一直都是一个常常被提及的问题。作为 `inline-block` 显示的 `textarea` 无法像 `block` 那样在宽度上自动的充满容器，于是为了让 `textarea` 能够撑满容器并且回避因 `padding` 和 `border` 而产生的额外计算，各种手法也就层出不穷。

比如，设置宽度为98%，比如，嵌套一个div然后设置它的 `padding-right` 等于内含的 `textarea` 的边框和 `padding` 之和，再让 `textarea` 的宽度100%。然后，当 `box-sizing` 大行其道的现在，很多人可能都已经忘了和 `textarea` 纠缠的种种美好回忆。一句 `box-sizing:border-box;` 像是救世主一样拯救了尚未完全支持公式计算的浏览器世界，顿时100%高度和宽度的诸多不便一下子变得豁然开朗，IE的盒模型就这样王者归来了!

前略，毕竟这文主要讨论的不是布局和历史，而是 webkit 的一些小脾气:)

问题是这样的：在一个宽高固定的有 `overflow:auto;` 的div容器里，100%高宽一个 `textarea`:
```
<div style="border:1px solid red;width:200px;height:200px;overflow:auto;">
	<textarea style="height:100%;margin:0;border:0;box-sizing:border-box;width:100%;overflow:auto;" 
	name="" id="" cols="30" rows="10"></textarea>
</div>
```

在 `box-sizing` 的威力下，这本应该是非常容易的事，并且在大多数浏览器下也确实如此。不过在webkit下，本应正好嵌入div的 `textarea` 还是将外部容器的滚动条顶了出来。尽管查看 `textarea` 的尺寸和边距没有任何发现，但是仍然会有幽灵一样的空白在其周围，使得滚动条不请自来。

通常遇到这种情况，往往会花上不少时间来寻找解决方法。但我是幸运的，我知道怎么解决这个东西，因为这不是第一次遇到(在以前第一次遇到的时候，我仍然花了相当多的时间)。我很快在 Google Reader 里翻出记忆里的曾经看过的一篇文章 [Removing whitespace around text fields](http://www.456bereastreet.com/archive/201210/removing_whitespace_around_text_fields/)，其内容就是答案：为 `textarea` 添加 `vertical-align:bottom;` 。

```
<div style="border:1px solid red;width:200px;height:200px;overflow:auto;">
	<textarea style="height:100%;margin:0;border:0;box-sizing:border-box;width:100%;overflow:auto;vertical-align:bottom;" 
	name="" id="" cols="30" rows="10"></textarea>
</div>
```

这样，问题就算是解决了。但这次遇到之后，又自己尝试了一下其他方法。最后，下面这些属性和值对这个例子里的代码同样有效：


1. `vertical-align:top;`
2. `vertical-align:middle;`
3. `margin-bottom:-5px;`(值可能取决于实际情况)


从这些代码来看，影响 `textarea` 在 webkit 中表现的，正是 `vertical-align:baseline;`。至于为什么，本文就不再深究下去了，也许以后有空的时候可以再深入看看。最后放上这次简单的 [DEMO](http://www.swordair.com/demos/remove-textarea-box-sizing-extra-space/)，方便查看和比对。
