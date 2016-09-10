# Flex的mx_internal命名空间

如果查看Flex的框架源码，就经常会看到有些属性和方法被加上了 mx\_internal 前缀。特别是调试的时候。于是就自然要去查找 mx\_internal 是什么。Adobe Developer Connection 上，有篇 [什么是mx\_internal](http://cookbooks.adobe.com/post\_What\_is\_mx\_internal\_-12212.html) 的文章，大体上说，mx\_internal 是一个命名空间，这个命名空间被Flex框架用来划分那些在将来的SDK发布中可能会做更改的方法和属性。

比如下面的代码
```
<?xml version="1.0" encoding="utf-8"?>
<mx:Application 
	xmlns:mx="http://www.adobe.com/2006/mxml" 
	layout="vertical" 
	creationComplete="init()">

	<mx:Script>
		<![CDATA[
			import mx.controls.Alert;
			import mx.controls.Text;
			
			import mx.core.mx_internal;
			use namespace mx_internal;
			
			private function init():void
			{
				Alert.show(String(txt.htmlTextChanged));
			}        
		]]>
	</mx:Script>
    
	<mx:Text id="txt"/>
</mx:Application>
```
其中，如果去掉这两句
```
import mx.core.mx_internal;
use namespace mx_internal;
```
那么编译的时候会直接报错： 1178: 试图访问不可访问的属性 `htmlTextChanged` (通过 static 类型 `mx.controls:Text` 引用)。
所以在不使用mx\_internal命名空间的情况下，不能访问到那些被隐藏起来的“不确定”的方法和属性。一旦使用这个命名空间，就可以像public那样直接访问到这些危险的但可能是非常有用的类成员。

上面的例子里，Text的`htmlTextChanged`就是一个在mx\_internal命名空间里的属性。它的定义在Text的父类Label里：
```
/**
 *  @private
 */
mx_internal var htmlTextChanged:Boolean = false;
```
因为未来版本的SDK可能会对这些方法和属性做出修改，所以只有在不得已的情况下，才会考虑去使用它们。使用它们的代价很可能是SDK升级时的不兼容，或者代码的大量修正，也意味着程序的升级或者改版被限制了。