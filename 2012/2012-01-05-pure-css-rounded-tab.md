# 纯CSS圆角Tab

Tab能算的上是网页里最为常见的组件，不论是作为内容切换，还是直接作为导航，形形色色的Tab扮演着各种交互角色。既然是重要的交互角色，那么无论其颜色还是形状都关乎整个交互过程——圆角是有意义的，在视觉上把Tab和其他分隔元素区别开来是必要的，就如同圆角按钮一样——可能很多时候圆角按钮都与整个设计风格格格不入，但却是必须的，因为交互元素的凸显比整个设计浑然一体更为重要。

在IE67日渐边缘的如今，2012可能是前端重心转移最为明显的一年。于此也就不去考虑过时浏览器的兼容性了，对于它们，能看看内容就已经不错了，管它美不美观错不错位，时代在进步，往前看才不至于失业......

今天琢磨了会写了下面这样一个CSS圆角Tab，和网上流行的圆角Tab不同之处在于：**Tab底部也使用圆角过渡**。

![pure css rounded tab navigation](https://swordair.com/content/images/2013/Dec/pure_css_rounded_tab_navigation.png)

我简单制作了一个DEMO，列出了HTML结构和CSS：[Demo](http://www.swordair.com/demos/pure-css-rounded-tab-navigation/)

为达到底部使用圆角过渡的目的，关键就是下面两点：

1. `border-radius` 的使用时显而易见的，这个属性在所有现代浏览器中工作良好，并且在无论是Firefox还是Chrome的更新里，都不再需要前缀。
2. 由于Tab下沿的圆角无法填补，需要用 `li` 的伪元素来定位填补，同时还需要 `a` 的伪元素来定位覆盖隐藏Tab下沿的边线。

具体结构我绘制了一张框图，感觉上这种圆角的参数很微妙...

![rounded tab html construction diagram](https://swordair.com/content/images/2013/Dec/rounded_tab_html_construction_diagram.png)

代码里面值得一提的可能只有一条：

```
.top-nav li.current:after{
	content:"\200B";
	position:absolute;
	display:block;
	width:100%;
	left:0;
	bottom:-5px;
	border-bottom:1px solid #fff;
}
```

那就是 `content:"\200B";`。其实之前的 [再谈清除浮动](/on-clearing-float-again/) 里也已经提到，这个 `U+200B` 字符是一个“零宽度空白”，其本身就不可见，所以也就不需要在使用 `visibility:hidden;` 来隐藏内容。

**2012/01/10更新**：非常巧合的，css-tricks的Chris Coyier在几天后也发了一篇关于圆角tab的文章 [(Better) Tabs with Round Out Borders](http://css-tricks.com/better-tabs-with-round-out-borders/)。对于Chris Coyier大神的圆角我只能说，“用阴影而不是伪元素来盖住直角多余的部分”这真太有创意了！他的实现使用了2个伪元素和box-shadow，而我用了3个伪元素，但没有使用box-shadow。讽刺的是，在IE67下轻微错位的我实现方式，可能更适应国情...