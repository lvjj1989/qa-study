MySQL 连接
===================================

使用mysql二进制方式连接
--------------------------------------
您可以使用MySQL二进制方式进入到mysql命令提示符下来连接MySQL数据库。

::


	[root@host]# mysql -u root -p
	Enter password:******

在登录成功后会出现 mysql> 命令提示窗口，你可以在上面执行任何 SQL 语句。

以上命令执行后，登录成功输出结果如下:
::

	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 2854760 to server version: 5.0.9

	Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

在以上实例中，我们使用了root用户登录到mysql服务器，当然你也可以使用其他mysql用户登录。
如果用户权限足够，任何用户都可以在mysql的命令提示窗口中进行SQL操作。
退出 mysql> 命令提示窗口可以使用 exit 命令，如下所示：
::

	mysql> exit
	Bye