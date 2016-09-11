# 翻译更新《HTML5与HTML4的差异》

自从4年前因为接下论坛任务开始，《HTML5与HTML4的差异》这份文档的翻译工作就一直视之是份内之事。在之前的2013年5月28日的版本之后，这份文档又更新了2次，限于精力有限，我只能跳过2014年9月18日的那个版本，直接重制了最新的2014年12月9日的版本，这也是W3C HTML5规范敲定后这份文档第一次的更新。

文档链接保持不变：**http://swordair.com/docs/html5-diff/**

并将内容维护放置在了Github：
https://github.com/iifksp/html5-diff

由于W3C HTML5规范已经在2014年敲定，所以这份工作笔记就开始往既定的规范靠拢，内容比起之前的有了大幅缩减。由于改动实在太大，我只能对内容做了逐行的比对，将主要变化记录，内容很多，相信难免有遗漏或是错误，欢迎吐槽。

#### 主要改动 ####

- 标题又加回了HTML5字样
- 大部分链接换为 `http://www.w3.org/TR/html5/`
- 文档删除了很多背景的描述性说明
- 移除了所有的更新日志
- 文档涵盖的范围有变化，只涵盖HTML5规范，从而移除了所有W3C HTML5， W3C HTML 5.1以及WHATWG HTML的不同点描述部分
- 移除了发展模式章节

#### 内容增减 ####

- 移除了仅包含在W3C HTML 5.1或WHATWG HTML中的新元素或新属性。
- 移除元素hgroup，data，dialog，menuitem，details
- 移除input的type值：datetime，month，week，datetime-local
- 新增元素 template，可被用于声明HTML片段，其会被脚本复制以及插入到文档中
- 新增描述：placeholder 属性不该被用作 label 元素的替代品
- 新增：textarea新属性minlength
- 新增：input新属性minlength
- 移除：style属性scoped
- 移除：menu 元素新属性 type 和 label
- 移除：iframe 新属性 seamless
- 移除：全局属性contextmenu
- 移除：draggable以及dropzone属性
- 变更描述： blockquote
- 移除：menu元素
- 新增描述：img的alt
- 变更：link 的 media 属性现在接受一个媒体查询**列表**并且默认为"all"。
- 废弃属性中移除了img的longdesc属性
- 废弃form的accept属性
- 废弃input的usemap属性
- 移除了commands接口
- 移除了配合 draggable 属性使用的拖放 API
- 移除了提示用的API showModalDialog()
- 添加了Navigator和External接口

#### 其他 ####
还有一些一些语言描述的变化，和之前的几次更新相似，主要是语气变得更加强烈，比如：section 与 h1,h2等标签共同使用，描述由“可以”，变为“**应该**”。

这个版本里，**我移除了译者注释**，如果有需要，请查阅旧版本。之前两个旧版本以及其翻译的日志：

- [与HTML4 的差异 - W3C 工作草案 2013年5月28日](http://swordair.com/docs/html5-diff/20130528/) | [重译《与HTML4的差异》](http://swordair.com/differences-from-html4-retranslation/)
- [HTML5 相对于 HTML4 的差异 - W3C 工作草案 2011年5月25日](http://swordair.com/docs/html5-diff/20110525/) | [HTML5 Differences from HTML4 译文](http://swordair.com/html5-differences-from-html4-translation/)

