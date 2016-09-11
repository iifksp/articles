# CSS细节问题回顾


早些年闲的蛋疼的时候，写了一些关于CSS无关紧要的细节的的文章，如今因为繁忙恐怕难就再有这种功夫胡扯了。这些文章离现在最近的也快要超过1年半，是时候拿出来更新和回顾一下了。

- 2012/11/28 - [那些CSS的细节问题(1)](http://swordair.com/details-in-css-part-1/)
- 2013/01/06 - [那些CSS的细节问题(2) —— 圆角响应区](http://swordair.com/details-in-css-part-2-rounded-response-area/)
- 2013/02/15 - [那些CSS的细节问题(3) —— 圆角边框和overflow](http://swordair.com/details-in-css-part-3-rounded-corners-and-overflow/)
- 2013/04/12 - [那些CSS的细节问题(4) —— 圆角边框和outline](http://swordair.com/details-in-css-part-4-rounded-border-and-outline/)

由于这段时间以来，浏览器发展有些出乎意料(比如Opera改核)，所以我目前测试的范围已经缩小到只有IE9，Firefox32，Chrome38，**本文结果也仅以这些版本为准**。

[细节(1)](http://swordair.com/details-in-css-part-1/) 里提到的内容有三点：

1. 鼠标指针样式的变化
2. 哪些CSS元素不占空间
3. 多重阴影的数量限制

第一点，浏览器是否会及时更新鼠标指针的样式。之前的结论是，Firefox是唯一即时更新鼠标指针样式的浏览器，不过时至今日，Chrome已经跟了上来，那我们把结论更新为：**IE是唯一不即时更新鼠标指针样式的浏览器**。

第二点，描述了哪些CSS元素不占空间。对于阴影不占据空间，虽标准早有定论，但早期版本Firefox实现错误，不过那也仅限于Firefox也嗑药飙版本号之前。如今，这部分内容不需要再特别说明。

第三点，我曾经蛋疼地试图寻找 `box-shadow` 的阴影数量限制，不过在测试数超过65535之后浏览器仍然正常渲染而放弃。得益于这些年各浏览器对于 `box-shadow` 的模糊算法的改进，也使得往日可能需要注意的一些细节变的更无关紧要。

[细节(2)](http://swordair.com/details-in-css-part-2-rounded-response-area/) 讨论的是圆角后响应区的问题。当时，Chrome对圆角外空白部分仍然作出相应，而根据标准所述，曲线边界外的区域不接受指针事件，所以Firefox的实现比较正确。如今，Chrome已经更正了这个问题，表现同Firefox以及IE一致——**仅圆角外的有内容的区域有响应**。

[细节(3)](http://swordair.com/details-in-css-part-3-rounded-corners-and-overflow/) 测试的是各浏览器圆角 + `overflow` 的结果，由于这部分标准的模糊，浏览器表现也各异。但是当时的结论如今依然适用：**所有的浏览器，在实现 `border-radius` 和 `overflow:hidden;` 共同作用的时候，都只是视觉上隐藏了文本，文本仍然有交互，即使那是曲面外的部分——理应没有任何的交互和响应**。

[细节(4)](http://swordair.com/details-in-css-part-4-rounded-border-and-outline/) 讨论的是 outline 和 border-radius 的共同作用结果。如今再看测试用例，结果几乎没有变化，outline 与 border-radius 的矛盾似乎依旧将持续下去。并且，**Firefox依旧没能描绘出最小轮廓(minimum outline)**。

所谓温故而知新，尽管本来就是些无关紧要的东西。虽都是我写的文章，但回过头看仿佛真的不是出自自己之手。对于更早的长篇大论类的，就更觉陌生异常。除了一排汗，更多的还是对于变迁的感慨，和蛋蛋的忧伤——尼玛又少了好多能拿来装B的条目！还让人活吗？！

今天周五，各位，周末愉快。