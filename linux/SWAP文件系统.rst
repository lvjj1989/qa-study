SWAP文件系统
================================================

::
	
	mkswap /dev/hda3  # 创建一个swap文件系统
	swapon /dev/hda3  # 启用一个新的swap文件系统
	swapon /dev/hda2 /dev/hdb3  # 启用两个swap分区