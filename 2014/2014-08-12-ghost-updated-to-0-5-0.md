# Ghost更新为0.5.0

今天早上收到了Ghost发来更新邮件，Ghost正式放出了0.5.0版本。由于一天都比较忙碌所以早上还没来得及看，下班后很兴奋的下来最新的版本体验了一番，结论就是这一次升级在表现的功能上最主要的多了多用户的功能，Ghost终于官方支持多作者模式。

更新没有遇到问题，短暂的重启应用后，马上尝试了一下新的casper(1.0)默认主题，变化有一些，但总体还是那个调调——大封面+大字体，直观大概印象的变化主要罗列为：

- 首页全幅的封面太丧心病狂了，内容直接跑第二屏...
- 明显的加强了订阅的位置，更为醒目
- 多用户的支持，使得新增加了作者的独立页面。在作者页上不仅列出了其全部作品，还可以单独订阅这个作者的文章。
- 主题的字体更大，由于还是加载了google字体所以严重影响了国内浏览的速度。

用了一会新主题，最后还是切回了这个casper旧版。我只能说新版太前卫了完全适应不过来啊...目前看来功能上需要尽快跟上的是单独的作者页。现阶段还是有点忙，等过段时间再说吧...

暴露静态内容的代码仍然在core/server/middleware/index.js中，像我这样有部分静态页面的可以直接找到`// Static assets`的区域添加自己的静态文件路径。

为了加快后台的速度，安装完后最好还要去掉Google字体的引用，模板文件位于core/server/views，将default.hb中的Google font引用注释掉。

```
<!-- <link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Open+Sans:400,300,700" /> -->
```

当时从wordpress切到ghost的时候，理所当然的多用户Ghost居然都不支持，到现在，Ghost不断发展和完善，已经越来越接近一个博客最核心功能的绝大部分。看着这样一个优秀的博客系统的成长，甚是欣慰。但愿其不要发展的像wordpress那般臃肿不堪，一直简单下去。