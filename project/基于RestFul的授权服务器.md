# 如何设计一个接口系统? #
2019/4/23 9:31:54 

----------
1. 环境
	
	jdk8，spring，springmvc，hibernate，sqlite3

2. 如何应用restful？

	所谓的restful就是在调用方来调用你的接口时根据请求方式就能知晓调的接口是用来干啥的。
	比如 
	> get方式请求/user/info加上用户的id，就知道这个接口是用来请求某个用户的身份信息的，
	> 
	> post是用来创建或更新，delete是用来删除的
	
	见名知意

3. 约定

	一个良好的接口系统必然有规范的返回信息，比如
	> {"status":0,"error":"ok","info":"xxx"}
	
	这是正常的返回信息，不正常的该怎么返回？
	> springmvc提供全局的异常处理机制：SimpleMappingExceptionResolver继承这个类
	> 并重写resolveException这个方法

	> springboot 中则更为方便，类注解@ControllerAdvice + 方法注解 @ExceptionHandler(Exception.class)即可做到捕捉controller异常和ajax异常，
	> 如果仅仅是捕捉controller异常则用@RestControllerAdvice即可

	`其实最好是提供一个公共的返回信息处理类`

4. 日期

	大多数接口中都含有日期，需对日期进行处理，内部存储以long型较好，方便比较日期大小，注意日期的转换加减，可以用jdk8处理

5. 判空

	接口传入的json转换为JSONObject时如何来判空？如果每个方法内使用前都进行一次判空会显得代码很繁琐，那该如何处理？
	> 写一个私有方法[递归]，对传入的参数进行循环校验，如果有某个参数为null，直接获取key然后抛个异常

6. 登陆

	如何保证登陆安全？
	> 用户名和密码前端md5加密并异或，对于短位密码可以防止字典爆破和哈希碰撞
	> 这样在后台比较的就是异或后的值
	
	> 但是这样还有一个问题，客户端是能看到你的js的，也就是说你的规则不是不可破解的，
	
	那这样该怎么做？
	> 最简单的办法 http -> https，这样传输时的数据就看不到了
	>> 参考 [https与http的区别](https://www.cnblogs.com/taomylife/p/4778006.html)
	
	>> [get和post的区别](https://segmentfault.com/a/1190000018129846#articleHeader0)

	>> 单纯的把请求方式改为表单post提交并不能保证数据的安全，至于为什么，自己写个程序抓个包看看就知道了，从抓包结果就可以看出来明显的区别是get的请求数据是在请求头里放着的，post是在请求体里存放的，但都是`可见的`。
	
	> [js混淆](https://www.zhihu.com/question/47047191)，他人看你的js代码会费很多精力

	>> 但值得一提的是，混淆也是能看到你代码里的逻辑的，只不过相对于你不混淆禁用js压缩而言要更加的耗费精力

	如何防止伪造cookie越权？
	> 采用时间戳hash的方式，在请求返回给客户端时将时间戳写入到cookie中，即便在有权限用户登陆的过程中，权限数据包被抓取了，恶意人员想要利用cookie模拟登陆时，可以判断cookie中的请求时间，如果判定是N之前的就可以拦截这个请求了。时间戳写入cookie中甚至不用加密，直接在写入时间的逻辑中指定减去或加上几个小时，这样即便别人利用这个cookie时间戳也会失效。`保险起见，最好还是做一个加密`。

----------
# 顺便再记录一下项目遇到比较诡异的情况 #
1. 项目用sqlite3，配置jdbc.url的路径是application.properties中进行的，jdbc.url	
 
	 我是这样写的

	> jdbc.url = jdbc:sqlite:/name.sqlite

	乍一看好像没啥问题。但是接口调用方在进行接口测试时有进行服务重启，偶然的情况下，重启会导致数据查不到，我认为是重启导致了数据丢失，请教过经理后思考应该不是数据库本身的问题，于是我自己本地部署了一下服务，然后去tomcat/bin目录下启动服务时偶然发现一个文件：`name.sqlite`。惊呆了我的天，为什么...  :-(
	好吧。在我写下为什么三个字时脑子中灵光一现，突然想到之前的name.sqlite，因为这个文件在程序首次启动时会自动创建，而它创建的位置是和pom.xml是同一个位置的，虽然还是不知道具体什么原因导致的，目前只能想到这么多了

	> jdbc.url 仅仅是告诉程序去哪里读数据库文件

	classpath是什么？
	> idea中一般把编译后的文件目录称为target下一般会有个classes目录，这就是所谓的classpath，在程序中一般在创建文件时会用到path，比如 File f = new File("/path");

	*这个时候的path就是指的项目根目录，也就是pom.xml所在的那一级目录，*日后再更
	
	那如果我想写道某个指定目录怎么办？
	> 类.class.getResource 可以获取编译后的classpath目录，然后可以随意写目录了，
	> 程序读取的是编译后的文件[`target`]的目录，不是你看到的程序目录

2. 项目里用的JdbcTemplate，查询就是queryForMap，但是sqlite的默认值设置似乎不生	效，所以有的字段默认值是null，用map查询出来时如果某字段是null就会出现没有相应的	key，造成这种bug。

	解决方式
	> hibernate有@Column注解，实体设置 name="key"，nullable=fasle，
	> columnDefinition="INT default 0"
	