# line-height导致的邮件图像间隙

众所周知，OutLook2007不是使用IE来渲染HTML邮件的。为了它所谓的安全性和扯淡的统一性，微软采用 office word 统一渲染，这就使得Outlook2007对HTML邮件支持非常有限。

不支持浮动和定位，那些属性统统会被过滤掉。还有糟糕的背景图片支持，使得我们开始重新使用table来布局，用图片拼接起促销的页面，为的可能只是那一点点兼容性。

上周就遇到了这个问题。table拼接的图片因为一个全局的`line-height`导致在Outlook2007里莫名其妙地出现了间隙。换句话讲，只能对文字使用`line-height`，一旦图片被应用`line-height`值，就会拼不起来。

![outlook2007 gap in picture](https://swordair.com/content/images/2013/Dec/outlook2007_gap_in_picture.jpg)

即使邮件的HTML本身在所有浏览器里显示都是正常的，但是上图这封邮件在发到各个邮箱后又是另一回事了。比如QQ邮箱下，IE6还是会莫名奇妙的出现间隙问题，不是`line-height`引起的，比起OutLook2007更加诡异，由于没能找到原因，就不详细写了。所以，如果邮件很重要，最好是能测试下主流邮箱的显示效果避免出现太大的问题。

邮件兼容性问题是在是比较复杂，推荐阅读：[系统邮件模板的邮箱兼容性](http://ued.alipay.com/wd/2008/11/04/%E7%B3%BB%E7%BB%9F%E9%82%AE%E4%BB%B6%E6%A8%A1%E6%9D%BF%E7%9A%84%E9%82%AE%E7%AE%B1%E5%85%BC%E5%AE%B9%E6%80%A7/) 以及 [如何编写兼容各主流邮箱的HTML邮件](http://ued.koubei.com/?p=239)。(23 December 2013: 很不幸的，似乎链接都已经挂掉了...)