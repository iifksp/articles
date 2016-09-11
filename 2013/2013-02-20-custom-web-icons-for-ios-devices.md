# 定义 iOS 设备的网页图标

苹果iOS设备(iPhone,iPad,iTouch)可以定义个一个类似原生app的的图标，将网页添加到主屏幕。这个设计主要用于Web应用程序，再通过定义启动画面和safari的渲染模式，让一个Web应用看起来就像是原生app一样。我觉得即使不是webapp，任何一个网站都可以考虑做这么一套图标，让用户将页面链接直接加到主屏幕方便他们直接一个点击就能访问到这个网站。

在苹果的官方文档里，这种用主屏幕图标代表的链接，称之为Web Clip(网页剪辑)。比如太平洋不仅适配了iPad的显示版本，还着重将这个功能提示的格外醒目，希望用户能尽可能利用系统的方便之处：

![pconline ipad version screenshot](https://swordair.com/content/images/2014/Jan/pconline_ipad_version_screenshot.jpg)

添加的方式很容易，制作对应各个分辨率的屏幕的图标，然后将它们放在网站的根目录下即可。但命名必须遵从apple给出的规则(就是我们常说的 `apple-touch-icon`)，这样iOS就会默认去寻找这些图片下载并作为图标使用：

- apple-touch-icon.png
- apple-touch-icon-57x57.png
- apple-touch-icon-72x72.png
- apple-touch-icon-114x114.png
- apple-touch-icon-144x144.png

下图就是添加前后的对比，可以看到，如果不定义这个图标的话，系统会默认使用网页的截图作为图标使用，效果是真真差的慌了...

![specify webpage icon for web clip](https://swordair.com/content/images/2014/Jan/specify_webpage_icon_for_web_clip.png)

由于现在苹果设备的规格不像以前那么单一，所以对应的尺寸也就越来越多。比如适配retina ipad就需要144x144像素的图标。图片处理上不需要添加圆角和高光，苹果是会自动为其添加“专利圆角”的哟～如果不希望苹果修饰图片，可以添加 "`-precomposed`" 后缀来阻止苹果改动你的图标：

- apple-touch-icon-precomposed.png
- apple-touch-icon-57x57-precomposed.png
- ......

如果目录下同时有连个版本的图片，苹果优先使用带后缀 `-precomposed` 的版本。比如一个需求57x57像素图标的设备，会依照下面的顺序进行搜寻。没有对应尺寸，则会选取较大的较接近的使用。

1. apple-touch-icon-57x57-precomposed.png
2. apple-touch-icon-57x57.png
3. apple-touch-icon-precomposed.png
4. apple-touch-icon.png

如果不是为整站而是专为某个页面添加的话，或者图标需要存放到默认路径以外的地方的话，可以在代码里指定:

```
<link rel="apple-touch-icon" href="touch-icon-iphone.png" />
<link rel="apple-touch-icon" sizes="72x72" href="touch-icon-ipad.png" />
<link rel="apple-touch-icon" sizes="114x114" href="touch-icon-iphone-retina.png" />
<link rel="apple-touch-icon" sizes="144x144" href="touch-icon-ipad-retina.png" />
```

放在主屏幕上的感觉其实和应用完全相同，除了归类组别的时候，系统会自动将组命名为“书签”。这时才意识到，这货只是个链接而已~对比两个图标就可以发现，太平洋的图标阻止了苹果的效果，而我的图标，顶着个大大的高光，配合苹果“圆角专利”，果然更加有浓浓的苹果味～ :)

![](https://swordair.com/content/images/2015/03/ios-bookmark-group.jpg)

最后说下个人观点：iPhone和iTouch受限于屏幕尺寸，即便分辨率不低，也无法有很好的浏览体验。所以最重要的图标尺寸无疑是72x72和144x144，分别对应iPad2/mini 以及 iPad3/4。对于新闻类站点，这种一键点击的优越性额外显著，所以可以看到网易也一样做的很好。但是，对于这种低投入高效果的伎俩却并没有被大范围的使用，更多的网站只是把精力用在了原生App上，舍得花大力气做App却连个图标都不放一下，忽略了很多用户更为简单的需求——更为快速的访问到网站，未免有些可惜。

#### 参考资料

- [Configuring Web Applications](http://developer.apple.com/library/ios/#documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html) from Safari Web Content Guide
