MySQL GROUP BY 语句
==============================================
GROUP BY 语句根据一个或多个列对结果集进行分组。

在分组的列上我们可以使用 COUNT, SUM, AVG,等函数。
::

	SELECT column_name, function(column_name)
	FROM table_name
	WHERE column_name operator value
	GROUP BY column_name;
