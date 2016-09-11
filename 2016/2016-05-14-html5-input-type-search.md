# HTML5 input 搜索框

众所周知，HTML5为input定义了更多的输入框类型，比如date，number等。input type="search"也是其中之一，但其定义里并没有过多描述这个类型的功能，反而是同type=text在外观和功能上都没有多大区别，那么要其何用？当前，各浏览器对新特性的实现都还是部分支持，即便全部支持，差异也还是免不了的。这里仅以Chrome，Firefox以及Edge为参考。

用惯Edge的人可能对Edge默认为input text添加清空按钮(输入框有内容时尾部会出现“x”，点击后会清空输入框内容)并不陌生，其添加的清空记号同样适用于input search。换言之search和text两种类型确无二致。

Firefox无论是text还是search，都没有做什么可见的变化。

最后是Chrome，其对于text与search的主要区别是，Chrome单独为input search提供了清空按钮。不止如此，输入内容后，按ESC键后Chrome也会自动清空内容。对于重度用户来说这些功能都挺贴心。

所以说，text和search确无特别明显的差异，二者的差异更多只是浏览器差异。搜索框使用search显然更为语义化，虽然当前未有明显差异不过长远来看是合理的。

另一个细节是，在移动端比如iOS，虚拟键盘会根据type的值给出不同的反应。如果是type search，会看到键盘右下角是“搜索”而不是一般的“前往”。

列举一些主流搜索引擎的input输入框代码：

```
<!-- google: -->
<input maxlength="2048" name="q" autocomplete="off" title="Search" type="text" value="" aria-label="Search" aria-haspopup="false" role="combobox" aria-autocomplete="both" dir="ltr" spellcheck="false">

<!-- bing: -->
<input name="q" title="Enter your search term" type="search" value="" maxlength="100" autocapitalize="off" autocorrect="off" autocomplete="off" spellcheck="false" role="combobox" aria-autocomplete="both" aria-expanded="false" aria-owns="sa_ul">

<!-- baidu: -->
<input type="text" name="wd" maxlength="100" autocomplete="off">

<!-- sou: -->
<input type="text" name="q" suggestwidth="528px" autocomplete="off">

<!-- sogou: -->
<input type="text" name="query" size="47" maxlength="100" autocomplete="off">
```

从中可以看得到，使用input search只有Bing而已。当然，bing和google比起国内浏览器代码明显要长一些，可见其在屏幕阅读器这样的细节上明显要做的更好。

虽然外观无影响，但是对交互是有影响的。最近收到一些交互稿，设计师对浏览器清空按钮显然不是很了解，往往在输入框重复制造这种清空按钮和交互行为，在多浏览器不同表现的当前，毫无必要。