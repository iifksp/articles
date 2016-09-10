# JavaScript最小时间片

《高性能javascript》有一段“定时器的精度”，说的是JS的定时器并不精确，并且因为系统定时器分辨率的关系，JS的定时器最小值建议设置为25ms。并提到大多数浏览器在定时器延迟等于或者小于10ms时表现的并不一致。

其他我查到的资料也显示，在大部分的浏览器里不允许小于10ms的延时。来自微软方的资料则说这个精度最小是16ms。对于动画的时间片而言，16ms的精度正好对应约为60FPS。当然，还有各种其他说法。

比如，[David Baron](http://dbaron.org/log/) 在 [setTimeout with a shorter delay](http://dbaron.org/log/20100309-faster-timeouts) 里说到：

> 小于10ms的延时会被强制延长，Chrome将这个延时缩减到2ms，但同时导致了[一些问题](http://code.google.com/p/chromium/issues/detail?id=888)。

不过我并没有在最新版本的Chrome里遇到他提到的“一些问题”，而且，也有资料说Chrome的是4ms，所以这个时间片问题也因时而异。在以前，我也蛋疼地用下面这段代码测试过浏览器的时间片识别度：

```
setTimeout(function(){
	alert('timeout first');
},2);
setTimeout(function(){
	alert('timeout second');
},1);
```

记得那时的结果Chrome只有数毫秒，Firefox长达10ms，Opera稍短。但时隔久远，记得已经不太清楚，如今用同样一段代码，跑出来的结果却是和之前完全不同(Chrome 和 Safari 的括号里的值，是修改后面一个延时为0的情况下的结果)：

<table>
	<tr>
		<td></td>
		<th>IE-8</th>
		<th>Firefox-14</th>
		<th>Chrome-21</th>
		<th>Opera-12.01</th>
		<th>Safari-5.1.7</th>
	</tr>
	<tr>
		<th>能分辨的时长</th>
		<td>1ms</td>
		<td>1ms</td>
		<td>1ms(2ms)</td>
		<td>5ms</td>
		<td>1ms(2ms)</td>
	</tr>
</table>

虽然这么测没什么依据，但多少还是可以说明问题。

后来我继续翻了很多资料，多次看到“HTML5中此值为4ms”这样一句话，但出处无从查到。匆匆浏览 [Timer定义](http://www.whatwg.org/specs/web-apps/current-work/multipage/timers.html#timers) 以及 [MDN文档](http://www.whatwg.org/specs/web-apps/current-work/multipage/timers.html#timers)，也没发现什么特别的说明，稍微有点失望...

既然没有什么特别的结果，还是洗洗睡吧...:)

**update**：一觉醒来，看到评论(平台迁移到Ghost后丢弃了评论数据)里[aoao](http://www.aoao.org.cn/)提供了 [http://blog.csdn.net/aimingoo/article/details/1451556](http://blog.csdn.net/aimingoo/article/details/1451556) 这个参考，读完后思路一下清晰了很多。有些新的想法，改天整理下再更新。