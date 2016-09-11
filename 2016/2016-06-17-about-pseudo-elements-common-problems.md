# 浅谈伪元素的常见问题

在之前一篇[一个IE8的新起点](https://swordair.com/new-beginning-of-ie8/)中，我简单列举了一些当前放开枷锁的CSS，其中之一就是伪元素。当然，在使用伪元素的时候依旧会遇到一些问题，我总结了一些，简单陈述，留个印象，那么当再次遇到的时候能够有所回想。

使用伪元素主要遇到的问题集中在IE8，因为IE8只是部分支持伪元素。首先，**IE8中设置伪元素的z-index可能无效**。这点广为人知，在使用伪元素制作冒泡箭头的时候会遇到，无法通过调节z-index使before置于after之上。

另一个较为常见的问题是，**无法以类名触发伪元素content的重绘**。如果使用过字体图标那么多半遇到过这个问题。因为大部分字体图标是通过伪元素content插入字符而成，比如在一个当前激活的导航里项上添加.active时，看到图标依旧保持着原来的颜色。其实这个不只是IE8会遇到，移动版Safari同样存在这个问题(偶发)，明明更新了类名，外观却不发生重绘。

另外，**使用:hover:after以及:hover:before之前必须申明:hover状态**也算是一个IE8中的使用事项。

IE8后续的一些版本同样存在一些关于伪元素的bug，比如，**rem单位无法作用于伪元素的行高**。这个影响不算很大，最多看到一些行距错位，百思不得其解最后发现原来是rem和伪元素搞的鬼:)

当然Firefox也无法幸免，即使是最新版本的Firefox中，**伪元素:before绝对定位出现在浮动的子元素之后**，这个行为有点不太符合预期。因为尚且无法找到对应的标准描述，我只是觉得IE和Chrome的做法更符合我的预期罢了。

另外一提，**chrome的早期版本(<25)，伪元素不支持动画过渡**。当然这个点已经是一个过去式了。最后，**filter滤镜无效**。

都是凭借记忆写的个人谈，可能存在些微纰漏不要怪我:)

随着新版本浏览器不断的更新，相信伪元素的问题终有一天可以到忽略不计的程度。而且即便是现在，我们也是很欢乐地在使用着。毕竟，不是人人像我这么倒霉碰上这么多杂七杂八的问题嘛～