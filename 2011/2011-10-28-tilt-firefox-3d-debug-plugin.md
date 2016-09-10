# Tilt: Firefox的3D化插件

**Tilt** 是一个将页面按照文档树的结构3D化的Firefox插件。它基于WebGL，看起来非常的酷，今年七月当 [tilt发布第一个alpha版](http://hacks.mozilla.org/2011/07/tilt-visualize-your-web-page-in-3d/) 的时候，我就被它惊艳的效果吸引了。时隔3月之后，这个插件发布了新的更新，同时Mozilla也发了篇新文章 [Debugging and editing webpages in 3D](http://hacks.mozilla.org/2011/10/debugging-and-editing-webpages-in-3d/)。

下面是我的网站在Firefox7下使用tilt的效果，3D效果把整个网页的层次展现的非常直观：

![](https://swordair.com/content/images/2013/Dec/tilt_swordair_colored.jpg)

抑或者用线框的样子：

![](https://swordair.com/content/images/2013/Dec/tilt_swordair_bordered.jpg)

这看起来真的是帅呆了。不过作为一只前端攻城狮，我也一样觉得这玩意恐怕是中看不中用，至少现在是。很难想象我不用firebug而用这么一个花哨的东西来调试我的页面。最为关键的还是性能问题，我可怜的老机器奔腾双核1.4G+2G+intel x300集显，实在应付不过来- -，卡得就像幻灯片一样...

不过这种层次图的想法真的不错，说不定以后哪天我们的调试工具也能这么酷，至少也能拿来装逼一下不是么？呵呵。

最后提一下安装和使用方法：

1. 我使用的Firefox7 (6.0应该也可，在往下的版本就不确定了)
2. 在 [addons.mozilla.org](https://addons.mozilla.org/en-US/firefox/addon/tilt/) 下载安装插件
3. 打开 Firefox 的 WebGL 支持：
  - 在地址栏输入 about:config 然后回车
  - 确认进入设置修改，搜索 webgl 过滤出相关项
  - 设置 webgl.disabled 为 false
  - 设置 webgl.force-enabled 为 true
4. 快捷键 Ctrl+Shift+M 启动 Tilt

Enjoy it！
