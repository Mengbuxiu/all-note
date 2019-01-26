# JAVA特性学习之 java.time.* #

----------
1. Ser.java 

	- implements Externalizable
		- extends java.io.Serializable

	- 该类用于日期序列化时向流中写入一个byte的数据类型，反序列化时限读取byte，再根据byte用相应的 类.readExternal进行处理
	
	![](https://i.imgur.com/OpSMa7o.png)

	- 由图可知，这些常量定义为byte类型
	- javadoc中也写到：
		`Each serializable class is mapped to a type that is the first byte in the stream. `
	- 每个序列化的类被映射到一个类型，而图片中定义的byte是流中的第一个字节。

2. Clock.java

	- abstract 
	- javadoc这样描述： `A clock providing access to the current instant, date and time using a time-zone.`
	- 简单来说就是一个时钟，提供不同时区的时间，这个类的实例用于确定当前时间，可以使用存储的时区[应该指的是自定义配置文件中的]来确定当前的时间和日期，因此可以替代 `System.currentTimeMillis()` 和 `TimeZone.getDefault()`