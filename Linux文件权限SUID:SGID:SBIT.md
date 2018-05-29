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
- Meaning:
	File: Set file group's access up on execution
	Location: Set files under same location into the same user group
- setting
	File: `chmod 2555 [path_to_file]`
	Directory: `chmod g+s [path_to_dir]`

#Sticky Bit
- Meaning:
	Used on shared directions, only the root user and the file owner can remove their own files
- Setting:
	`chmod +t [path_to_directory]`
	`chmod 1777 [path_to_directory]`

#隐藏文件属性：
- 设置/删除隐藏文件属性
	`chattr [参数] [path_to_file]`:
- 查看
	`lsattr [path to file]`

#文件访问控制列表ACL
- 设置/删除 指定用户对文件/文件夹权限：setfacl
	`setfacl -RM u:xuwx:rwx /root`
	-R for path/-m for file/-b for remove 
- 查看 : `getfacl [path_to_file]`

#su&sudo
- 在root下通过visudo编辑sudo服务的配置文件
	`[root@linuxprobe ~]# visudo
 	96 ##
 	97 ## Allow root to run any commands anywhere
 	98 root ALL=(ALL) ALL
 	99 linuxprobe ALL=(ALL) /usr/bin/cat`


