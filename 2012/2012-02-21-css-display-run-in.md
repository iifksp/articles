# CSS display: run-in;

`display:run-in;` 是一个非常有趣的属性，虽然作为普通流里的一个环节，但却很少有人问津。Chris Coyier 曾经写过一篇 [CSS Run-in Display Value](http://css-tricks.com/run-in/)，简述了这个属性的作用。除此以外，它几乎不被人所讨论。

对于[其定义](http://www.w3.org/TR/css3-box/#run-in-boxes)，大致上就是下面这三点：


> 1. If the run-in box contains a block box, the run-in box becomes a block box.
2. If a sibling block box (that does not float and is not absolutely positioned) follows the run-in box, the run-in box becomes the first inline box of the block box. A run-in cannot run in to a block that already starts with a run-in or that itself is a run-in.
3. Otherwise, the run-in box becomes a block box.


翻译成中文就是下面的意思：


1. 如果 *run-in box* 包含 *block box*，那么这个 *run-in box* 也成为 *block box*
2. 如果紧跟在 *run-in box* 之后的兄弟节点是 *block box*，那么这个 *run-in box* 就会做为此 *block box* 里的 *inline box*，*run-in box* 不能进入已经一个已经以 *run-in box* 开头的块内，也不能进入本身就是 `display:run-in;` 的块内
3. 否则，*run-in box* 成为 *block box*


可见 `display:run-in;` 会根据上下文环境变换表现形式：如果后面是一个块，那么就并入其中成为行内头。如果后面是行，则继续独立保持成块。

对于这个属性，也许我们不禁要问，它有什么用？比如，我们完全可以用 `float`、`inline-block` 甚至是 `inline` 替代从而达到类似的视觉效果。然而，典型应用里，`h1 - h6`无法包含在 `p` 里，我们既要达到语义又要达到这样的视觉效果的话，唯有使用 `display:run-in;`。`float` 毕竟无法让标题inline在整个段落的开头，文字的大小会影响换行和布局。而 `inline-block` 以及 `inline` 则不符合语义。

再来看看主流浏览器的表现(结果可能和平时我们队浏览器的概念不太相同)：

- Opera可能是最早完整支持这个属性的浏览器，至少在简单的测试组合里，其都能完整的符合定义的描述。
- 自IE8开始，`display:run-in;` 在IE里也能像Opera一样完整支持。
- Firefox的表现非常令人失望，直到现在(Firefox9)都对这个属性置若罔闻。
- Webkit(Chrome和Safari)的表现非常有趣，其能部分支持 `display:run-in;`，正确将块作为行内元素塞入跟随的块中，不过在很多方面表现的并不正确。比如，如果跟随块里包含其它块，那么它会无法正确包含 run-in块。当连续重复出现多个run-in的时候，webkit会不管三七二十一都将其并入跟在后面的块里。更为有趣的是，每次刷新都只能先并入2个，但每次重绘(比如调整窗口大小)都会再多并入一个...太诡异了。

我写了简单的DEMO：http://www.swordair.com/demos/demo-display-run-in/ ，用不同的浏览器打开可以看到不同的结果。特别的，如果现在用webkit你也许还能看到它诡异的表现:)

客观的来说，这个属性的使用面确实非常狭窄，何况当前的浏览器支持还如此不济的情况下，无人问津是在自然不过的事了。可以想见，即便它能被浏览器很好的支持，也仅能使用在非常特定的场合。

而我之所以写它的目的是为之后的另一篇文章做铺垫。发了这么多文章以后我似乎总是难以避免自己的长篇大论，于是自己必须有意识地把它们拆解开来，以此控制每篇文章的长度。
