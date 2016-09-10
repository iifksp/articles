# WOFF和Google Font API

[Web Design Is 95% Typography](http://www.informationarchitects.jp/en/the-web-is-all-about-typography-period/) ，这并不夸张。尽管我不是设计师，和字体打交道的时间相对较少，却也知道其重要性。不过 [Typography](http://en.wikipedia.org/wiki/Typography) 一词并没有非常正式的官方中译，通常称为“字体排印”，而我们Web开发者讨论的也只是其中一个子集 [Web typography](http://en.wikipedia.org/wiki/Web_typography) 。对此我没有什么深入研究，我强烈推荐 [Type is Beautiful Blog](http://www.typeisbeautiful.com/) ，不论是Web开发者还是设计师，都应该对 Typography 这个我们日常工作内容的超集有所了解——对于前端恐怕尤为如此，毕竟浏览器便是此分支中的一个排版引擎。

本文主要讨论的是 WOFF，以及 Google Font API 中 WOFF 调用的相关应用。

## WOFF TTF OTF EOT

设计师在设计网页的时候常常遇到字体问题，尽管设计 Banner 的时候可以用图片的形式表现出他们喜欢的字体，但对于Web内容来说却是无能为力的。客户端安装的字体总是有限的，设计因此遇到的限制只能通过网络的方式解决，于是就出现了网络字体。当页面加载的时候同时下载字体资源就能保证页面按照设计呈现，但这样遇到2个问题，第一，带宽的限制，这使得字体的文件的尺寸不能过大，完整拉丁字母集的字体文件通常都在数百K，相对来说还是有些太大。第二，字体的版权问题，免费和商业永远是个矛盾。所以网络字体的应用并不顺利，尽管不断在发展，却乏人问津。

[WOFF](http://en.wikipedia.org/wiki/Web_Open_Font_Format)是Web Open Font Format的缩写，作为从2009年才开始发展的网络字体格式，现在就已经迅速被广泛的支持。原因可能很简单，WOFF更小，并且包含来源信息。虽然WOFF可以看做是 [TrueType](http://en.wikipedia.org/wiki/TrueType) (TTF)以及 [OpenType](http://en.wikipedia.org/wiki/OpenType) (OTF)的再包装，但不同的是，WOFF是直接压缩了的字体格式而无需服务器再做额外的压缩，这使得相对于TTF格式，其体积通常要小40%以上。由于对于 WOFF、TrueType、OpenType 的由来说明超出了本文的讨论范围，推荐阅读[字体数字化简史与 WOFF](http://www.typeisbeautiful.com/2009/11/1617)获得完整的概念。

不过即使被广泛支持，我们仍然很少能感觉到其存在，原因很简单，中文字体的文件大小从一开始就失去了作为网络字体的可能——通常数十M的大小，只能眼睁睁地看着各种拉丁字体的百花齐放，自己维持的着老掉牙的宋体... 经常浏览外国的网站的话，对于网络字体应该不陌生，不过要在国内找出几个使用的例子还真有些难度。前端时间刚巧看到萝卜的图床的首页[dulei.si](http://dulei.si/)使用了WOFF(**更新**：原页面内容已更改)，虽然只有左下角几个简单的英文字母，却能看出页面编写者的用心，并且下面说的Google做的事也许能在不久将来让中文字体少量应用网络字体格式成为可能。

那么如何使用WOFF？其实WOFF和更早先被支持的TTF和OTF格式在使用方面并没有什么不同，都是通过CSS规则@font-face定义字体名然后再在样式表里使用。@font-face是CSS2中的一部分，现在可以在[CSS Fonts Module Level 3](http://www.w3.org/TR/css3-fonts/#font-face-rule)中找到更为详细的说明。比如下面的代码：
```
@font-face {
	font-family: fontname;
	src:	url(link/to/the/font/fontname.woff) format("woff"),
		url(link/to/the/font/fontname.ttf) format("truetype");
}
body {
	font-family: fontname, Times, Times New Roman, serif;
}
```
由于firefox从3.5开始支持TTF和OTF，并从3.6开始支持WOFF，所以给出文件的两个版本，支持WOFF的浏览器会下载WOFF的文件，支持@font-face但不支持WOFF的浏览器会使用同一种字体的truetype版本。不过这是一段相当过时的版本，Firefox的当前版本是5，并且3.5版本以下的使用数量已经非常少，所以在各个其他浏览器逐步跟进的之后，可以直接只用WOFF。

当然，IE的问题总是需要单独拿出来说事。IE早在5.0就实现了@font-face，当然是**部分的实现了**， 那还是在1997年。但是很遗憾包括IE8在内都只支持其专有格式[Embedded Open Type](http://en.wikipedia.org/wiki/Embedded_OpenType)(EOT)，一种压缩的网页嵌入字体格式，并且这种格式防止了字体的拷贝，也许更多的商业字体会欢迎EOT，不过到头来还是只有IE支持渲染EOT。

所以不论是何种网络字体，EOT，TTF，OTF，WOFF，它们的区别就我们使用来说，只是在于压缩、版权、浏览器支持范围这几个不同而已。就目前的支持范围来说，WOFF有用最为广泛的支持和最为合适的条件，任何一个现代浏览器中它都能良好工作，这其中包括IE9。而之后，将这些不同点融合在一起提供一个统一的使用方式，那就是 Google Font API 所做的事了。

## Google Font API

Google提供了一个[font API](http://code.google.com/webfonts/)使我们可以方便的载入我们喜欢的web字体，并且使我们不需要考虑过多的浏览器对字体格式支持的细节。而我们所要做的仅仅只是在HTML里链接一个样式表文件，然后使用其中的字体。比如：

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>WOFF and Google Font API</title>
		<meta name="description" content="" />
		<meta name="author" content="iifksp" />
		<link rel="stylesheet" type="text/css" href="http://fonts.googleapis.com/css?family=Tangerine">
		<style type="text/css">
			h1 {
				font-family: 'Tangerine', serif;
				font-size: 48px;
			}
		</style>
	</head>

	<body>
		<div>
			<header>
				<h1>WOFF and Google Font API</h1>
			</header>
			<footer>
				<p>&copy; Copyright 2011 by iifksp</p>
			</footer>
		</div>
	</body>
</html>
```

下图展示了字体使用后的对比效果。效果还是相当不错。

![fonts contrast](https://swordair.com/content/images/2013/Dec/fonts_contrast.png)

我们可以从[Google Fonts API目录](http://code.google.com/webfonts)里选择喜欢的字体使用。从上面的代码就能看到使用上非常简单，只是link一个样式文件[http://fonts.googleapis.com/css?family=Tangerine] ，告诉google我们选择的字体，之后的使用和一般操作无异。而正是这句外部资源载入，就是 google font api 为我们做了全部工作。

使用不同浏览器访问这个URL:  [http://fonts.googleapis.com/css?family=Tangerine] 得到的结果自然是不一样的，Google 根据浏览器不同返回不同的适用的样式表，返回字体内容也不相同，对支持WOFF字体格式的浏览器，返回WOFF资源是首选。但对其他浏览器，比如IE6，返回的是引用EOT格式的资源——**Google Font API 保证了返回后的引用的资源能被各个浏览器可用**。比如Chrome的返回内容是这样的：

```
@media screen {
@font-face {
  font-family: 'Tangerine';
  font-style: normal;
  font-weight: normal;
  src: local('Tangerine'), url('http://themes.googleusercontent.com/font?kit=HGfsyCL5WASpHOFnouG-RD8E0i7KZn-EPnyo3HZu7kw') format('woff');
}
}
```

Firefox访问同样的URL得到的内容稍有不同，没有 @media screen 这句 media query。

当然这只是此API最为基本的使用方法，[Google Font API Getting Started](http://code.google.com/apis/webfonts/docs/getting_started.html) 提供完整的使用方法，所以这里不在冗叙，大体上就是Google给我们提供了按需索取的便捷方式。实际上，我们在对大标题使用漂亮字体的时候，并不是需要加载全部的字体文件，因为标题里的字母只是一小个子集——Google允许我们只请求需要的字体的部分(虽然这部分功能还处于beta阶段)：

在url附加内容参数就能以更少的请求量完成同样的事：
[http://fonts.googleapis.com/css?family=Tangerine<span style="color:#f30">&text=WOFF%20and%20Google%20Font%20API</span>]

这使得中文使用网络字体成为一种可能，虽然现在还不行，但相信未来部分标题可以做到。那么只请求所用内容的字体相比请求整个字体文件减少了多少下载量呢？诉求开发工具的firebug和webkit开发人员工具能给出答案，只是，**当前只有后者能够回答**:)

虽然我也更喜欢使用firebug，不过这次它不给力了，不能正确显示下载资源的大小和内容，在响应里是空白，下载大小是个问号(?)，并且**只有当内容从缓存读取时firebug才能正确显示文件尺寸**。反观chrome，却清楚的告诉了我内容以及下载的文件大小。

![compare firebug with webkit develop tool](https://swordair.com/content/images/2013/Dec/compare_firebug_with_webkit_develop_tool.png)

可以看到，这个字体文件的大小是31KB，还算好，不过如果只请求 WOFF and Google Font API 这几个字母的话，尺寸仅4.3KB，已经非常理想。如果这时候去看Chrome显示的内容，会清楚的看到字体内容的情况——这点firebug尚未做到。下图是请求整个字体文件和只请求所需内容的结果对比，很有趣，不是嘛？

![google font api optimizing requests](https://swordair.com/content/images/2013/Dec/google_font_api_optimizing_requests.png)

说到这里这个话题就差不多了，Google Font API 给我们使用网络字体带来了巨大的便利，虽然现在字体还不多功能也尚未完整，但有朝一日一定会派上用场的，特别是中文，我期待这有一天能用上各种各样的漂亮的字体，来编织更加多彩的网页。

## 扩展阅读

- [Mozilla Supports Web Open Font Format](http://blog.mozilla.com/blog/2009/10/20/mozilla-supports-web-open-font-format/)
- [Web Open Font Format for Firefox 3.6](http://hacks.mozilla.org/2009/10/woff/)
- [Industry Support for WOFF and Firefox 3.6](http://hacks.mozilla.org/2010/01/industry-support-for-woff-and-firefox-3-6/)
- [WOFF File Format(draft of 2009-10-23)](http://people.mozilla.com/~jkew/woff/woff-spec-latest.html)
- [WOFF 使用指南](http://www.typeisbeautiful.com/2010/01/1903)