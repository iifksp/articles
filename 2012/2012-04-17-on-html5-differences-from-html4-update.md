# HTML5相对于HTML4的差异更新浅析

去年我翻译了 [HTML5 differences from HTML4](http://dev.w3.org/html5/html4-differences/)，时隔约快一年了，这份工作草案再次更新。而我也开始着手更新自己的翻译稿。

这次的更新量相当大，所以翻译完整更新以及修订需要一些时间，但我会尽力最快地拿出更新稿。今天乘空的时候稍稍对比了一下两者，如果你读过去年5月25日版本，无论是[英文原文](http://www.w3.org/TR/2011/WD-html5-diff-20110525/)，或者是[我的翻译稿](http://www.swordair.com/docs/html5-differences-from-html4/)，那么你就可以从下面了解一些当前的最显而易见的改动。

## 新元素

WHATWG增加了一个 `data` 元素，用来把内容标注成机器可以读取的值。举个最简单易懂的例子：

```
I have <data value="8">eight</data> apples.
```

一目了然，data只是为机读服务的。这里 `data` 元素的 `value` 属性是必须出现的，其值代表了 `data` 元素中的内容的机读格式。

## 新属性

`object` 元素多了一个新属性 `typemustmatch`，使得嵌入外部资源更加安全。

`img` 元素多了一个新属性 `crossorigin`，用以在获取过程中使用CORS(Cross-Origin Resource Sharing)，并在成功后允许图像数据被 `canvas` API 读取。

增加了全局属性 `translate`，用来给翻译器给出翻译提示(哪些需要被翻译，哪些则必须保持原样)

## 元素变动

当用户代理不支持由 `script` 元素调用的脚本语言时，`noscript` 元素不再被渲染。

`script` 元素现在可以用于脚本或自定义数据块。

## 大量的其他更新

- 大量的属性变动
- 增加了一整节内容：Content Model Changes
- API章节的大量修订


这部分非常庞大，基本上需要重译。我暂时还没有足够的空闲时间完整看完。但一旦我完成了这部分内容，也会第一时间放出更新稿。谢谢大家的支持:)