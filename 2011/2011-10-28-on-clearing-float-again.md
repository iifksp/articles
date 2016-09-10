# 再谈清除浮动

清除浮动对于任何CSSer来说都是基本中的基本，但我不愿称其为基础，因为最近我浏览网页看到各种充斥的清除浮动的方法后，才彻底明白，虽然各种方法被大量的使用，却甚为混乱。最糟糕的是很多有问题的流传版本的搜索排名都非常靠前，用神大人的话说就是：“错误的知识是毫无意义的”。

然而，坊间流传的很多方法其实也称不上错误，而只是有些偏差而已——往往使用中不会出现问题，但是严格地说，它们是不准确的，难道我们搞技术的不应该严谨一点么？于是，怀着一种略显复杂的心情，试图用我所知道的来“再谈”清除浮动。虽然不知道能有多少人看到这篇文章，但偶尔我还是想吐槽的——“那样做，不妥。”

鉴于现在google的访问在我镇实在够呛，所以我只好用百度为例。如果用百度搜索“清除浮动”，在结果里可以看到五花八门的解释。百度文库甚至还剽窃了一些略有错误的文章并对不准确信息的散播做出了杰出的贡献。清除浮动不出这三种方式：


1. 空标签清除浮动
2. overflow
3. 伪元素after


这里略过第一种方式，对后两种方式的准确解释，我推荐蓝色理想yoom版主写的wiki页面：[清除浮动](http://wiki.blueidea.com/index.php?title=%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8)。然后我们来仔细看看网络上流传的**偏差**到底在哪里？

## 慎用overflow
虽然可能多数文章对overflow能清除浮动有介绍，但其中却甚少会描述overflow的弊端，使得现在网页上广泛存在下面这样的清除浮动的代码：
```
.clearfix{overflow:auto;zoom:1;}
/* or */
.clearfix{overflow:hidden;zoom:1;}
```
看似毫无问题，实则存在隐患，因为并不是所有的情况下都适用。1年多前，我曾在 [CSS定位机制之一：普通流](/css-positioning-schemes-normal-flow/) 一文中详细讨论过普通流以及相关概念：格式化上下文。**设置overflow为非visible值，将导致容器生成新的格式化上下文，其布局、相对于浮动的行为等会发生显著变化，清除浮动只不过这一系列变化的其中一个附带作用**。

出于这样的原因，我们能看到更多弊端的表现，不论是早先Firefox无端的产生focus、还是在某些情况下触发滚动条、截断绝对定位的层，等等等等，太多了。光是这年看到的类似讨论就已经足够让我对其敬而远之。

始终应该明白，如果单纯要清除浮动，何以要使用影响整个布局的overflow呢？我个人的观点是，如果单单要清除浮动，**overflow就应该果断不使用**，“慎用”只是一种委婉的说法，因为毕竟有时候overflow是必须的。

所以我认为任何介绍浮动的文章都必须提及overflow，以及，其废弃原因。

## after伪元素的注意点
使用伪元素清除浮动是推荐的做法，同时偏差也就出现了。首先，伪元素和伪类是两个完全不同的概念，大部分的人却将两者混淆。可以在很多文章里看到“after伪类”这样偏差的信息，虽然这只是一个小问题并不会影响到理解和使用，不过我真心不愿看到错误概念的散播。

然后就是讨论实际问题了。

使用after伪元素清除浮动的方法源自这篇 [How To Clear Floats Without Structural Markup](http://www.positioniseverything.net/easyclearing.html)，如果把代码全部拿来应该是这样的：
```
<style type="text/css">

  .clearfix:after {
    content: ".";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
    }

.clearfix {display: inline-block;}  /* for IE/Mac */

</style><!-- main stylesheet ends, CC with new stylesheet below... -->

<!--[if IE]>
<style type="text/css">
  .clearfix {
    zoom: 1;     /* triggers hasLayout */
    display: block;     /* resets display for IE/Win */
    }  /* Only IE can see inside the conditional comment
    and read this CSS rule. Don't ever use a normal HTML
    comment inside the CC or it will close prematurely. */
</style>
<![endif]-->
```
鉴于Mac版IE的稀少到可以忽略的占有量以及验证问题，代码可以简化到下面这样：
```
.clearfix:after{content:".";display:block;height:0;clear:both;visibility:hidden;}
.clearfix{*zoom:1;}
```

上面这段已经我们异常熟悉的代码了：after伪元素内容是一个点，本身用来清除浮动，其他代码则是为让这个伪元素不可见。所以很多其他的版本里，可能还会添加 font-size:0;line-height:0; 来进一步保证元素不可见。而 zoom:1; 则用于触发IE6/7的hasLayout。这个版本并不严谨，原因在于没有对IE6/7单独照顾而只是粗暴的放出一条  zoom:1 打发它们。但这些代码是准确的，在此基础之上，就出现了网上流传的偏差，我随意摘录一段：

> content属性是必须的，但其值可以为空，蓝色理想讨论该方法的时候content属性的值设为 "." ，但我发现为空亦是可以的。

**很遗憾，这段代码里的content为空是不妥当的。**原版代码之所以content有内容，是为了避免Firefox在使用 `content:""` 时造成的额外空隙。并且不论是空格还是空字符串，firefox直到现在7.0仍然表现有问题。所以content有内容是必须的。简单给出firefox额外空隙的demo代码如下：
```
<!DOCTYPE HTML>
<html lang="en-US">
<head>
	<meta charset="UTF-8">
	<title></title>
	<style type="text/css">
		
		.clearfix:after{content:"";display:block;font-size:0;line-height:0;height:0;clear:both;visibility:hidden;}
		
		body{background:#666;padding:0;}
		.outer{background:#eee;}
		.inner{float:left;width:200px;height:200px;margin:10px;background:#ccc;}
		p{margin:4em 0;}
	</style>
</head>
<body>
	<div class="clearfix outer">
		<div style="" class="inner"></div>
		<div style="" class="inner"></div>
	</div>

	<p>这个段落会造成Firefox出现额外空隙</p>
</body>
</html>
```
当然多数情况下这并不发生，这使得代码偏差并不被察觉。但特定情况下会造成问题。虽然后来我查阅了 [content](http://www.w3.org/TR/CSS2/generate.html#content) 和其值 [string](http://www.w3.org/TR/CSS2/syndata.html#strings) 的定义但是标准并无此例相关的详细描述。

然而，这段代码并非完全没有改进余地，虽然下面提到的改进方式并未被大量使用所以没有足够的证据证明它们的严谨，但至少我认为比简单移除content内容的做法靠谱的多。

### 改进版本一
Unicode字符里有一个“零宽度空格”，即 **[U+200B](http://www.fileformat.info/info/unicode/char/200b/index.htm)** ，使用这个字符替代content的内容“点”，可以缩减代码量——因为这个字符本身就是不可见的，所以不必再重复使用 `visibility:hidden;` 来把它藏起来。然后代码就变成了下面这样：
```
.clearfix:after{clear:both;content:"\200B";display:block;height:0;}
.clearfix{*zoom:1;}
```
看起来清爽了很多不是嘛？

### 改进版本二
[A new micro clearfix hack](http://nicolasgallagher.com/micro-clearfix-hack/) 一文提到了一种新的after伪元素清除浮动的方式，其将content的内容设为空，并将display的值改为table：
```
/* For modern browsers */
.clearfix:before,
.clearfix:after {
	content:"";
	display:table;
}

.clearfix:after {
	clear:both;
}

/* For IE 6/7 (trigger hasLayout) */
.clearfix{
	zoom:1;
}
```
注意，这段代码里还同时包括了顶部“防空白边折叠”的部分代码。这段代码工作的很好，在firefox里也并无之前的问题。

## 总结
虽然只是一个浮动清除，却也引出了这么多东西了。对于平常使用来说，我更倾向于after伪元素的旧版本代码，它应用广泛更可靠也被更好的证明总是正确的，尽管改进方案可能看起来明快得多。

最后是闲话，CSS3选择器组件以及CSS3命名空间组件“昨天”正式成为W3C官方推荐标准，从而成为继CSS3颜色组件之后，第二和第三个CSS3推荐标准组件。可喜可贺。之所以写是“昨天”，是因为这篇文章我写了两天......

欢迎各种评论，当然因为我的个人水平有限，所到之处想必也有不妥之处，欢迎指正。