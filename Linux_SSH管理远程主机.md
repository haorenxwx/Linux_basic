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
	指定con-name,
```	
	nmcli connection add con-name company ifname eno16777736 autoconnect no type ethernet ip4 192.168.10.10/24 gw4 192.168.10.1
```



#



