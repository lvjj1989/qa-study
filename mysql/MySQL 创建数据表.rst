MySQL 创建数据表
===========================================

创建MySQL数据表需要以下信息：

* 表名
* 表字段名
* 定义每个表字段

以下为创建MySQL数据表的SQL通用语法：

::

	CREATE TABLE table_name (column_name column_type);

以下例子中我们将在 RUNOOB 数据库中创建数据表runoob_tbl：

::

	CREATE TABLE IF NOT EXISTS `runoob_tbl`(
	   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
	   `runoob_title` VARCHAR(100) NOT NULL,
	   `runoob_author` VARCHAR(40) NOT NULL,
	   `submission_date` DATE,
	   PRIMARY KEY ( `runoob_id` )
	)ENGINE=InnoDB DEFAULT CHARSET=utf8;


实例解析：

* 如果你不想字段为 NULL 可以设置字段的属性为 NOT NULL， 在操作数据库时如果输入该字段的数据为NULL ，就会报错。
* AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。
* PRIMARY KEY关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号分隔。
* ENGINE 设置存储引擎，CHARSET 设置编码。


