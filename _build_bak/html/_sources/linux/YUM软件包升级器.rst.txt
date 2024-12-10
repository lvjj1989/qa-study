YUM软件包升级器
==============================

::

	yum install package_name  # 下载并安装一个rpm包
	yum localinstall package_name.rpm  # 将安装一个rpm包，使用你自己的软件仓库为你解决所有依赖关系
	yum update package_name.rpm  # 更新当前系统中所有安装的rpm包
	yum update package_name  # 更新一个rpm包
	yum remove package_name  # 删除一个rpm包
	yum list  # 列出当前系统中安装的所有包
	yum search package_name  # 在rpm仓库中搜寻软件包
	yum clean packages  # 清理rpm缓存删除下载的包
	yum clean headers  # 删除所有头文件
	yum clean all  # 删除所有缓存的包和头文件
	