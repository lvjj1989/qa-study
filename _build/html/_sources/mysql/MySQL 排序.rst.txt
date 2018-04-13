MySQL 排序
=========================================
如果我们需要对读取的数据进行排序，我们就可以使用 MySQL 的 ORDER BY 子句来设定你想按哪个字段哪种方式来进行排序，再返回搜索结果。
以下是 SQL SELECT 语句使用 ORDER BY 子句将查询数据排序后再返回数据：

::

	SELECT field1, field2,...fieldN table_name1, table_name2...
	ORDER BY field1, [field2...] [ASC [DESC]]

* 你可以使用任何字段来作为排序的条件，从而返回排序后的查询结果。
* 你可以设定多个字段来排序。
* 你可以使用 ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。 默认情况下，它是按升序排列。
* 你可以添加 WHERE...LIKE 子句来设置条件。