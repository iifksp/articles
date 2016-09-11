# 使Chrome字体渲染更平滑

**更新 2014/9/5**：没想到今天无意中看到google更新到了chrome37，chrome字体渲染问题居然已经被解决了！坑啊！早知道不码那么多字了...

--------------

Chrome在文字渲染上始终存在一些不大不小的问题，如早期我在[Chrome中的text-shadow](http://swordair.com/text-shadow-bug-in-chrome/) 一文中提到的字体阴影抗锯齿bug，虽然影响不大，却总是不如人意。

最近让我觉得不爽的依旧是Chrome(似乎只涉及windows版本)的字体渲染的问题，Chrome在小字体显示的时候无法准确使字体平滑，从而在字体不大不小的时候，可以清楚地看到文字的锯齿。而当使用字体图标时，这种感觉就变得更为明显。

稍稍尝试就会发现，值**48px**是一个分界点。即在字体大于48px的时候并不会出现锯齿，这应该与Chrome渲染字体时使用的算法不同有关。而如果字体很小(<20px)，也会因为线迹太细而不够明显。当字体48px时，锯齿达到最大化。

![](https://swordair.com/content/images/2014/Aug/diff-font-smooth-rander-in-chrome-36.png)

这张截图只有在网页点对点显示时，可以清楚地展示出锯齿。这种前后的差异应该是渲染算法变化导致，但实则我们对算法本身如何并不关心，而是关心如何解决(方法只适用于在使用网络字体的情况下)。

通常对于webkit(blink)，字体的渲染主要通过下面的CSS控制：
```
text-rendering: optimizeLegibility;
-webkit-font-smoothing: antialiased;
```
但在这个例子这些值并不会起什么作用。

在 [Horrible font rendering with Google Web Fonts on Chrome for Windows](https://code.google.com/p/chromium/issues/detail?id=137692) 评论里，有人提到**使用SVG替代WOFF**可以避免这种情况的发生，而实际效果也确实如此。

3年多年，我写过一篇 [WOFF和Google Font API](http://swordair.com/woff-and-google-font-api/)，在其中描述了WOFF这种新的网络字体。虽然现在WOFF已经成为了主流的网络字体格式，但当时其支持还非常不完善，Chrome可说是支持最为完备的浏览器，但时至今日，却还没有解决WOFF字体平滑问题。

由于除了Chrome以外我们仍然希望浏览器使用WOFF字体格式，所以单单将SVG提前是不够的，在Chrome解决这个问题之前，我们不得以只好用些hack。以常见的网络图标字体FontAwesome为例，其源代码如下：

```
@font-face {
  font-family: 'FontAwesome';
  src: url('../fonts/fontawesome-webfont.eot?v=4.1.0');
  src: url('../fonts/fontawesome-webfont.eot?#iefix&v=4.1.0') format('embedded-opentype'), url('../fonts/fontawesome-webfont.woff?v=4.1.0') format('woff'), url('../fonts/fontawesome-webfont.ttf?v=4.1.0') format('truetype'), url('../fonts/fontawesome-webfont.svg?v=4.1.0#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
```
利用media query匹配webkit，使用SVG替代WOFF。
```
@media screen and (-webkit-min-device-pixel-ratio:0) {
    @font-face {
        font-family: 'FontAwesome';
        src: url('../fonts/fontawesome-webfont.svg?v=4.1.0#fontawesomeregular') format('svg');
    }
}
```
代码里的`-webkit-min-device-pixel-ratio:0`我们应该并不陌生，这是少数的能定位webkit的hack。但这个方案并不完美，我早些年前做的 [CSS Hack Table](http://swordair.com/tools/css-hack-table/#ex-m1) 里有写明理由。

最后，对于非吹毛求疵的人来说，并不建议对这些锯齿做任何处理。虽然在我看来这种不大不小的问题Chrome修起来总是异常缓慢，但还是要有“信心”。套用之前看到的bug评论：

> 好极了，点击左上角的星号然后祈祷你能再活上个50年。

不要忘了，这不是bug！这，是特性！