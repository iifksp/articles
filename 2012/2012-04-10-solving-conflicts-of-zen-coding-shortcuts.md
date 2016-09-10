# 解决 Zen Coding 快捷键冲突

[Zen Coding](http://code.google.com/p/zen-coding/) 是个好东西，Npp 装上 Zen Coding 之后，书写 HTML 的速度几乎无可匹敌。但是它也会带来非常麻烦的快捷键冲突问题，因为它截获了定义的所有快捷键(它定义了非常多)，使得在其他软件里这些快捷键完全没了反应。

最典型的就是 Ctrl+Y，如果同时打开 Npp 以及其他编辑器，恢复(Ctrl+Y)操作就不再可用，因为快捷键被 Zen Coding 先截走了。

更糟糕的是，如果同时使用Photoshop的话，切图变得相当不便。很多快捷键比如常用的合并图层(Ctrl+E)和合并可见图层(Ctrl+Shift+E)等等，都只能靠点菜单，效率实在太低。一开始的时候我也就忍了，看在代码速度的份上，在其他时候麻烦些也就算了，大不了关了Npp。但是后来渐渐发觉，如果切图数量多的话，很多时候 Zen Coding 的速度优势被其他软件中的不便所抵消。忍了这么多年来，终于还是忍不下去了。

其实这很容易，就是之前没能想到。在Npp的插件目录里随便一翻就找到了 Zen Coding.js，如果偶然看到文件末尾，那么事情就简单了。文件第8010行(版本不同行数可能有变化)，罪魁祸首果然是 Zen Coding。

```
addMenuItem('Evaluate Math Expression', 'evaluate_math_expression', 'Ctrl+Y');
```

直接注释掉这行，然后重启Npp，妥妥的~ 而其他被占用的快捷键也一样可以简单解除，或者，也可以改成别的。