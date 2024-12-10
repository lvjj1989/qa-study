MySQL 删除数据表
=========================================
MySQL中删除数据表是非常容易操作的， 但是你再进行删除表操作时要非常小心，因为执行删除命令后所有数据都会消失。

::

	DROP TABLE table_name ;
在命令提示窗口中删除数据表
--------------------------------------
在mysql>命令提示窗口中删除数据表SQL语句为 DROP TABLE ：

::

	root@host# mysql -u root -p
	Enter password:*******
	mysql> use RUNOOB;
	Database changed
	mysql> DROP TABLE runoob_tbl
	Query OK, 0 rows affected (0.8 sec)
	mysql>