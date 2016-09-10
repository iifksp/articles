# jQuery plugin - Scroll to Top

“返回顶部”的应用相当广泛，在典型的超长页面里，它几乎是必须的。比如新浪微博里，页面的长度因数据的加载而长达数千像素，它的“返回顶部”并没有太多特别之处，唯一有趣的是那个向上箭头是有两个字符组合而成而并非图片:
```
<a href="javascript:;" id="base_scrollToTop" class="W_gotop" style="visibility:hidden;">
	<span>
		<em class="sj">♦</em>
		<em class="fk">▐</em>
		返回顶部
	</span>
</a>
```
正是这种细微之处，体现了前端的创想力以及对页面性能的进益求精。

对于像渣浪微博这样的网站动画滚动是不合适的，因为页面太长，长到动画滚动显得太过僵硬，而且对于这类网站性能才是第一位的，动画用在这种地方显然很多余。不过对于个人网站或者展示类网站，页面长度只是稍稍偏长，用动画过渡就显得不那么突兀了。

一个典型的动画滚动到顶部的代码片段可能和下面这个极为神似：

```
function scrollToTop(a, f){
	a = a || 0.1;
	f = f || 10;
	var d_x = b_x = w_x = 0,
		d_y = b_y = w_y = 0;
	if (document.documentElement) {
		d_x = document.documentElement.scrollLeft || 0;
		d_y = document.documentElement.scrollTop || 0;
	}
	if (document.body) {
		b_x = document.body.scrollLeft || 0;
		b_y = document.body.scrollTop || 0;
	}
	var w_x = window.scrollX || 0,
		w_y = window.scrollY || 0;
	var x = Math.max(w_x, Math.max(b_x, d_x)),
		y = Math.max(w_y, Math.max(b_y, d_y));
	var speed = 1 + a;
	window.scrollTo(Math.floor(x / speed), Math.floor(y / speed));
	if(x > 0 || y > 0) {
		setTimeout(scrollToTop,f,a,f);
	}
}
```

基本上也都是如此，可能参数上稍有不同。如果使用jQuery，滚回到顶部也许是最为常用的代码片段，简单到只有一句：

```
$('body,html').animate({scrollTop:0},300);
```
之所以同时动画 `body` 和 `html` 元素，是因为各浏览器对于 `scrollTop` 这个属性的解释并不相同：

- Firefox和IE的标准模式下，只有设置 `html` 的 `scrollTop` 是有效的，对 `body` 的设置会被忽略。
- Firefox和IE的怪异模式、以及Safari和Chrome(任意模式)，只有设置 `body` 的 `scrollTop` 是有效的，对 `html` 的设置会被忽略。

因而，我们必须同时设置 `html` 和 `body`，但另一个问题也随之而来：Opera会重复设置这两个值从而使得动画变成了“破画”一般，尤其是在长页面里。好在新的Opera似乎已经修复了这个问题，至少我测试的时候没有发现。尽管不完全，因为Opera至今关于 `scrollTop` 的bug仍然偏多，比如设置 `textarea` 的 `scrollTop` 没有作用(Opera 11.06)。

关于Opera的这个重复设置 `scrollTop` 的bug，网上有一些解决办法，比如最为直接的就是嗅探Opera并单独只动画 `html`。但这样并不可取，原因是未来Opera必会慢慢修复这些bug，基于浏览器嗅探的代码会变得过时。[jQuery smooth scroll bugs](http://www.zachstronaut.com/posts/2009/01/18/jquery-smooth-scroll-bugs.html) 中有一段关于 `scrollTop` 的兼容代码，其思想是尝试移动1px来测试浏览器是否完好支持 `scrollTop`。写到这里，巧合是当我今天去访问这个参考网址的时候页面跳出了最近风风火火的“抵制SOPA”的抗议(这几天cb上不是一直在刷SOPA的信息嘛~)：

![stop sopa](https://swordair.com/content/images/2013/Dec/stop_sopa.png)

打开页面的时候，我真心被国外站长对于争取自身权利的那份热诚深深感染到了:)

最后放上自己的一个jQuery插件Scroll to Top的DEMO，不得不说，这类插件真是满地都是——虽然我的题目是插件，但其实内容多半和插件没半毛钱关系...~hhhe，点解[DEMO](http://www.swordair.com/demos/jquery-plugin-scroll-to-top/)查看演示。