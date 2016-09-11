# 固定站点为Windows磁贴

对于一些常用的功能性网站，将他们固定为windows开始菜单里的磁贴多多少少可以带来一些访问上的便利。尽管每个人都使用习惯迥异，不过总还是有些人热衷去体验系统更新带来的变化。win10公测开始我就毫不犹豫放弃了win7，不过个人并没有使用win8的经验，因为从windows95一路而来的开始菜单的缺失令我无法忍受。win10的开始菜单很好用，win7确实可以安息了...所以就从win10开始吧。

固定磁贴的代码很简单，在meta标签里指定对应的磁贴图片即可，例如：

```
<meta name="msapplication-square150x150logo" content="image-url.png"/>
```

如同字面上一样容易理解，指定的是150x150的图片位置。当然看完微软的参考资料：[Pinned site metadata reference](https://msdn.microsoft.com/en-us/library/dn255024(v=vs.85).aspx)，可以指定出相当复杂完整的列表，不过倒不是非常有必要。如果觉得编写不方便，通过[buildmypinnedsite](http://www.buildmypinnedsite.com/en)这个工具网站也可以很方便生成代码，不过生成质量一般。比如：

```
<meta name="msapplication-square310x310logo" content="/msapplication-square310x310logo.png" />
<meta name="msapplication-wide310x150logo" content="/msapplication-wide310x150logo.png" />
<meta name="msapplication-square150x150logo" content="/msapplication-square150x150logo.png" />
<meta name="msapplication-square70x70logo" content="/msapplication-square70x70logo.png" />
```

内容也可以配置在browserconfig.xml中然后通过meta引用，这也是更为推荐的做法。设置好meta之后，就可以打开edge浏览器，将网站“固定到开始屏幕”了。

![固定到开始屏幕](https://swordair.com/content/images/2016/01/pinned-site-2.jpg)

用作实例的是我自己的向日葵头像，对Metro来说相当的不搭。所以比较好的图片应该是**半透明白色icon**，就像windows自身的风格一样。

![](/content/images/2016/01/pinned-swordair.jpg)

对于一个用户频繁访问的网站来说这么做是有意义的，简单的配置给用户一条通往网站的捷径，特别是对于那些确实有这种系统使用习惯的人而言。不过不知是什么原因，类似京东淘宝这样的最符合用列的网站却并没有这么做，无奈我只能举例：什么值得买～

另外，想起之前写的[定义 iOS 设备的网页图标](http://swordair.com/custom-web-icons-for-ios-devices/)，其实本质上是一样的东西。

最后说个注意点，在选择图片的时候务必慎重些，因为固定到开始屏幕之后，图片在客户端是有缓存的。如果你不小心被客户缓存了测试的icon就真心烦人了。我搜索并尝试了约半小时都未能正确的去掉缓存下来的icon，而且无资料说明缓存时间是多久...所以对于用户来说要去掉几乎是不可能的。并且如果你在测试时花了很多时间去清除缓存仍然未果，那么更换操作用户是最快的选择。(win10最新的更新似乎已经修正了这个问题)







