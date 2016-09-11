# 使用Ghost的隐私限制功能停用Google字体

从版本0.5.2开始，Ghost提供了一种更为便捷的隐私控制功能，基于配置文件config.js，可以关闭默认开启的隐私相关的第三方服务，其中也包括Google字体。

由于国内屏蔽Google服务所以加载Google字体会使得网站假性抽风。对于前台页面，我们可以使用国内的字体服务来避免，或者干脆[缓存Google字体副本](http://swordair.com/cache-google-font-locally/)到本地服务器。但Ghost后台同样严重依赖于Google字体，每次更新版本后修改core文件模板可以解决问题，但总是这么改也确实很麻烦，而且事实上我也是一直这么干的。直到这次我更新Ghost到0.5.5，发现后台的模板文件已经变成这样：
```
{{#unless skip_google_fonts}}
    <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Open+Sans:400,300,700" />
{{/unless}}
```
既然多了这个`skip_google_fonts`，显然是新加出来的配置项。查了一下文档才发现Ghost从0.5.2开始就已经提供了这个贴心的隐私限制功能。利用这个功能就可以更简单的停用Google字体，只需在配置文件config.js里加上：
```
privacy: {
	useGoogleFonts: false
}
```
这样每次升级Ghost就再也不必去修改core文件了。其他的可以停用的第三方服务被列在 [PRIVACY.md](https://github.com/TryGhost/Ghost/blob/master/PRIVACY.md) 这个文件里。

我认为版本0.5.5的最重要改进是修复了内存泄漏问题。之前我使用0.5.2也确实遇到了内存泄漏。Ghost正常使用的内存徘徊在100M左右，但内存泄漏后往往在线不出几天就将近400M以上。所以对于还在使用老版本，特别是0.5.2的用户，建议尽早升级到最新。