# 像Photoshop一样地使用CSS

无论是作为前端，还是作为设计师，应该对下面这个界面颇为熟悉。

![photoshop layer style](https://swordair.com/content/images/2014/Jan/photoshop_layer_style.png)

Photoshop的图层样式，恐怕是网页设计里最常接触的操作界面之一，因为传统Web设计很大程度上就是图层的样式层叠。很多年前，我还是一个蹩脚的设计师的时候，就常常看那些设计教程里大段大段的关于图层样式调整的描述，所以直到现在即使自己成了前端，但那些样式的调整都历历在目。而现在，如果用一个前端的眼光来看待Photoshop的图层样式，是可以把层的概念和样式直接映射到代码里的。

CSS究其本质，无非是一种描述性语句，与我们操作GUI的界面而言其实没有什么太多不同：选项和参数组成了完整描述使得浏览器能准确地反映出来。随着CSS3的普及，大部分的图层样式就能直接对应于CSS代码，而不需要我们再去切图。

如你所想，投影、内阴影、外发光、内发光，乃至斜面浮雕和光泽，都部分或全部的由 box-shadow 模拟出来。在之前的 [CSS3 box-shadow 详解(2)](https://swordair.com/details-on-css3-box-shadow-part-2/) 里已经讨论过 box-shadow 模拟阴影、发光、边框，乃至渐变，所以说 box-shadow 的用处非常广泛。但同时也必须注意使用上的问题：box-shadow 的过多使用，特别是大范围的模糊值和扩展值，**会导致Opera浏览器的滚动条卡顿**，可见其在处理阴影和扩展和模糊时效率并不高。

特别的，box-shadow 可以用作边框的“描边”使用——相当于PS样式里的描边以及 编辑->描边 选项，其可以在边框之外或之内，额外描边。并且加之多重阴影，描边也可以不受限制。更为重要的是，阴影产生的描边不产生额外空间，也就不会影响任何的布局。当然，这只有IE9以上才能做到。但如果想让IE8也实现描边的话，该如何做?

一个变通的方法是用伪元素+绝对定位来模拟：

```
.inside-border {
	position:relative;
	border:1px solid red;
}
.inside-border:after{
	content:"";
	position:absolute;
	top:0;
	left:0;
	bottom:0;
	right:0;
	border: 1px solid blue;
}
```

文字图层的效果自然对应于 text-shadow，虽然功能和 box-shadow 类似，但模拟效果远不及后者。而且，因为IE9令人大跌眼镜地不支持 text-shadow，所以普及文本阴影还需时日。但光从现在的 text-shadow 所能实现的效果来看，已经基本可以映射文字图层的简单效果，甚至在某些方面更出色——比如多重阴影下能轻易实现3D的字体，这样例子也非常多，比如 Mark Otto 的 [3D Text Using Just CSS](http://markdotto.com/playground/3d-text/)。你甚至可以找到制作这类3D字体的工具，比如 [3D CSS text generator](http://www.3dcsstext.com/)。

使用 text-shadow 有一个小细节，就是用户选中的文本也一样会应用阴影，但如果使用了模糊参数，那么选中文本后的颜色变化可能使得文字变得难读。这里，要么直接控制选中文本的样式，要么就在选中时移除阴影样式：

```
::-moz-selection{text-shadow:none;}
::selection {text-shadow:none;}
```


对于不支持阴影的浏览器，模拟方式大都使用绝对定位层叠的方式实现。对于像伪元素这类方式还是可以接受，但是如果要额外标签和内容恐怕就得不偿失了。对于文本阴影的模拟也是如此。在过去，对模拟通常都是重复相同的内容并用绝对定位层叠，或者是滤镜，但都不应推荐，甚至在CSS3的现在都不值一提。

最后，无论是对于文本还是图层，还可以叠加一个图片遮罩来实现“图片叠加”，Stephen Morley 在其 [Textured and patterned text using CSS](http://code.stephenmorley.org/html-and-css/textured-and-patterned-text/) 里讨论了这种方法。

对于其他诸如webkit-filter这类尚不成熟的应用，这里不在多做讨论。但是相信未来，简单图层的设计是可以绕开Photoshop的，因为越来越多的可用的样式描述，正在逐渐接近图层样式本身。
