Linux_Iptables&Firewall.md

# Firewall tools
#### RHEL 6：iptable:
- 配置好的防火墙策略交由内核层面的netfilter网络过滤器来处理
 
#### REHL 7: firewalld
- 把配置好的防火墙策略交由内核层面的nftables包过滤框架来处理

#### 策略
- 从上到下读取配置规则，找到合适的立刻匹配，没有合适的执行默认规则
- Reject: 显示 Destination Port Unreachable
- Drop: 显示 0 received, 100% packet loss

# Iptable
- 查看已有的防火墙规则链: iptables -L
- 清空已有的防火墙规则链: iptables -F
- 设置默认规则: iptables -P INPUT DROP（默认链规则只能是drop不能rejrct）
- 向INPUT链中添加允许ICMP流量进入的策略规则(允许ping命令检测行为)：iptables -I INPUT -p icmp -j ACCEPT
- 允许指定网段主机访问22port，拒绝其他的(把允许的放在前面，拒绝的放后面)：
	
	```
	iptables -I INPUT -s 192.168.10.0/24 -p tcp --dport 22 -j ACCEPT
	iptables -A INPUT -p tcp --dport 22 -j REJECT
	```
- 向INPUT规则链中添加拒绝所有人访问本机12345端口的策略规则：
	
	```
	iptables -I INPUT -p tcp --dport 12345 -j REJECT
	iptables -I INPUT -p udp --dport 12345 -j REJECT
	```

- 向INPUT规则链中添加拒绝192.168.10.5主机访问本机80端口（Web服务）的策略规则：

	```
	iptables -I INPUT -p tcp -s 192.168.10.5 --dport 80 -j REJECT
	```

- 向INPUT规则链中添加拒绝所有主机访问本机1000～1024端口的策略规则：

	```
	iptables -A INPUT -p tcp --dport 1000:1024 -j REJECT
	iptables -A INPUT -p udp --dport 1000:1024 -j REJECT
	```

- 执行保存命令让iptable生效：
	
	```
	service iptables save
	```

# Firewalld
- 加入区域概念，同事有几套防火墙策略根据不同场景选择

# SNAT(source network translation)
- 解决IP地址匮乏，多个内网用户通过一个外网IP接入Internet
