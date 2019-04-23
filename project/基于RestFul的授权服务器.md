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

4. 日期

	大多数接口中都含有日期，需对日期进行处理，内部存储以long型较好，方便比较日期大小，注意日期的转换加减，可以用jdk8处理

5. 判空

	接口传入的json转换为JSONObject时如何来判空？如果每个方法内使用前都进行一次判空会显得代码很繁琐，那该如何处理？
	> 写一个私有方法，对传入的参数进行循环校验
	