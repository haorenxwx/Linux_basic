Linux_SSH管理远程主机.md

# 配置网卡参数
- nmtui --可视化界面，配置主机IP信息
- 网卡配置文件：
```
	vim /etc/sysconfig/network-scripts/ifcfg-xxxxx
	systemctl restart network	
```
- nmcli 
	- 查看network device 
```
	nmcli connection show
	nmcli con show enoxxxxx` 
```

	- 创建连接
	指定con-name,autoconnect, 和ip4地址

```	
	nmcli connection add con-name company ifname eno16777736 autoconnect no type ethernet ip4 192.168.10.10/24 gw4 192.168.10.1
```

- 绑定两块网卡
	- 两块网卡config初始化设置，分别vim
```
	TYPE=Ethernet
	BOOTPROTO=none
	ONBOOT=yes
	USERCTL=no
	DEVICE=eno33554968
	MASTER=bond0
	SLAVE=yes
```
	- 将绑定后的设备命名为bond0并填写ip等信息，两块网卡共同提供服务
```
	TYPE=bond
	BOOTPROTO=none
	ONBOOT=yes
	USERCTL=no
	DEVICE=bond0
	IPADDR=192.168.10.10
	PREFIX=24
	DNS=192.168.10.1
	NM_CONTROLLED=no
	
```
	- linux内核支持网卡绑定驱动
	mode0 平衡负载模式，自动备援，需要交换机设备支持
	mode1 自动备援模式，鼓掌替换
	mode6 平衡负载模式，自动备援，不需要交换机
```
	vim /etc/modprobe.d/bond.conf
	set:
	alias bond0 bonding
	options bond0 miimon=100 mode=6
```

# ssh
- 生成密匙并上传主机：
```
	ssh-keygen
	ssh-copy-id <hostip>
```

# scp 远程传输命令
```
	# -P:certain port, -r:transfer folder
	#from local to server
	scp local/location <destination ip>:destination/location/
	scp /root/readme.txt 192.168.10.20:/home
	#from server to local
	scp 192.168.10.20:/home /root/readme.txt
```

# screen 
### 解决网络异常中断
	- 网络中断时回话恢复
```
	#install
	yum install screen


	#创建名为backup的会话窗口，运行的操作都会被记录下来
	screen -S backup
	#查看正在运行的会话
	screen -ls
	#退出会话
	exit


	#用screen执行要运行的命令，记录命令中的操作，命令执行结束后scrren会话也结束
	#创建：
	screen -S linux
	tail -f /var/log/messages
	#通过关闭窗口（类似断网效果）
	#查看并恢复：
	screen -ls
	screen -r linux

	tail -f /var/log/messages
	#可以观测继续程序自动继续执行
```

### 同时控制多个远程终端窗口
	- 每个会话独立运行
### 会话共享，多个用户输入输出信息共享
```
	ssh 192.168.10.10
	screen -S linuxprobe
	screen -x #两个主机可以见到相同的命令
```


