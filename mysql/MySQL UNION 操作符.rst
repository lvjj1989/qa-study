MySQL UNION 操作符
==================================
MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。

MySQL UNION 操作符语法格式：

::

	SELECT expression1, expression2, ... expression_n
	FROM tables
	[WHERE conditions]
	UNION [ALL | DISTINCT]
	SELECT expression1, expression2, ... expression_n
	FROM tables
	[WHERE conditions];

* expression1, expression2, ... expression_n: 要检索的列。
* tables: 要检索的数据表。
* WHERE conditions: 可选， 检索条件。
* DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。
* ALL: 可选，返回所有结果集，包含重复数据。
