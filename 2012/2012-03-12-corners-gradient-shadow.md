# 边角阴影渐变

最近看一些网站的时候经常看到边角阴影，所以单独拿出来说说。

比如 Nettuts+ 的设计，主内容区的右上角有非常浅的渐变，并有1px的白色勾线。这些细节设计虽然非常淡，甚至很多人根本不会去注意到这些，但它们累加起来的效果非常出色——不论我们有否发觉，重要的是我们确实受到了这些细节的视觉上的影响。

![gradient gray corner](https://swordair.com/content/images/2014/Jan/gradient_gray_corner.jpg)

虽然 Nettuts+ 用了一张png的边角渐变的图片，但显然代码也能实现。线性渐变作为CSS3的标配，虽然使用上需要加上麻烦的前缀，但放眼望去用上代码渐变的页面也已经相当普遍。使用上需要注意的是 Chrome 9- 以及 Safari 4- 使用的都是非标的 webkit-gradient 语法：

```
corner-gradient:before{
content:"\200B";display:block;width:120px;height:120px;position:absolute;right:1px;top:1px;
background:-moz-linear-gradient(45deg,rgba(232,232,232,0) 59%,rgba(232,232,232,0.65) 100%);
background:-webkit-gradient(linear,left bottom,right top,color-stop(59%,rgba(232,232,232,0)),color-stop(100%,rgba(232,232,232,0.65)));
background:-webkit-linear-gradient(45deg,rgba(232,232,232,0) 59%,rgba(232,232,232,0.65) 100%);
background:-o-linear-gradient(45deg,rgba(232,232,232,0) 59%,rgba(232,232,232,0.65) 100%);
background:-ms-linear-gradient(45deg,rgba(232,232,232,0) 59%,rgba(232,232,232,0.65) 100%);
background:linear-gradient(45deg,rgba(232,232,232,0) 59%,rgba(232,232,232,0.65) 100%);
}
```

[DEMO](http://www.swordair.com/demos/corner-gradient/)

也难怪前段时间各路大神对与属性前缀的争论了，看到这么一大坨一个意思的代码，能不蛋疼么。

对比下代码和图片的优缺点。

<table>
	<tr>
		<th>Code</th>
		<th>Image</th>
	</tr>
	<tr>
		<td>
			<ol>
				<li>无图片请求</li>
				<li>代码更容易维护，扩展性好</li>
				<li>加载时不会发生抖动</li>
				<li>兼容性差，IE9-无斜角度渐变，Opera渐变非常糟糕</li>
				<li>缩放网页定位不会有误差</li>
			</ol>
		</td>
		<td>
			<ol>
				<li>额外的图片请求</li>
				<li>兼容性好</li>
				<li>缩放网页时，背景图片定位会出现偏差，造成间隙</li>
				<li>加载时有抖动</li>
			</ol>
		</td>
	</tr>
</table>

优势显而易见。更高的性能，更容易维护，当然兼容也一塌糊涂。值得一提的是 Opera 的角度渐变那叫一个难看，渐变的非常生硬，让我联想起它刚刚支持 box-shadow 时的模糊算法，一样糟糕透了。不过怎么不好，也总好过IE——其滤镜的渐变也只能提供纵横，显得异常过时。