# MySQL 知识点 #

----------
1. 内连接、左外、右外的区别

   ![](https://i.imgur.com/JyRVwiK.png)
   ![](https://i.imgur.com/th6hkNX.png)
   ![](https://i.imgur.com/lpfARSE.png)
	> 从图里看的很明显，所谓的内连接、左外、右外其实质就是数学中的交、并集

2. mysql系统执行流程

	![](https://i.imgur.com/tkwylOY.png)

3. 函数

	无参函数
	> mysql> CREATE FUNCTION f1() RETURNS VARCHAR(30) -> enter
	> 
    > RETURN DATE_FORMAT(NOW(),'%Yyear%mmonth%dday %Hhour%mminute%ssecond');
	
	调用 
	SELECT f1();
	
	有参函数
	![](https://i.imgur.com/cVG5HIe.png)

	函数体

	> 由合法sql组成
	> 
	> 函数体可以是简单的SELECT或INSERT语句
	> 
	> 复合结构[超过两个以上的语句]用BEGIN...END包含
	> 
	> 复合结构可以包含声明、循环、控制结构等等
 
4. 存储过程

	存储过程是SQL语句和控制语句的预编译集合，以一个名称存储并作为一个单元处理
	
	优点
	>  增强SQL语句的功能和灵活性 
	>> 因为可以写判断条件
	>
	>  实现较快的执行速度 
	>> [预编译] 第一次运行时和以前一样，第二次直接从内存获取  
	>          
	>  减少网络流量 
	>> 比如删用户， delete userid from table where userid = 3，传输的是这一整条语句。
	>> 如果使用存储过程，我们只需要把存储过程的名称以及用户id传过去就可以。
	   
	过程体
	> 由合法sql组成，可以是“任意”sql
	>> 加引号是因为不能由过程体创建数据表和数据库，任意是指CRUD和多表连接操作
	>
	> 复合结构[超过两个以上的语句]用BEGIN...END包含
	> 复合结构可以包含声明、循环、控制结构等等

	无参存储过程
	> CREATE PROCEDURE sp1() SELECT VERSION()
	> 
	> CALL sp1() / CALL sp1
	
	有参存储过程

	修改默认定界符 [ ; -> // ]
	> DELIMITER //
	> 
	> CREATE DEFINER = CURRENT_USER PROCEDURE `removeUserById`(IN `id` int)
	> 
	> BEGIN
	> 
	> DELETE FROM users WHERE id = id
	> 
	> END
	> 
	> //
	> 
	> DELIMITER ;   修改默认定界符 [ // -> ;]

	IN-OUT-存储过程
	> 注意使用INTO对接参数	
	  ![](https://i.imgur.com/T7jAgT1.png)
	  CALL removeUserAndReturnUserNums(userid,@name)

	变量
	> 局部变量：DECLARE declare @id int,@name varchar(20),
	> 
	>> 存活于 BEGIN...END 之间,且使用DECALRE要放在第一行
	>
	> 全局变量：@param_name
	> 
	>> 全局有效
	>
	>用户变量： set @i = 1
	>
	>> 只对当前的mysql用户生效
       
5. 存储过程和函数的区别

	- 存储过程实现的功能更复杂；函数的针对性更强
	- 存储过程可以返回多个值；函数只能返回一个值
	- 存储过程一般独立来运行；函数可以作为其他sql的一部分来出现

6. CRUD
	> insert
	>> INSERT INTO 表名（字段名1，字段名2，…）VALUES（值1，值2，…）
	>
	>> INSERT INTO 表名 SET 字段名1=值1[,字段名2=值2，…]
	>
	> DELETE FROM 表名 [WHERE 条件表达式]
	> 
	> TRUNCTE [TABLE ] 表名
	>> - DELETE 后面可以跟WHERE子句指定删除部分记录，TRUNCATE只能删除整个表的所有记录
    >> - 使用TRUNCATE语句删除记录后，新添加的记录时，自动增长字段（如本文中student表中的 id 字段）会默认从1开始，而使用DELETE删除记录后，新添加记录时，自动增长字段会从删除时该字段的的最大值加1开始计算（即原来的id最大为5，则会从6开始计算）。所以如果是想彻底删除一个表的记录而且不会影响到重新添加记录，最好使用TRUNCATE来删除整个表的记录。 
    >
	> UPDATE 表名 SET 字段名1=值1，[ 字段名2=值2，…] [ WHERE 条件表达式 ]
	
7. 存储引擎

	MySQL可以将数据以不同的技术存储在文件(内存)中，这种技术就称为存储引擎。
	每种存储引擎使用不同的存储机制、索引技巧、锁定水平，最终提供广泛且不同的功能。

	- MyISAM
	- InnoDB
	- Memory
	- CSV
	- Archive
	
	并发控制
	- 锁
		- 共享锁(读锁)
			> 在同一时间段内，多个用户可以读取同一个资源，读取过程中数据不会发生任何改变
		- 排他锁(写锁)
			> 在任何时候只能有一个用户写入资源，当进行写锁时会阻塞其他的读锁或写锁操作
		- 会有额外的系统开销，需在开销与系统安全之间寻求平衡
		
	- 锁颗[锁的力度，即精确加锁]
	
	  	- 表锁
		  	> 开销小的锁策略
	  	- 行锁
		  	> 开销大的锁策略
	  	
	事务[保证数据的完整性]

	- 特性
		- 原子性(Atomicity)
		- 一致性(Consistency)
		- 隔离性(Isolation)
		- 持久性(Durability)

	索引[对数据表中的一列或者多列进行排序的一种结构]

	索引类型
	- FULLTEXT
		- 全文索引，目前只有MyISAM引擎支持。其可以在CREATE TABLE ，ALTER TABLE ，CREATE INDEX 使用，不过目前只有 CHAR、VARCHAR ，TEXT 列上可以创建全文索引。
	- HASH
		- 由于HASH的唯一（几乎100%的唯一）及类似键值对的形式，很适合作为索引。
		- HASH索引可以一次定位，不需要像树形索引那样逐层查找,因此具有极高的效率。但是，这种高效是有条件的，即只在“=”和“in”条件下高效，对于范围查询、排序及组合索引仍然效率不高。
	- BTREE
		- BTREE索引就是一种将索引值按一定的算法，存入一个树形的数据结构中（二叉树），每次查询都是从树的入口root开始，依次遍历node，获取leaf。这是MySQL里默认和最常用的索引类型。
	- RTREE
		- RTREE在MySQL很少使用，仅支持geometry数据类型，支持该类型的存储引擎只有MyISAM、BDb、InnoDb、NDb、Archive几种。相对于BTREE，RTREE的优势在于范围查找。
		
	索引种类
	- 普通索引：仅加速查询

	- 唯一索引：加速查询 + 列值唯一（可以有null）

	- 主键索引：加速查询 + 列值唯一（不可以有null）+ 表中只有一个

	- 组合索引：多列值组成一个索引，专门用于组合搜索，其效率大于索引合并

	- 全文索引：对文本的内容进行分词，进行搜索

	- 索引合并，使用多个单列索引组合搜索
 
	- 覆盖索引，select的数据列只用从索引中就能够取得，不必读取数据行，换句话说查询列要被所建的索引覆盖

	> https://blog.csdn.net/liutong123987/article/details/79384395

	各种存储引擎的优缺点
	![](https://i.imgur.com/NYPWcKU.png)

	引擎特点分析

	- 存储限制
		
		- InnoDB 明显数据量较少，Memory[内存]，从字面即可知，数据存在内存中

	CSV存储引擎
	- 特点：
		- 没有索引、不能为NULL、不能自增

		- 更新和删除时先写入到临时文件，然后在rnd_end()函数中重新生成数据文件

	- 应用场景：

		- 数据存储为CSV文件格式，不用进行转换

	BlackHole[黑洞引擎]
	- 特点
		- 写入数据都会消失
	- 应用场景
		- 一般用于数据复制的中继 

	如何修改MySQL存储引擎
	- 修改MySQL配置文件
		- default-storage-engine = engine 修改的是默认支持引擎
	- 创建数据表时特定修改
		- CREATE TABLE tb1(s1 VARCHAR(10)) ENGINE = `MyISAM`; 
	- 表建好后修改数据表引擎
		- ALTER TABLE table_name ENGINE[=] engine_name
	
8. 死锁

	![](https://i.imgur.com/QcghFhp.png)
	innodb是怎么探知死锁的？

	> 直观方法是在两个事务相互等待时，当一个等待时间超过设置的某一阀值时，对其中一个事务进行回滚，另一个事务就能继续执行。这种方法简单有效，在innodb中，参数innodb_lock_wait_timeout用来设置超时时间。

    仅用上述方法来检测死锁太过被动，innodb还提供了`wait-for graph`算法来主动进行死锁检测，每当加锁请求无法立即满足需要并进入等待时，wait-for graph算法都会被触发。

	`wait-for graph`原理是什么？
	
	比如有四辆车，互相首尾连接，这就是死锁
	![](https://i.imgur.com/oB3eJ2I.png)

	我们怎么知道上图中四辆车是死锁的？
	> 他们相互等待对方的资源，而且形成环路！我们将每辆车看为一个节点，当节点1需要等待节点2的资源时，就生成一条有向边指向节点2，最后形成一个有向图。我们只要检测这个有向图是否出现环路即可，出现环路就是死锁！这就是wait-for graph算法。
	> 
	> https://www.cnblogs.com/LBSer/p/5183300.html

9. 存储过程优缺点


	优点　　	

	- 运行速度：对于很简单的sql，存储过程没有什么优势。对于复杂的业务逻辑，因为在存储过程创建的时候，数据库已经对其进行了一次解析和优化。存储过程一旦执行，在内存中就会保留一份这个存储过程，这样下次再执行同样的存储过程时，可以从内存中直接调用，所以执行速度会比普通sql快。
	
	- 减少网络传输：存储过程直接就在数据库服务器上跑，所有的数据访问都在数据库服务器内部进行，不需要传输数据到其它服务器，所以会减少一定的网络传输。但是在存储过程中没有多次数据交互，那么实际上网络传输量和直接sql是一样的。而且我们的应用服务器通常与数据库是在同一内网，大数据的访问的瓶颈会是硬盘的速度，而不是网速。
	
	- 可维护性：的存储过程有些时候比程序更容易维护，这是因为可以实时更新DB端的存储过程。  有些bug，直接改存储过程里的业务逻辑，就搞定了。
	
	- 增强安全性：提高代码安全，防止 SQL注入。这一点sql语句也可以做到。
	 
	- 可扩展性：应用程序和数据库操作分开，独立进行，而不是相互在一起。方便以后的扩展和DBA维护优化。
	
	缺点

	- SQL本身是一种结构化查询语言，但不是面向对象的的，本质上还是过程化的语言，面对复杂的业务逻辑，过程化的处理会很吃力。同时SQL擅长的是数据查询而非业务逻辑的处理，如果如果把业务逻辑全放在存储过程里面，违背了这一原则。
	
	- 如果需要对输入存储过程的参数进行更改，或者要更改由其返回的数据，则您仍需要更新程序集中的代码以添加参数、更新调用，等等，这时候估计会比较繁琐了。
	
	- 开发调试复杂，由于IDE的问题，存储过程的开发调试要比一般程序困难。
	
	- 没办法应用缓存。虽然有全局临时表之类的方法可以做缓存，但同样加重了数据库的负担。如果缓存并发严重，经常要加锁，那效率实在堪忧。
	
	- 不支持群集，数据库服务器无法水平扩展，或者数据库的切割（水平或垂直切割）。数据库切割之后，存储过程并不清楚数据存储在哪个数据库中。
	
	总结

	-  适当的使用存储过程，能够提高我们SQL查询的性能。
	
	-  存储过程不应该大规模使用，滥用。
	
	-  随着众多ORM 的出现，存储过程很多优势已经不明显。
	
	-  SQL最大的缺点还是SQL语言本身的局限性——SQL本身是一种结构化查询语言，我们不应该用存储过程处理复杂的业务逻辑——让SQL回归它“结构化查询语言”的功用。复杂的业务逻辑，还是交给代码去处理吧

	> https://www.cnblogs.com/zhangweizhong/p/3871785.html

9. mysql远程登陆

	mysql>GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%'IDENTIFIED BY 'mypassword' WITH GRANT OPTION; 

  	如果是固定ip就这么写
 	mysql>grant all privileges on *.* to 'root'@'192.168.0.49'identified by '123' with grant option;

	推送设置到内存或重启服务器也行
　　mysql>FLUSH PRIVILEGES 