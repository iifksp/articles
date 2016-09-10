# 一个二进制的Web新世界

出于战略性原因，IE始终对 [WebGL](http://www.khronos.org/webgl/) 有复杂的感情。Apple，Google，Mozilla以及Opera都已经成为了WebGL工作组的成员，仅剩微软处境极为尴尬。支持WebGL意味着自己苦心经营的DX被孤立在标准之外，不支持单独自立门户的话又是与标准为敌，更要被众多开发者唾弃，如今微软日渐势弱，对于是否在IE10上支持WebGL，对于微软而言绝对将是一个“非常艰难的决定”。

然而就在这一系列关系还没理清之际，微软IE博客昨天发布了一篇 [Working with Binary Data using Typed Arrays](http://blogs.msdn.com/b/ie/archive/2011/12/01/working-with-binary-data-using-typed-arrays.aspx)，显然IE10将确实地支持Type Arrays，加上之前已经支持的Chrome12以及Firefox7，三巨头无疑即将把整个Web推进二进制的新世界。（当然，国内自然是遥遥无期）

不过讽刺的是，Typed Arrays却是由WebGL定义引入的，随后被标准所采纳，包括FileAPI、XHR，甚至是Web Sockets都在考虑是否扩展以支持二进制数据的控制。可见微软的处境不仅仅是与一个WebGL这样的眼中钉为敌，更重要的是，不管愿意不愿意，它都可能必须对标准所采纳的竞争对手的游戏规则低头。

那么什么是Typed Arrays？完整的定义可以在WebGL里找到，同名文档见 [Typed Array Specification](http://www.khronos.org/registry/typedarray/specs/latest/)，而微软指向的文章是ECMAScript.org Wiki上的同名词条 [Typed Arrays](http://wiki.ecmascript.org/doku.php?id=strawman:typed_arrays)，我想应该不是每个人都有兴趣去浏览文档，所以我就翻译一部分概述内容：

- 一种显式的缓冲区类型 ArrayBuffer 被引入。其以明确的长度创建，并且在其生命周期内都是固定长度。ArrayBuffer 中的内容不能被直接访问。
- 同时引入一系列的类型用以描述如何解析 ArrayBuffer 中的字节。比如，Int32Array 将 ArrayBuffer（或其分区段） 中的字节看作 32位带符号整数解析。（译注：即以32bit signed integers作视图）
- 同一个ArrayBuffer可以有多个不同的视图，以支持构建复杂的数据结构，尽管有一定的难度。
- 引入 DataView 类型，用以对来自底层 ArrayBuffer的字节的基础数据类型的任意的索引读写。
- 其目的是，在较小性能损失下，尽可能地做到原生字节的访问，并同时保证安全。

说了这么多，不如直接看微软给出的DEMO：[Binary File Inspector](http://ie.microsoft.com/testdrive/HTML5/TypedArrays/)，怎么说呢，看起来像UltraEdit的二进制视图。终于，JS可以读取二进制了！当然仅限于浏览器支持的文件，不过当前，这已经足够了。

相关的示例代码上面的链接里都有，我就不在冗述。有一点我想提一下，就是“在众多讨伐声中貌似已入暮年”的flash。

以前有人问我AS和JS主要能力上的区别的时候，我总是会把二进制读写放在很前面讲，并非因为读写二进制就似乎无所不能，而是因为这确实是它们间的主要区别之一。flash.utils 包里的 ByteArray 与今天提到的 命题何其相似，甚至在读写二进制控制方面AS仍然略强于JS，然放眼现在的flash移动版失利的形势，各个方面flash都在被HTML5追赶。flash虽然强虽然领先，但在标准之势下，也难挡众多蓄意或非蓄意的围攻。这样的情况也确确实实的发生在微软身上：

今天，微软你实现了源自WebGL而后标准化的  Typed Arrays。

明天，WebGL，微软你能说不么？
