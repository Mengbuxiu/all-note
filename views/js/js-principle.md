# js原理 #

1. 网络

	eadyState 是XMLHtttpRreadyState是XMLHttpRequest对象的一个属性，用来标识当前XMLHttpRequest对象处于什么状态。

	readyState总共有5个状态值，分别为0~4，每个值代表了不同的含义
	
	0 || UNSEND  未发送初始化状态： 此时，已经创建了一个 XMLHttpRequest 对象

	1 || OPENED  准备发送状态：此时，已经调用了XMLHttpRequst对象的open方法，并且XMLHttpRequest对象已经准备好一个请求发送到服务器端

	2 || HEADERS_RECEIVED 已经发送状态：此时，已经发送状态：此时，已经通过send方法把一个请求发送到服务器，但是还没有收到一个响应(请求头接收完毕)

	3 || LOADING 正在接收状态，此时，已经接收到HTTP响应头部信息，但是消息体部分还没有全部收到 （请求数据正在加载中)

	4 || DOME 完成响应状态；已经完成了HTTP响应的接收
	
	
	
	status :是XMLHttpRequest对象的一个属性，表示相应的HTTP状态码。
	
	在HTTP1.1协议下，HTTP状态总共可分为5大类常见的我列举一下：
	
	101状态码，表示服务器将通知客户端使用更高版本的HTTP协议。
	200 Ok 文档正确返回
	301 Redirect 永久重定向。一直从其他地方获取资源
	302 Redirect 临时重定向。 临时到其他地方获取资源
	303 see other、307 Temporary Redireact 将客服端重定位到一个负载不大的服务器上，用于负载均衡和服务器失联
	404 Not Found 无法获取这个资源
	500 Internal Server Error 服务器错误

	https://www.jianshu.com/p/17ec86ed7e42

	总结：state + readyState 判断返回是否成功
