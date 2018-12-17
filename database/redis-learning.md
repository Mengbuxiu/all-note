# reids 知识点 #
1. 常见数据结构及应用场景
	
	**string** 普通的set和get，做简单的kv缓存

	**hash** 类似map的一种结构，主要是用来存放一些对象，把一些简单的对象给缓存起来，后续操作的时候，你	可以直接仅仅修改这个对象中的某个字段的值,操作hset，hget，hlen等等

	**list** 有序列表，重要命令 lrange，做分页，但是要考虑其性能

	**set** 无序集合，自动去重
		如果你需要对一些数据进行快速的全局去重，你当然也可以基于jvm内存里的HashSet进行去重，但是如果你的某个系统部署在多台机器上呢？`得基于redis进行全局的set去重`
		
		场景	
			可以基于set玩儿交集、并集、差集的操作，比如交集吧，可以把两个人的粉丝列表整一个交集，看看俩人的共同好友是谁
	
	**sorted set** 排序的set，去重但是可以排序，写进去的时候给一个分数，自动根据分数排序，这个可以玩	儿很多的花样，最大的特点是有个分数可以自定义排序规则
		
		比如说你要是想根据时间对数据排序，那么可以写入进去的时候用某个时间作为分数，人家自动给你按照时间排序了

		排行榜：将每个用户以及其对应的什么分数写入进去，zadd board score username，
		接着zrevrange board 0 99，就可以获取排名前100的用户；zrank board username，
		可以看到用户在排行榜里的排名


2. redis过期策略
	> 定期删除 + 惰性删除
	>> 所谓定期删除，指的是redis默认是每隔100ms就随机抽取一些设置了过期时间的key，检查其是否过期，如果过期就删除。
	>
	>>在你获取某个key的时候，redis会检查一下 ，这个key如果设置了过期时间那么是否过期了？如果过期了此时就会删除，不会给你返回任何东西。并不是key到时间就被删除掉，而是你查询这个key的时候，redis再懒惰的检查一下。
	
	>通过上述两种手段结合起来，保证过期的key一定会被干掉。

	如果定期删除漏掉了很多过期key，然后你也没及时去查，也就没走惰性删除，此时会怎么样？如果大量过期key堆积在内存里，导致redis内存块耗尽了，咋整？

	> 答案是：走内存淘汰机制。
	 
	    1）noeviction：当内存不足以容纳新写入数据时，新写入操作会报错，这个一般没人用吧，实在是太恶心了	
	   
		2）allkeys-lru：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的key（这个是最常用的）

		3）allkeys-random：当内存不足以容纳新写入数据时，在键空间中，随机移除某个key，这个一般没人用吧，为啥要随机，肯定是把最近最少使用的key给干掉啊

		4）volatile-lru：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，移除最近最少使用的key（这个一般不太合适）

		5）volatile-random：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，随机移除某个key

		6）volatile-ttl：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，有更早过期时间的key优先移除

3. java版LRU算法实现
	
		public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    
			private final int CACHE_SIZE;
	
		    // 这里就是传递进来最多能缓存多少数据
		    public LRUCache(int cacheSize) {
			/* 这块就是设置一个hashmap的初始大小，同时最后一个true指的是让linkedhashmap按照访问顺序来进行排序，最近访问的放在头，最老访问的就在尾*/
		        super((int) Math.ceil(cacheSize / 0.75) + 1, 0.75f, true); 
		        CACHE_SIZE = cacheSize;
		    }
		
		    @Override
		    protected boolean removeEldestEntry(Map.Entry eldest) {
		        return size() > CACHE_SIZE; // 这个意思就是说当map中的数据量大于指定的缓存个数的时候，就自动删除最老的数据
		    }
		} 
	


4. redis与memcached
	
	> 区别：
	>> Redis支持服务器端的数据操作：Redis相比Memcached来说，拥有更多的数据结构和并支持更丰富的数据操作，通常在Memcached里，你需要将数据拿到客户端来进行类似的修改再set回去。这大大增加了网络IO的次数和数据体积。
	>
	>> Redis在存储小数据时比Memcached性能更高。而在100k以上的数据中，Memcached性能要高于Redis
	>
	>> memcached没有原生的集群模式，需要依靠客户端来实现往集群中分片写入数据；但是redis目前是原生支持cluster模式的

	redis的线程模型![](https://i.imgur.com/SBDuYsL.png)

5. redis如何负担 10W+ QPS[每秒查询率]？
	> 注：如果有人跟你说redis不能支撑高并发，那一定是因为他在单机上使用了redis。
	
	主从架构 -> 读写分离 -> 支撑10万+读QPS的架构
	![](https://i.imgur.com/sdZtcjl.png)
