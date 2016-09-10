# 关闭Wordpress的post revision特性

WordPress从2.6开始就多了一个比较烦人的特性，就是 post revision 。这个特性使得作者可以回顾每次更新的文章内容，可以回滚到之前的版本。这对于多人作业的博客系统来说非常有用，但是对于单独作者的个人博客来说，显得非常无用。最糟糕的是，每次还会在wp_posts中添加新的记录，如果修改的很多的话，不一会，就会让整个数据库充满revision，而那些恰恰可能是毫无用处的。

Google后从Lester的博客上找到了[解决方法](http://lesterchan.net/wordpress/2008/07/17/how-to-turn-off-post-revision-in-wordpress-26/)，虽然还有很多其他方案，但是Lester给出的办法很有效。

要关闭post revision特性，只要在wp-config.php中加入如下行：
```
define('WP_POST_REVISIONS', false);
```
同时他还提供了删除多余revision数据的SQL语句：
```
DELETE a,b,c  
FROM wp_posts a  
LEFT JOIN wp_term_relationships b ON (a.ID = b.object_id)  
LEFT JOIN wp_postmeta c ON (a.ID = c.post_id)  
WHERE a.post_type = 'revision'
```
在执行前先备份数据库。

这样一来，数据库干净了许多，revision不会再生成，但autosave仍然有效。
在Lester博客的评论里我还看到有[WP-CMS](http://wordpress.org/extend/plugins/wp-cms-post-control/)可以关闭这个特性，并提供更多关于wordpress接口的控制。