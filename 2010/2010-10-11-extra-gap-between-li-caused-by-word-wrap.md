# word-wrap导致的li额外间隙

有些问题可能很少有机会遇到，前段时间我就在工作中遇到一个由`word-wrap`引起的兼容问题。因为当 `word-wrap:break-word` 这种 rule被加到了全局CSS里后，出现问题就显得非常隐蔽，所以排查了不少时间才找到原因。不过这也不算大问题，因为造成这种情况是多种因素的巧合。如下图：

![word wrap li img gap](https://swordair.com/content/images/2013/Dec/word_wrap_li_img_gap.gif)

造成这种莫名其妙间隙的原因就是对li应用了`word-wrap`的结果。但是单单应用`word-wrap`还不足以使IE6和IE7出现反常，另一个必要条件就是没有对li指定宽度。如果每个单元都是由图像的高宽撑大的而没有设置宽度，这样再加上`word-wrap`就出现了上面那张图中的情况。

既然反复测试中知道了原因那么解决方法也就显而易见了。解决方法有3个：

1. 对li应用`word-wrap:normal;`
2. 限定li元素的高宽;
3. 对图片的万精油`display:block;`

随后闲话说说word-wrap，一个微软的私有属性，但现在也已经被CSS3标准吸纳。虽然这是一个非常实用的属性，但是在某些特定情况下，对旧浏览器仍然还是会造成一些影响的。