Linux储存结构和磁盘划分.md

#FHS filesystem hierarchy srantard
- /boot 开机所需文件-kernal,开机菜单和所需配置文件
- /dev 以文件形式存放任何设备和接口
- /etc 配置文件


#物理设备命名规则
- /dev/sda5 
- /dev 表示硬件设备所在的目录
- sd 表示SC/SI 设备
- a 表示硬盘的顺序号
- 1 表示分区的顺序号

- 文件系统的信息保存在inode中
- 文件内容保存在block块中

#挂载硬件设备(attach filesystem found on some device)
- 用户使用硬盘响应分区的时候，需要先将其与一个已存在的文件目录进行关联
- /etc/fstab  config




