# Linux命令学习 #

1. 命令

	- 文件上传： Xshell中		rz -> enter
	- 查看指定占用端口的程序 
		- lsof -i:端口号 
		- netstat -tunlp|grep 端口号
	- 文件查找
		- find / etc -name 'xxx*':`模糊查找`
		- find / -name 'xxx':`精确查找`