# mx:WebService导致的浏览器崩溃

这算是回头去记录一些以前遇到的问题。

在一个视频监控的项目里，忽然在调试的时候出现浏览器常常崩溃的情况。而且一旦崩溃后就调试不能。但是项目编译确没有问题，swf文件照常生成，打开相应网页也能在浏览器中正常工作，唯独不能调试。不论使用IE还是FF都出现同样的问题。

多方查看后确定造成这种情况的原因竟然是一段webservice的mxml的代码。这段代码非常普通，以前也有用，但偏偏这次出现这种怪现象。代码大体如下：

```
<mx:WebService id="webService" wsdl="http://localhost/webservice.asmx?wsdl">
	<mx:operation name="GetInfo"
                resultFormat="object"
                result="GetInfo_result(event);"
                fault="GetInfo_fault(event);" />
</mx:WebService>
```

更糟糕的是无论怎么修改这段代码哪怕精简的到只剩下

```
<mx:WebService id="webService"></mx:WebService>
```

浏览器继续崩溃的噩梦。

然而，一旦整个移除这段代码后，程序就恢复如初，可以调试，运行起来也一切正常。最后我仍没有找到问题的蛛丝马迹。难道还是自己犯下的某个低级错误？更可笑的是在加入这段看似正常的代码后，如果偶然可以正常调试一次，但是第二次浏览器必然继续崩溃。之后如果重启系统也许还能正常调试一次，第二次继续崩溃...

最后，经过半天的奋斗后，无奈之下我只好删除那段代码，然后导入`mx.rpc.soap.WebService`包，用AS代码去调webservice。

```
var service:WebService = new WebService();
service.loadWSDL("http://localhost/webservice.asmx?wsdl");
service.addEventListener(FaultEvent.FAULT, serviceFaultHandler);
service.GetInfo.addEventListener(ResultEvent.RESULT, serviceResultHandler);
service.GetInfo();
```

如此才一切正常。奇怪啊...仅此记之。