# 为WordPress主题添加Featured Image

WordPress在进步，当然也日渐肥胖，很多新功能对于像我这样单纯的博主而言没什么实际用途，反倒是一种负担。我从WordPress2.5开始使用至今，虽然也一直有自动更新版本，但说实话，当真没在意过新版本多了哪些新功能。所以直到同事问我一个关于Featured Image的问题的时候，我的第一反应就是，这货是个啥呀？

原来WP在2.9时添加了一个文章略缩图(Post Thumbnails)，3.0时更名为精选图片(Featured Image)——我没用过中文版，不知道是不是这么翻译的。这个功能允许文章关联一张图片作为缩略图，显示在list页里。好吧，我深深地OUT了。作为一个2.5版本的WPer制作的主题自然也就只有2.5级别，也难怪自2.7以后的WP主题审核之复杂渐渐突破了天际、各种API要求令人眼花缭乱了。

Google了一下，sitepoint 的这篇 [How to Add Featured Image Thumbnails to Your WordPress Theme](http://www.sitepoint.com/how-to-add-featured-image-thumbnails-to-your-wordpress-theme/) 说的挺清楚的，马上如法炮制一下吧～不过老版本WP里方法 `add_theme_support` 并不存在，所以为了使主题更好的兼容旧版本的WP，文中直接调用的方式有些不妥，起码应该检测一下再调用：

```
if (function_exists('add_theme_support')) {
	add_theme_support('post-thumbnails');
}
```

这种兼容对于像我这种老版的主题来说非常有必要，现在的WP真的是越来越肥硕了，哪天倒回去用WP旧版也说不定。将上面的代码加入 function.php 之后，再在循环里调用 the_post_thumbnail 显示当前文章的 Featured Image 就行了。当然使用前也应该验证下函数：

```
if(function_exists('has_post_thumbnail') && has_post_thumbnail()) the_post_thumbnail();
```

再添加一些必要的样式就成了。不过当我做完这些发现一个问题，就是新版媒体管理器在选择 Featured Image 的时候居然无法指定图片的尺寸！我了个去～

这算Bug么？由内容发布者决定图片的尺寸不是理所当然的么？但现在，一旦选择 Set featured image 跳转到媒体库，选择任何图片都不会出现尺寸选项，但这些选项在常规添加图片到文章的时候都是存在而且正常的...似乎这是一个问题，随后我在 wordpress.org 官方论坛找到这么个描述完全相符的帖子：[Featured Image Sizes Missing - 3.5](http://wordpress.org/support/topic/featured-image-sizes-missing-35)，可惜到现在3.5.1问题似乎并没有被解决。

所有设置缩略图大小的方法得到的结果都只是等比例缩放或裁剪原图，并没有调用后台已经处理过的缩略图(150x150)。看来只能通过在主题调用处指定图片尺寸是最快的解决方法，不过有点一刀切了：

```
the_post_thumbnail('thumbnail')
```

希望这个问题早点被解决。最后我虽然在主题了加入了这个功能，但没打算去用它。可能出于习惯，自己的WP使用层次似乎永远停在了2.5时期...