MySQL 创建数据库
========================================================
使用 mysqladmin 创建数据库
--------------------------------------

使用普通用户，你可能需要特定的权限来创建或者删除 MySQL 数据库。

所以我们这边使用root用户登录，root用户拥有最高权限，可以使用 mysql mysqladmin 命令来创建数据库。

实例

::

	[root@host]# mysqladmin -u root -p create RUNOOB
	Enter password:******

以上命令执行成功后会创建 MySQL 数据库 RUNOOB。
