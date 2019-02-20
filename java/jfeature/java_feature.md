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

	- 是一个abstract类
	- javadoc这样描述： `A clock providing access to the current instant, date and time using a time-zone.`
	- 简单来说就是一个时钟，提供不同时区的时间，这个类的实例用于确定当前时间，可以使用存储的时区[ZoneId的static块中定义的]来确定当前的时间和日期，因此可以替代 `System.currentTimeMillis()` 和 `TimeZone.getDefault()`
	- 所有的日期时间类都有一个工厂方法*now()*在默认的时区中使用系统时钟 `All key date-time classes also have a * {@code now()} factory method that uses the system clock in the default time zone.`
	- 为保证其它类的运行，这个抽象类必须被小心地实现 `This abstract class must be implemented with care to ensure other classes operate correctly.`
	> The returned instants from {@code Clock} work on a time-scale that ignores leap seconds, as described in {@link Instant}. 
	>> 此处需要注意，从Clock返回的时间不考虑闰秒，对于一些特殊的行业要非常小心！具体详查https://baike.baidu.com/item/闰秒/696742 
	>
	>>解决方案：使用外部的NTP服务器实现此抽象类
	
	- millis()方法可以替代currentTimeMillis(),`Gets the current millisecond instant of the clock.`
	- 大多数应用程序应该避免使用此方法并使用{@link Instant}来表示时间线上的瞬间而不是原始的毫秒值。 提供该方法是为了允许在高性能用例中使用时钟，其中对象的创建是不可接受的[对象不可创建]。`Most applications should avoid this method and use {@link Instant} to represent an instant on the time-line rather than a raw millisecond value. This method is provided to allow the use of the clock in high performance use cases where the creation of an object would be unacceptable.`
 		 