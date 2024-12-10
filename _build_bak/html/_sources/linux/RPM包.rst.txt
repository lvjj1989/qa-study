RPM包
===============================

::

	rpm -ivh package.rpm  # 安装一个rpm包
	rpm -ivh --nodeeps package.rpm  # 安装一个rpm包而忽略依赖关系警告
	rpm -U package.rpm  # 更新一个rpm包但不改变其配置文件
	rpm -F package.rpm  # 更新一个确定已经安装的rpm包
	rpm -e package_name.rpm  # 删除一个rpm包
	rpm -qa  # 显示系统中所有已经安装的rpm包
	rpm -qa | grep httpd  # 显示所有名称中包含 "httpd" 字样的rpm包
	rpm -qi package_name  # 获取一个已安装包的特殊信息
	rpm -qg "System Environment/Daemons"  # 显示一个组件的rpm包
	rpm -ql package_name  # 显示一个已经安装的rpm包提供的文件列表
	rpm -qc package_name  # 显示一个已经安装的rpm包提供的配置文件列表
	rpm -q package_name --whatrequires  # 显示与一个rpm包存在依赖关系的列表
	rpm -q package_name --whatprovides  # 显示一个rpm包所占的体积
	rpm -q package_name --scripts  # 显示在安装/删除期间所执行的脚本l
	rpm -q package_name --changelog  # 显示一个rpm包的修改历史
	rpm -qf /etc/httpd/conf/httpd.conf  # 确认所给的文件由哪个rpm包所提供
	rpm -qp package.rpm -l  # 显示由一个尚未安装的rpm包提供的文件列表
	rpm --import /media/cdrom/RPM-GPG-KEY  # 导入公钥数字证书
	rpm --checksig package.rpm  # 确认一个rpm包的完整性
	rpm -qa gpg-pubkey  # 确认已安装的所有rpm包的完整性
	rpm -V package_name  # 检查文件尺寸、 许可、类型、所有者、群组、MD5检查以及最后修改时间
	rpm -Va  # 检查系统中所有已安装的rpm包- 小心使用
	rpm -Vp package.rpm  # 确认一个rpm包还未安装
	rpm2cpio package.rpm | cpio --extract --make-directories *bin*  # 从一个rpm包运行可执行文件
	rpm -ivh /usr/src/redhat/RPMS/`arch`/package.rpm  # 从一个rpm源码安装一个构建好的包
	rpmbuild --rebuild package_name.src.rpm  # 从一个rpm源码构建一个 rpm 包