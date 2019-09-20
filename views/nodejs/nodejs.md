# 前后端分离系统的踩坑 #

----------
1. cnpm、npm、yarn

	> https://blog.csdn.net/qq_32614411/article/details/80894605
	> 
	> 总结一下，cnpm、npm、yarn都是nodejs的包管理工具，区别在于，npm是nodejs的亲儿子，从国外服务器下载依赖，所以有时会很慢，cnpm属于国内淘宝的镜像包管理工具，yarn是经过重新设计的崭新的npm客户端，由一些高级开发人员维护。
	> 
	> 我为什么要做一点记录？
	> 
	> 因为合作开发时如果你的队友用cnpm而你用npm，可能会引发一系列不可预测的问题。。。通常是版本异常 :(

2. package-lock.json是什么？

	> 锁定安装时的包的版本号，并且需要上传到git，以保证其他人在npm install时大家的依赖能保证一致。 
	> 
	> https://www.cnblogs.com/cangqinglang/p/8336754.html