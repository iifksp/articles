# CSS选择器距离无关

“CSS选择器距离无关”的说法有点挫，但个人觉得，其实在描述上还是挺准确的。Eric Meyer 在最近写了一篇文章 [Negative Proximity](http://meyerweb.com/eric/thoughts/2012/03/07/negative-proximity/)，讲述了一个很容易被人忽略的选择器细节：**CSS选择器没有元素距离远近的概念**。

简单地说，下面这样的CSS片段：

```
body h1 {color: red;}
html h1 {color: green;}
```

虽然相比 `<html>`，`<h1>` 更接近 `<body>`，但是并没有对其规则的特殊性产生影响。两条规则的优先级是一样的，所以后定义者胜出。其他具体内容可以查看 Eric 的原文。

由于这种情况既不常见也不重要，所以不会在日常工作里困惑我们。我在阅读的过程中顺便翻译了一下 [Calculating a selector’s specificity](http://www.w3.org/TR/css3-selectors/#specificity)：

### 9. 计算选择器的特殊性

选择器的特殊性如下方式计算：


1. 计数ID选择器的数量(= a)
2. 计数类名选择器、属性选择器选择器，以及伪类的数量(= b)
3. 计数类型选择器和伪元素的数量(= c)
4. 忽略通配符选择器


在否定伪类里的选择器正常计数，但是否定自身不计数为伪类。

(在一个大基数的数字系统里)将三个数字 a-b-c 串联后给出特殊性。

**示例：**

```
*               /* a=0 b=0 c=0 -> specificity =   0 */
LI              /* a=0 b=0 c=1 -> specificity =   1 */
UL LI           /* a=0 b=0 c=2 -> specificity =   2 */
UL OL+LI        /* a=0 b=0 c=3 -> specificity =   3 */
H1 + *[REL=up]  /* a=0 b=1 c=1 -> specificity =  11 */
UL OL LI.red    /* a=0 b=1 c=3 -> specificity =  13 */
LI.red.level    /* a=0 b=2 c=1 -> specificity =  21 */
#x34y           /* a=1 b=0 c=0 -> specificity = 100 */
#s12:not(FOO)   /* a=1 b=0 c=1 -> specificity = 101 */
```

注释1：允许重复出现相同的简单选择器，并可以增加特殊性。

注释2：定义在HTML style 属性里的样式特殊性遵循CSS2.1里的描述。

### 译后语

如果我没有记错，CSS3选择器是最早达成推荐标准的CSS3部分之一，仅次于CSS3颜色组件，甚至CSS4选择器部分都已经差不多了。尽管现有浏览器的支持上广泛度上还很成问题，但相对完善的选择器标准，值得一看。

