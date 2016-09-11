# Chrome的onload动画bug

一开始的起的标题是《Chrome中CSS3 transition和form导致的意外的onload动画》，不过太长了，所以最后删减了一下。但至少我遇到这个chrome的bug的时候，确实是transition和form同时出现时。

这个bug的具体的表现是：chrome会在页面加载后自动开始一段动画。

我使用的是chrome 36.0.1985.125 m，算是当前的最新版。页面中的全局样式添加了`<a>`的transition，而且还是非常偷懒的使用了all这样的参数，所以最后整个页面在加载完成后，所有的`<a>`都开始了过度效果，从尺寸到颜色，结果就导致了的整个页面的抖动。示例代码：

```
<!DOCTYPE HTML>
<html lang="en-US">
<head>
	<meta charset="UTF-8">
	<title></title>
    <link rel="stylesheet" href="style.css" />
</head>
<body>
<div class="wrapper">
	<a href="">Color Anime</a>
    <form action=""></form>
</div>
</body>
</html>
```
引用的样式文件内容也很简单：
```
a{color: #f30;
text-decoration:none;
-webkit-transition:all 1s;
-moz-transition:all 1s;
-o-transition:all 1s;
-ms-transition:all 0.3s;
transition:all 1s;}
```
经过一系列的尝试，发现这个bug只发生在齐备3个条件的情况下：

1. 使用了css3 transition。
2. 页面中有任意一个from，即便是空的。
3. 样式表文件为外链。

换言之，**如果将样式直接写在页面里，是不会触发这个动画bug的**。

之后就google了一下，看看是否有些其他信息。在chromium项目里看到了一些反馈，有好几个bug报告和这里描述的如出一辙，只是条件稍有不同。比如 [Chrome CSS3 transition explode bug when form has three or more input elements](https://code.google.com/p/chromium/issues/detail?id=332189)，不仅给出了重现bug的例子，甚至还给出了hack方法：**在`<script>`标签里至少有一个字符**。

我试了试果然有效，诡异的处理方式啊，不过文中有一点没有提到，就是**这个`<script>`标签必须出现在`<form>`之前**。所以这个bug在实际工作中可能不常见，因为很少有页面满足这些条件。但是这确实是个非常让人讨厌的bug，一个纯CSS页面每次加载完都在面前动画一下，要是出现在某个内部系统里，当真要人命啊:)

可惜的是，虽然bug提出的很早，但至今都还未能修复。好几个相同的bug报告都已经被关闭，状态都是WONTFIX，可恶啊。这让我想起了几年前的Chrome Text-shadow的bug，一样好多人提，google一样慢如蜗牛的修，虽然最好还是修好了。好在google关掉的bug也有人继续提，所以，当前如果遇到这个bug，解决的方法，要么把样式写在页面里，要么就加上一个内容为空行的`<script>`标签，然后坐等google未来几年内可以搞定他。