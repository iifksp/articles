# 单独代理(Lone surrogates)

之前翻译《[与HTML4 的差异](https://www.swordair.com/docs/html5-diff/)》时遇到的名词，因为当前关于这个词的参考资料还较少，所以就写一点整理。

对unicode熟悉的人应该对代理(surrogates)都不陌生，代理区指的就是在基本多语言平面(Basic Multilingual Plane, BMP)中保留的2048个码位(或称为码点，code point)，范围0xD800-0xDFFF，这段空出来的码位被用来代理UTF16的其余16个辅助平面的字符映射。具体可以参见文后的参考资料，特别是北大中文论坛的一篇讨论非常值得一读。

其实我是从来没有想过代理会单独出现，因为按照标准代理必须是成对的，所以自然也没有去想过，如果代理真的单独出现会怎么样，而这反应出的是自己思维的局限。单独代理，在不同的程序里结果也不同，限于能找到的资料有限，也不能武端地得出什么明确的结论。实际上自己还花了一些时间翻阅了unicode的定义，不过对于阐述正确的常态情况的标准而言，要找到异常情况的处理，只能寄希望于标准编者大发慈悲的注释。果然最后没有结果，倒是对unicode本身的理解有了意外收获...囧。

嘛，其实和前端看似有关实则也基本不沾边的，也就是标准2012年3至9月间的这个变更了：

> 在WebSocket send() 中，单独代理(Lone surrogates) 被转换为 U+FFFD而不是抛出异常


### 参考资料

- [UTF-8, UTF-16, UTF-32 & BOM FAQ](http://www.unicode.org/faq/utf_bom.html) from The Unicode Consortium
- [Unicode 6.2.0 - index](http://www.unicode.org/versions/Unicode6.2.0/) from The Unicode Consortium
- [Unicode 6.2.0 - General Structure](http://www.unicode.org/versions/Unicode6.2.0/ch02.pdf) from The Unicode Consortium
- [Unicode 6.2.0 - Special Areas and Format Characters](http://www.unicode.org/versions/Unicode6.2.0/ch16.pdf) from The Unicode Consortium
- [UTF-16](http://zh.wikipedia.org/wiki/UCS-2) from wikipedia
- [UTF-16 and UTF-32 codecs should reject (lone) surrogates](http://mail.python.org/pipermail/python-bugs-list/2011-September/147361.html) from python mail list
- [关于Surrogate的讨论](http://www.pkucn.com/viewthread.php?action=printable&tid=263113) from 北大中文论坛 www.pkucn.com