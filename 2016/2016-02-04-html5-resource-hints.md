# HTML5 资源提示(Resource Hints)

预加载是常见的优化页面的手段，基于推断预知用户的交互从而预先加载即将使用的内容并快速切换给用户，典型场景是加载后续分页的内容。早前，预加载在很多场景里已经被其他一些技术方式替代，比如XHR，比如css background等等，这些预处理技术特定于不同的应用，性能也不能与浏览器原生功能相提并论。更糟糕的是，不当的加载以及冲突反而会使得页面性能劣化。

HTML5的资源提示(Resource Hints)基本上可以简单地理解为预加载，浏览器根据开发者提供的后续资源的提示进行**有选择性**的加载和优化。“有选择性”这一项是必须的且极其重要的，也是有别早先替代方案的重点，因为很多情况下，预加载会受到所分配到的计算资源以及带宽资源的限制，浏览器有权放弃那些成本较高的加载项。

目前Resource Hints还处于w3c工作草案的阶段(官方文档：[https://www.w3.org/TR/resource-hints/](https://www.w3.org/TR/resource-hints/))，但实际上主流的浏览器都已经部分支持了其中的主要功能。对开发者来说，这些特性是不容错过的低投入建设点，简单的几行代码却可以带来相当值回票价的改进。

典型的Resource Hints是这样的一系列link标签：

```
<link rel="dns-prefetch" href="//swordair.com">
<link rel="prefetch" href="/swordair.js" as="script">
<link rel="prerender" href="//swordair.com/next-page.html">
<link rel="preconnect" href="//swordair.com">
```

## DNS预取 `dns-prefetch`

DNS的解析成本是相当高昂的，对于多域名资源的网站来说这部分的开销在初次加载页面是显得额外突出。如果在整个流程里提前解析好即将要使用的其他域名的话，可以很大程度上减轻首次加载的开销。

```
<link rel="dns-prefetch" href="//pic.swordair.com">
```



## 资源预取 `prefetch`

`prefetch`最早是由firefox实现的，将可能用到的资源预先加载，功能和字面意思如出一辙。MDN也对prefetch有较为细节的说明: [Link prefetching FAQ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Link_prefetching_FAQ)，包括一些后台行为，但普通开发者也许也不用知道的那么精确，大体的例子就是这样：
```
<link rel="prefetch" href="//swordair.com/next-page.html" as="html" crossorigin="use-credentials">
```
`as`以及`crossorigin`属性都为可选值。`as`属性用于指定预取资源的类型，浏览器据此可以优化获取的过程，比如设置合理的请求头，决定传输优先级等。`crossorigin`属性则用来表示特定资源的CORS(跨源资源共享)策略。

## 页面预渲染 `prerender`

`prerender`除了将页面获取之外，如名所述，还会执行后台渲染，这使得导航切换更为迅速。
```
<link rel="prerender" href="//swordair.com/next-page.html">
```
由于渲染将会预加载目标页面及其相关资源，整个过程就不仅是预取那么简单，后台逻辑也自然复杂很多。标准里列出了一些参考逻辑，但这都与页面开发关系不大。与`prefetch`可以获取各种资源不同，`prerender`的目标即是HTML，并且**一次只能预渲染一个页面**。

## 资源预连 `preconnect`

`preconnect`用于初始化一个预先的连接，其中包含有DNS，TCP握手，以及可选的TLS协议，作用就是减少潜在的建立连接的开销。主要在一些web应用里会用到。


## 总结与实例

- `dns-prefetch`仅用于预载DNS信息
- `prefetch`用于预取各种资源
- `prerender`仅用于页面的预渲染
- `preconnect`用于连接的建立，其中也包含DNS

另外，微软的文档：[Prerender and prefetch support](https://msdn.microsoft.com/library/dn265039(v=vs.85).aspx)，提供了一些标准里没有的细节，值得一看，但很明显一些条件仅仅只适用于IE。

由于标准里很多逻辑并非强制而是以建议的形式，所以个个浏览器处理逻辑不同并不奇怪。这里仅以Chrome为例说明prerender。当一个页面存在prerender时，可以通过查看chrome浏览器的任务管理器获知，所有与prerender的页面都会以前缀prerender的形式显示：

![chrome preredener](/content/images/2016/02/chrome-preredener.png)

页面同时指定多个prerender，浏览器也仅会执行第一个。prerender初始被引入chrome时是有有效期的，维持30秒没有被使用的话会被丢弃，但当前的版本在资源充分的情况下并不会这么做。但是当页面跳出后，如果目标不是这个预渲染的页面，那么这个后台预先渲染好的页面还是会被丢弃。如果跳转到目标页就是这个已经渲染的页面，那么你将会看到这个页面瞬间就会切换到你的面前，就仿佛在切换chrome tab一样。

在很多情况下prerender是不会起作用的，但各个浏览器又不相同，具体可以参照各浏览器官方文档。

Resource Hints虽然作用可能不算广泛，但对于典型应用和场景，效果不可谓不明显:)

### 浏览器支持
http://caniuse.com/#feat=link-rel-dns-prefetch
http://caniuse.com/#feat=link-rel-prefetch
http://caniuse.com/#feat=link-rel-prerender
http://caniuse.com/#feat=link-rel-preconnect

BGM @ Daybreak by X-Ray Dog