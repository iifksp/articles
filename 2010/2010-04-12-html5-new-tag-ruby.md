# HTML5 新标签 ruby
HTML5也许真的离我们不是很遥远。今天匆匆浏览了一下3月10日版本的关于HTML5和HTML4区别的W3C的草案: http://dev.w3.org/html5/html4-differences/#changed-elements ，就更加觉得仿佛就在眼前一般。在新标签里看到了ruby的名字突然就联想到了ruby语言。虽然这个ruby和ROR没什么关系，但引起了我想要去了解的兴趣。

ruby元素是用来标记称之为 ruby 注释的。ruby 注释是用[ruby字符](http://en.wikipedia.org/wiki/Ruby_character)标示在汉字等东亚字符的上方或者右方用来标示的拼音的那部分。就像小时候读课文时，标记在文字上的拼音，就是ruby字符，或者称之为ruby(rubi)。

尽管ruby是HTML5的新元素，但W3C里有关ruby的定义不仅限于草案。这里面已经有[ruby注释的推荐标准](http://www.w3.org/TR/ruby/)，甚至有[ruby的CSS3组件定义的工作草案](http://www.w3.org/TR/2001/WD-css3-ruby-20010216/)。而在HTML5的[ruby元素的定义](http://dev.w3.org/html5/spec/text-level-semantics.html#the-ruby-element)里，可以看到关于这个标签的说明

> The ruby element allows one or more spans of phrasing content to be marked with ruby annotations. Ruby annotations are short runs of text presented alongside base text, primarily used in East Asian typography as a guide for pronunciation or to include other annotations. In Japanese, this form of typography is also known as furigana.

随后我就做了相应的浏览器测试，代码如下：
```
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Ruby Tag Test</title>
	<style>
		rt{
			font-size:12px;
		}
	</style>
  </head>
  <body>
    <p>ruby tag test</p>
	<p>
		<ruby>
			漢 <rt> かん </rt>
			字 <rt> じ　 </rt>
		</ruby>
	</p>
	<p>
		<ruby>
			漢 <rt> ㄏㄢˋ </rt>
			字 <rt> ㄗˋ　 </rt>
		</ruby>
	</p>
	<p>
		<ruby>
			汉 <rt> hàn </rt>
			字 <rt> zì  </rt>
		</ruby>
	</p>
  </body>
</html>
```
这段HTML在Chrome和IE9pre上都表现良好，尽管现在浏览器对于ruby标签的支持并不完善。Opera 9.64，FireFox 3.5.9，Safari 4.0.4都未能正确的显示，因为没装最新版，也不知道最新版本下会怎么样。有意思的是，IE678都支持ruby标签，虽然显示上ruby注释的字体很小，但是下面的CSS同样有效~
```
rt{
	font-size:12px;
}
```
甚至用IEtester开个IE5.5也一样可以很好的显示出ruby注释。这不禁让我感觉很怪。google后找到一份关于Internet Explorer对ruby注释的标准支持文档(MS-RUBY-Internet-Explorer-Ruby-Annotation-Standards-Support.pdf，日期03/17/2010)，里面提及了IE对于Ruby注释的支持。这包括IE7的标准模式和怪异模式、IE8的怪异模式IE7模式和IE8模式。

这下让我有点汗颜了，IE6用了这么久，居然不知道它还能支持这种东西...

其他浏览器方面，[MikeSmith](http://sideshowbarker.net/)的博文里谈到，Chrome的开发者Roland Steiner在WebKit里添加了ruby的支持([r50495](http://trac.webkit.org/changeset/50495)，相关的[bug 28420](https://bugs.webkit.org/show_bug.cgi?id=28420))。这使得WebKit 的nighty和Chrome dev支持了ruby标签。

### 浏览器兼容更新

**2010/05/19**

<table>
	<tr>
		<th>浏览器</th>
		<th>支持</th>
		<th>字体</th>
		<th>备注</th>
	</tr>
	<tr>
		<td>FireFox 3.6.3(Gecko/20100401)</td>
		<td>未正确解析</td>
		<td>N/a</td>
		<td>html5.enable = true</td>
	</tr>
        <tr>
		<td>Opera 10.53(Presto/2.5.24 Version/10.53)</td>
		<td>未正确解析</td>
		<td>N/a</td>
		<td> </td>
	</tr>
        <tr>
		<td>Safari(win) 4.0.5(531.22.7)</td>
		<td>未正确解析</td>
		<td>N/a</td>
		<td> </td>
	</tr>
	<tr>
		<td>Chrome 5.0.366.2</td>
		<td>正确解析</td>
		<td>12px</td>
		<td> </td>
	</tr>
	<tr>
		<td>IE6(6.0.2900.5512)</td>
		<td>正确解析</td>
		<td>8px</td>
		<td> </td>
	</tr>
	<tr>
		<td>IE7(tester)</td>
		<td>正确解析</td>
		<td>8px</td>
		<td> </td>
	</tr>
	<tr>
		<td>IE8(tester)</td>
		<td>正确解析</td>
		<td>8px</td>
		<td> </td>
	</tr>
	<tr>
		<td>IE9-preview</td>
		<td>正确解析</td>
		<td>8px</td>
		<td> </td>
	</tr>
</table>


