# sevlet 3 api #

----------
1. 概述
	1. 什么是servlet？
	
		> servlet 是以java技术为基础，被容器管理，生成动态内容的组件。
		
		> 像其他的java技术，servlet由平台独立的java class 编译成 平台共用的字节码，可以被动态加载和运行在使用java技术的web服务器中。
		
		> 容器，有时候被称为servlet引擎，被web服务器扩展用来提供servlet功能。servlet通过一个被servlet容器实现的请求/响应的实例和web客户端交互。
		
	2. 什么是Container容器？
		
		> servlet 容器 是web服务器或应用程序服务器的组成部分，提供了在网络之上发送请求和响应的服务，编码MIME请求，格式化MIME响应。一个servlet容器包含并管理servlet的生命周期。
		
		> servlet容器可以构建到主机Web服务器中，或通过Web服务器的本地扩展API作为一个组件载入。Servlet容器能被构建或载入到启用web的应用服务器中。
		
		> 所有servlet容器都必须支持HTTP作为请求和响应的协议，但是，其他基于请求/响应的协议，如HTTPS（HTTP over<+> SSL）可能会受到支持。HTTP规范规定容器必须支持HTTP/1.0 和 HTTP/1.1协议。因为容器在RFC2616(HTTP/1.1)中被描述为一个缓存机制，在请求到达servlet之前Servlet Container 可能会修改来自客户端的请求，
  