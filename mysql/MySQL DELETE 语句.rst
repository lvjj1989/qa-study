MySQL DELETE 语句
=======================================
你可以使用 SQL 的 DELETE FROM 命令来删除 MySQL 数据表中的记录。

你可以在 mysql> 命令提示符或 PHP 脚本中执行该命令。
以下是 SQL DELETE 语句从 MySQL 数据表中删除数据的通用语法：

::

	DELETE FROM table_name [WHERE Clause]

* 如果没有指定 WHERE 子句，MySQL 表中的所有记录将被删除。
* 你可以在 WHERE 子句中指定任何条件
* 您可以在单个表中一次性删除记录。

当你想删除数据表中指定的记录时 WHERE 子句是非常有用的。
