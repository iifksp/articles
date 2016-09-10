# Chrome的alert与重复focus

前段时间，在写一个placeholder插件的时候，遇到了一个Chrome下的诡异问题，一段简单的代码，就可以使得浏览器弹出无限循环的 alert 提示框。

我用的代码片段精简到最后是这样的：

```
<input id="test" placeholder="placeholder" type="text" />
<script type="text/javascript">
	document.getElementById('test').onfocus = function(){
		alert('focus!');
	}
</script>
```

可能是一时图方便，没log而是用lalert。结果**当点掉alert提示框，Chrome会重复 `focus` 事件，而其他浏览器则不会。**

我用的是 Chrome 18，看起来在 alert被点掉的瞬间，输入框再次获得了一次焦点，而这次的焦点获取同样触发了 `focus` 事件，再次弹出alert，然后如此循环往复...

好在这种现象只存在于Chrome里，其他浏览器都完全正常，虽然它们在点掉 alert 弹出框后的行为也不尽相同，但是无一会重复触发 `focus`。

随后我测试了主流浏览器的行为，整理一张简单的表以备参考:
<table>
	<tr>
		<th>浏览器</th>
		<th>行为</th>
	</tr>
	<tr>
		<td>IE8</td>
		<td>点掉alert后，输入框保留焦点，不重复触发focus</td>
	</tr>
	<tr>
		<td>Firefox 11</td>
		<td>点掉alert后，输入框焦点丢失，不重复触发focus</td>
	</tr>
	<tr>
		<td>Chrome 18</td>
		<td>点掉alert后，输入框保留焦点，重复触发focus陷入循环</td>
	</tr>
	<tr>
		<td>Opera 10.60</td>
		<td>点掉alert后，输入框保留焦点，不重复触发focus</td>
	</tr>
	<tr>
		<td>Safari 10.60</td>
		<td>点掉alert后，输入框保留焦点，不重复触发focus</td>
	</tr>
</table>

可见 IE8，Opera 和 Safari 的表现是一样的，**唯独 Firefox 会丢失焦点**，还有就是**唯独 Chrome 会重复 focus**。