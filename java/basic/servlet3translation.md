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
		
		> 所有servlet容器都必须支持HTTP作为请求和响应的协议，但是，其他基于请求/响应的协议，如HTTPS（HTTP over<+> SSL）可能会受到支持。HTTP规范规定容器必须支持HTTP/1.0 和 HTTP/1.1协议。因为容器在RFC2616(HTTP/1.1)中被描述为一个缓存机制，在请求到达servlet之前Servlet Container 可能会修改来自客户端的请求，container在将响应发送到客户端之前可能会修改由servlet产生的响应，或者可能在不遵守RFC2616规范的情况下响应请求将请求发送到servlet。
		
		> servlet容器可能会对servlet执行的环境设置安全限制。在Java平台，标准版（J2SE，v.1.3或更高版本）或Java中
		在Platform，Enterprise Edition（Java EE，v.1.3或更高版本）环境中，应使用Java平台定义的权限体系结构放置这些限制。 例如，高端应用程序服务器可能会限制Thread对象的创建，以确保容器的其他组件不会受到负面影响。

		> Java SE 6是必须构建servlet容器的底层Java平台的最低版本。
  
	3. web容器请求过程

		> 1.客户端（例如，Web浏览器）访问Web服务器并发出HTTP请求。
		
		> 2.请求由Web服务器接收并传递给servlet容器。servlet容器可以在与主机Web服务器相同的进程中运行，在同一主机上的不同进程中运行，也可以在与其处理请求的Web服务器不同的主机上运行。
		
		> 3.servlet容器根据其servlet的配置确定要调用的servlet，并使用表示请求和响应的对象调用它。
		
		> 4.servlet使用请求对象来查找远程用户是谁，可能已将HTTP POST参数作为此请求的一部分发送，以及其他相关数据。 servlet执行它编程的任何逻辑，并生成数据以发送回客户端。它通过响应对象将此数据发送回客户端。
		
		> 5.一旦servlet处理完请求，servlet容器就会确保正确刷新响应，并将控制权返回给主机Web服务器。

	4. 比较servlet和其他技术

		在功能方面，servlet位于通用网关接口（CGI）程序和专有服务器扩展（如Netscape Server API（NSAPI）或Apache模块）之间。与其他服务器扩展机制相比，Servlet具有以下优势：

		- 它们通常比CGI脚本快得多，因为使用了不同的过程模型。
		- 它们使用许多Web服务器支持的标准API。
		- 它们具有Java编程语言的所有优点，包括易于开发和平台独立性。
		- 他们可以访问Java平台可用的大量API。