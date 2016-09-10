# Flash捕获实时视频

使用Flash Media Live Encoder ，可以非常方便的录制实时视频(live video)。Flash Media Live Encoder 的选项很多而且很细，品质控制也很不错。如何在AS3中实现这些？

参考FMS的Help，我简单的封装了这样一个类：
```
package com.swordair.as3
{	
	/**
	 * 
	 * @author iifksp
	 * 
	 */
	import flash.events.NetStatusEvent;
	import flash.media.Camera;
	import flash.media.Microphone;
	import flash.net.NetConnection;
	import flash.net.NetStream;

	public class LiveVideoCapturer
	{
		private var nc:NetConnection; 
		private var ns:NetStream; 
		private var cam:Camera; 
		private var mic:Microphone;

		public function LiveVideoCapturer(address:String = null)
		{
			nc = new NetConnection(); 
			nc.addEventListener(NetStatusEvent.NET_STATUS, onNetStatus); 
			nc.connect(address);
			nc.client = this;
		}
		public function onBWDone():void{
		}
		private function onNetStatus(event:NetStatusEvent):void{ 
			trace(event.info.code); 
				if(event.info.code == "NetConnection.Connect.Success"){ 
					publishCamera();
			}
		}
		private function publishCamera():void{ 
			cam = Camera.getCamera(); 
			cam.setMode(640,480,20);
			cam.setQuality(50000,90);
			mic = Microphone.getMicrophone(); 
			ns = new NetStream(nc); 
			ns.attachCamera(cam); 
			ns.attachAudio(mic); 
			ns.publish("swordair", "live");
		}
	}
}
```
这个类简单的获取摄像头和麦克风，然后调用`NetStream`对象`ns`的`attachCamera`和`attachAudio`将其附加，最后通过对象`ns`的`publish()`方法发布到Flash Media Server。

FMS会回调`ns`的`client`对象的`onBWDone()`方法，所以指定`client`并添加`onBWDone()`。这里将`client`指向类本身然后添加了一个public的`onBWDone()`方法供FMS调用。如果缺失`onBWDone()`方法，可能得到这样的错误：
```
Error #2044: 未处理的 AsyncErrorEvent:。 text=Error #2095: flash.net.NetConnection 无法调用回调 onBWDone。 error=ReferenceError: Error #1069: 在 flash.net.NetConnection 上找不到属性 onBWDone，且没有默认值。
```
整个过程就是：获取资源 - 建立连接 - 发布；`ns.publish()`的两个参数分别是流的标识字符串和发布方式，发布方式可以使live(默认，不录制)、record(录制，覆盖同名)和append(录制，附加同名)。

如果要回放录制的视频，对于flash的`Video`而言，使用相同的连接`nc`创建一个`NetStream`对象，调用其`play()`方法，最后使用`Video`对象`attachNetStream()`播放该流。
```
var nsPlayer = new NetStream(nc); 
nsPlayer.play("swordair"); 
var vidPlayer = new Video(); 
vidPlayer.attachNetStream(nsPlayer); 
addChild(vidPlayer); 
```
对于Flex的`VideoDisplay`，可以简单的设置rtmp地址：
```
<mx:VideoDisplay 
	id="vd1"
	source="rtmp://localhost/live/swordair" 
	width="100%" 
	height="100%" 
	autoBandWidthDetection="false" 
	live="true" 
	maintainAspectRatio="true"/>
```
Flex4的`VideoPlayer`相当出色，可惜至少在我测试的时候(FlashBuilder4 beta 2 SDK Build 4.0.0.10485)，出现了严重的延迟问题。回放实时流的延迟甚至超过15秒钟，这简直无法忍受。最终放弃`VideoPlayer`而改用`mx:VideoDisplay`，延迟在2-3秒。也可能只是我的设置问题，但是`VideoPlayer`的相关参考还相对较少，希望Flex4正式可以解决这些可能存在的问题。

#### 参考资料
[Adobe Flash Media Server 3.5 - Capturing live video](http://help.adobe.com/en_US/FlashMediaServer/3.5_Deving/WS5b3ccc516d4fbf351e63e3d11a0773d56e-7ff0.html)