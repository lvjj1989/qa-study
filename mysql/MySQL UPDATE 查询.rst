MySQL UPDATE 查询
==================================================

如果我们需要修改或更新 MySQL 中的数据，我们可以使用 SQL UPDATE 命令来操作。.

以下是 UPDATE 命令修改 MySQL 数据表数据的通用 SQL 语法：

::

	UPDATE table_name SET field1=new-value1, field2=new-value2
	[WHERE Clause]

* 你可以同时更新一个或多个字段。
* 你可以在 WHERE 子句中指定任何条件。
* 你可以在一个单独表中同时更新数据。

当你需要更新数据表中指定行的数据时 WHERE 子句是非常有用的。

