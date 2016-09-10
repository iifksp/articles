# 开发过程中的调试样式

甚少有人论起这种主题，因为使用的人真的不多。这些开发过程中的用于调试的CSS虽然可能很管用而且很高效，但毕竟不是必须的——实际上，即使是我自己都未曾在自己的开发过程中使用过。但我并不是不信这些调试CSS的作用，它们能很快的通过视觉层面指出问题，但显然它更加适合标准流程化的开发进度而不是像现在的大多数开发那样的随意。

正因为用的少，所以知道的也不多。看下面的样式就明白是怎么回事了：
```
/**
 * Catching unsized images during development
 * Any images without width and height attributes will be drawn with a red border
 * IE9+, FF, Chrome, Opera
 */
img:not([width]):not([height]) {
	border: 2px solid red !important;
}

/**
 * Catch links/ahref without a URL or with null src values
 */
a[href="#"],
a[href=""]{
	background:#ff0 !important;
}
```
很简单，就是用选择器定位到那些开发中的疏漏，然后给出视觉上的不同。这样的调试样式可以单独放在一个CSS文件里然后再开发过程中引用，在部署时移除。

之所以说不必要，是因为在随意的开发里这样的代码显得有些多余。但这些细小的技巧，确是无数经验或一时灵感的产物。比如第一个样式出自著名的 37signals，原文 [CSS tip: Spot unsized images during development](http://37signals.com/svn/posts/2979-css-tip-spot-unsized-images-during-development)。我相信普通的CSSer都能写出这类样式，但思路受限恐怕不多人会去这么想到——至少我就觉得日复一日的工作里自己不会凝结出这样的Trick。

所以这种技术重要的并不在于技术本身，因为其并没有什么特别之处，而是在于其中蕴含的思路——开发，流程，如何更快更准确的定位问题，哪怕是细微的问题，如果不能再直接的代码层面保证开发的精度，那么间接用代码辅助视觉又会如何？不失大体，注重细节。真心觉得，很多牛人之所以牛，并不在乎其技术层级，而在乎于其进益求精。