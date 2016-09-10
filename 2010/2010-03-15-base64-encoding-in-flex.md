# Flex的Base64编码

前段时间碰到关于Flex的Base64编码的问题，今天打算写个小节。

### dynamicflash编码类

最早认识的AS3编码类是 `com.dynamicflash.util.Base64` ，这个类可以从 http://dynamicflash.com/goodies/base64/ 下载到。

当前的直接下载链接是 [Base64-1.1.0.zip](http://dynamicflash.com/downloads/base64/Base64-1.1.0.zip)。

查看源代码，这是一个非常简约的类，111行代码整洁而优雅。具体使用也非常简单，将build里的 as3base64.swc 放到 libs 目录里将其导入到工程，直接调用其静态方法后就返回了想要的结果。

#### 从字符串编码和解码
```
import com.dynamicflash.util.Base64;
 
var source:String = "Hello, world!";
var encoded:String = Base64.encode(source);
trace(encoded)
 
var decoded:String = Base64.decode(encoded);
trace(decoded);
```
#### 从二进制数据编码和解码
```
import com.dynamicflash.util.Base64;
 
var obj:Object = {name:"Dynamic Flash", url:"http://dynamicflash.com"};
var source:ByteArray = new ByteArray();
source.writeObject(obj);
var encoded:String = Base64.encodeByteArray(source);
trace(encoded);
 
var decoded:ByteArray = Base64.decodeToByteArray(encoded);
var obj2:Object = decoded.readObject();
trace(obj2.name + "(" + obj2.url + ")");
```

### Flex内置工具类

然而 Flex3 本身就已经提供了 base64 编码和解码的工具类。在 `mx.util` 包里，分别是 `Base64Encoder` 和 `Base64Decoder` 。前者将字符串或 ByteArray 编码为 Base64 编码的字符串，后者则用于将 Base64 编码的字符串解码为 ByteArray。

Flex Examples 上有一个 [关于如何将图像编码成Base64字符串](http://blog.flexexamples.com/2008/03/17/displaying-an-image-saved-as-a-base64-encoded-string-in-flex-3/) 的例子，这个例子很好的说明了 Base64Decoder 的用法。

与 `com.dynamicflash.util.Base64` 相比，`Base64Encoder` 和 `Base64Decoder` 的编码和解码并非静态方法，在构造对象后调用 `encode()` 或者 `decode()` ，结果会被添加到类内部缓冲区，直到调用 `toString()` 或者 `toByteArray()` 将结果返回。所以两者都提供了清除缓冲区以及设置初始化状态的方法 `reset()` 。

### 其他类的Base64

还有一个 `mx.graphics.ImageSnapshot` 类，用于捕获实现了 `flash.display.IBitmapDrawable` 的任何 Flash 组件（包括 Flex UIComponent）的快照，这个类也有一个Base64编码的方法，用于将 ImageSnapshot 转换为 Base-64 编码的String。
```
import mx.graphics.ImageSnapshot;
import mx.graphics.codec.*;

var jpgEnc:JPEGEncoder = new JPEGEncoder();
var ohSnap:ImageSnapshot;
ohSnap = ImageSnapshot.captureImage(img, 0, jpgEnc);
var str:String = ImageSnapshot.encodeImageAsBase64(ohSnap);
```