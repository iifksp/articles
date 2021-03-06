# 那些JavaScript的细节问题(1)

JS写的多了以后，日常也就会遇到各种各样扯淡的问题。这些问题往往只是无关紧要的细枝末节，但偶尔有时候，也会给我们造成一些困扰。于是，我打算不时的拿出一些这类问题讨论和记录，这样，需要答案的人可以找到。没事看看的朋友，也可以当作消遣:)

鉴于这类问题浏览器相关性很强，所以每次我都有必要说明测试环境，我把它们写在最后，当我讨论这些问题的时候，都默认限制在这个测试环境之内。

## 1. alert() 内容为空时，出现什么？

先来一个比较简单的。如果你未曾尝试，想象一下，直接无参数调用 `alert()` 会跳出什么？

Chrome 和 Safari 弹出内容是 "undefined" 的窗口，Opera 与 IE 都弹出空内容。前两者似乎都还能够理解，但 Firefox 很奇怪，它完全无动于衷——连个弹出框都没有，Firefox 对空调用的 `alert()` 不会有反应。

没事拿这个测5个浏览器的人恐怕不多...正因为如果你拿着Firefox在调试某段代码，并且偷懒地在代码里加这么个空调用，那么之后的窘迫就足以让你把这个语句在所有浏览器里测试一遍了。

## 2. 边框是否是元素的一部分？

是，这不是理所当然的么~ 这个问题的本质其实是，为什么边框响应鼠标事件，但是margin却不会？

好吧，我不得不承认，即使换一个问法，结果仍然是理所当然的。所有的JS的事件描述都仅仅限制到元素，所以说，既然边框响应了事件，那么结论就是，**边框就是元素的一部分**。但是为什么？

真正难回答的是，如何在标准里找到相关描述。边框是元素最后的边界线，其之外的 `outline`，`margin` 以及 `box-shadow`，无论是否影响布局，都已无法称之为元素本身。也正因为此，我们能用透明边框处理一些有趣的东西，因为其虽然透明，却仍是边框，仍是元素。

## 3. `click` 先触发还是 `blur` 先触发？

这一期的最后一个问题，当鼠标点击使得焦点从一个 `input` 上移除的时候，是 `click` 先触发还是 `blur` 先触发？毫无疑问，是 `blur` 先。原因很简单，click的完成是在鼠标按键弹起时候的事了，按下到弹起已经是足够长的时间，`blur`自然是要快。所以这个问题这么问是不对的。

真正的问法是：**`mousedown` 先触发还是 `blur` 先触发？**

这样问的话，就容易让思路陷入混乱。4年前，在我还是个非常非常菜的菜鸟的时候，在应聘前端实习生的时候遇到了这个问题，当时我想了一下，回答“`mousedown` 先触发”。然后面试官告诉我，说“应该是`blur` 先触发”，他举了个比喻：你还没有离开这间房间(意指`blur`)，如何能在外面站立？他让我回去自己试一试。

之后我其实没有尝试，因为那之后并没有走前端这条路，而是成了一名 linux 的 SA，之后一年又在服务器架构/网页设计/邮件/代码管理/PHP/Flash/Ruby等多个领域流亡，渐渐也就忘了这个问题。直到去阿里工作重拾前端的职位，才想起还有这么一个心结的问题。

直到现在，我仍然坚持自己当时的选择，`mousedown` 先触发，没有鼠标在外部点击的事件在先，input 何以知道要 `blur`？然而，IE 与 Firefox 的确是 `blur` 触发在先，只是并非所有浏览器都是这么理解的。其实除了IE 和 Firefox 外，其他浏览器都是 `mousedown` 触发在前，我很高兴地看到，自己当年的回答也并非完全是错的。只是当年还没有 Chrome，而 Opera 也太边缘化。~~可见，在这个问题上，其实并没有定论。~~感谢一楼评论(换用Ghost之后所有的评论都丢弃了)的这位朋友帮忙做了最新浏览器的测试补全，可见现代浏览器的表现是一致的，`mousedown` 都触发在前，IE也不例外。

这次的问题就到这里了，下次再凑些出来。然后，也打算弄个CSS的系列出来，不过没打算再去写那些该死的Hack，而是着眼于现代浏览器，着眼于那些难以在标准里找到根据的细节。

测试环境：

- OS: WinXP
- Browser: Chrome-21，Firefox-12，Opera-12.01，IE-8，Safari-5.1.7
