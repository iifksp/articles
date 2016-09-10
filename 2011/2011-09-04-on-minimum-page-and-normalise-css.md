# 浅谈Minimum page & Normalise.css

阅读本文，请先阅读 [CSS项目Minimum Page简单介绍](http://www.zhangxinxu.com/wordpress/2011/09/css%E9%A1%B9%E7%9B%AEminimum-page%E7%AE%80%E5%8D%95%E4%BB%8B%E7%BB%8D/)。本文不是在介绍Minimum Page项目，而是完全是我对 Minimum Page 以及 Normalise.css 的一些个人看法。

关于CSS Reset的话题素来以已久，我的观点Reset对于个人是不必要的，但对于团队来说是重要的一环，因为需要避开个体成员的编写差异。Reset没有对错，只有适合与不适合之说。关于Reset有什么不明白的，可以阅读Maxdesign的 [CSS Reset - a simpler option。](http://www.maxdesign.com.au/articles/css-reset/)
你可以在 [CSS:resetr](http://cssresetr.com/) 看到各种流行的CSS Reset，以及他们的效果。这是个不错的工具，如果你正在寻找一个合适的Reset，那里会有直观的选择。下面就讨论下minimum page 以及 Normalise.css。

Normalise.css 以及 minimum page 都是为了避免浏览器使用传统 CSS Reset 的重复复写。仔细比较两者，Normalise.css 更为偏向传统的 CSS Reset，而 minimum page 其实已经掺入了很多人为的"合理"的样式，从而干预了相当一部分的浏览器默认样式，也正因为如此，在项目里 Normalise.css 可能会更受欢迎，因为它偏向开发人员已经熟知的传统的 CSS Reset，更为纯粹也容易被团队掌握。不过，两者都是参考意义远大于实用性，直接投入开发都不合适。

我相信很少有团队会放弃自己已经长时间使用、成员都已经颇为熟悉的Reset，诸如meyer CSS Reset或者是 YUI CSS Reset 之类，而对于个人而言，Reset又显得有些不够轻便，或许这类Reset的使用更适合新兴团队。这也是为什么无论是 Normalise.css 还是 minimum page，都不提供最小版本供开发直接使用，按需取用才是使用它们的适当的原则。

但是它们都无疑极具参考价值，与不同浏览器一棒子打死--"all to zero"的做法不同，仅仅对不同样式的部分单独统一需要更为精细的调整，这也使得它们在浏览器表现的不同上都造诣深厚。比如minimum page project里的 UA-CSS 就非常具有参考价值。我们通过学习它们可以更多的了解浏览器本身，而不需我们自己搜集整理各个浏览器的不同。

张博的文章写的相当赞，我在他那里学到了不少东西。这次的文章里有些翻译不是很精确。比如"Normalise.css takes a stricter approach and avoids much in the way of styling"译为"normalise.css采取更严格的方法，避免过多样式"，意思有偏差，因为它避免的不是样式的多少，而是浏览器不同样式的方式的多寡，所以译为"Normalise.css采取了更严格的态度从而避免了大量浏览器差异"更好些。

换言之，minimum page 更为宽容，它不会执着于所有浏览器表现完全一致，它的 all to zero 程度比起 Normalise.css 更浅，如果要我形容 minimum page 的话，我觉得它：非常"合理"。