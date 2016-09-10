# Chrome中的text-shadow

text-shadow作为CSS3里被现代浏览器支持最早、也是最广的属性，我们对它已经非常熟悉。现代浏览器除了IE9之外，全部无前缀支持text-shadow。和box-shadow类似，但远没有box-shadow那么复杂。它的作用只是对文本添加阴影:
```
text-shadow: 2px 2px 3px blue;
```
其用法有大致上有两种，一种是在浅色背景上应用深色阴影凸显出文字的层次。另一种，就是在深色背景上使用浅色阴影创造出类似浮雕的内嵌的效果。最后用的比较少的，就是单纯的雾化文本，也就是X和Y的偏移设为0——不过这样对文本的可读性没什么好处，也没什么技术含量，所以完全不值得提倡。淘宝曾在首页的热卖单品区单纯的使用text-shadow雾化文本，但现在已经移除了这种蛋疼的效果。

我并不打算花时间展开text-shadow，因为这个属性相当简单，所以text-shadow本身并不是本文的重点。这里要写的是：**text-shadow在Chrome下的bug以及关于这个bug的一些有趣的事**。

Chrome早在2.0的时候就支持text-shadow，但直到现在的Chrome12dev都仍然存在text-shadow的bug。之所以这个bug一直被忽略，主要是它在显示上只是有微小的不同。不过即使如此，对于网站可用性要求较高的开发者是完全不能放过这种bug带来的影响的。下面讨论的问题的目标都是chrome，虽然safari同为webkit，却并不存在这个问题。

熟悉Chrome的人都知道，在对文本使用text-shadow属性之后，文本会变得有点**模糊**，甚至会变得不可阅读，并且在那些字体较小的文本上这种感觉会特别明显。下面是我截取的论坛一位朋友的博客里的文字，由于使用text-shadow，**Chrome下12像素的中文宋体几乎不可读**。相比而言，firefox就完全没有受到影响。

![chrome firefox small font text shadow](https://swordair.com/content/images/2013/Dec/chrome_firefox_small_font_text_shadow.png)

虽然字体稍大的情况下，Chrome的这种模糊就马上显得不是很起眼，甚至在20px以上的字体上，chrome显示的似乎还更加平滑，但对小字体来说，这其实是一种噩梦，这也是为什么这个问题从Chrome Version: 3.0.195.21 (Official Build 26042)被报告后，被122人标星关注着。

如果放大Chrome的渲染结果，就能看到为什么字体会这么模糊。**原因就是：Chrome对所有使用text-shadow的文本施加了一种灰度抗锯齿，并且因此和ClearType的子像素抗锯齿冲突，从而无法显示带有text-shadow属性的ClearType字体。**

![chrome firefox text shadow](https://swordair.com/content/images/2013/Dec/chrome_firefox_text_shadow.png)

关于[ClearType](http://en.wikipedia.org/wiki/ClearType) 这里不多说，可以参考微软或者是wiki的解释。Chrome令人匪夷所思的就是在实现text-shadow的时候，不仅关闭了ClearType的子像素抗锯齿(The sub-pixel anti-aliasing)，同时对所有文本额外附加了它自己的灰度抗锯齿(grey anti-aliasing)，并且还让webkit的 -webkit-font-smoothing 成了摆设。结果怎么样呢？对于英文而言，这种灰度模糊对文本的损失还在可以接受的范围内，但对于构型复杂的像素字体宋体而言，轻微的模糊都是致命的。

其实不用text-shadow不就得了，何况IE9都压根不支持。但如果你和我一样刚睡醒闲的蛋疼，还是可以关注一下这个问题。看看[这个问题在stackoverflow上的讨论](http://stackoverflow.com/questions/4046142/google-chrome-text-shadow-rendering) ，以及看看[这个问题在chromium项目里的讨论](http://code.google.com/p/chromium/issues/detail?id=23440) 。有些老外开发者的抱怨真的非常有趣，我这里随意摘录一些：

>Chrome 7 is out and still this bullshit blur. Little surprise as nobody attempts to fix it. Are you guys understaffed? Maybe I could do it for you?

>Chrome 7已经发布仍然带着这个扯淡的模糊。没有人试图去修复它让人有点惊讶。人手不足？也许我可以帮帮你们？

>They will not be able to fix in the next milestone as they are not even attempting to fix it. This is what the status Available means. It can stay like that forever. Nobody even bothers to respond something here. I am really dissapointed with Chrome project.

>他们不能够在下个里程碑里修复因为他们从来没有这么想过。那就是这个bug Available状态的意思，这个状态能一直保持下去。没人在意这里。我对chrome项目真的非常失望。

>Good, click on the star on the top left and pray that you live at least another 50 years.

>好极了，点击左上角的星号然后祈祷你能再活上个50年。

一连串漫长的讨论之后，终于在44楼出现一个[解决方法](http://jsbin.com/acalu4) ，对于Chrome使用下面这样的CSS：
```
text-shadow:0 0 0 transparent, 0 1px 1px #FF0;
```
又见多重阴影！对第一个阴影使用0 0 0 transparent的参数，实际上这个阴影不显示，但是确实修正了chrome的这个bug。我不禁感慨开发者的伟大，总是不遗余力地发现着各种神奇。

3月21日，就在关注了这个问题几个月后，我收到了订阅的邮件，问题状态从Available改成了Assigned，为这个问题暂时画上了句号:)

**4/12更新：**今天收到邮件，Webkit r83541将会修复此问题。   
**4/13更新：**今天再次收到邮件，状态居然更新到fix！！！何等的神速！未来的dev估计很快就能看到。   
**10/14更新：**过去的6个月里，此bug经历了数次状态变化，从fix退回到assigned，然后一直有订阅邮件寄到我的邮箱里，讨论仍旧很激烈。一段工作的繁忙过后回头再看这个问题时，我突然发现Chrome15已经完整解决了这个灰度抗锯齿bug。虽然google修复得不快，但总算告一段落了。   
