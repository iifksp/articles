# 闲谈CSS单位rem

前段时候有个朋友问我rem需要注意些什么，我回她说这个参考网上一搜一大把。因为如果我没有记错2011年左右这个rem的单位就已经被各大技术博客讨论的铺天盖地。而现在已经快2016年了，不得不感慨时间的流逝。曾经的IE6已经消亡，虽然过程缓慢，但相信IE8最终也会和IE6一样成为历史。到了那个时候，rem就可以真正走上历史舞台了。如是，我觉得自己还是有必要拿rem出来写一写，一来证明一下我这个博客只是诈尸，二来对当前可能没人讨论到的方面做一个补充。不过自己年纪大了，不太适合长篇大论，小标题就不拟了，主要是简短闲谈。

rem是一个好东西。如果没有兼容IE8的顾虑，rem可以说是一个非常理想的单位。rem和em一样是相对单位，只是有别于em相对于父元素，rem相对于根元素`html`，即root em。这样的设定使得rem天生具备扁平级联的特性。早期IE不支持px单位的缩放，这是人们使用em单位的一个主要原因，但em单位存在过深的级联性，加大了组件的控制的难度，同时在某些嵌套标签非常多的时候，欠考虑的em也常常会出现意料之外的结果。正可谓成也级联败也级联。控制的好，很优雅，但恕我所见，这种优雅并不是很有必要，所以在大多数情况下，我总是推荐别人使用px。和年纪也有关系吧，毕竟刚毕业那会明明还那么热爱em～

当然如果不考虑IE8，直接使用rem是更好的选择。和em一样，将基础设置为10px是方便使用的第一步，比如`html{font-size:62.5%}`，这样所有页面元素可以用10的倍数来表示，1.5rem = 15px，以此类推，计算和使用都非常便捷——这看起来很美好，其实并没有什么用。比如Chrome的亚语言版本会强制字体大小12px以上，这个时候上面这个算式是无法达成的，你会发现即便`html`只是拿来做基础，如果不使出些旁门左道来，我们无法将其计算值设置为10px，当打开调试工具一看，12px搅黄了整个节奏。

那么基础设置成100px？这的确是可行的。只不过无端多出很多类似`0.15rem`这样的代码，嘿，还能剩掉个0写作.15rem提升一下逼格。不过，如果真这样用还是会遇到些小问题，我在早期浏览器刚刚支持rem的时候，总是能遇到浏览器在计算值的时候，闪烁出那硕大的100px字体。虽然在现代浏览器里我已经几乎没有遇到，但我已经习惯性地把基础设置为20px，虽然计算起来略有麻烦，但就个人使用应该没有问题。

字体说完了，接下来说边框。既然是标准单位，边框能用吗？当然没问题。但这里放开脑洞需要探寻一下边框是怎么处理rem的。假设现在的基础是10px，当设置边框0.1rem的时候，就是1px的红线，这点毫无疑问。那设置成0.01rem的时候呢？0.01rem自然是不够1px线宽的，所以不会有线。换个问题，如果是0.09rem呢？

计算上并没有什么疑问，0.09 * 10 = 0.9px，那么浏览器会认为0.9px是1px吗？

答案是否定的。这里我有些故弄玄虚了:)，其实这个问题跟rem半毛钱关系都没有。这和我们直接编写`border:0.9px solid red;`一样无用。好吧，绕回来，其实边框是不需要使用rem值的，上面是原因之一。更重要的是，**边框并不需要级联的特性**。所以边框用rem可以说是纯属强迫症行为～

所以说rem就是这么简单一个单位，简单到如果不谈它的定义它的行为，都写不出什么内容。但我还是想办法把文章拉长了！

未来，rem会显得稍微重要些，但也仅此而已了。很多年前，我们憧憬着一处样式在所有的设备上都能work，甚至是未来的超超超高风辨率屏幕，未来的虚拟现实技术，从而选择了看起来美好的东西，但事实往往会削弱这种愿景。当年IE无法缩放px，如今不在是问题。当年的流体布局混合进了当前响应式设计里，iPhone的出现让我们有了viewport，新的技术会为兼容旧的内容而努力，故而，选择了px贯彻始终的话，也决然，并不是一种罪过:)

总结：**rem是基于根元素的相对扁平级联计算单位**，典型适用于需要整体调整字体大小的场景，如多语言网站。