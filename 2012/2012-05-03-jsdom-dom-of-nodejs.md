# jsdom——node.js的DOM

最近一周一直在写node，感觉很爽。不过 node 虽好却没有 DOM 还是有很多不方便的地方。好在有 [jsdom](https://github.com/tmpvar/jsdom) —— 一个 W3C DOM 的 JS 实现。用这玩意相当犀利，它不仅可以将文档解析成 DOM，而且，你还可以用 YUI 或着 jQuery 去操作生成的 DOM。这在从页面中提取数据时格外有用。

虽然在类Unix系统上安装jsdom非常简单，但在window上就要麻烦许多，下面这些依赖还得独立安装。

- ~~[node-gyp](https://github.com/TooTallNate/node-gyp)~~ (nodejs 0.6.13 以上，node-gyp 已经被包含在 npm 中)
- [Python](http://www.python.org/ftp/python/2.7.3/python-2.7.3.msi)
- [Microsoft Visual C++ Express 2010](http://www.microsoft.com/visualstudio/en-us/products/2010-editions/visual-cpp-express)

装完后我赶紧使用了一下：

```
var jsdom = require('jsdom');
jsdom.env('http://www.swordair.com',['http://localhost/aptana/code/js-lib/jquery-1.7.1.js'],function(errors, window){
	console.log("contents:", window.$("div.blog-update-title").next().html());
});
```

效果妥妥的，**无法想象的便利和快捷**！

----

下面是我走的很多弯路，但作为错误信息源保留下来，以便有需要的人能够搜索到并解决他们的问题。

一开始，我就按照 [jsdom](https://github.com/tmpvar/jsdom) 的 README 直接安装：
```
npm install jsdom
```
当然，那肯定是不行的。我的系统是xp并且几乎没有安装过什么常用开发工具。结果报错，信息如下：

出现如下错误信息：
```
D:\xampp\htdocs\aptana\code\js\nodejs\playground\node_modules\jsdom\node_m
odules\contextify>node "C:\Program Files\nodejs\node_modules\npm\bin\node-gyp-bi
n\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild
info it worked if it ends with ok
ERR! Error: Python does not seem to be installed
    at failNoPython (C:\Program Files\nodejs\node_modules\npm\node_modules\node-
gyp\lib\configure.js:78:14)
    at Object.oncomplete (C:\Program Files\nodejs\node_modules\npm\node_modules\
node-gyp\lib\configure.js:66:11)
ERR! not ok
npm WARN optional dependency failed, continuing contextify@0.1.2
jsdom@0.2.14 ./node_modules/jsdom
├── cssom@0.2.3
├── request@2.9.202
└── htmlparser@1.7.6
```
根据 ERR! Error: Python does not seem to be installed，应该是没有安装Python的缘故。赶紧到官方网站下了Python，2.x.x和3.x.x都装了(后来知道其实用的是Python27)。

在搜索过程中，了解到可能还需要安装 node-gyp，所以顺便把 node-gyp 也一并装上：
```
npm install -g node-gyp
```

继续 `npm install jsdom` 安装 jsdom，不过仍然报错：

```
D:\xampp\htdocs\aptana\code\js\nodejs\playground\node_modules\jsdom\node_m
odules\contextify>node "C:\Program Files\nodejs\node_modules\npm\bin\node-gyp-bi
n\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild
info it worked if it ends with ok
info downloading: http://nodejs.org/dist/v0.6.15/node-v0.6.15.tar.gz
info downloading: http://nodejs.org/dist/v0.6.15/node.lib
spawn C:\Python27\python.exe [ 'd:\\Documents and Settings\\jianwang2\\.node-gyp
\\0.6.15\\tools\\gyp_addon',
  'binding.gyp',
  '-ID:\\xampp\\htdocs\\aptana\\code\\js\\nodejs\\playground\\node_modules
\\jsdom\\node_modules\\contextify\\build\\config.gypi',
  '-f',
  'msvs',
  '-G',
  'msvs_version=2010' ]
ERR! Error: Can't find "msbuild.exe". Do you have Microsoft Visual Studio C++ 20
10 installed?
    at Object.oncomplete (C:\Program Files\nodejs\node_modules\npm\node_modules\
node-gyp\lib\build.js:105:20)
ERR! not ok
npm WARN optional dependency failed, continuing contextify@0.1.2
jsdom@0.2.14 ./node_modules/jsdom
├── cssom@0.2.3
├── request@2.9.202
└── htmlparser@1.7.6
```
错误信息 ERR! Error: Can't find "msbuild.exe". Do you have Microsoft Visual Studio C++ 2010 installed? 显示无法找到编译器 msbuild.exe，并询问我是否安装有 Visual Studio C++ 2010。我擦，当然没有！

这里我偷懒了一下(后来证明是个错误)，我知道 .Net 4.0 里包含有 msbuild.exe，所以直接到微软官方下载了 .Net 4.0。安装，运行 `npm install jsdom`，还是不行：
```
D:\xampp\htdocs\aptana\code\js\nodejs\playground\node_modules\jsdom\node_m
odules\contextify>node "C:\Program Files\nodejs\node_modules\npm\bin\node-gyp-bi
n\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild
info it worked if it ends with ok
spawn C:\Python27\python.exe [ 'd:\\Documents and Settings\\jianwang2\\.node-gyp
\\0.6.15\\tools\\gyp_addon',
  'binding.gyp',
  '-ID:\\xampp\\htdocs\\aptana\\code\\js\\nodejs\\playground\\node_modules
\\jsdom\\node_modules\\contextify\\build\\config.gypi',
  '-f',
  'msvs',
  '-G',
  'msvs_version=2010' ]
spawn C:\WINDOWS\Microsoft.NET\Framework\v4.0.30319\msbuild.exe [ 'build/binding
.sln',
  '/clp:Verbosity=minimal',
  '/nologo',
  '/p:Configuration=Release;Platform=Win32' ]
D:\xampp\htdocs\aptana\code\js\nodejs\playground\node_modules\jsdom\node_
modules\contextify\build\contextify.vcxproj(1,685): error MSB4019: The imported
 project "D:\Microsoft.Cpp.Default.props" was not found. Confirm that the path
in the <Import> declaration is correct, and that the file exists on disk.
ERR! Error: `C:\WINDOWS\Microsoft.NET\Framework\v4.0.30319\msbuild.exe` failed w
ith exit code: 1
    at Array.0 (C:\Program Files\nodejs\node_modules\npm\node_modules\node-gyp\l
ib\build.js:176:25)
    at EventEmitter._tickCallback (node.js:192:40)
ERR! not ok
npm WARN optional dependency failed, continuing contextify@0.1.2
jsdom@0.2.14 ./node_modules/jsdom
├── cssom@0.2.3
├── request@2.9.202
└── htmlparser@1.7.6
```
信息显示 msbuild.exe 被找到，不过编译失败。这次是报

```
error MSB4019: The imported project "D:\Microsoft.Cpp.Default.props" was not found. Confirm that the path in the <Import> declaration is correct, and that the file exists on disk.
```

好吧，看来还是得安上 Microsoft Visual Studio C++ 2010。到官网下了 [Microsoft Visual C++ Express 2010](http://www.microsoft.com/visualstudio/en-us/products/2010-editions/visual-cpp-express)，一阵漫长的等待后，再次运行安装jsdom，终于成功：

```
D:\xampp\htdocs\aptana\code\js\nodejs\playground\node_modules\jsdom\node_m
odules\contextify>node "C:\Program Files\nodejs\node_modules\npm\bin\node-gyp-bi
n\\..\..\node_modules\node-gyp\bin\node-gyp.js" rebuild
info it worked if it ends with ok
spawn C:\Python27\python.exe [ 'd:\\Documents and Settings\\jianwang2\\.node-gyp
\\0.6.15\\tools\\gyp_addon',
  'binding.gyp',
  '-ID:\\xampp\\htdocs\\aptana\\code\\js\\nodejs\\playground\\node_modules
\\jsdom\\node_modules\\contextify\\build\\config.gypi',
  '-f',
  'msvs',
  '-G',
  'msvs_version=2010' ]
spawn C:\WINDOWS\Microsoft.NET\Framework\v4.0.30319\msbuild.exe [ 'build/binding
.sln',
  '/clp:Verbosity=minimal',
  '/nologo',
  '/p:Configuration=Release;Platform=Win32' ]
  contextify.cc
d:\Documents and Settings\jianwang2\.node-gyp\0.6.15\src\node_object_wrap.h(57)
: warning C4251: 'node::ObjectWrap::handle_' : class 'v8::Persistent<T>' needs
to have dll-interface to be used by clients of class 'node::ObjectWrap' [D:\xam
pp\htdocs\aptana\code\js\nodejs\playground\node_modules\jsdom\node_module
s\contextify\build\contextify.vcxproj]
          with
          [
              T=v8::Object
          ]
d:\Program Files\Microsoft Visual Studio 10.0\VC\include\xlocale(323): warning
C4530: C++ exception handler used, but unwind semantics are not enabled. Specif
y /EHsc [D:\xampp\htdocs\aptana\code\js\nodejs\playground\node_modules\js
dom\node_modules\contextify\build\contextify.vcxproj]
C:\Program Files\MSBuild\Microsoft.Cpp\v4.0\Microsoft.CppBuild.targets(990,5):
warning MSB8012: TargetPath(D:\xampp\htdocs\aptana\code\js\nodejs\playgro
und\node_modules\jsdom\node_modules\contextify\build\Release\contextify.dll) do
es not match the Linker's OutputFile property value (D:\xampp\htdocs\aptana\swo
rd-code\js\nodejs\playground\node_modules\jsdom\node_modules\contextify\build\R
elease\contextify.node). This may cause your project to build incorrectly. To c
orrect this, please make sure that $(OutDir), $(TargetName) and $(TargetExt) pr
operty values match the value specified in %(Link.OutputFile). [D:\xampp\htdocs
\aptana\code\js\nodejs\playground\node_modules\jsdom\node_modules\context
ify\build\contextify.vcxproj]
C:\Program Files\MSBuild\Microsoft.Cpp\v4.0\Microsoft.CppBuild.targets(991,5):
warning MSB8012: TargetExt(.dll) does not match the Linker's OutputFile propert
y value (.node). This may cause your project to build incorrectly. To correct t
his, please make sure that $(OutDir), $(TargetName) and $(TargetExt) property v
alues match the value specified in %(Link.OutputFile). [D:\xampp\htdocs\aptana\
code\js\nodejs\playground\node_modules\jsdom\node_modules\contextify\buil
d\contextify.vcxproj]
     Creating library D:\xampp\htdocs\aptana\code\js\nodejs\playground\no
  de_modules\jsdom\node_modules\contextify\build\Release\contextify.lib and obj
  ect D:\xampp\htdocs\aptana\code\js\nodejs\playground\node_modules\jsdom
  \node_modules\contextify\build\Release\contextify.exp
  Generating code
  Finished generating code
  contextify.vcxproj -> D:\xampp\htdocs\aptana\code\js\nodejs\playground\
  node_modules\jsdom\node_modules\contextify\build\Release\contextify.dll
info done ok
jsdom@0.2.14 ./node_modules/jsdom
├── cssom@0.2.3
├── request@2.9.202
├── htmlparser@1.7.6
└── contextify@0.1.2 (bindings@0.3.0)
```

虽然有几个警告，不过看到 info done ok，说明安装完成了。至此绕了一个大圈终于装完 jsdom。其实也是后来才发现的，自己没仔细看 node-gyp 的说明，人家写的清清楚楚的：

- Python (v2.7.2 recommended, v3.x.x not yet supported)
- Microsoft Visual C++ (Express version works well)

所以以后万不可盲目，说明先看看清楚，走别人的弯路，让别人无路可走～