# 晖星Presto的陨落

这段时间前端界的头条大新闻无疑是“Opera即将逐步放弃自己的Presto核心转而采用Webkit”，虽然之前的新闻还只是Opera在移动浏览器上使用Webkit，但一旦变成桌面也一样的情况，立刻就成了一颗重磅炸弹，粉碎了这个领域里虚伪的和平。Webkit几乎垄断了移动浏览器市场，现在，似乎也即将称霸桌面。欢喜者似乎幻视到了一个统一的标准的平台恍然在眼前招手，担心者回顾过往揣测着眼前的巨兽仿佛看到了IE6，各种评论在各个论坛炸开了锅，各种声音削尖了脑袋此起彼伏，就好象是迎接三国鼎立时代的序幕一般。哦！壮哉，我大Opera，壮哉，我大Presto。如同一颗晖星从天划过，标志着一个时代的终结，和另一个时代的开幕。

可能所有人都没想到这样的结局，如果说，在移动领域采用webkit是鉴于其霸主地位的无奈之举，那桌面端的全面缴械几乎意味着Opera对于Webkit的认同。一个存在快要20年的浏览器内核所走就走了，简直是难以置信。但确是事实，2013年2月12日，世上最独特的自有内核浏览器——Opera，其官方博客发布了这则消息，标题为：[300 million users and move to WebKit](http://my.opera.com/ODIN/blog/300-million-users-and-move-to-webkit/)，这样一篇意义非凡的文章我还是耐心地看完了，包括文章下面的诸多评论——有人说这是明智的决定，也有人说要告别Opera，其实最无法接受这个事实的，应该还是那些坚持使用Opera的用户，他们担心Opera就此依附于Google，甚至有人觉得，比起选择Webkit，还不如选Gecko。


Bruce Lawson 陈述里，对更换内核给出了下面这样解释：

> When we first began, back in 1995, we had to roll our own rendering engine in order to compete against the Netscape and Internet Explorer to drive web standards, and thus the web forward. When we started the spec that is now called “HTML5″, our goal was a specification that would greatly enhance interoperability across the web.

> The WebKit project now has the kind of standards support that we could only dream of when our work began. Instead of tying up resources duplicating what’s already implemented in WebKit, we can focus on innovation to make a better browser. Opera innovations such as tabbed browsing, Speed Dial and data-saving compression that speeds up page-load, have been widely copied and improved the web for all.

随后，我看到 [css3.info 发文](http://www.css3.info/opera-announces-switch-to-webkit-rendering-engine/)称 Bruce Lawson 在其自己的博客里陈述了更多原因：

> Opera’s Presto engine was a means to an end; a means for a small, European browser company to challenge the dominance of companies who, at that time, hoped to “win” the web through embracing, extending and extinguishing web standards.   
Presto showed that it was possible to make a better browser while supporting standards. Other vendors have followed this path; the world has changed.
These days, web standards aren’t a differentiator between browsers. Excellent standards support is a given in modern browsers. Attempting to compete on standards support is like opening a restaurant and putting a sign in the window saying “All our chefs wash their hands before handling food”.
Rendering engines are now highly interoperable – largely due to the progress commonly known as “HTML5″, begun by Opera in 2004, then joined by Mozilla, in order to protect the web from proprietary platforms, keep it open and promote interoperability.   
It seems to me that WebKit simply isn’t the same as the competitors against which we fought, and its level of standards support and pace of development match those that Opera aspires to.

**Presto已功成身退**，给我的是这样一种感觉，淡淡的哀伤和深深的遗憾。Presto完成了开创Web标准的使命，如今，Opera将不再重复Webkit已经实现的功能，做重复造轮子的事，而是把精力放在浏览器体验上。此时此刻，恐怕是Mozilla的心里最不好受了。

早前，W3c就webkit前缀的问题已经有过一番争论，这次的事件再一次将这种Webkit威胁论推到了风口浪尖。就像OnePiece里的四皇一样，4大核心一直维持着一种微妙的平衡，现在的这种失衡在真正达到三足鼎立的新平衡之前，会彻底改变整个浏览器的格局，尽管Opera的市场份额几乎可以忽略，但是它近20年的历史影响力足够让人为它的落幕扼腕叹息。

我不是Opera的忠实用户，但作为一个前端其实仍然希望看到浏览器的百花齐放——尽管是怀揣着众所周知的矛盾。Webkit虽如日中天但也暗流涌动，Mozilla身上的担子更重了，IE的日子更难受了。而身处于这样浏览器乱世，作为前端还是该干嘛干嘛吧...就如同Opera所说的那样：

> Keep coding to the standards, not to individual rendering engines; test across browsers - Opera, Firefox, Chrome, Safari and Internet Explorer; use all vendor prefixes and an unprefixed form in your CSS and JavaScript. 

Web需要标准，但任何一个内核和浏览器都不是标准，它们为标准所用，却都不应单独主导标准。无论是曾经的Trident，还是现在的Webkit，当前它们不仅不是标准，未来，我们还得防止它们成为标准。

Opera，为Web标准化贡献了太多太多，终究还是英雄迟暮。别了，Presto。