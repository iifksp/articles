# node编辑BT种子

在又一波净-网行动之后，迅雷倒下了，虽然这期间它居然还神奇的上市成功，但已经彻底被曾经的会员所唾弃。就目前看来，诸如百度离线之类，替代品还是有一些。回归主题内容，bt种子的内容是会被侦测的，最简单的就是内容的标题，这也是下载软件对于“敏感词”的最基本的判定。要绕过去就需要去掉这些敏感的部分。

bt种子，即torrent文件，其简单的格式定义可以在维基百科中找到。torrent包含Tracker信息和文件信息两部分，而我们需要编辑的就是文件信息。

torrent的文件信息经过Bencode编码而成，所以，要修改最简单的方法就是使用bencode editor，展开文件内容后依次修改内容。这样做完全OK，只是效率未免太低。如果碰到一大波敏感，有种要改到死的感觉。这个时候，可以求助于网上的一些在线编辑器，可以做到批量修改。

当然，偶尔也会遇到在线工具抽风的情况，而且这类站点多为个人维护，作为一个小站长也深知维护之不易，那天挂掉并不稀奇。所以啊，准备好备用方案也不无道理。

最后方案还是选择了node——用[node-torrent](https://github.com/fent/node-torrent)存取文件信息，再用[blueimp-md5](https://github.com/blueimp/JavaScript-MD5)简单将所有内容名全部hash一遍，简单粗暴，但好用就可以了。下面就是随手写的代码：

```
var nt = require('nt');
var md5 = require('blueimp-md5').md5;
var fs = require('fs')

nt.read('src.torrent', function(err, torrent) {
	if (err) throw err;
	var info = torrent.metadata.info;
	
	if(info.files){ // multi files
		for(var i= 0;i< info.files.length;i++){
			var file = info.files[i];
			var len = file.path.length;

			var str = file.path[len - 1];
			var arr = str.split('.');
			var ext = '.' + arr[arr.length - 1];
			arr.pop();
			var fname = arr.join('.');

			file.path = [md5(fname) + ext];
			delete file['path.utf-8'];	
		}
		
		var name = info.name;
		info.name = md5(name);
		delete info['name.utf-8'];
		
		info['publisher'] = '';
		info['publisher-url'] = '';
		delete info['publisher-url.utf-8'];
		delete info['publisher.utf-8'];
	}else{ // single file
		var name = torrent.metadata.info.name;
		var arr = name.split('.');
		
		var name8 = torrent.metadata.info['name.utf-8'];
		var arr8 = name8.split('.');

		torrent.metadata.info.name = md5(torrent.metadata.info.name)+ '.' + arr[arr.length-1];
		torrent.metadata.info['name.utf-8'] = md5(torrent.metadata.info['name.utf-8'] )+ '.' + arr8[arr8.length-1];
	}

	// write to new file
	torrent.createWriteStream('src-hashed.torrent');
});
```
我只测试了一次，看到下载没有问题，就没再怎么修改。另外，有些种子设置了修改限制，修改会导致下载失败。除开这些，种子本身也不能太冷门，毕竟相比迅雷的无节操网络，网盘离线还是要弱很多的。

心血来潮一时兴起写的，不足之处难免。对于不知此方法的兄弟，我只能帮你到这儿了...
