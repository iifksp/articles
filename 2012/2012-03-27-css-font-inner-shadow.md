# CSS实现字体内阴影

上篇 [像Photoshop一样地使用CSS](https://swordair.com/using-css-as-photoshop/) 讲到PS和CSS的关系日趋紧密。但是我遗漏了一些，那就是字体的阴影效果的实现。对于投影，`text-shadow` 能很好的满足，但是如果是字体的内阴影或内发光，又该如何？熟悉CSS3 `text-shadow` 的朋友都知道，`text-shadow` 并没有类似 box-shadow 的 inset 值来表示内外阴影，所以虽然 `text-shadow` 虽然也支持多阴影，但在内阴影方面却无能为力。

但真的是这样吗？先看看下面的字体效果(IE10+，以及其他所有浏览器，如果你是通过RSS，请到我的博客查看效果)——

<div style="line-height:1em;font-family:Helvetica,Arial,sans-serif;font-weight:bold;font-size:5em;color:#09f;color:rgba(0,102,204,0.7);text-shadow:1px 3px 6px #fff,0 0 0 #000,1px 3px 6px #fff;">SwordAir.com</div>

通常，我们实现字体内嵌效果有两种途径：

- 在深色背景上使用浅色投影
- 直接在字体上添加暗部，造成视觉上的内嵌

而今天要讨论的就是后者——CSS模拟实现字体内阴影效果。

其实，这只是应用了一些小招数，你看了下面的CSS片段也许就会立即明白是怎么回事:)，而关键点就是，**用RGBA透明色模拟字体内阴影效果**。

```
body{background:#fff;}
.inset-text{
	font-family:Helvetica,Arial,sans-serif;font-weight:bold;font-size:5em;
	color:rgba(0,102,204,0.7);
	text-shadow:1px 3px 6px #fff,0 0 0 #000,1px 3px 6px #fff;
}
```

原理很简单，`text-shadow` 始终处于字体之下，所以用 `text-shadow` 的多重阴影先在字体实色之下模拟出内嵌阴影的效果，然后，通过将字体的透明度降低，达到字体内阴影的模拟效果。当然这种模拟是有局限的，比如，背景色和模拟阴影必须相同，不然就穿帮了，呵呵。其次，在不支持RGBA的浏览器里，不能发挥作用，而且还必须在RGBA之上添加默认颜色以保证老浏览能至少显示实色：

```
.inset-text{
	font-family:Helvetica,Arial,sans-serif;font-weight:bold;font-size:5em;
	color:#09f;
	color:rgba(0,102,204,0.7);
	text-shadow:1px 3px 6px #fff,0 0 0 #000,1px 3px 6px #fff;
}
```

最后，如果你选中上面那段示例的文本，可以看到很明显的模糊。这在之前也以及提到过，因为多重阴影的作用在选中时依然有效，所以为了文本的可读性，应该将选中时的文本阴影去掉。

```
::-moz-selection{text-shadow:none;}
::selection{text-shadow:none;}
```