# J.U.C #

----------
1. 几个方法
	- Thread>join
		- 一直活动直到这个线程死亡
		- join(10)
			- 活动10毫秒消亡
		- t.join()方法阻塞调用此方法的线程，直到线程t完成，此线程再继续 
	- Thread.sleep()
		- 导致当前正在执行的线程休眠（暂时停止执行）指定的毫秒数
	- Thread>yield()
		- 让出cpu使用权，和其他线程重新竞争
	- Thread>start()
		- 启动一个线程
	- Thread>interrupt() 
		- 通知目标线程中断，即设置中断标志
		- 如果当前线程在调用`wait`、`join`、`sleep`时被打断，则当前线程的中断状态则会被清除并抛出`InterruptedException`
	- Thread.interrupted() 
		- 测试当前线程是否被中断【中断为true】，并且会清除当前线程的中断状态 
	- Thread>isInterrupted()
		- 线程的中断状态不受此方法影响

2. 为什么web开发几乎遇不到多线程？

	如果做 java web 方面开发的话几乎用不到多线程！因为有多线程的地方 servlet 容器或者其他开发框架都已经实现掉了！

	一般在网络应用程序中使用多线程的地方非常多！
	
	另外，你说的拷贝文件使用多线程，那是没有用的！以多线程来提高效率的场景一般在 CPU 计算型，而不是在 IO 读写型。CPU 可以会有多个核心并行处理计算，但是磁盘 IO 就没这功能了，磁头只有一个，根本不可能靠多线程提高效率！
	
	一般来说，磁盘 IO 的并发能力为 0，也就是说无法支持并发！网络 IO 的话由于带宽的限制的，使用多线程处理最多也只能达到带宽的极值。
	
	对于磁盘 IO 来说，多线程可以用于一个线程专门用于读写文件，其他的线程用于对读取数据进行处理，这样才有可能更好地利用 CPU 资源。
	
	如果仅仅是单纯的文件复制，使用多线程操作的话，会使用磁头在磁盘上不停地进行寻道操作，使得效率更为低下！

	---------------
	摘自 https://bbs.csdn.net/topics/390037268

3. volatile
	
	只能保证修改变量后其他线程可以感知到，但两个线程同时修改一个变量时还是会有冲突
4. 重(chong)入锁

	相比 synchronized 更灵活
	> 可以手动获得锁和释放锁，如 
	> 
	> reentrantLock.lock()
	>
	> try { i++; }
	>
	> finally{ reentrantLock.unlock() }  

	为什么称为重入锁？
	
	> 因为你可以这样,一个线程*可以多次获得锁，
	> 要求必须释放相同的次数 
	> reentrantLock.lock();
	> reentrantLock.lock();
	>
	> try { i++; }
	>
	> finally{ reentrantLock.unlock();reentrantLock.unlock(); } 

	使用重入锁还有一个好处
	> 重入锁提供中断处理能力
	>>中断响应
	>如果一个线程正在等待锁，那么它仍然可以收到一个通知，
	>被告知无需再等待，可以停止工作了。对于处理死锁有一定的好处。
	>
	> tryLock(时长,单位) 
	> 等待一定时长后获取到锁返回true，否则返回false
	>
	> tryLock() 