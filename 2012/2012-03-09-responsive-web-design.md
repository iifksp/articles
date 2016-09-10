# 响应式Web设计

响应式Web设计([Responsive Web Design](http://en.wikipedia.org/wiki/Responsive_Web_Design) – RWD)一般是指那些使用CSS3 Media Query特性制作站点，其可以适应不同视窗尺寸的布局。

虽然很早就已经有了类似RWD的概念，但直到最近一年里才开始变得特别流行，各种文章、例子、工具、模板，不断地从无到有，诸如：

- [响应式Web设计50个例子和最佳实践](http://designmodo.com/responsive-design-examples/)
- [21个响应式Web设计工具](http://www.netmagazine.com/features/21-top-tools-responsive-web-design)
- [响应式Web设计的模板和框架](http://designmodo.com/responsive-templates-frameworks/)


这些资源，仿佛都是一下子冒出来似的…这要归功于IE9的发布，从而使得主流浏览器上升到全部支持Media Query的程度。

然而眼花缭乱的资料堆里，可供细细品读的资料其实也就那么几篇：[ETHAN MARCOTTE](http://www.alistapart.com/authors/m/emarcotte) 的 [Responsive Web Design](http://www.alistapart.com/articles/responsive-web-design/)，以及其引用的 [JOHN ALLSOPP](http://www.alistapart.com/authors/a/johnallsopp) 的 [A Dao of Web Design](http://www.alistapart.com/articles/dao/)。

无论是在UX角度还是维护角度，RWD的优势都显而易见。虽然很多移动设备都默认就可以缩放页面，但是RWD就意味着最少的缩放，最少的滚动，最少的用户行为。我们不必要求用户拨动他们的手指来调整页面的舒适度，因为这种舒适度是我们一开始就设计好的。

作为设计师，RWD算得上是一种限制，甚至一些设计的权利已经被前端所剥夺。为了达到良好的自适应效果，很多设计点上不得不做出偏向代码的妥协。所以可能单纯的视觉设计师可能并不喜欢RWD。然而对于前端来说，RWD非常迷人：一处代码，却处处不同。开发人员只要开发维护一个版本，就能使页面在不同的设备里均呈现良好的访问性效果，何乐而不为？

一旦牵着到RWD，就不可避免的使得页面设计本身趋向相似。看看那些RWD的页面就会发现，很容找出他们的共同点：流式的布局、网格化的排布以及相对而言死板的布局。之所以如此，是因为对于RWD而言，流体图片([Fluid Images](http://www.alistapart.com/articles/fluid-images/))和流体网格(Fluid Grids)是必须，完善的RWD视觉设计里必须以它们为限制。

**而随着RWD的各种或浅或深的应用，一些非常常见的IE trick的弊端也开始显现**。

比如，IE低版本的“浮动双倍边距bug”是如此常见，以至于我们几乎是可以闭着眼睛在浮动元素上添加 display:inline; 来触发IE的haslayout来修正。由于浮动元素独自成块并构建自己的块格式化上下文，所以这句 display:inline; 的有无对现代浏览器来说毫无影响。当然问题也就随之而来，在 Media Query 里仅仅取消这个元素的浮动是不够的，还需要同时将 display 置回 block。即使是经验丰富的人也可能在这里浪费一些时间，因为他们可能意识不到一个长久写惯的trick会在这时候捅你一刀。

最后，虽然RWD如此流行并且是个好东西，不过对于Web而言也并非必须。网站依据自身特点可以酌情引入部分这种功能，以此来改善移动设备的浏览体验，毕竟有些站点本身就不适合做RWD的考量。