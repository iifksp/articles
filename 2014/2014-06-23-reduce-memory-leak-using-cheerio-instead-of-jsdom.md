# 用cheerio替代jsdom减少内存泄漏

[jsdom](https://github.com/tmpvar/jsdom)用来解析获取的页面并生成dom，在抓取页面数据的时候非常方便好用，但是，jsdom似乎存在内存泄漏的问题。在数据量较小的情况下并不明显，数据一大马上就会变得可以感知。并且`window.close()`的调用并不能完全解决问题，只能延缓这种泄漏的速度。

在node执行时加入`--trace_gc`参数可以看到内存清理的信息。下面的示例数据略乱，只是放出来看看的。

```
[3657]   163984 ms: Scavenge 190.5 (220.0) -> 179.2 (222.0) MB, 57 ms (+ 43 ms in 131 steps since last GC) [Runtime::PerformGC].
[3657]   164732 ms: Scavenge 193.3 (222.0) -> 182.6 (224.0) MB, 17 ms (+ 67 ms in 141 steps since last GC) [Runtime::PerformGC].
[3657]   166696 ms: Scavenge 198.5 (226.0) -> 187.5 (229.0) MB, 18 ms (+ 65 ms in 145 steps since last GC) [Runtime::PerformGC].
[3657]   169790 ms: Scavenge 201.2 (230.0) -> 190.3 (232.0) MB, 42 ms (+ 59 ms in 111 steps since last GC) [allocation failure].
[3657]   170529 ms: Scavenge 203.4 (232.0) -> 191.6 (235.0) MB, 32 ms (+ 65 ms in 122 steps since last GC) [Runtime::PerformGC] [incremental marking delaying mark-sweep].
[3657]   171214 ms: Scavenge 205.7 (235.0) -> 194.2 (237.0) MB, 20 ms (+ 74 ms in 147 steps since last GC) [allocation failure] [incremental marking delaying mark-sweep].
[3657]   171988 ms: Scavenge 208.4 (238.0) -> 197.5 (240.0) MB, 22 ms (+ 57 ms in 111 steps since last GC) [Runtime::PerformGC] [incremental marking delaying mark-sweep].
[3657]   172844 ms: Mark-sweep 208.3 (240.0) -> 172.3 (237.0) MB, 133 ms (+ 650 ms in 1025 steps since start of marking, biggest step 158.227051 ms) [StackGuard GC request] [GC in old space requested].
```

基于这样的原因，[cheerio](https://github.com/cheeriojs/cheerio)就作为jsdom以外的新的选择进入了视线里。cheerio的简介是 Fast, flexible, and lean implementation of core jQuery designed specifically for the server. 敏捷轻量是其特点，在一般的DOM操作上完全可以替代jsdom。

对于一个jsdom的典型用法的例子：
```
var jsdom = require("jsdom");
jsdom.env(url, ["http://code.jquery.com/jquery.js"], function(errors, window){
	var title = $('h1.title').text().trim();
});
```
使用cheerio后，改变的部分并不多：

```
var cheerio = require('cheerio');
request.get(url, function(e, r, body){
	$ = cheerio.load(body);
    var title = $('h1.title').text().trim();
});
```
单纯从网页抓取数据而言，cheerio已经足够了，不仅快而且占用低。虽然某些情况下还是得使用jsdom，但到那个时候再用回jsdom也不迟。

#### 参考资料 ####

1. [jsdom closed issue - Massive Memory Leaks, 100 loops](https://github.com/tmpvar/jsdom/issues/466)
2. [JSDOM Memory leaks](http://lukeberndt.com/2011/jsdom-memory-leaks/)
3. [jsdom and node.js leaking memory](http://stackoverflow.com/questions/13893163/jsdom-and-node-js-leaking-memory)
4. [Memory leak in Node.js scraper](http://stackoverflow.com/questions/5718391/memory-leak-in-node-js-scraper)