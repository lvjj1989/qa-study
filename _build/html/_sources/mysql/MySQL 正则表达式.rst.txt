MySQL 正则表达式
======================================

在前面的章节我们已经了解到MySQL可以通过 LIKE ...% 来进行模糊匹配。
MySQL 同样也支持其他正则表达式的匹配， MySQL中使用 REGEXP 操作符来进行正则表达式匹配。
简单，因为MySQL的正则表达式匹配与这些脚本的类似。
下表中的正则模式可应用于 REGEXP 操作符中。

.. figure:: /_static/mysql/re.png
    :width: 20.0cm


了解以上的正则需求后，我们就可以根据自己的需求来编写带有正则表达式的SQL语句。以下我们将列出几个小实例(表名：person_tbl )来加深我们的理解：
查找name字段中以'st'为开头的所有数据：
::

	mysql> SELECT name FROM person_tbl WHERE name REGEXP '^st';

查找name字段中以'ok'为结尾的所有数据：
::

	mysql> SELECT name FROM person_tbl WHERE name REGEXP 'ok$';


查找name字段中包含'mar'字符串的所有数据：
::

	mysql> SELECT name FROM person_tbl WHERE name REGEXP 'mar';

查找name字段中以元音字符开头或以'ok'字符串结尾的所有数据：
::

	mysql> SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$';