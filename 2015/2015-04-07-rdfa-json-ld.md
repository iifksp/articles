# JSON-LD与互联数据(Linked Data)

之所以想起来写些关于JSON-LD的东西，是因为Ghost对其提[供了支持](https://github.com/TryGhost/Ghost/commit/23e98aa8dcad3ae79b0460cfb8abd35575096b0c)。而由于我总是跳着更新，所以最近才刚刚发觉到，但这真是极好的...

[JSON-LD](http://en.wikipedia.org/wiki/JSON-LD) 是 JavaScript Object Notation for Linked Data的缩写，是一种基于JSON表示和传输互联数据（Linked Data）的方法。对于JSON我们格外熟悉，但对于**互联数据**这个词，可能部分人会既熟悉而又觉得陌生，因为单字面意思已经很清楚，但要表达出其完整含义也并不容易。

互联数据可以说是语义网(Semantic Web)的一部分，但相较于1998年语义网概念的提出，互联数据这个一样显而易见的概念实际出现却要晚了数年。这些年我们一直在强调Web的语义，即便是HTML5那一堆新增标签也不外如是。语义并不是简单的将数据放到Web上，而是对于不仅仅是人，机器也必须都能识别并准确的处理以及高效的利用。而互联数据则更要求数据的指向性，当我们有部分数据时，即可找到对应的相关联的其他数据。听起来Web不一直是这样吗？并非如此，当数据从数据库里被拼装成页面后，可能对于人而言找到数据可能轻而易举，但对于机器而言，很难将页面恢复到原始数据，也缺乏语义的判断。若无标准来统一语义和数据互联，对于机器来说整个Web其实是乱糟糟的。

为了使机器可以准确理解，处理和整合数据，我们有了语义Web。而其组成部分RDF(Resource Description Framework)即是用于描述网络资源的W3C标准。[RDF工作组](http://www.w3.org/2011/rdf-wg/wiki/Main_Page)目前已经停止了活动，但通过艰苦的努力，已经将[JSON-LD 1.0](http://www.w3.org/TR/2014/REC-json-ld-20140116/)及[JSON-LD 1.0处理算法和API](http://www.w3.org/TR/2014/REC-json-ld-api-20140116/)推进到了W3C的标准状态。

回过头来再看JSON-LD实际例子，比如Ghost的JSON-LD输出(因为比较长，所以略去一些)：
```
<script type="application/ld+json">
{
    "@context": "http://schema.org",
    "@type": "Article",
    "publisher": "葵中剑@剑空",
    "headline": "浅读 Media Query Level 4",
    "url": "http://swordair.com/some-parts-of-media-query-level-4/",
    "keywords": "CSS, web standard, media query"
}
</script>
```
JSON一目了然，这也是为什么标准选择JSON格式的原因，基于JSON并通过社区的努力标准化，本质上实现的是：**通过将概念映射到IRI使得数据自描述**(self-descriptive)。可以看到例子里的JSON包含一个上下文"@context"，并使用了Schema.org提供的数据结构，使得JSON里的描述字段"publisher", "headline"都有了约定的精确定义(在类型[Article](http://schema.org/Article)中列出)。搜索引擎就可以通过这些约定的概念，更为准确的找到(处理)我们搜索的数据。

对于JSON-LD本身这里不具体讨论，定义可以参看[标准文档](http://www.w3.org/TR/json-ld)，虽然类目繁多，但开发人员使用JSON-LD并不困难，只需要了解关键字"@context"以及"@id"就可以使用其基本功能。

Google在2013年底IO大会上宣布兼容JSON-LD，特别是其Gmail等产品。而这几年我也看到部分开源库已经开始提供返回的JSON—LD的API。毫无疑问，在这个RESTful API的Web时代，JSON-LD确有用武之地。

最后提一下与JSON-LD相似的RDFa(通过属性嵌入RDF)，同样是提供上下文，但属性嵌入到方式注定RDFa只适合小范围的片段嵌入以代替Mircodata，而难以用作大量纯粹数据的定义方式。而添加RDFa的难度也往往大于JSON-LD，毕竟是需要去接受已经存在的内容的情况。当然，搜索引擎对于非可见数据，无论是RDFa还是JSON-LD，应该依旧只是参考作用。搜索引擎也理应总是偏向可见的内容。

#### 参考资料

- [json-ld.org](http://json-ld.org/)
- [RDFa Core 1.1 - Second Edition](http://www.w3.org/TR/2013/REC-rdfa-core-20130822/)
- [schema.org](http://schema.org/)
- [Embedding Schemas in Emails](https://developers.google.com/schemas/tutorials/embedding-schemas-in-emails)
- [W3C发布JSON-LD 1.0的正式推荐标准](http://www.chinaw3c.org/archives/393/)
- [Linked Data - Tim Berners-Lee](http://www.w3.org/DesignIssues/LinkedData.html)
- [Building Next-Gen Web APIs with JSON-LD and Hydra](http://www.slideshare.net/lanthaler/building-next-generation-web-ap-is-with-jsonld-and-hydra)
