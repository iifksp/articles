# PureMVC AS3 源码分析

PureMVC不仅仅是AS的经典MVC模式的框架，同时也有其他语言的实现，比如JS，php，C#。我接触PureMVC也只是这半年里的事，伴随着一个项目应用了这个边学边用的AS3框架。PureMVC很容易使用，那时我参考的是Cliff Hall的Best Practices，例子讲述了一个使用PureMVC的登陆过程。非常详细，而且很多注意点提示地都非常深刻。

![puremvc_architecture](http:swordair.com/content/images/2013/Dec/puremvc_architecture.jpg)

PureMVC不是基于flash本身的事件机制，而是采用了一种发送通知的观察者机制。这里主要分析这种通知的机制。

首先是INotifier.as
```
public interface INotifier
{
	function sendNotification( notificationName:String, body:Object=null, type:String=null ):void; 
}
```
这个类只有一个`sendNotification()`方法。我们知道，PureMVC里全部的模块(command, mediator, proxy, facade)都可以发送通知，所以它们都实现了INotifier接口。所不同的是proxy不接受任何的通知，它只是送出通知。所以对于proxy来说，它不注册监视通知并作出动作的observer。

Observer类直接实现了IObserver接口，后者只有简单的4个方法定义。Observer则在这4个方法的基础上增加了对应类成员的get方法。对于Observer类来说，最重要的是`notifyObserver()`方法。
```
public function notifyObserver( notification:INotification ):void
{
	this.getNotifyMethod().apply(this.getNotifyContext(),[notification]);
}
```
这个方法将调用对应于某个对象的方法来接受并处理通知。
对于mediator来说，就是调用其自身的`handleNotification()`方法。这点可以从View.as中看到。
```
// Create Observer referencing this mediator's handlNotification method
var observer:Observer = new Observer( mediator.handleNotification, mediator );
```
而对于command来说，就是调用Controller的`executeCommand()`方法，这个方法实例化对应的command然后调用其`execute()`去具体执行一个通知行为。
在controller.as中，`registerCommand()`里调用的是view的`registerObserver()`。
```
public function registerCommand( notificationName : String, commandClassRef : Class ) : void
{
	if ( commandMap[ notificationName ] == null ) {
		view.registerObserver( notificationName, new Observer( executeCommand, this ) );
	}
	commandMap[ notificationName ] = commandClassRef;
}
```
当observer通知执行某个command时，首先调用的就是controller的`executeCommand()`，随后这个方法里，依据commandMap的映射关系，找出对应于该通知的command，然后实例化，调用`execute()`执行。
```
public function executeCommand( note : INotification ) : void
{
	var commandClassRef : Class = commandMap[ note.getName() ];
	if ( commandClassRef == null ) return;

	var commandInstance : ICommand = new commandClassRef();
	commandInstance.execute( note );
}
```
那么`notifyObserver()`是由谁调用的？作为整个observer的核心调度方法，View类的`notifyObservers()`将通知派发到全部相关的observer那里。
```
public function notifyObservers( notification:INotification ) : void
{
	if( observerMap[ notification.getName() ] != null ) {
		
		// Get a reference to the observers list for this notification name
		var observers_ref:Array = observerMap[ notification.getName() ] as Array;

		// Copy observers from reference array to working array, 
		// since the reference array may change during the notification loop
		var observers:Array = new Array(); 
		var observer:IObserver;
		for (var i:Number = 0; i < observers_ref.length; i++) { 
			observer = observers_ref[ i ] as IObserver;
			observers.push( observer );
		}
		
		// Notify Observers from the working array				
		for (i = 0; i < observers.length; i++) {
			observer = observers[ i ] as IObserver;
			observer.notifyObserver( notification );
		}
	}
}
```
首先，获取对应某个通知的observer的列表引用。这个observerMap是在注册observer时被赋值的一个通知对应observer数组的数组。当用通知名索引这个observerMap时得到的就是一个相关observer的数组。
然后拷贝这个相关observer数组，因为持有这个数组的引用是不够的，这个数组可能会在整个通知过程中被更改。
最后，对每个observer调用自身的`notifyObserver()`方法。这样原先注册过的这个通知的方法就会开始执行。到这里，整个观察者机制就已经差不多了。

回过头来看Facade，真是应有尽有！不仅持有全部核心类的引用，所有添加、移除、获取方法一应俱全~所以Facade无所不能，在整个PureMVC里起到一个核心作用。它调用相应的model、controller、view的方法，而这几个核心类，各自管理着自己的那部分模块。

PureMVC AS3很简练，很轻量，10个接口，11个类，代码简洁而优雅，虽然在应用上可能是有局限性的(有人说它高不成低不就)，但它绝对是一个优秀的框架