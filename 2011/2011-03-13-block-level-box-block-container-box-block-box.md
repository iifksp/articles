# Block-level Box & Block Container Box & Block Box

前几天，一位叫做colin朋友邮件问了我一个关于包含块的疑问，大致是说，css2.1定义里有这样一句：

>For other elements, if the element's position is 'relative' or 'static', the containing block is formed by the content edge of the nearest block container ancestor box.

但是国内的翻译资料，却是这样的：

>它的包含块由它最近的块级、单元格（table cell）或者行内块（inline-block）祖先元素的内容框创建
到底包不包括行内块(inline-block)？

凭着一些以前对CSS标准的印象，我模糊地给出了一个我的理解，即inline-block虽然对外是行内元素，但是其内部格式化成块，能包含块级元素，所以仍然属于block container。虽然也没有错误，但显然不够清晰，也没能给出标准的参考依据。于是今天乘着有空翻了一下标准，发现自己的有部分知识结构随着时间慢慢模糊掉了，不过好在有印象，花一点时间能串的起来。

其实包含块的这个定义里的block container，确实包含了块级、单元格和行内块，造成混淆的原因是有如标题的这三个形似的概念导致的。

block container，如其名直译成**块容器**非常合适，能容的下块级元素的容器就能称为block container。和**块级框**(block-level box)的不同，block container是一个独立的概念，所以即使是行元素inline-block，由于其内部格式化为块因而能容纳块级元素，所以同样是block container。

block-level box是我们最熟悉的**块级框**，根据标准，其中除了table以及替换元素以外，每一个块级框都是一个block container。并且每一个是 block container 的 block-level box，称为 block box。

所以，**块级框**(block-level box)、**块容器**(block container)、**块框**(block box)，是完全不同的三个概念。定义在[CSS2.1 —— 9.2.1 Block-level elements and block boxes](http://www.w3.org/TR/2010/WD-CSS2-20101207/visuren.html#block-boxes):

>Except for table boxes, which are described in a later chapter, and replaced elements, a block-level box is also a block container box. A block container box either contains only block-level boxes or establishes an inline formatting context and thus contains only inline-level boxes. Not all block container boxes are block-level elements: non-replaced inline blocks and non-replaced table cells are block containers but not block-level boxes. Block-level boxes that are also block containers are called block boxes.

所以，中文译文里的定义是准确的，大概是在翻译某个章节的时候出于纳入零散的知识点的目的，就把某个概念的锚点一起推广开来了。不知道是不是豆豆猫翻译的，因为在之前只见过他博客上的断断续续的译本。而且这种翻译风格，非常博客化，不像是严谨的文档校译。

不过这次一查之下，看到一个[W3Help.org](http://www.w3help.org/zh-cn/)的网站，风格与W3C一致，内容多为W3C的中文译本和参考，只是风格仍然很博客化。大概是太久没关注国内的动态，出了几个参考级站点居然都没察觉囧...虽然内容仍然很有限，一些翻译仍然没有统一（比如W3Help上同时出现了“格式化上下文”和“格式化内容”两个概念，但都翻译自同一个术语formatting context），却仍已是不可多得了。

标准里的诸多术语其实非常严谨，本来标准就是一个描述，所以明白了各种概念之后，标准也就仅是一个认知性的文档。不过其中的相似的概念如果不仔细理解和区分，确实容易产生各种混淆。

自从来到杭州上班后就很少有时间静下心来看东西，Google reader里的订阅更是过了千记，而从前这个数字从来没有超过10。感觉自己有点落伍啊~赶紧加把劲才是。最后感谢colin，我写下此文，给你一个清晰答案的同时，也是给其他需要这个答案的人、更是给我自己一个清晰的答案:)