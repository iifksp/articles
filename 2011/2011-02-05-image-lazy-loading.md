# Image Lazy Loading

**文章比较老了，下面的内容已经过时。现代浏览器中无法实现伪延迟加载。文中提到的插件所实现的伪延迟加载在现代浏览器下也没有效果。**

[延迟加载 Lazy Loading](http://en.wikipedia.org/wiki/Lazy_loading) 是一种设计模式，和预加载 Preloading 相反，最简单的说法就是“按需加载”。如果用在网页里，最多的就是图片的延迟加载。对于一张自上而下的充满图片的页面，尤其是图片列表形式，Lazy Loading的效果会非常的明显，能大幅提高加载速度和体验。大概是近期发觉自己博客里的图片日益变多，所以就打算赶下流行（似乎很大一部分博客都有使用），也简单做一下图片延迟加载。

虽然大部分的框架和插件都提供类似功能，但是代码原理也大都是类似——获取视点即viewpoint的高宽、图片的位置以及当前浏览页面部分的位置，由此判断图片是否在我们的浏览范围之内，如果是则加载它。但在具体做的时候也有很多差异，保存原来图片的src、移除src、判断是否加载、移除已经加载的事件或者数组元素等等，这些通常都有。以[JQuery插件LAZY LOAD](http://plugins.jquery.com/project/lazyload)为例，它就是用增加的original属性保存原来图片的src，然后再按需加载它们。

就在不久前，我把自己博客对[YUI](http://developer.yahoo.com/yui/3/) 3.2的引用改成了对[JQuery](http://jquery.com/) 1.4.4的引用，所以这次索性就使用了其插件LAZY LOAD。

**更新**：这个插件在现代浏览器下已经毫无效果，甚至会造成图片重复下载。官网也已挂出相关告示。此插件确实已经没有必要。不过也基本无害。

[Lazy Load Plugin for jQuery](http://www.appelsiini.net/projects/lazyload)因为JQuery的流行以及使用上的便利被广泛应用，效果也确实不错。在其主页上标注的“灵感来自 Matt Mlinac 制作的 [YUI ImageLoader](<a rel="external nofollow" href="http://developer.yahoo.com/yui/imageloader/">) 工具箱”不禁又让我怀念起YUI。具体使用上，加载 jquery.min.js 以及 jquery.lazyload.mini.js 并调用 `$("img").lazyload();` 就行了。

我加载的是Google Ajax API CDN，以及一个lazyload的压缩版。
```
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.5.0/jquery.min.js"></script>
<script type="text/javascript" src="http://www.appelsiini.net/download/jquery.lazyload.mini.js"></script>
```
```
$(document).ready(function(){
	$("img").lazyload();
});
```
关于更多的使用方式，插件主页上都有列举，并且能够找到国人翻译的版本，所以这里不再冗叙。如果想看看源代码，可以看看下面的链接：

- http://code.jquery.com/jquery-1.5.js
- http://www.appelsiini.net/download/jquery.lazyload.js

当然，对于wordpress来说这么做也许太过麻烦，用 [jQuery Image Lazy Load WP](http://wordpress.org/extend/plugins/jquery-image-lazy-loading/) 应该是最直接和方便的办法了。但我真的不是很喜欢插件的形式，何况这个功能对图片不多的个人博客来说，本来就不那么重要。

最后，我也找出了我接触过的框架的对应的图片延迟加载的应用，一并整理在这里：

- MooTools：[MooTools LazyLoad](http://davidwalsh.name/lazyload)
- YUI 3: [ImageLoader](http://developer.yahoo.com/yui/3/imageloader/)
- Prototype: [lazierLoad](http://www.bram.us/projects/js_bramus/lazierload/)