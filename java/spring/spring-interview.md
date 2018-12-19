1.请你谈一下对spring的理解
	spring简单来说就是一个bean工厂，用来管理bean的生命周期和框架集成。
	常用的地方有 
	
		DI[依赖注入]，也叫IOC/[控制反转]。
		DI的方式有三种，
	
		1是set注入，比如有个Person类，你想给他注入技能skill类
		那么你可以在实例化Person类的基础上调用person类的set方法
		person.set(new skill())，在person类里有相应的set方法对
		Skill类初始化。
		不推荐，耦合度太高。
	
		2是构造函数注入，在实例化类的时候通过构造函数初始化skill类，虽然比set注入要好一点，但还是耦合度较高。
	
		3就是使用spring框架自带的BeanFactory，在你想要注入类上加@Autowired/@Inject注解[使用方式和Autowired一样，不同的是@Autowired有一个request属性，默认为true，改为false时spring找不到对应的bean时不会报错]
	
		@Autowired一般和 @Configuration[代表这个类由spring容器进行管理]、@ComponentScan[包扫描，默认当前包开始扫描](basepackages=”path”)组合使用。
	
		官方注释给出的解释是 由Spring的依赖注入工具自动装配
	
		当然也可以通过配置文件注入，比如在<bean>标签中 写 id='xxx' class='package_path' 不过很少用了，毕竟注解更直观些。
		
		DI最大的好处就是解耦

----------


	然后就是 AOP
	
	AOP意思是面向切面编程，有两种实现方式，一种是XML，一种是注解。目的是将系统中非核心的业务提取出来，进行单独处理。比如事务、日志和安全等
	拿日志来说，
		用Aspect注解声明一个切面类，在这个类中声明切点[pointcut]以及通知[after/before/around..]同时还要在配置文件中配置你用的是什么代理
		JoinPoint是用来在通知中获取方法名称以及参数的


----------
# springMvc #
1. 处理流程
![](https://i.imgur.com/dslMVgb.png)
	1、dispatcherServlet:是核心控制器，客户端所有的请求都发送到这   个servlet，并且由这个servlet进行请求的分发。
	springMVC不提倡页面到页面的直接跳转，要求都经过servlet跳转。
	
	2、handlerMapping：映射器。dispatcherServlet负责分发请求到不同的servlet中，根据handlerMapping映射器进行分发。
	
	3、控制器：这个是需要程序员编写的组件。（有3件事要做：接收请求获取参数、调用业务逻辑、页面导航）
	
	4、ModelAndView:模型视图。Model模型负责保存数据，view实现页面导航。直接使用这个spring中已经提供好的类。
	
	5、ViewResolver:视图解析器。页面导航的时候处理路径问题。这个视图解析器在springMVC的配置文件中使用。
	
	6、View：视图。即页面
