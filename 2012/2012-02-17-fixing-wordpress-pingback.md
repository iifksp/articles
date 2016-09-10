# 修复Wordpress的Pingback

不知道是从何时起自己博客的Pingback挂掉了，自己也因为长期没有收到什么Pingback最终还是发觉了这个问题，只是，平时就比较繁忙加之这个功能缺失并无太大的影响，于是就一直搁置在一旁，结果一搁就是1年了。在多次自己文章Pingback失败之后我再也无法忍受这种文章的孤立感，在这种积怨的驱动下，我居然整整花了大约有1天时间来查原因，无奈双重问题让我走了一些弯路，不过最后还是顺利解决了。然后，我打算记录下这一路查错的过程，也许需要的人可能会用到。

一开始，我能想到的就是配置问题，我直接Google了 pingback wordpress not working，并查看了前三页所有的文章，结果没有一个有效。我尝试了很多配置，修改htaccess放宽权限、修改config文件等待，不过问题似乎不是出在这种地方。

随后我怀疑是主题的问题，但我切回默认主题后问题依旧。我尝试禁用所有的插件，但问题依旧(后来发觉并非如此)。我尝试升级wordpress到最新，还是无效。我的博客既发不出也收不到Pingback。

出于某种不爽的情绪，我索性阅读了 [Pingback 1.0定义](http://www.hixie.ch/specs/pingback/pingback)。从这份定义文档里，以及 [Quest for Self Pingbacks](http://lists.automattic.com/pipermail/wp-hackers/2009-September/027425.html) 一文里，我了解了wordpress的Pingback做了些什么：


1. 当一篇文章发布的时候，wordpress检测并根据设置，对文章里的链接进行Ping的操作。它会安排Ping回的事件并立即执行。
2. 目标收到Ping之后，会检查源页面里是否含有目标页面的URL，并检查`title`等其它要素。
3. 通过检测后，目标页生成一个新的评论。


简单的搜索后没有发现合适的Pingback调试工具，无奈只好下载 xmlrpc for php 库，自己做客户端发送Pingback做测试。又是阅读了一堆使用文档，然后终于断断续续地写了这些测试代码：

```
require_once('./lib/xmlrpc/xmlrpc.inc');

$site_linking_from = "http://www.swordair.com/blog/2011/07/666";
$site_linking_to = "http://www.swordair.com/blog/2010/12/522";

$host_url = "swordair.com";

$xmlrpc_client = new xmlrpc_client("/blog/xmlrpc.php", $host_url, 80);
$xmlrpc_client->setDebug(1);

$xmlrpc_message = new xmlrpcmsg("pingback.ping", array(new xmlrpcval($site_linking_from),new xmlrpcval($site_linking_to)));
$xmlrpc_response = $xmlrpc_client->send($xmlrpc_message);

echo $xmlrpc_response->faultString();
```

但光有这些还不够，我还得trace下wordpress的部分代码。在最新版3.3.1里，处理xmlrpc的是 `xmlrpc.php`，在页面中由下面的代码指定：

```
<link rel="pingback" href="http://www.swordair.com/blog/xmlrpc.php" />
```

这个文件里，真正处理xmlrpc的wordpress xmlrpc实现是其引用的 `class-wp-xmlrpc-server.php`，在 `class-wp-xmlrpc-server.php` 第3409行就是处理pingback的回调函数：

```
/**
 * Retrieves a pingback and registers it.
 *
 * @since 1.5.0
 *
 * @param array $args Method parameters.
 * @return array
 */
function pingback_ping($args) {
```

然后，浏览了一下这个处理函数，其中的错误信息正好就是 [Pingback 1.0定义](http://www.hixie.ch/specs/pingback/pingback) 里的错误类型，只是wordpress检测时的顺序稍有差异：

```
ERROR 33: The specified target URL cannot be used as a target. It either doesn't exist, or it is not a pingback-enabled resource.
ERROR 00: The source URL and the target URL cannot both point to the same resource.
ERROR 48: The pingback has already been registered.
ERROR 16: The source URL does not exist.
ERROR 32: We cannot find a title on that page.
```

根据函数代码，这些错误表示下面的意思(光从错误信息其实还比较抽象，但是直接看代码就明白了它是如何检测的)：

- ERROR 33: 无法解析的URL，页面ID不存在或者Ping没有开启
- ERROR 00: 一个地址不能同时作为源和目标
- ERROR 48: 说明这个pingback之前已经被录入
- ERROR 16: wp_remote_fopen 返回false，即无法建立连接
- ERROR 32: 源页没有标题

这样一来，工具就齐全了。出于一些原因，我不得不在线上直接调试，于是我还特意找来Disable RSS插件禁用了wordpress的RSS生成。用上面的测试代码，加上对处理函数的分析，再加上错误信息的列表，一遍遍调试终于，Ping通了。

然而讽刺的是，当我运行测试代码，从输出力看到最后那条错误信息的时候，相当无语：Error: Please enter a valid seccode. 居然是验证码插件在作怪！

这次真心大意了，费了九牛二虎之力，居然问题出在一个插件上。尽管一开始我就禁用插件测试，但似乎是受到了当时服务器阻塞的影响，让我放松了对插件的嫌疑。实际上我从很早以前就开始使用的这个wp-seccode验证码插件一直工作良好，以前也并没有屏蔽过Pingback。不过我记得我曾经升级过一次，也许就是因为那次的升级，wp-seccode就将所有的Pingback挡在门外了。

wp-seccode简单小巧，是我喜欢的主要原因。而且垃圾评论实在很多，把验证码一关掉就蹦出来好多。所以我没打算放弃使用，而是决定直接修改插件。

在 `pingback_ping` 函数的最后可以看到评论是如何生成的：

```
$commentdata = compact('comment_post_ID', 'comment_author', 'comment_author_url', 'comment_author_email', 'comment_content', 'comment_type');

$comment_ID = wp_new_comment($commentdata);
do_action('pingback_post', $comment_ID);

return sprintf(__('Pingback from %1$s to %2$s registered. Keep the web talking! :-)'), $pagelinkedfrom, $pagelinkedto);
```

于是我赶紧找了一下 `wp_new_comment` 的参考，果然，它有个 `preprocess_comment` 的 Hook。然后在到wp-seccode插件目录里，找到在 `run.php` 里的 `preprocess_comment` 函数，在判断位置添加了下面的代码，让插件直接略过pingback的处理：

```
if($commentdata['comment_type'] != 'pingback'){}
```

至此，Pingback终于回来了。

一番折腾终于有了结果，我不用更换插件，Pingback也能继续工作。虽然此次问题的成因非常简单，但是种种原因让我在初期错过了识别，结果导致了杀鸡用牛刀。为此我额外地阅读了Pingback的定义以及xmlrpc库的使用文档，真够蛋疼的...但以后如果再有什么有关xmlrpc以及pingback的问题，我自己都可以回来阅读此文，因为无论是在国内外，这里对于Pingback的调试说得已经相当详细:)