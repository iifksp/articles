# nodejs目录遍历

近期解决一个问题的时候，需要一个简单的目录遍历。目录遍历挺常见，操作一个文件夹里的所有文件，替换或者添加删除某些东西是非常普遍的操作。由于 nodejs 本身并没有提供类似的API，所以这部分就得由自己实现。

虽然没有直接的遍历API，但是 nodejs 的文件操作也已经非常便利，用 `fs.readdir` 和 `fs.stat` 这两个API的组合就能达到目的。

出于参半程序员的懒惰的劣根性，其实在这之前我也搜索过看看是否有现成的可以拿来用，也确实有这种完善的 module，比如 [node-walk](https://github.com/coolaj86/node-walk)，我试用过后觉得还是非常不错的：

```
var walk = require('walk'),
	files = [];

function getFileList(){
	var walker  = walk.walk('/dirName', { followLinks: false });

	walker.on('file', function(root, stat, next) {
	    files.push(root + '/' + stat.name);
	    next();
	});
	
	walker.on('end', function() {
	    console.log(files);
	});
	
	console.log(files)
}
```


但是我使用其同步方法的时候并未达到同步的效果，不知是我哪里用法有问题。后来因为寻找原因的时间太长也就只好先放弃。其实我只是需要一个简单的同步遍历，所以用module就显得复杂化了。当然搜索过程中也找到了几个简单实现，但代码搞的复杂了，明明递归能轻易做到的情况下却用了数组循环...

所以最后吧，还是得自己写：

```
var fs = require('fs'),
	fileList = [];
	
function walk(path){
	var dirList = fs.readdirSync(path);
	dirList.forEach(function(item){
		if(fs.statSync(path + '/' + item).isDirectory()){
			walk(path + '/' + item);
		}else{
			fileList.push(path + '/' + item);
		}
	});
}

walk('/dirName');

console.log(fileList);
```

递归调用 `walk` 遍历一个路径里所有的目录并将文件添加到文件列表里。这么做的结果是典型的深度优先，但往往程序往往对结果列表的序列有层次上的要求。所以如果按照广度优先的话，代码上可能需要略微调整一下：

```
function walk(path){
	var dirList = fs.readdirSync(path);
	
	dirList.forEach(function(item){
		if(fs.statSync(path + '/' + item).isFile()){
			fileList.push(path + '/' + item);
		}
	});
	
	dirList.forEach(function(item){
		if(fs.statSync(path + '/' + item).isDirectory()){
			walk(path + '/' + item);
		}
	});
}
```

调整的也不多，只是每次需要先push所有的文件，然后再依次遍历子目录。

偷鸡不成蚀把米，想着偷懒，结果绕个圈回来还是自己写来的快——这么点懒都要偷，真是无药可救了。