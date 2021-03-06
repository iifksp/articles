# 兼容性良好的HTML邮件(EDM)

EDM(E-mail marketing 即邮件营销)在几乎所有的商业公司都免不了成为一种重要的推广手段，所以作为一个前端难免还是要与邮件打交道——只是邮件模板的编写和传统页面有很大的不同。很早以前，我写过一篇 [line-height导致的邮件图像间隙](https://swordair.com/gap-on-images-in-mail-caused-by-line-height/)，那是我第一次遇到诸如此类的问题。文中的两个参考链接是当时最值得一读的两篇专稿，到了今天，它们仍然可以当仁不让地作为最佳实践的准则。只是其中一篇链接因为口碑UED网站的消失而一并失效，不过仍然可以根据标题在网络上找到数以万计的拷贝。

而在这里，我单纯列出我自己总结的一些个人经验，它们虽然或多或少地在我的邮件工作里扮演重要角色，但可能并不是在任何情况下都保持准确。

众所周知 OutLook2007 为了它操蛋的安全性从而使得整个邮件倒退回了2000年前，为了邮件的兼容性你不得不使用很多废弃的标签、属性，但它们很有效，被任何时候都有效和安全——突然发觉DW此刻灵魂附体，满眼的 `table` 仿佛让你回到了前端概念兴起之前。然后，如果你开始些邮件模板并打算为其在各个邮箱里的兼容性努力，下面的几条建议可能有些用：


1. 由于CSS可能被过滤，必须使用 `table` 布局。在很多无奈的情况下经常会遇到多层 `table` 的嵌套，对此不必过分在意。
2. 不要使用背景，因为很多情况下都不被支持，即便有些邮箱确实支持背景图片，最后的结果往往也并不好。这一点使得设计出来的页面如果太花哨的话，直接都成了切图。
3. 所有的图片必须指定高宽和 `alt` 属性。这点很容易理解，因为很多邮箱里，图片不是默认加载的，往往加载前需要用户的许可。那么高宽的指定可以使邮件在没有图片撑出样子前也能保持良好的大小结构，`alt` 更可以明确告知图片的内容让用户选择是否下载它们。
4. 样式全部改成行内，直接加在对应的HTML标签上。这点很有必要，因为大部分邮件无法完整的继承样式，并且邮箱的默认样式也会对邮件产生一些头疼的干扰。所以如果你想让邮件在所有邮箱里看起来都一致，那么，**所有的样式最好都单独指定**，特别是IE的怪异模式下，单独设置字体和对其是必须的。
5. 出于兼容性的考虑，经常需要使用到废弃的标签和元素。尽可能只使用HTML，少用样式。在很多情况下，两者最好同时出现。比如，`border="0"` 和 `style="border:0;"`。其实这种双重保障经常可以在一些引用资源上看到，比如 facebook 的 like 按钮。
6. 无论图片是否用块显示，`line-height` 都会影响图片的拼接，所以要确保其只作用于文字范围。具体可以看我之前的 [line-height导致的邮件图像间隙](https://swordair.com/gap-on-images-in-mail-caused-by-line-height/)。
7. 只要不是行内的图片，样式 `style="display:block;border:0 none;margin:0;pading:0;"` 几乎是标配。避免默认行高以及边框的影响，可以避免大部分邮箱图片拼接遇到的问题。特别是IE下，对图片添加链接默认出现的紫色边框，常容易被忽略，这时0边框是必须的。
8. 不要简写颜色，比如#fff，要完整写成#ffffff。简写的颜色在IE怪异模式下会出些小问题。
9. 竟可能不要使用span，因为其在某些邮箱里会导致换行。
10. `table` 的高度使用td撑起来，别给 `table` 写高度，那样往往不准确。
11. 如果不想邮件被转发后支离破碎，邮件最好不要用多个 `table` 拼装，而是要嵌套起来。


当然编写的原则远不止这些，但遵守这些已经可以避免大部分的兼容问题。如果你的目标不是绝大部分邮箱，那么你可能需要一份 [邮箱的CSS支持参考](http://www.campaignmonitor.com/blog/post/2533/a-guide-to-css-support-in-emai-2/#pc) 来帮你排忧解难。

如果你针对 outlook，那么微软官方的

- [Word 2007 HTML and CSS Rendering Capabilities in Outlook 2007 (Part 1 of 2)](http://msdn.microsoft.com/en-us/library/aa338201(v=office.12).aspx)
- [Word 2007 HTML and CSS Rendering Capabilities in Outlook 2007 (Part 2 of 2)](http://msdn.microsoft.com/en-us/library/aa338200(v=office.12).aspx)

也将是必不可少的参考。

最后在废话一下邮件的设计原则——因为很多时候往往遇到设计师出来的稿件完全不适合邮件环境的情况。邮件本来就是作为一种通知信息，所以列出主要内容和标题，让用户最快的得到他们想要的信息应该是最优先的。

邮件当然可以做的很独特，这能让邮件不被淹没显得鹤立鸡群，但是独特在大量视觉上，大量的圆角和背景的话，邮件的兼容编写就会变得非常坎坷。在邮件里加其他复杂功能更加是要不得的，比如复杂搜索框（注意是“复杂的”）——邮件是干这事的吗？如果遇到业务强烈要求把一个复杂的搜索框加入邮件里的时候，作为前端，应该拒绝之。

大清早起来写的，写的不尽如人意地方，欢迎指正和各种吐槽~:)