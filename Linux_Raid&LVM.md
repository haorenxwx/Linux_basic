Linux_Raid&LVM.md 

# RAID
## 定义
- 多个硬盘组成的阵列
- 分散读写提升性能
- 多个重要副本备份 reduntancy

## 常见方案
### RAID 0
- 串联，提升读写速度
- 无法备份或恢复数据

### RAID 1
- 绑定两块以上的硬件设备，数据同时写到多块硬盘上
- 发生故障后使用热交换的方式恢复数据正常使用
- 利用率下降

### RAID 5
- 每块磁盘中的parity部分储存其他硬盘的奇偶校验信息

### RAID 10
- RAID 1+RAID 0技术的一个组合体
- 先组成"RAID 0" 保证数据的安全，再用"RAID 1"串联提升读写速度

## mdadm 创建和管理RAID
### 一块物理设备出现损坏不能继续使用
- 删除设备
	
	```sh
	mdadm /dev/md0 -f /dev/sdb 
	mdadm -D /dev/md0
	umount /RAID
	mdadm /dev/md0 -a /dev/sdb

	```
### 磁盘阵列+备份盘
	```sh
	mdadm -Cv /dev/md0 -n 3 -l 5 -x 1 /dev/sdb /dev/sdc /dev/sdd /dev/sde
	: -x 备份盘
	```

# LVM

## 定义

- 用户可以根据实际需求调整硬盘分区大小
- VG, volume group(卷组)
- PV, physical volumn(物理卷)
- LV, logical volumn(逻辑卷)
- PE, physical extent(基本单元)

## 部署

	```
	pvcreate /dev/sdb /dev/sdc 
	:添加两块硬盘设备支持LVM技术
	vgceate storage /dev/sdb /dev/sdc
	:把硬件加到卷组中
	lvcreate -n vo -l 37 storage 
	:以个数为单位进行切割，基本单元大小4MB 4*37MB大小的逻辑卷
	mkjs.ext4 /dev/storage/vo 
	:把逻辑卷格式化并挂载使用
	df -h
	echo "/dev/storage/vo /linuxprobe ext4 defaults 0 0" >> /etc/fstab
	:查看并写入配置文件
	```

## 扩容

	```
	umount /linuxprobe
	:若需要扩容，需要卸载 设备和挂载点的关联
	lvextend -L 290M /dev/storage/vo 
	:扩容至290MB
	e2fsck -f /dev/storage/vo
	resize2fs /dev/storage/vo
	:检查硬盘完整性并重置硬盘容量
	mount -a
	df -h
	:重新挂载并且查看状态
	:https://www.linuxprobe.com/chapter-07.html
	```

