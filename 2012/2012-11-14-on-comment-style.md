# 闲话注释风格

《一个码农的自我修养》一书花了相当多的篇幅来阐述代码注释的重要性，可见注释真的很重要！好吧，其实没这本书～

不过，应该没人能质疑注释的重要性，因为没多少人愿意接手和维护一堆纯粹的逻辑符号，虽然个人项目另当别论，但不是每个人的记忆力都能突破天际地记得“原来我曾经写过这样的代码”。所以，我个人对那些写了一堆代码没一句注释的伪牛人是嗤之以鼻的。但写这篇文章的自己可能也是相当的小题大做，其实注释只要是简明清楚，都应该来者不拒，而没必要太注重形式。一个团队还是保持某一种统一的风格比较好，在大框架下的求同存异，每个人也都应该保留适合自己的空间。

本文旨在罗列一些笔者遇到的注释，仅仅只是闲谈。每个人都有适合自己的风格，无所谓好坏。

无论是C风格的注释，还是后来C++增加的单行注释，我们都再熟悉不过了：
```
/* Basic Comment */
// Basic Comment
```

但个人而言，单行注释我更多地是用在调试上，我几乎不用单行注释来写注释。单行注释常常被用在画框上，但个人感觉太粗，影响视觉。而且真正的注释和调试代码最好在视觉上分开，所以我几乎不用，以至于到后来看到自己顺手写的单行注释会非常别扭，出于一种强迫下重新改回C语言的那种风格。

```
///////////////////////////////////////
//
//
// This is a comment hat
//
//
///////////////////////////////////////
function swordair(){}

///////////////////////////////////////
// This is a comment hat
function swordair(){}
```

所以本文中的单行注释，只是一笔路过。更重要的原因是，CSS里无法使用——虽然很多人提出在CSS里支持单行注释，但直到现在也没有结果。原因很简单，因为CSS里经常出现 `http://` 这样的字符串，解释器需要做额外区分。而且即使浏览器支持了，当前的众多CSS格式化工具和压缩器也都需要改进算法，所以CSS的单行注释还是遥遥无期的事情。所以，即使从统一性角度看，似乎也应该选择最为古老的注释方式 `/**/`

除了最为常见的单行 `/× comment ×/` 以外，下面这种多行的块形注释也是比较常见，特别是在Java里。这种注释方式对于函数，类，以及文档都非常有效，因为它提供了足够多的信息量：

```
/*
 * Group comment block.
 * Ideal for multi-line explanations and documentation.
 */
```

另一种较为常见的注释类似于一个大标题，非常醒目，类似于HTML里 `h1` 的效果：

```
/*------------------------------------*\
   COMMENT CONTENTS
\*------------------------------------*/
function swordair(){}
```

当然，如果觉得这样的标题太大，也完全可以用小一些的标题，甚至组合大小标题达到层次的效果。常见的小标题格式可能是下面这样的：

```
/* common */
/* --------------------------------------------- */
function swordair(){}
/* --------------------------------------------- */
```

```
/* Generic Table
---------------------------------------------------------- */
function swordair(){}
```

也有人喜欢用等号代替减号，这样的标题视觉上更加突出。而且长度上的不同也可以用作层次的划分。

```
/* ==========================================================================
   Section comment block
   ========================================================================== */

/* Sub-section comment block
   ========================================================================== */
```


大部分的注释就是由 **`/**/`** 配合**星号`*`** / **减号`-`** / **等号`=`**组合起来划分区域的。因为这些字符足够简单，能制造边框效应。只要你愿意，完全可以在这些条条框框里留下任何你喜欢的信息：

```
/*SwordAir*/
/***********************************************************
 *                                                         *
 *    ***                          *     *                 *
 *   *                             *    * *    *           *
 *    *                            *   *   *               *
 *     *  *   *   *  ***  ** *** ***   *   *   *  ** ***   *
 *      *  *  **  * *   *  **   *  *  *******  *   **      *
 *       *  **  **  *   *  *    *  *  *     *  *   *       *
 *   ****   *   *    ***   *     ** * *     *  *   *       *
 *                                                         *
 ***********************************************************/
```

嘛～献丑，其实我知道自己的代码画非常难看～希望有一天能稍微拿的出手一些:)

实际上，还有更多字符可以参与到注释中来，而且它们的效果显而易见。在没有任何说明的情况下，任何人都能一看就明白：

```
/*↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓*/
function swordair(){}
/*↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑*/
```

混合文字和箭头的方式也算是挺常见的，而且往往效果很好。你能很直观的看到代码从哪里开始到哪里结束。

```
/* ↓↓↓ Function Start ↓↓↓ */
function swordair(){}
/* ↑↑↑ Function End ↑↑↑ */
```

不只是箭头，三角形也表示一种同样的意思：
```
/* ▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼▼ */
function swordair(){}
/* ▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲ */
```

其他字符也一样。当然箭头和三角都是比较常见的，之所以常见，也是有原因的。其一是因为直观，其二，其他的一些字符可能在编辑器里**乱码**，这样反而让代码混乱。如果是用win7或者如aptana，遇到乱码的情况要少很多。但在winXP下，notepad++在等宽字体的配置下也只能很好地显示几种，箭头和三角都是其中之一。

不过，在网页上，它们的显示完全不成问题。它们可以说是代码注释里的非主流。

```
/* ☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰☰ */

/* ✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓✓ */

/* ✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗✗ */
```
好吧,最后这几个，连非主流都算不上，其实完全没人用～ Just For Fun!