# PNG的压缩和优化

有关PNG的压缩和优化的主题其实也算是前端相当常见的部分，只是当前端把大把大把的时间和精力放在代码上面的时候，往往总是忽视对于图片的很多效果立竿见影的优化，而之所以这里主要说的是PNG，是因为相比JPG和GIF，PNG有更多压缩的空间。

写此文的契机是前端时间正好回顾了一篇 [sitepoint](http://www.sitepoint.com) 上的老文，《[PNG8 – The Clear Winner](http://www.sitepoint.com/png8-the-clear-winner/)》，主要讲述如何使用fireworks创建和利用PNG8的半透明，并兼容老浏览器如IE6-。虽然这篇文章和本文并没有任何关系，却让我回想起一些渐渐被遗忘的知识，作为一个coder的不断coding中渐渐忽视的问题——PNG压缩。

压缩过程毫无技术性，使用诸如 [PngOptimizer](http://psydk.org/PngOptimizer.php) 或者 [PNGOUT](http://www.advsys.net/ken/util/pngout.htm) 等工具可以轻而易举地将Photoshop、Fireworks生成的PNG图像继续缩小。我几乎总是使用 PNGOUT，原因是使用过各种工具比较得出的结论——PNGOUT生成的文件几乎总是最小的。所以**之后的讨论也是以PNGOUT作为基准的**。

当然在这里，我也额外罗列一些我曾经使用过的工具。所有工具其主页都有详细的使用和说明，所以这里也不再冗述。

- [PNGOUT](http://www.advsys.net/ken/util/pngout.htm)(本文基准工具)
- [Pngcrush](http://pmt.sourceforge.net/pngcrush/)
- [OptiPNG](http://optipng.sourceforge.net/)
- [PNGGauntlet](http://pnggauntlet.com/)
- [Smush.it](http://www.smushit.com/)
- [TweakPNG](http://entropymine.com/jason/tweakpng/)

AliExpress是我的老东家，我打心底里喜欢这家公司和我原属的团队。所以虽然已经离开，但仍不时看看曾经自己努力维护的网站。我有点惭愧在过去从未能想到去优化其首页的图片，只专注代码其实绝对让一个前端走进死胡同。

AliExpress的主页主要的sprite有三张，当前值以及压缩值如下表：

<table width="100%">
	<tr>
		<th>文件</th>
		<th>原始文件大小(bytes)</th>
		<th>压缩后(bytes)</th>
		<th>体积减小(bytes)</th>
		<th>减小百分比</th>
	</tr>
	<tr>
		<td>top_v5.png</td>
		<td>15111</td>
		<td>12107</td>
		<td>3004</td>
		<td><span style="color:#6c3">-20%</span></td>
	</tr>
	<tr>
		<td>spr_we_buyer_common.png</td>
		<td>16516</td>
		<td>11289</td>
		<td>5227</td>
		<td><span style="color:#6c3">-32%</span></td>
	</tr>
	<tr>
		<td>sprite_home.png</td>
		<td>11562</td>
		<td>9824</td>
		<td>1738</td>
		<td><span style="color:#6c3">-16%</span></td>
	</tr>	
</table>

对于这3张 sprite，总共将节约约10K的图片大小。对于首页来说，是相当有意义的。我没有测试 detail 和 list 页，如果连带到这些页面的话，效果应该就更为明显。

之前就说过，这不是什么了不起的技术，只是想到了和想不到，仅此而已。因为我现在在携程打酱油，所以举得例子也和携程有关——今天(2011/10/17)我对携程的首页的PNG图片也全部都审查了一遍，结果是中文站首页的PNG几乎全部都压过，但却仍然有**漏网之鱼**——这也就是为什么说，人们容易忽视它，而不是不了解它。

<table>
	<tr>
		<th>文件</th>
		<th>原始文件大小(bytes)</th>
		<th>压缩后(bytes)</th>
		<th>体积减小(bytes)</th>
		<th>减小百分比</th>
	</tr>
	<tr>
		<td>phone_list.png</td>
		<td>22227</td>
		<td>15172</td>
		<td>7055</td>
		<td><span style="color:#6c3">-32%</span></td>
	</tr>
</table>

这张 phone_list.png 是用于底部携程无线的背景的，更是达到了22K，且不论这张图的原始基础是否有良好的压缩，至少直接用PNGOUT对着它都能压掉7K。对于首页来说，7K仍然有意义。

不过对于携程英文网站的测试让我颇为沮丧，因为还同时发现了一些其他问题，反推到中文站首页，这些小问题也一并存在。这里我仅仅讨论PNG的问题，列出几张大的PNG压一压的话，对这种一定要用大量图片来实现的设计，效果确是立竿见影的。

<table>
	<tr>
		<th>文件</th>
		<th>原始文件大小(bytes)</th>
		<th>压缩后(bytes)</th>
		<th>体积减小(bytes)</th>
		<th>减小百分比</th>
	</tr>
	<tr>
		<td>bg_header110621.png</td>
		<td>61372</td>
		<td>53778</td>
		<td>7594</td>
		<td><span style="color:#6c3">-13%</span></td>
	</tr>
	<tr>
		<td>un_bg_findbox.png</td>
		<td>15191</td>
		<td>11556</td>
		<td>3635</td>
		<td><span style="color:#6c3">-24%</span></td>
	</tr>
	<tr>
		<td>un_form_elements.png</td>
		<td>6941</td>
		<td>4664</td>
		<td>2277</td>
		<td><span style="color:#6c3">-33%</span></td>
	</tr>
	<tr>
		<td>un_bg_home.png</td>
		<td>15171</td>
		<td>11495</td>
		<td>3676</td>
		<td><span style="color:#6c3">-25%</span></td>
	</tr>
	<tr>
		<td>un_btns.png</td>
		<td>16301</td>
		<td>12332</td>
		<td>3969</td>
		<td><span style="color:#6c3">-25%</span></td>
	</tr>
	<tr>
		<td>btn_book.png</td>
		<td>8497</td>
		<td>7873</td>
		<td>624</td>
		<td><span style="color:#6c3">-8%</span></td>
	</tr>
	<tr>
		<td>bg_book.png</td>
		<td>7215</td>
		<td>6522</td>
		<td>693</td>
		<td><span style="color:#6c3">-10%</span></td>
	</tr>
	<tr>
		<td>un_bg_ad.png</td>
		<td>4990</td>
		<td>2355</td>
		<td>2635</td>
		<td><span style="color:#6c3">-53%</span></td>
	</tr>
</table>

未列出全部，但已经压掉了25K，很可观了，特别是对于主页来说。

最后推荐阅读PNG优化里有名的 [A guide to PNG optimization](http://optipng.sourceforge.net/pngtech/optipng.html)。

虽然我也曾是个蹩脚的设计师，不过这恐怕是我成为一个前端以来，第一次如此关注图片而非代码。**有些曾经的东西，偶然的随意拾起来，虽然可能不足挂齿，却可能真的是个宝**——我拾起的不是PNG压缩，而是对过往努力的怀念，**是一个时期的证明**。