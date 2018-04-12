MySQL LIKE 子句
==========================================

我们知道在 MySQL 中使用 SQL SELECT 命令来读取数据， 同时我们可以在 SELECT 语句中使用 WHERE 子句来获取指定的记录。

WHERE 子句中可以使用等号 = 来设定获取数据的条件，如 "runoob_author = 'RUNOOB.COM'"。

但是有时候我们需要获取 runoob_author 字段含有 "COM" 字符的所有记录，这时我们就需要在 WHERE 子句中使用 SQL LIKE 子句。

SQL LIKE 子句中使用百分号 %字符来表示任意字符，类似于UNIX或正则表达式中的星号 *。

如果没有使用百分号 %, LIKE 子句与等号 = 的效果是一样的。

以下是 SQL SELECT 语句使用 LIKE 子句从数据表中读取数据的通用语法：

::

	SELECT field1, field2,...fieldN 
	FROM table_name
	WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'

* 你可以在 WHERE 子句中指定任何条件。
* 你可以在 WHERE 子句中使用LIKE子句。
* 你可以使用LIKE子句代替等号 =。
* LIKE 通常与 % 一同使用，类似于一个元字符的搜索。
* 你可以使用 AND 或者 OR 指定一个或多个条件。
* 你可以在 DELETE 或 UPDATE 命令中使用 WHERE...LIKE 子句来指定条件。

SQL 通配符
--------------------------------------
在 SQL 中，通配符与 SQL LIKE 操作符一起使用。

SQL 通配符用于搜索表中的数据。

在 SQL 中，可使用以下通配符：
.. figure:: /_static/mysql/like.png
    :width: 20.0cm