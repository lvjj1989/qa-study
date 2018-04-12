MySQL 删除数据库
===========================================
使用 mysqladmin 删除数据库
--------------------------------------
使用普通用户登陆mysql服务器，你可能需要特定的权限来创建或者删除 MySQL 数据库。

所以我们这边使用root用户登录，root用户拥有最高权限，可以使用 mysql mysqladmin 命令来创建数据库。

在删除数据库过程中，务必要十分谨慎，因为在执行删除命令后，所有数据将会消失。

以下实例删除数据库RUNOOB(该数据库在前一章节已创建)：

::

	[root@host]# mysqladmin -u root -p drop RUNOOB
	Enter password:******

执行以上删除数据库命令后，会出现一个提示框，来确认是否真的删除数据库：
::

	Dropping the database is potentially a very bad thing to do.
	Any data stored in the database will be destroyed.

	Do you really want to drop the 'RUNOOB' database [y/N] y
	Database "RUNOOB" dropped

