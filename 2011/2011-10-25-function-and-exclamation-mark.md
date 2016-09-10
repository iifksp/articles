# function与感叹号

最近有空可以让我静下心来看看各种代码，function与感叹号的频繁出现，让我回想起2个月前我回杭州最后参加团队会议的时候，[@西子剑影](http://weibo.com/exqy)抛出的一样的问题：**如果在function之前加上感叹号 (!) 会怎么样**？比如下面的代码：
```
!function(){alert('iifksp')}()        // true
```
在控制台运行后得到的值时true，为什么是true这很容易理解，因为这个匿名函数没有返回值，默认返回的就是undefined，求反的结果很自然的就是true。所以问题并不在于结果值，而是在于，为什么求反操作能够让一个匿名函数的自调变的合法？

平时我们可能对添加括号来调用匿名函数的方式更为习惯：
```
(function(){alert('iifksp')})()        // true
```
或者：
```
(function(){alert('iifksp')}())        // true
```
虽然上述两者括号的位置不同，不过效果完全一样。

那么，是什么好处使得为数不少的人对这种叹号的方式情有独钟？如果只是为了节约一个字符未免太没有必要了，这样算来即使一个100K的库恐怕也节省不了多少空间。既然不是空间，那么就是说也许还有时间上的考量，事实很难说清，文章的最后有提到性能。

回到核心问题，为什么能这么做？甚至更为核心的问题是，为什么必须这么做？

其实无论是括号，还是感叹号，让整个语句合法做的事情只有一件，就是**让一个函数声明语句变成了一个表达式**。
```
function a(){alert('iifksp')}        // undefined
```
这是一个函数声明，如果在这么一个声明后直接加上括号调用，解析器自然不会理解而报错：
```
function a(){alert('iifksp')}()        // SyntaxError: unexpected_token
```
因为这样的代码混淆了函数声明和函数调用，以这种方式声明的函数 `a`，就应该以 `a();` 的方式调用。

但是括号则不同，它将一个函数声明转化成了一个表达式，解析器不再以函数声明的方式处理函数a，而是作为一个函数表达式处理，也因此只有在程序执行到函数a时它才能被访问。

所以，**任何消除函数声明和函数表达式间歧义的方法，都可以被解析器正确识别**。比如：
```
var i = function(){return 10}();        // undefined
1 && function(){return true}();        // true
1, function(){alert('iifksp')}();        // undefined
```
赋值，逻辑，甚至是逗号，各种操作符都可以告诉解析器，这个不是函数声明，它是个函数表达式。并且，对函数一元运算可以算的上是消除歧义最快的方式，感叹号只是其中之一，如果不在乎返回值，这些**一元运算都是有效的**：
```
!function(){alert('iifksp')}()        // true
+function(){alert('iifksp')}()        // NaN
-function(){alert('iifksp')}()        // NaN
~function(){alert('iifksp')}()        // -1
```
甚至下面这些关键字，都能很好的工作：
```
void function(){alert('iifksp')}()        // undefined
new function(){alert('iifksp')}()        // Object
delete function(){alert('iifksp')}()        // true
```
最后，括号做的事情也是一样的，消除歧义才是它真正的工作，而不是把函数作为一个整体，所以无论括号括在声明上还是把整个函数都括在里面，都是合法的：
```
(function(){alert('iifksp')})()        // undefined
(function(){alert('iifksp')}())        // undefined
```
说了这么多，实则在说的一些都是最为基础的概念——语句，表达式，表达式语句，这些概念如同指针与指针变量一样容易产生混淆。虽然这种混淆对编程无表征影响，但却是一块绊脚石随时可能因为它而头破血流。

最后讨论下性能。我在jsperf上简单建立了一个测试：http://jsperf.com/js-funcion-expression-speed ，可以用不同浏览器访问，运行测试查看结果。我也同时将结果罗列如下表所示（由于我比较穷，测试配置有点丢人不过那也没办法：奔腾双核1.4G，2G内存，win7企业版）：
<table>
	<tr>
		<th rowspan="2">Option</th>
		<th rowspan="2">Code</th>
		<th colspan="4">Ops/sec</th>
	</tr>
	<tr>
		<th>Chrome 13</th>
		<th>Firefox 6</th>
		<th>IE9</th>
		<th>Safari 5</th>
	</tr>
	<tr>
		<th>!</th>
		<td>!function(){;}()</td>
		<td style="text-align:right;">3,773,196</td>
		<td style="text-align:right;">10,975,198</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">2,810,197</td>
	</tr>
	<tr>
		<th>+</th>
		<td>+function(){;}()</td>
		<td style="text-align:right;background:#9cee82;">21,553,847</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">1,812,238</td>
	</tr>
	<tr>
		<th>-</th>
		<td>-function(){;}()</td>
		<td style="text-align:right;background:#9cee82;">21,553,847</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">1,864,155</td>
	</tr>
	<tr>
		<th>~</th>
		<td>~function(){;}()</td>
		<td style="text-align:right;">3,551,136</td>
		<td style="text-align:right;">3,651,652</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">1,876,002</td>
	</tr>
	<tr>
		<th>(1)</th>
		<td>(function(){;})()</td>
		<td style="text-align:right;">3,914,953</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;background:#9cee82;">3,025,608</td>
	</tr>
	<tr>
		<th>(2)</th>
		<td>(function(){;}())</td>
		<td style="text-align:right;">4,075,201</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;background:#9cee82;">3,025,608</td>
	</tr>
	<tr>
		<th>void</th>
		<td>void function(){;}()</td>
		<td style="text-align:right;">4,030,756</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;background:#9cee82;">3,025,608</td>
	</tr>
	<tr>
		<th>new</th>
		<td>new function(){;}()</td>
		<td style="text-align:right;background:#ffc0cb;">619,606</td>
		<td style="text-align:right;background:#ffc0cb;">299,100</td>
		<td style="text-align:right;background:#ffc0cb;">407,104</td>
		<td style="text-align:right;background:#ffc0cb;">816,903</td>
	</tr>
	<tr>
		<th>delete</th>
		<td>delete function(){;}()</td>
		<td style="text-align:right;">4,816,225</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">2,693,524</td>
	</tr>
	<tr>
		<th>=</th>
		<td>var i = function(){;}()</td>
		<td style="text-align:right;">4,984,774</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;">565,982</td>
		<td style="text-align:right;">2,602,630</td>
	</tr>
	<tr>
		<th>&amp;&amp;</th>
		<td>1 &amp;&amp; function(){;}()</td>
		<td style="text-align:right;">5,307,200</td>
		<td style="text-align:right;">4,393,486</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">2,565,645</td>
	</tr>
	<tr>
		<th>||</th>
		<td>0 || function(){;}()</td>
		<td style="text-align:right;">5,000,000</td>
		<td style="text-align:right;">4,406,035</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">2,490,128</td>
	</tr>
	<tr>
		<th>&amp;</th>
		<td>1 &amp; function(){;}()</td>
		<td style="text-align:right;">4,918,209</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">1,705,551</td>
	</tr>
	<tr>
		<th>|</th>
		<td>1 | function(){;}()</td>
		<td style="text-align:right;">4,859,802</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">1,612,372</td>
	</tr>
	<tr>
		<th>^</th>
		<td>1 ^ function(){;}()</td>
		<td style="text-align:right;">4,654,916</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">1,579,778</td>
	</tr>
	<tr>
		<th>,</th>
		<td>1, function(){;}()</td>
		<td style="text-align:right;">4,878,193</td>
		<td style="text-align:right;background:#9cee82;">12,135,960</td>
		<td style="text-align:right;background:#9cee82;">572,694</td>
		<td style="text-align:right;">2,281,186</td>
	</tr>
</table>

可见不同的方式产生的结果并不相同，而且，差别很大，因浏览器而异。

但我们还是可以从中找出很多共性：**new方法永远最慢**——这也是理所当然的。其它方面很多差距其实不大，但有一点可以肯定的是，感叹号并非最为理想的选择。反观**传统的括号，在测试里表现始终很快**，在大多数情况下比感叹号更快——所以平时我们常用的方式毫无问题，甚至可以说是最优的。**加减号在chrome表现惊人**，而且在其他浏览器下也普遍很快，相比感叹号效果更好。

当然这只是个简单测试，不能说明问题。但有些结论是有意义的：括号和加减号最优。

但是为什么这么多开发者钟情于感叹号？我觉得这只是一个习惯问题，它们之间的优劣完全可以忽略。一旦习惯了一种代码风格，那么这种约定会使得程序从混乱变得可读。如果习惯了感叹号，我不得不承认，它比括号有更好的可读性。我不用在阅读时留意括号的匹配，也不用在编写时粗心遗忘——

当我也这么干然后嚷嚷着这居然又节省了一个字符而沾沾自喜的时候，却忘了自己仓皇翻出一本卷边的C语言教科书的窘迫和荒唐......任何人都有忘记的时候，当再捡起来的时候，捡起的就已经不单单是忘掉的东西了。

2011-10-31更新：如果你使用aptana，那么在使用（!+-）时要注意一点，它们会让aptana的解析失效，导致Outline窗口没有任何显示。但是就代码本身而言，其运行没有任何问题。