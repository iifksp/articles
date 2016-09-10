# 浏览器模式

关于浏览器模式，一直以来的理解是这样的：浏览器厂商出于那些老站点的向后兼容的目的，创建了两种模式。即标准模式(standards mode)和怪异模式(quirks mode)。在标准模式里，浏览器按照规范渲染页面，而在怪异模式里，浏览器以一种老式的或者是模拟老式浏览器的渲染方式表现页面。

这些并没有错，但是还不够全面和深入。当我回顾《CSS Mastery》的时候，也让我想起了很多渐渐淡忘的、并且也可能是无关紧要的其他碎片。
比如，两种模式最大的差异的例子就是IE盒模型的解释。IE如此，Opera 7也是如此。再比如，Mozilla和Safari的第三种“准标准模式(almost standards mode)”，只是在处理表格的方式上有些细微的差异，其他与标准模式无异。等等。

一直以来，确保DOCTYPE的正确也是非常重要的事。浏览器根据DOCTYPE是否存在以及是何种DOCTYPE来确定渲染方式。如果总结如表，应该是这个样子。
<table>
	<tr>
		<th>DOCTYPE</th>
		<th>MODE</th>
	</tr>
	<tr>
		<td>XHML + 形式完整DOCTYPE</td>
		<td><span style="color:green">标准模式</span></td>
	</tr>
	<tr>
		<td>HTML 4.01 + strict DTD</td>
		<td><span style="color:green">标准模式</span></td>
	</tr>
	<tr>
		<td>DOCTYPE包含URL和transitional DTD</td>
		<td><span style="color:green">标准模式</span></td>
	</tr>
	<tr>
		<td>DOCTYPE只包含transitional DTD</td>
		<td><span style="color:red;">怪异模式</span></td>
	</tr>
	<tr>
		<td>DOCTYPE不存在或形式不完整</td>
		<td><span style="color:red;">怪异模式</span></td>
	</tr>
</table>
这张由我根据《CSS Mastery》一书所列出的表并不怎么完整，[Alastair Campbell](http://www.google.com/profiles/alastc)有一个更加全面的[关于IE浏览器模式和DOCTYPE的表格](http://alastairc.ac/testing/IE_Doctypes/)。

另外一个可能有点过时的，是Eric Meyer[关于DOCTYPE switching的表格](http://meyerweb.com/eric/dom/dtype/dtype-grid.html)。多年之后我再去看这个链接的时候，发觉它居然还在:) 
而现在，我更喜欢看[QuirksMode](http://www.quirksmode.org/)上的资料。

实际工作中，一般都不会忘记去添加DOCTYPE，所以很多情况都只是我们无意为之。比如IE6，当DOCTYPE不是页面的第一个元素的时候，就会进入怪异模式。这导致在页面开头添加XML文件的可选声明
```
<?xml version="1.0" encoding="utf-8"?>
```
也会使页面表现出人意料。之后的IE7修复了这个bug，但却不完全。在IE7中，一个xml声明并不会再导致进入怪异模式，但是这并不表示在DOCTYPE之前加入其他东西也能不触发。比如html注释。
```
<?xml version="1.0" encoding="utf-8"?> 
<!-- ... and keep IE7 in quirks mode --> 
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" 
	"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```
这段代码依旧触发IE7的怪异模式，而触发的原因仅仅只是一段html注释。所以在DOCTYPE前，一段html注释也会导致怪异模式下不可预料的表现。

另一个需要小心的陷阱就是BOM头。当php处理UTF文件的时候会把BOM读成字符，include之后就可能会跑到DOCTYPE前面，从而再次触发IE的怪异模式。所以保存UTF8编码的时候可以选无BOM，BOM对于UTF8的意义本来就不大。

前段时间写的HTML5，DOCTYPE简化到只剩下
```
<!DOCTYPE html>
```
对此w3cschool上的解释是这样的：HTML 4.01 中的 doctype 需要引用一个 DTD，这是因为 HTML 4.01 基于 SGML。HTML 5 不基于 SGML，也不需要引用 DTD，但是需要声明文档类型让浏览器按照它们应该的方式来运行。

事实证明，IE只要认到 `<!DOCTYPE html` 这串字符就会用标准模式...

也许HTML5的日子真的就这么迫近了，新的总结，那就是后话了，呵呵。
最后记录些DOCTYPE，发觉自己总是在复制他们~

HTML 4.01 Strict
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```
HTML 4.01 Frameset
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```
HTML 4.01 Transitional
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```
XHTML 1.0 Strict
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```
XHTML 1.0 Frameset
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```
XHTML 1.0 Transitional
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```
XHTML 1.1
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```