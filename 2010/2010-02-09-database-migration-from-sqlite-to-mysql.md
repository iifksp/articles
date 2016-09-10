# SQLite向MySQL的数据库迁移

这是我一份工作文档拷贝。*08/03/2009 created by iifksp*

SQLite向MySQL的迁移(migration)分为3步

1. 将数据库从SQLite导出。
2. 修改SQLite的.sql文件内容，使其兼容MySQL的sql语句格式。
3. 将数据导入MySQL。

其中第二步最为关键。下面的兼容性修改，并**不能100%保证迁移的成功**，但这些是我所知的全部差异，并且在我工作中的案例中表现良好。

环境：
```
ubuntu-9.04-desktop-i386
firefox-3.5
sqlite_manager-0.5.0b3-fx+tb+sm
phpMyAdmin-3.2.0-all-languages
mysql  Ver 14.12 Distrib 5.0.75, for debian-linux-gnu (i486) using readline 5.2
```
## 1. SQLite导出
可以使用各种工具导出库，我使用FireFox的SQLite的管理插件SQLite Manager。使用FireFox访问https://addons.mozilla.org/en-US/firefox/addon/5817 并安装此插件，要求FireFox版本高于3.5。

使用SQLite Manager插件打开数据库(.db .sqlite3)文件，选择导出。将整个库文件导出为.sql查询语句。

## 2. SQL语句兼容性修改
为了保证SQL语句的兼容，需要将SQLite的特有的格式，修改为MySQL的格式。下面为我总结的一般规则(下面的方括号应被忽略)：
1. 将 ["] 改为 [\`]，也可以移除全部的 ["] ，但是如果有一些函数名作为字段名(e.g. regexp)时将会遇到错误。需要注意一些默认为 ["] ，其作用不在字段上的，不应被替换而应当被保留。
2. 移除所有的 [BEGIN TRANSACTION] [COMMIT] 以及 任何包含 [sqlite_sequence] 的(整)行。
3. 将所有 [autoincrement] 改为 [auto_increment]。
4. 将所有 ['f'] 改为 ['0'] 并将所有 ['t'] 改为 ['1']，这一项包含了[boolean DEFAULT 't'] 和 [boolean DEFAULT '1'] 的不同，以及 [boolean DEFAULT 'f'] 和 [boolean DEFAULT '0'] 的不同，以及 被插入表中的值 的两个数据库间的差异。
将修改完的文件保存。注：要特别小心，别把表内数据给换掉，如果恢复数据库后发觉不对，要及时将数据改回。

## 3. MySQL的导入
在MySQL中新建同名的空数据库，使用如phpmyadmin的导入库的功能，将.sql文件导入。至此数据迁移完毕。

之后需要修改应用程序数据库链接的指向等。