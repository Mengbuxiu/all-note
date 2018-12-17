#Session#
> session 指 服务器上维护的session域

1. 早期是基于 tomcat + redis 的方式做的
	
	就是用一个叫做Tomcat RedisSessionManager的东西，让所有我们部署的tomcat都将session数据存储到redis即可。
	在tomcat的配置文件中，配置一下

	> 严重耦合。比如我要把tomcat缓存jetty，你再配一遍？

2. 推荐基于 springsession + redis

	> 解耦。现在spring是主流，spring session 是一种更优的选择。
	
	
	pom.xml
	
	<dependency>
	  <groupId>org.springframework.session</groupId>
	  <artifactId>spring-session-data-redis</artifactId>
	  <version>1.2.1.RELEASE</version>
	</dependency>
	<dependency>
	  <groupId>redis.clients</groupId>
	  <artifactId>jedis</artifactId>
	  <version>2.8.1</version>
	</dependency>
	
	spring配置文件中
	
	<bean id="redisHttpSessionConfiguration"
	     class="org.springframework.session.data.redis.config.annotation.web.http.RedisHttpSessionConfiguration">
	    <property name="maxInactiveIntervalInSeconds" value="600"/>
	</bean>
	
	<bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
	    <property name="maxTotal" value="100" />
	    <property name="maxIdle" value="10" />
	</bean>
	
	<bean id="jedisConnectionFactory"
	      class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory" destroy-method="destroy">
	    <property name="hostName" value="${redis_hostname}"/>
	    <property name="port" value="${redis_port}"/>
	    <property name="password" value="${redis_pwd}" />
	    <property name="timeout" value="3000"/>
	    <property name="usePool" value="true"/>
	    <property name="poolConfig" ref="jedisPoolConfig"/>
	</bean>
	
	web.xml
	
	<filter>
	    <filter-name>springSessionRepositoryFilter</filter-name>
	    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
	</filter>
	<filter-mapping>
	    <filter-name>springSessionRepositoryFilter</filter-name>
	    <url-pattern>/*</url-pattern>
	</filter-mapping>
	
	示例代码
	
	@Controller
	@RequestMapping("/test")
	public class TestController {
	
	@RequestMapping("/putIntoSession")
	@ResponseBody
	    public String putIntoSession(HttpServletRequest request, String username){
	        request.getSession().setAttribute("name",  “leo”);
	
	        return "ok";
	    }
	
	@RequestMapping("/getFromSession")
	@ResponseBody
	    public String getFromSession(HttpServletRequest request, Model model){
	        String name = request.getSession().getAttribute("name");
	        return name;
	    }
	}
 

![](https://i.imgur.com/iPQwmuo.png)