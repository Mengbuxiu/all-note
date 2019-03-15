# Linux命令学习 #

1. 命令

	- 文件上传： Xshell中		rz -> enter
	- 查看指定占用端口的程序 
		- lsof -i:端口号 
		- netstat -tunlp|grep 端口号
	- 文件查找
		- find / etc -name 'xxx*':`模糊查找`
		- find / -name 'xxx':`精确查找`
	- 查看指定进程
		- ps -ef | grep '进程名称'
	- 杀死指定进程
		- kill -9 'pid' 
	- 查看内存占用百分比 
		- top 
	- 软链接
		- ln -s 链接地址 目标地址
			- 完成后在目标地址的目录下 `ll`会看到 -> 链接

	- 移动文件
		- mv [option] 源文件/目录 目标文件/目录

	- 重命名目录名称
		- mv 源文件/目录 目标文件/目录