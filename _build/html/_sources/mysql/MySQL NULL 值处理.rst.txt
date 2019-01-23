MySQL NULL 值处理
=============================================

我们已经知道 MySQL 使用 SQL SELECT 命令及 WHERE 子句来读取数据表中的数据,但是当提供的查询条件字段为 NULL 时，该命令可能就无法正常工作。

为了处理这种情况，MySQL提供了三大运算符:

* IS NULL: 当列的值是 NULL,此运算符返回 true。
* IS NOT NULL: 当列的值不为 NULL, 运算符返回 true。
* <=>: 比较操作符（不同于=运算符），当比较的的两个值为 NULL 时返回 true。
关于 NULL 的条件比较运算是比较特殊的。你不能使用 = NULL 或 != NULL 在列中查找 NULL 值 。

在 MySQL 中，NULL 值与任何其它值的比较（即使是 NULL）永远返回 false，即 NULL = NULL 返回false 。

MySQL 中处理 NULL 使用 IS NULL 和 IS NOT NULL 运算符。
