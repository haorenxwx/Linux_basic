#SUID

- Meaning: Set owner User ID up on execution
	让二进制程序的执行者临时拥有执行者拥有的权限
- eg: 'passwd'&'ping'comman
- setting: 
	 `chmod u+s file1.txt `
	 `chmod 4750 file1.txt`
- find SUID setting files
	 `find / perm -40000`	 

#SGID