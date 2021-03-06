# Linux命令学习 #

1. 命令
	- 滚动查看日志
		
		- tail -f catalina.out
		
	- 跳到文件底部 
	
	  - shift+G
	
	- 搜索关键字
	
	  - / + 关键字 或  ? + 关键字
	
	- 抓包
		
		- tcpdump -i eth0 port 3306 
		
	- linux互传文件 
		
		- scp ip:srcPath destPath
		
	- 添加静态路由
		
		- https://www.cnblogs.com/wanghuaijun/p/8059664.html 
		
	- 文件上传： Xshell中		
		
		- rz -> enter
		
	- 端口
		- 查看指定占用端口的程序 
			- lsof -i:端口号 
			- netstat -tunlp|grep 端口号
		- firewall-cmd --list-ports
			- 查看已开放的端口
		- firewall-cmd --zone=public --add-port=80/tcp --permanent
			- 开启端口
	- firewall-cmd --reload 
			- 重启firewall
		- systemctl stop firewalld.service 
		- 停止firewall
		- systemctl disable firewalld.service 
			- 禁止firewall开机启动
	
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
		
	- 使用tar压缩文件
	
	 	- tar -zcvf test.tar.gz ./test/
	
		该命令表示压缩当前文件夹下的文件夹test，压缩后缀名为test.tar.gz
	
		如果不需要压缩成gz，只需要后缀为tar格式的，那么输入如下命令：
	
		- tar -cvf test.tar ./test/

 

	- 使用tar解压文件
	
		- tar -xzvf test.tar.gz  
	
	该命令表示把后缀为.tar.gz的文件解压到当前文件夹下。
	
	如果压缩文件的后缀是.tar，没有gz，则使用命令:
	
		- tar -xvf test.tar


	- 压缩
		- zip
			- 语 法：zip [-AcdDfFghjJKlLmoqrSTuvVwXyz$][-b <工作目录>][-ll][-n <字尾字符串>][-t <日期时间>][-<压缩效率>][压缩文件][文件...][-i <范本样式>][-x <范本样式>] 
			- 补充说明：zip是个使用广泛的压缩程序，文件经它压缩后会另外产生具有".zip"扩展名的压缩文件。 
			- 参 数： 
				- A 调整可执行的自动解压缩文件。 
				- b<工作目录> 指定暂时存放文件的目录。 
				- c 替每个被压缩的文件加上注释。 
				- d 从压缩文件内删除指定的文件。 
				- D 压缩文件内不建立目录名称。 
				- f 此参数的效果和指定"-u"参数类似，但不仅更新既有文件，如果某些文件原本不存在于压缩文件内，使用本参数会一并将其加入压缩文件中。 
				- F 尝试修复已损坏的压缩文件。 
				- g 将文件压缩后附加在既有的压缩文件之后，而非另行建立新的压缩文件。 
				- h 在线帮助。 
				- i<范本样式> 只压缩符合条件的文件。 
				- j 只保存文件名称及其内容，而不存放任何目录名称。 
				- J 删除压缩文件前面不必要的数据。 
				- k 使用MS-DOS兼容格式的文件名称。 
				- l 压缩文件时，把LF字符置换成LF+CR字符。 
				- ll 压缩文件时，把LF+CR字符置换成LF字符。 
				- L 显示版权信息。 
				- m 将文件压缩并加入压缩文件后，删除原始文件，即把文件移到压缩文件中。 
				- n<字尾字符串> 不压缩具有特定字尾字符串的文件。 
				- o 以压缩文件内拥有最新更改时间的文件为准，将压缩文件的更改时间设成和该文件相同。 
				- q 不显示指令执行过程。 
				- r 递归处理，将指定目录下的所有文件和子目录一并处理。 
				- S 包含系统和隐藏文件。 
				- t<日期时间> 把压缩文件的日期设成指定的日期。 
				- T 检查备份文件内的每个文件是否正确无误。 
				- u 更换较新的文件到压缩文件内。 
				- v 显示指令执行过程或显示版本信息。 
				- V 保存VMS操作系统的文件属性。 
				- w 在文件名称里假如版本编号，本参数仅在VMS操作系统下有效。 
				- x<范本样式> 压缩时排除符合条件的文件。 
				- X 不保存额外的文件属性。 
				- y 直接保存符号连接，而非该连接所指向的文件，本参数仅在UNIX之类的系统下有效。 
				- z 替压缩文件加上注释。 
				- $ 保存第一个被压缩文件所在磁盘的卷册名称。 
				- <压缩效率> 压缩效率是一个介于1-9的数值。
	
		- unzip
			- 语 法：unzip [-cflptuvz][-agCjLMnoqsVX][-P <密码>][.zip文件][文件][-d <目录>][-x <文件>] 或 unzip [-Z]
	
			- 补充说明：unzip为.zip压缩文件的解压缩程序。
			
			- 参 数：
				
				- c 将解压缩的结果显示到屏幕上，并对字符做适当的转换。
				
				- f 更新现有的文件。
				
				- l 显示压缩文件内所包含的文件。
				
				- p 与-c参数类似，会将解压缩的结果显示到屏幕上，但不会执行任何的转换。
				
				- t 检查压缩文件是否正确。
				
				- u 与-f参数类似，但是除了更新现有的文件外，也会将压缩文件中的其他文件解压缩到目录中。
				
				- v 执行是时显示详细的信息。
				
				- z 仅显示压缩文件的备注文字。
				
				- a 对文本文件进行必要的字符转换。
				
				- b 不要对文本文件进行字符转换。
				
				- C 压缩文件中的文件名称区分大小写。
				
				- j 不处理压缩文件中原有的目录路径。
				
				- L 将压缩文件中的全部文件名改为小写。
				
				- M 将输出结果送到more程序处理。
				
				- n 解压缩时不要覆盖原有的文件。
				
				- o 不必先询问用户，unzip执行后覆盖原有文件。
				
				- P<密码> 使用zip的密码选项。
				
				- q 执行时不显示任何信息。
				
				- s 将文件名中的空白字符转换为底线字符。
				
				- V 保留VMS的文件版本信息。
				
				- X 解压缩时同时回存文件原来的UID/GID。
				
				- [.zip文件] 指定.zip压缩文件。
				
				- [文件] 指定要处理.zip压缩文件中的哪些文件。
				
				- d<目录> 指定文件解压缩后所要存储的目录。
				
				- x<文件> 指定不要处理.zip压缩文件中的哪些文件。
				
				- Z unzip -Z等于执行zipinfo指令
		- cp src des 复制命令
	
	- 解压缩
		- *.tar 用 tar -xvf 解压
	
		- *.gz 用 gzip -d或者gunzip 解压
	
		- *.tar.gz和*.tgz 用 tar -xzf 解压
	
		- *.bz2 用 bzip2 -d或者用bunzip2 解压
	
		- *.tar.bz2用tar -xjf 解压
	
		- *.Z 用 uncompress 解压
	
		- *.tar.Z 用tar -xZf 解压
	
		- *.rar 用 unrar e解压
	
		- *.zip 用 unzip 解压
	- 改变一个或多个文件的存取模式
		- chmod [options] mode files
		- 只能文件属主或特权用户才能使用该功能来改变文件存取模式。mode可以是数字形式或以who opcode permission形式表示。who是可选的，默认是a(所有用户)。只能选择一个opcode(操作码)。可指定多个mode，以逗号分开。
			- + 提权
				- 比如 +x 赋予执行权
			- http://www.cnblogs.com/younes/archive/2009/11/20/1607174.html
	- 查看文件/目录
		- ll
			- 针对不同的用户可以给出相应的权限(r->读, w->写,x->执行）
				- linux用9位来表示文件的权限，每3位代表	一类用户。
				- 从左到右每组分别表示：属主，组用户，其	它用户。第一位表示文件的类型。
				-  - 普通文件
				   d   目录文件
				   b   块设备文件
				   c   字符设备文件
				   l   符号链
				   p   管道特殊文件
		- ll -h file_name
			- 查看指定文件的信息
	
	- ps
		- ps -aux
			- 显示所有进程信息[更详细]
		- ps -ef
			- 显示所有进程信息
		- ps aux --sort=-pcpu | head -5
			- 通过cpu或者内存使用排序
		- ps -p 3150 -L
			- 显示进程id为3150的进程的所有线程
	
	- scp
		- scp -r srcFile ip:tgtFile
			- 向另一台电脑发文件
	
	- rm
		- rm -rf file/access
			- 删除目录下所有文件，包含子目录
		- rm -r filework
			- 删除目录
		- rm -r *
			- 删除当前目录下的所有文件及目录 
	- source
		-  /etc/profile 
		- 使修改的环境变量立即生效 

2. 环境配置

	- 静态ip
		- 进入/etc/sysconfig/network-scripts目录，找到该接口的配置文件（ifcfg-enp0s3）。如果没有，请创建一个
	- 网络服务
		- 重启
			- systemctl restart network.service

	- win 下压缩文件tar.gz的办法
		- 先压缩成tar，再压缩为gz

	- 重启防火墙：专治能ping不能shell
		- systemctl restart firewalld

3. 可执行文报：段错误(吐核)

	https://blog.csdn.net/qq_42351880/article/details/85332621