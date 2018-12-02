# spring笔记 —— 《spring in action》 #

2018/12/1 14:30:37 

1. 什么是spring？

	> spring是一个开源框架，是用来简化java开发的。

2. spring如何简化java开发？
	
	> 
		- 基于pojo的轻量级和最小侵入性编程
		- 通过依赖注入和面向接口实现松耦合
		- 基于切面和惯例进行行声明式编程
		- 通过切面和模板减少样板式代码

3. 我们常叨逼叨的“高内聚，低耦合”为什么耦合是低而不是无？
 
	> 任何事物皆有两面性。
	 耦合具有两面性，紧密耦合代码难以测试、复用，并呈现出‘打地鼠式’的bug，另一方面，完全不耦合的代码什么也做不了。

4. spring如何控制耦合度？
	
	*依赖注入[DI]*

	> 通过DI，对象的依赖关系将由系统中负责协调各对象的第三方组件在创建对象的时候进行设定。
	>> 依赖注入会将所依赖的关系自动交给目标对象，而不是让对象自己去获取依赖。
	>
	> 对象无需自行创建或管理它们中的依赖关系。
	>> 以前没有spring是对象自行创建或管理自己的依赖关系，现在统一由spring监管。是不是很像秦始皇时推行的中央集权制？:）
	
	> 依赖注入的方式
	>> 构造器注入，注入接口，那么实现接口的类就可随意替换
	>
	>> setter/getter注入
	>
	>> 注解注入


5. spring装配`组件间的协作行为`方式？
	> 自动装配+自动扫描
	>>  @Autowired：自动装配，查找容器中满足条件的Bean，注入。
	>>  @Configuration+ @ComponentScan(basepackages=”main”) : config.java文件中使用，制定扫描基本包，作为Spring上下文的配置文件。
	> 
	> java代码装配bean
	>> 区别方法一：不需要事先定义好装配策略，因为会在config.java中定义。
	> 
	> XML
	>> @ImportResource("config/config.xml")
	>> ClassPathXmlApplicationContext 加载位于应用程序类路径下的一个或多个XML配置文件
	>>> <pre class="prettyprit lang-java">
	ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("/xml_name");
	ClassName name = ctx.getBean(ClassName.class);
	</pre>
	
	更多关于 DI 的 书籍，Hhanji R.Prasanna 《Dependency Injection》,覆盖依赖注入所有的内容。 —— 不是我说的 :]
	

	----------

6. 面向切面编程

	> 诸如 `日志、事务管理、安全`等这样的系统服务经常融入到自身具有的核心业务逻辑组件中去，这些系统服务通常被称为`横切关注点`，因为它们会跨越系统的多个组件。
	>> 在整个系统中，关注点（例如日志和安全）的调用经常散布到各个模块中，而这些关注点并不是模块的核心业务 
	> 
	> spring 如何解决这种问题？
	> 
	>> AOP.AOP能够使这些服务`模块化`，并以`声明`的方式将它们应用到它们需要`影响的组件`中去。所造成的结果就是这些组件会有`更高的内聚性并且会更加关注自身的业务`
	
	xml 配置 AOP使用案例
	![](https://i.imgur.com/RmpHUEr.png)
	> 也就是在执行函数 embarkOnQuest 之前调用 singBeforeQuest 之后调用 singBeforeQuest

7. 使用spring模板消除样板式代码（`重复代码`）
	> `模样板式代码举栗子`
	> ![](https://i.imgur.com/m8yL4mJ.png)
	>
	> 再举个栗子，spring的JdbcTemplate（利用java5特性JdbcTemplate实现）
	> ![](https://i.imgur.com/AQIN4Zd.png)
	>
	> 二者差别可见一斑
	
8. spring容器

	> 基于spring的应用中，你的应用对象生存于spring容器中。
	> spring容器负责创建对象，装配，配置并管理它们的整个生命周期，从生存到死亡
	>
	>> {new -> finalize()}
	>
	> spring自带多个容器实现,可以归纳为两种不同的类型
	>> bean工厂[接口定义：beans.factory.beanFactory]是最简单的容器，提供基本的DI支持
	>
	>> 应用上下文[接口定义：context.ApplicationContext]基于BeanFactory构建，并提供应用框架级别的服务，例如从属性文件解析文本信息以及发布应用时间给感兴趣的事件监听者
	>
	> bean工厂对大多数应用太低级，因此，应用上下文比bean工厂更受欢迎

9. 常见的应用上下文

	> 基于java配置类加载spring应用上下文、spring web应用上下文
	> 
	> 1.AnnotationConfigApplicationContext、2.AnnotationConfigWebApplicationContext
	>
	> 类路径[all]下的XML加载上下文定义，把应用上下文的定义文件作为类资源
	> 
	> 3.ClassPathXmlApplicationContext
	> 
	> 从文件系统路径[指定的]下的XML加载上下文定义
	> 
	> 4.FileSystemXmlApplicationContext
	> 
	> 从web应用下的XML加载上下文定义
	> 
	> 5.XmlWebApplicationContext

	**所谓的应用上下文就是spring容器嘛，这个容器可以比喻为一个盒子，盒子用来管理java类的整个生命周期，盒子上有个门，即 `context.getBean()`，这个方法可以获取到容器内的java类实例。**
	`Return the bean instance that uniquely matches the given object type, if any.`