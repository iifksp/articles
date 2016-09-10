# CSS4 Media Queries 新特性

来自 [css3.info](http://www.css3.info) 的消息，CSS4 Media Queries 已经成型，尽管它现在还只是一份[编辑草案](http://dev.w3.org/csswg/mediaqueries4/)，不过我们仍然可以从中看到新的 Media Queries 所具备的特质。在 [Changes Since the Media Queries Level 3](http://dev.w3.org/csswg/mediaqueries4/#changes-2012) 一节里，就能看到 CSS4 Media Queries 的新特性。

当前，主要添加的是这三个新的特性值："`script`", "`pointer`", "`hover`"

## `script`

如果没有记错，Zakas曾经就提过增加 `script` 特性值的建议。于是我很快翻阅了他之前的博客，并找到了这篇文章：[Proposal: Scripting detection using CSS media queries](http://www.nczonline.net/blog/2012/01/04/proposal-scripting-detection-using-css-media-queries/)。文章说的是，通过增加 `script` 媒体值，用来判断浏览器是否支持或者启用脚本功能，从而对应不同情况编写不同样式。

这样做可以让我们避免使用额外的类名来控制这些无脚本情况下的样式，并且，因为这些多余的类目的管理通常也非常费神，所以提供脚本检查的样式区分将独立并极大简化样式控制：

```
@media screen and (script) {
    /* 脚本启用时的样式 */
}

@media screen and not (script) {
    /* 脚本禁用时的样式 */
}
```

没想到，CSS4已经包含了这个功能，虽然 Zakas 提出的脚本特性值还包括一些更为高级的检测特性，但是CSS4刚刚成型，日后也还会不断完善。当前的note里就有这样一句：

> A future level of CSS may extend this media feature to allow fine-grained detection of which script is allowed to run.

意指将扩展此特性值的并细化检测粒度。这样的话，未来脚本相关的样式控制会变的平滑许多。

## `pointer`

随着各种触摸设备的流行，标准也是与时俱进，`pointer` 值就是用来检测设备的指针的存在和精度的。其值分为三种：


1. '`none`' : 表示没有指针存在
2. '`coarse`' : 表示存在不精确的指针，比如iphone类触摸设备。
3. '`fine`' : 表示存在精确的指针，比如PC机器(尽管我的G1用了8年已经有点不怎么精确了:)~)


这里，触摸类设备被划分到 `coarse` 里，没有指针并不是说屏幕上是否有指针，而指的是指向点击。并且，当前定义里，任何在选中小目标时有困难或者干脆无法点到的指针设备，都被归到 `coarse` 里

根据指针精度从而加大某些点击按钮或控件是典型应用，比如在触摸设备上增大 check box 的尺寸，让目标变的更容易点击。

## `hover`

`hover` 属性值的出现其实也是在情理之中，是想，当前如此之多的触摸设备，其实它们都没有 `hover`。当前hover值几乎就是为鼠标而生的。这就会导致问题，所以，看到这个特性值的时候，我就想到的iOS，以及 Zakas 的另一篇文章：[iOS has a :hover problem](http://www.nczonline.net/blog/2012/07/05/ios-has-a-hover-problem/)。文中描述了iOS的 `hover` 以及苹果如何解决触摸设备下的 `hover` 问题——对于有元素显示隐藏变动的 `hover`，iOS使用两次点击显示 hover 的变化。

现在，我们有了更好的办法。hover 特性值能帮助我们对是否支持 `hover` 的设备在样式上直接做区分。这太酷了，特别是对于那些纯CSS实现的下拉菜单而言。对于某些设备部分支持 `hover` 的情况，草案也有所描述，详细可以查看 [4.16. hover](http://dev.w3.org/csswg/mediaqueries4/#hover) 章节。

末了，我想说，当前虽无浏览器能支持CSS4，但整个浏览器大跃进的现在来看，情况还是挺乐观的。CSS3 Media Queries 是最早成为 CSS3 推荐标准的模块之一，所以我相信，CSS4 Media Queries 也能够最早的完成并被广泛的支持。而这也会把响应式Web设计推向新的高度。

**2012-9-14 更新：**准确说来，CSS4是个错误的概念，正确说法是 **Media Queries Level 4**。