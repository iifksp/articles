# JavaScript字符串连接性能

起初今天碰到的字符串拼接的性能问题并不是关于JavaScript的，而是有关PHP的，名为[PHP中的高性能字符串连接](http://blogs.sitepoint.com/2010/10/05/high-performance-string-concatenation-in-php/)( High-Performance String Concatenation in PHP )。众所周知，PHP是用“点”来连接字符串的：
```
$str = 'a' . 'b';
$str .= 'c';
```
并且和JavaScript类似，PHP也可以通过Array来拼接字符串：
```
$str = implode(array('a', 'b', 'c'));
```
问题就是，当拼接达到上万的数量级之后，哪一种性能更好呢？
```
// standard string append
$str = '';
for ($i = 30000; $i > 0; $i--) {
	$str .= 'String concatenation. ';
}
```
```
// array join
$str = '';
$sArr = array();
for ($i = 30000; $i > 0; $i--) {
	$sArr[] = 'String concatenation. ';
}
$str = implode($sArr);
```
如果看过 Zakas 的《JavaScript高级程序设计》的话，一定会记得那个在第85页关于字符串连接性能的问题。

其实和其他很多语言一样，JS的字符串是不可变的。每次在字符串上的改变，都会存在一个废弃旧的实例并重新创建一个新实例的过程。单单两个字符串的简单拼接，就涉及好几个这样的过程。一旦这种冗余的过程被重复上万的数量级，就会遇到性能问题。然而数组则不会，它只是在一开始不断扩展数组的大小，并在最后只创建拼接结果的字符串一次，从而省去了上万实例化的开销。并且按照Zakas给出的方法测试的结果，可以节省50%以上的时间。

但上面PHP的执行结果却并非如此。虽然我没有自己测试，但援引 Craig Buckler 的表述，结果就很清楚了。

> Unsurprisingly, PHP is optimized for string handling and the dot operator will be the fastest concatenation method in most cases.

甚至，这可能不仅仅只是一个性能上的问题，如果数组开的太大面临的是内存的压力。难道 Zakas 错了？当然不可能，且不说把JS的理论直接放到PHP上是否合适，但就当时IE6时代 Zakas 完全正确，可惜放到现在，可能就略显过时了。

下面的是一段 Zakas 的测试使用加号和使用数组的join方法性能的代码：
```
function StringBuffer() {
	this.__strings__ = new Array;
}

StringBuffer.prototype.append = function (str) {
	this.__strings__.push(str);
	//this.__strings__[this.__strings__.length] = str;
};

StringBuffer.prototype.toString = function () {
	return this.__strings__.join("");
};

var d1 = new Date();
var str = "";
for (var i=0; i < 10000; i++) {
	str += "I am the bone of my sword ...";
}
var d2 = new Date();
 
document.write("Concatenation with plus: " + (d2.getTime() - d1.getTime()) + " milliseconds");
 
var buffer = new StringBuffer();
d1 = new Date();
for (var i=0; i < 10000; i++) {
	buffer.append("I am the bone of my sword ...");
}
var result = buffer.toString();
d2 = new Date();
 
document.write("<br />Concatenation with StringBuffer: " + (d2.getTime() - d1.getTime()) + " milliseconds");

```
在拼接的字符串那里，我使用的是下面这段话：

> I am the bone of my sword. Steel is my body and the fire is my blood. I have created over a thousand blades. Unknow to death, nor know to life. Have withstood plain to create many weapons. Yet those hands will never hold anything. So as I play, unlimited blade works.

顺便扯点别的，这周我刚看完 fate / stay night UBW，那情节真够破碎的，真是大感失望。可怜的 Archer 连个咒语都没时间念全。这里就让这段话出个场吧~

现在如果你点击[这里](http://www.swordair.com/demos/string-concatenation-plus-vs-stringbuffer/)，可以看到我写好的Demo在各个浏览器的运行效果。如果不想亲自尝试，请看我列出的表格。

<table>
	<tr><th>浏览器</th><th>加号连接</th><th>Join连接</th></tr>
	<tr><td>Chrome 6.0.479.0</td><td>1毫秒</td><td>5毫秒</td></tr>
	<tr><td>Opera 10.62</td><td>18毫秒</td><td>19毫秒</td></tr>
	<tr><td>Firefox 3.6.8</td><td>26毫秒</td><td>31毫秒</td></tr>
	<tr><td>Safari 5.33.18.5</td><td>2毫秒</td><td>34毫秒</td></tr>
	<tr><td>IE6</td><td>38437毫秒</td><td>94毫秒</td></tr>
</table>

这张表是在一台AMD X2 4000+ 2G内存的台式机上测得的。你没看错IE的数值，当然我也没笔误。由于我的机器上只有IE6，所以IE7和IE8以及新的FF4、IE9beta都尚未测试，但是凭感觉，IE7的情况应该和IE6类似，IE8的情况应该和FF3.6.8类似。所以 Zakas 那段优化代码几乎完全没用武之地，刚才PHP的情况几乎是直接复制到JS身上。要说的是，Chrome和Safari，特别是Chrome，真的很恐怖，这么多的操作居然1毫秒都几乎不用。

难道是浏览器对于加法拼接做了什么优化？也许吧。但是不管怎么说，String总归是一个不可变的数据，在不改变处理方式的情况下，如何能将性能提升到这种地步？我的猜测是现代JS引擎对于对象废弃和创建已经非常快速，快到这种过程在上万数量级完全感觉不到瓶颈一般。Zakas 在写书的时候还没有Chrome，浏览器对于某些执行性能的颠覆，完全出人意料。

当然还有些有趣的其他情况，当测试数据，这里指的是待拼接的字符串的长度变化后，也会得到写不同的结果。比如，将上面那段字符串加长一倍，Chrome 和 Safari 的加法连接的表现仍旧抢眼。Opera 则是始终拥有非常相近的数据结果。FF在字符串加长后，加法连接性能会有明显的下降.....等等。

这次也许能有一个结论，那就是，**尽管无论出于代码的可读性还是执行性能考虑，我们都应该使用加号拼接字符串，然而，使用数组对于IE6、7的提升是如此的巨大以至于最为保守折中的做法仍然是使用数组**，因为相对于IE6、7的提升，其他浏览器用加号的做法带来的提升甚至可以忽略不计。

