# 再谈box-shadow

早先，我详细地写过2篇关于 `box-shadow` 的文章: [CSS3 box-shadow 详解(1)](/details-on-css3-box-shadow-part-1/) 以及 [CSS3 box-shadow 详解(2)](/details-on-css3-box-shadow-part-2/)。但用现在的眼光来看，内容已经有些旧了。趁着这片闲言碎语，对 `box-shadow` 再做一些补充。

`box-shadow` 是我最喜欢的CSS3特性之一，它能创建凸显层次的阴影，但却还可以充当很多角色：边框/层叠/渐变/勾线，等等。当前浏览器的支持程度已经非常理想，除了IE8及以下，所有浏览器都已经更新到无前缀的支持——这跟一年前的情况很不同，那时，一些浏览器如firefox实现的 `box-shadow` 还存在占用空间的bug，Webkit类的模糊算法非常僵硬，随着这一年的发展这一些问题都已被克服。如今存在的问题只有两个，**部分移动版浏览器的不支持**，以及**Opera在多阴影高模糊值时的效率低下**。但相信这些问题也都能尽早解决。

之前虽然详细讲了 `box-shadow` 的方方面面，但唯独对IE模拟用其他引用链接一笔带过，现在做一个小补充。众所周知，IE低版本实现 `box-shadow` 是用滤镜的，这并不是推荐的做法：
```
.box-shadow{
	-moz-box-shadow: 3px 3px 4px #000;
	-webkit-box-shadow: 3px 3px 4px #000;
	box-shadow: 3px 3px 4px #000;
	background:#fff; /* for IE filter, required */
	-ms-filter: "progid:DXImageTransform.Microsoft.Shadow(Strength=4, Direction=135, Color='#000000')"; /* For IE 8 */
	filter: progid:DXImageTransform.Microsoft.Shadow(Strength=4, Direction=135, Color='#000000'); /* For IE 5.5 - 7 */
}
```
现在这种代码已经显得太碍眼了，对于IE低版本我更倾向于放任不管：
```
.box-shadow{box-shadow: 3px 3px 4px #000;}
```
当然，总有需求需要IE8-有阴影，如果身在需求主导的环境下，借滤镜一用也算是无奈之举。使用滤镜的话，给对应box一个背景颜色是必需的，否则你会在box内部以及字体上同时看到丑陋的阴影效果。