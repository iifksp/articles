# 那些CSS的细节问题(2) —— 圆角响应区

之前《那些CSS/JavaScript的细节问题》我都是凑足3个再开始写，现在发觉，很多时候要凑出3个古里古怪的问题还真不是件易事~ 有的时候想到要写一个问题，想等凑多些再写，但又总是一直拖着，好端端的讨论话题都过期了。更糟糕的是，往往好不容易凑足了数量，结果之前要写的东西居然又模糊不清了...以后，在一定时间里凑不出数量的话，也只好先写起来再说:)

所以今天，就只有一个问题，关于边框圆角：**圆角边框是否影响元素的响应区域**？

想象一下，一个 `div` 通常被我们认知为一个盒子(box)，现在在它的周围有一个圆角边框，那么在这个矩形盒子的四个角上，必然会出现一些空隙（特别是当这个圆角矩形变成一个圆时候）。那么对于这个元素而言，那些空隙还是不是元素的一部分？

换个方式问，元素的响应区域，到底是原始盒模型的矩形区域，还是圆角边框所构成的圆形区域？当然，如果浏览器表现都一样，我恐怕也就不会提这种问题了。

具体情况如何，我们还是直接问浏览器吧！为了方便比对，最简单的方法就是用 `:hover` 伪类看鼠标的状态变化。我做了一个简单的 [DEMO](http://www.swordair.com/demos/border-radius-affects-response-area/)，你可以用不同浏览器查看结果。当然如果你懒得看的话，下面就是这个DEMO的两种结果：

![border radius response](https://swordair.com/content/images/2014/Jan/border_radius_response.png)

- 左边，响应区与圆角无关，鼠标状态在初始的box的范围里全都有变化。浏览器包括Chrome，Safari，Opera。
- 右边，圆角外围的部分无响应，仅仅只有边框内部区域有效。但如果内部包含内容，并且内容在边框之外的话，也一样有效。浏览器包括Firefox，IE9

我测试使用的都是最新的浏览器，FF-17，Chrome-23，IE-9(很遗憾身边没有IE-10)，Opera-12，Safari-5，现在，该相信谁呢？还是说，它们一个都不对？

如果是平常，我一定优先考虑Chrome和Firefox的结果，问题是，它们站在了对立面上。但就结果而言，我可能更容易接受Firefox的做法：以边框为界，圆角外的区域应当不作响应。试想，我总不能画一个圆而让这个圆的不可见的外接矩形作为交互区域吧？这多别扭！

既然浏览器各执一词，那就只能翻翻游戏规则了。没想到，W3C标准里居然非常明确地写明了这种情况：

> Also, the area outside the curve of the border edge does not accept pointer events on behalf of the element.

呵呵，Chrome你个混球！看看你都干了神马！还把 Opera 也一起拖下水，Good Job～好欢乐啊～

水落石出了，换句话讲，`border-radius` 是会缩减响应区的，其圆角曲线外的那些空白区域是没有交互和响应的。也正因为此，标准特别提醒要作者注意圆角边框的元素的交互尺寸。而任何在在做怪应用而遇到这个问题的人，也完全不必去理会 Chrome 和 Opera，因为，这次真相掌握在了IE和Firefox手中！

PS：下篇那些CSS的细节问题，还是讲圆角，更加蛋疼的圆角麻烦问题还在后面呢～

#### 参考资料
1. [CSS Backgrounds and Borders Module Level 3 - 5.3. Corner Clipping](http://www.w3.org/TR/css3-background/#corner-clipping) from W3C
2. [Cascading Style Sheets Level 2 Revision 1 (CSS 2.1) Specification - 8.1 Box dimensions](http://www.w3.org/TR/CSS21/box.html#box-dimensions) from W3C
3. [CSS Reference border-radius](https://developer.mozilla.org/en-US/docs/CSS/border-radius) from MDN
4. [CSS Reference background-clip](https://developer.mozilla.org/en-US/docs/CSS/background-clip) from MDN
