# 漫谈font-size

起因是下面的这句话：
```
font-size: 75%; /* Resets 1em to 11px */
```
这是我曾经喜欢的wordpress主题 Bito 的第一句CSS。印象里还是记得默认值是16px，那么75%就是12px了。不过这只是表面问题，其实大部分人都不怎么关系字体大小的本质。

## 从标准看起

[W3Cschools的font-size参考](http://www.w3schools.com/css/pr_font_font-size.asp)相当简单，只是简单的列举了属性的可取值。并且[CSS2.1 Specification RC20090908](http://www.w3.org/TR/2009/CR-CSS2-20090908/fonts.html#propdef-font-size)里，关于font-size的定义也并不多。

大体上，font-size的值非常宽泛，即可以是关键词定义的绝对值，可以是百分比或者 em 的相对值，还可以是绝对单位px。在实际工作中，我自己都似乎快习惯于用px定义字体大小，很少使用到那些关键字或者比例，但可能这不是个好习惯。回想自己学习CSS的过程里的例项，大部分都是用em或者百分比的。

## 关于px

对font-size直接应用px值，这样做非常精确，而且也方便，所以我们习以为常。当对font-size赋予px值意味着浏览器将会把文本渲染成指定的像素那般高。并且通常情况是，西文字符在9px以下、中文字符在12px以下时，文字将难以辨认。使用像素单位的主要问题是两个：

1. 当把文字设定为固定px值后，IE6以下浏览器将不能对其缩放。
2. 固定尺寸后失去级联特性。

老外的眼里，第一点似乎是个大问题，因为他们把网站的可访问性和易用性看的很重，无法看清字几乎是不能容忍的。而第二点，只要不做弹性布局或者设置字体调节器之类的应用，并且针对的只是显示器，也就不是大问题。这恐怕也是导致px这么泛滥的原因。好用，简单，并且最重要的是——精确。

但是对于易用性来说，px似乎不是个好东西。
[W3C Web 可访问性指南](http://www.w3.org/TR/1999/WAI-WEBCONTENT-19990324/#tech-relative-units)：在标记语言的属性值和样式表属性值里使用相对单位而不是绝对单位。[...]比如，在CSS里，使用 ‘em’ 或者 百分比长度 而不是使用绝对单位 ‘pt’ 或者 ‘cm’ 。

## 百分比的使用

百分比和em都是相对值，相对于父元素的字体大小。1em等于1个字体的大小，[Chris Coyier](http://css-tricks.com/)认为em是基于大写字母 M 的宽度(除此以外，我找不到更好的参考)。使用百分比和em可以很好使得各个元素间产生级联的比例关系。

鉴于windows的浏览器，默认的字体大小是medium，即 16ppem (后面会讲到)下显示为16px。所以75%就是12px，而62.5%就是10px。

基于[font-sizeのパーセント表記一覧](http://webtech-walker.com/archive/2008/05/16032443.html)里的百分比的零散表格，我重新组织了下面这张整表。下表仅仅作为参考，因为百分比的微弱递减或递增不会对文本产生可视化的效果(当然也有例外)，反而容易产生出混乱的级联百分比关系。

<table>
	<tr>
		<th>值\相对值</th>
		<th>12px</th>
		<th>13px</th>
		<th>14px</th>
		<th>15px</th>
		<th>16px</th>
	</tr>
	<tr>
		<th>10px</th>
		<td>84%</td>
		<td>77%</td>
		<td>72%</td>
		<td>67%</td>
		<td><span style="color:red;">63%</span></td>
	</tr>
	<tr class="even">
		<th>11px</th>
		<td>92%</td>
		<td>85%</td>
		<td>79%</td>
		<td>74%</td>
		<td>69%</td>
	</tr>
	<tr>
		<th>12px</th>
		<td>100%</td>
		<td>93%</td>
		<td>86%</td>
		<td>80%</td>
		<td><span style="color:red;">75%</span></td>
	</tr>
	<tr class="even">
		<th>13px</th>
		<td>109%</td>
		<td>100%</td>
		<td>93%</td>
		<td>87%</td>
		<td>82%</td>
	</tr>
	<tr>
		<th>14px</th>
		<td>117%</td>
		<td>108%</td>
		<td>100%</td>
		<td>94%</td>
		<td>88%</td>
	</tr>
	<tr class="even">
		<th>15px</th>
		<td>125%</td>
		<td>116%</td>
		<td>108%</td>
		<td>100%</td>
		<td>94%</td>
	</tr>
	<tr>
		<th>16px</th>
		<td>134%</td>
		<td>124%</td>
		<td>115%</td>
		<td>107%</td>
		<td>100%</td>
	</tr>
	<tr class="even">
		<th>17px</th>
		<td>142%</td>
		<td>131%</td>
		<td>122%</td>
		<td>114%</td>
		<td>107%</td>
	</tr>
	<tr>
		<th>18px</th>
		<td>150%</td>
		<td>139%</td>
		<td>129%</td>
		<td>120%</td>
		<td>113%</td>
	</tr>
	<tr class="even">
		<th>19px</th>
		<td>159%</td>
		<td>147%</td>
		<td>136%</td>
		<td>127%</td>
		<td>119%</td>
	</tr>
	<tr>
		<th>20px</th>
		<td>167%</td>
		<td>154%</td>
		<td>143%</td>
		<td>134%</td>
		<td>125%</td>
	</tr>
	<tr class="even">
		<th>21px</th>
		<td>175%</td>
		<td>162%</td>
		<td>150%</td>
		<td>140%</td>
		<td>132%</td>
	</tr>
	<tr>
		<th>22px</th>
		<td>184%</td>
		<td>170%</td>
		<td>158%</td>
		<td>147%</td>
		<td>138%</td>
	</tr>
	<tr class="even">
		<th>23px</th>
		<td>192%</td>
		<td>177%</td>
		<td>165%</td>
		<td>154%</td>
		<td>144%</td>
	</tr>
	<tr>
		<th>24px</th>
		<td>200%</td>
		<td>185%</td>
		<td>172%</td>
		<td>160%</td>
		<td>150%</td>
	</tr>
	<tr class="even">
		<th>25px</th>
		<td>209%</td>
		<td>193%</td>
		<td>179%</td>
		<td>167%</td>
		<td>157%</td>
	</tr>
	<tr>
		<th>26px</th>
		<td>217%</td>
		<td>200%</td>
		<td>186%</td>
		<td>174%</td>
		<td>163%</td>
	</tr>
</table>

之所以选择了参考了这位日本开发者的资料是有原因的。比如，我们常常全局定义字体的相对medium(通常为16px)的62.5%，也就是10px，然后再对文档的个个其他部分定义em值。相对于10px，1.2em就是12px，1.6em就是16px，依次类推。这样的比例使用起来很直观，并且会在所有的浏览器里表现完好，当然除了IE。

IE会错误的显示字体大小(感觉上是字体框架有细微的变大)，并且仅限于中文。除非我们将比例设置成63%而不是62.5%。所以当我看到这张表中63%的时候，我觉得同是非西欧字符，这张表应该更具有参考价值。不过这些值我也没能自己去全部测试，偷懒一下～

em似乎比百分数更加直观，而且对于一个字体的宽度的把握往往也更容易。同时它们有相同的级联性，使用的时候总是要避免规则的叠加。比如定义 div{font-size:1.2em;} ，那么在div里包含div就会出问题。1.2emx1.2em的叠加会使得文字比想象中的大——但这还不要紧，如果使用的是0.8em？缩小倍率的多次叠加很快会使得文字变得无法分辨。

无论是em还是百分比，都是相对值。并且常常相对于medium。但是关于medium这类关键字，就有了更多的话题。

## 关键字

我对于font-size的理解一直停留在上面说的那些，直到半个月前，我读到这篇[Toward a standard font size interval system](http://style.cleverchimp.com/font_size_intervals/altintervals.html)。

回到之前的[CSS2.1 Specification RC20090908](http://www.w3.org/TR/2009/CR-CSS2-20090908/fonts.html#propdef-font-size)，标准把关键字定义为这样：

<table>
	<tr>
		<th>CSS absolute-size values</th>
		<td>xx-small</td>
		<td>x-small</td>
		<td>small</td>
		<td>medium</td>
		<td>large</td>
		<td>x-large</td>
		<td>xx-large</td>
		<td style="background:#eee;">&nbsp;</td>
	</tr>
	<tr>
		<th>HTML font sizes</th>
		<td>1</td>
		<td style="background:#eee;">&nbsp;</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>7</td>
	</tr>
</table>
回顾略显古老的[HTML 3.2 Reference Specification - Font](http://www.w3.org/TR/REC-html32#font)，这七个关键字是否是对应于已经实现的HTML font标记不得而知，但是上面的对应关系并不是一一对应的，medium对应的是size 3。[Html Font Size Tutorial CSS Style](http://www.htmlcss.org/font-size.html)上可以清楚的看到全部的关键字效果。

下面这张图，更加直观些。但是注意第二行。同样对于English Font加上larger，IE8和FF渲染为18px，Opera、Chrome和Safari则渲染为19px。

![font size compare](/content/images/2013/Dec/font_size_compare.png)

下面的表截取自[Toward a standard font size interval system](http://style.cleverchimp.com/font_size_intervals/altintervals.html)里的Synoptic table，对应于windows常用的96dpi的情况。是的，96dpi，我们几乎从来不去改变这个默认的值，除非也许某个人因为高分屏字太小的折磨，而主动调大到了120dpi。

<table cellpadding="3" cellspacing="0">
	<tr>
		<td style="text-align:right;background:black;color:#FFCC00;">CSS absolute size keywords:</td>
		<td style="background:#FFFFCC;">xx-small</td>
		<td style="background:#FFFF99;">x-small</td>
		<td style="background:#FFFF66;">small</td>
		<td style="background:#FFFF00;">medium</td>
		<td style="background:#FFFF66;">large</td>
		<td style="background:#FFFF99;">x-large</td>
		<td style="background:#FFFFCC;">xx-large</td>
		<td style="background:#808080;">&nbsp;</td>
	</tr>
		<td style="text-align:right;background:black;color:#FFCC00;">HTML absolute font sizes:<br />(interpolated Mozilla values)</td>
		<td style="background:#FFFFCC;">1</td>
		<td style="background:#808080;">&nbsp;</td>
		<td style="background:#FFFF66;">2</td>
		<td style="background:#FFFF00;">3</td>
		<td style="background:#FFFF66;">4</td>
		<td style="background:#FFFF99;">5</td>
		<td style="background:#FFFFCC;">6</td>
		<td style="background:#FFFFCC;">7</td>
	<tr>
		<td style="text-align:right;background:black;color:#FFCC00;">HTML headings:<br />(interpolated Mosaic values)</td>
		<td style="background:#FFFFCC;">h6</td>
		<td style="background:#808080;">&nbsp;</td>
		<td style="background:#FFFF66;">h5</td>
		<td style="background:#FFFF00;">h4</td>
		<td style="background:#FFFF66;">h3</td>
		<td style="background:#FFFF99;">h2</td>
		<td style="background:#FFFFCC;">h1</td>
		<td style="background:#808080;">&nbsp;</td>
	</tr>
	<tr>
		<td style="text-align:right;background:#FFCC00;color:black;">normalized scaling factor:</td>
		<td>60% - 3:5</td>
		<td>75% - 3:4</td>
		<td>89% - 8:9</td>
		<td>100% - 1:1</td>
		<td>120% - 6:5</td>
		<td>150% - 3:2</td>
		<td>200% - 2:1</td>
		<td>300% - 3:1</td>
	</tr>
	<tr>
		<td style="text-align:right;background:black;color:#FFCC00;">px computed from a 16 ppem base:<br /><br />e.g., 12pt @ 96ppi or 16pt @ 72ppi<br />(XP 5.0 UA default)</td>
		<td>10px<br /><br /><span style="font-size:10px">E</span></td>
		<td>12px<br /><br /><span style="font-size:12px">E</span></td>
		<td>14px<br /><br /><span style="font-size:14px">E</span></td>
		<td>16px<br /><br /><span style="font-size:16px">E</span></td>
		<td>19px<br /><br /><span style="font-size:19px">E</span></td>
		<td>24px<br /><br /><span style="font-size:24px">E</span></td>
		<td>32px<br /><br /><span style="font-size:32px">E</span></td>
		<td>48px<br /><br /><span style="font-size:48px">E</span></td>
	</tr>
</table>

牵扯到一个很重要的概念，ppem。它指的是Pixels per em，即每个字体大小的像素数，定义了字符在屏幕上的易读性。关于更多的信息，我强烈建议阅读[Toward a standard font size interval system](http://style.cleverchimp.com/font_size_intervals/altintervals.html)原文。

当把xp的dpi从96调整到120， 整个系统的尺寸，包括图形和文字都被放大了。此时，如果打开IE，font-size:medium 不再是16px，而是20px。虽然我也试过Chrome和FireFox，但它们没有变化。我不明白原因，如果有人知道，麻烦请告诉我声: ）

这再次的提醒了我们，CSS不是只为显示器而设计的，而且也不是专为windows而设计的。我们有时可能需要考虑更多更多。
水平有限，文中如有错误望指正~