deb包
====================================

::

	dpkg -i package.deb  # 安装/更新一个 deb 包
	dpkg -r package_name  # 从系统删除一个 deb 包
	dpkg -l  # 显示系统中所有已经安装的 deb 包
	dpkg -l | grep httpd  # 显示所有名称中包含 "httpd" 字样的deb包
	dpkg -s package_name  # 获得已经安装在系统中一个特殊包的信息
	dpkg -L package_name  # 显示系统中已经安装的一个deb包所提供的文件列表
	dpkg --contents package.deb  # 显示尚未安装的一个包所提供的文件列表
	dpkg -S /bin/ping  # 确认所给的文件由哪个deb包提供
	APT  # 软件工具 (Debian, Ubuntu 以及类似系统)
	apt-get install package_name  # 安装/更新一个 deb 包
	apt-cdrom install package_name  # 从光盘安装/更新一个 deb 包
	apt-get update  # 升级列表中的软件包
	apt-get upgrade  # 升级所有已安装的软件
	apt-get remove package_name  # 从系统删除一个deb包
	apt-get check  # 确认依赖的软件仓库正确
	apt-get clean  # 从下载的软件包中清理缓存
	apt-cache search searched-package  # 返回包含所要搜索字符串的软件包名称