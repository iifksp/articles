# CSS 百分比 margin & padding

前段时间我同事对于margin和padding应用百分比值似乎有些误解，觉得可能是个普遍问题，所以觉得有必要拿出来单独写一下。

margin和padding都可以使用百分比值的，但有一点可能和通常的想法不同，就是 **margin-top | margin-bottom | padding-top | padding-bottom 的百分比值参照的不是容器的高度，而是宽度**。

引用标准(2.1)原来的表达：

> The percentage is calculated with respect to the width of the generated box's containing block. Note that this is true for 'margin-top' and 'margin-bottom' as well. If the containing block's width depends on this element, then the resulting layout is undefined in CSS 2.1.

> The percentage is calculated with respect to the width of the generated box's containing block, even for 'padding-top' and 'padding-bottom'. If the containing block's width depends on this element, then the resulting layout is undefined in CSS 2.1.

宽度参照宽度这点毫无疑问，但高度参照的也是宽度这点，可能就和通常的思路相左，因为毕竟height应用百分比参照的，依然是容器的高度。

关于为什么标准要这么定义，有几种通常的解释，就一并(个人)分析一下。由于padding和margin类似，下文就只以padding-top为例。

第一种说法是，padding-top如果以容器高度为参照，那么子元素应用padding值将会继续加高容器的高度，容器高度的变化又会反过来继续影响子元素的padding-top，陷入一个无限循环。

对于不定高的容器来说情况确实如此，但对定高容器则并不成立，这点和height类似，这也是为什么height的容器也必须确定好高度。也就是说，如果padding-top要参照容器高度，就必须和height一样做两种处理。

第二种说法则更为可靠，**为了保持padding(margin)四个值的一统一**。

padding(margin)的值无论引用何种计量，四个值都一直保持统一，难道单独给百分比开特例？结合第一条的多情况处理，如果标准定义padding-top参照容器高度的话，恐怕要列出至少4条以上的例外说明——而这对无论谁，都没有好处:)

事实上，垂直padding参照体是宽度这点也非常有用。虽然早期固化像素的设计中百分比值几乎不应用在padding或者margin上，但随着移动自适应的布局的需要，百分比也逐渐稀疏平常。比如配合background-sizing保持背景的比例，配合media query调整对应的间距，不一而足。这些应用都基于一个事实：**无论垂直还是水平，百分比值始终参考宽度**。

而宽度，实际上，正是自适应和现代web设计的灵魂。

