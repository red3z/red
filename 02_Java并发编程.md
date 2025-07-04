# 线程状态切换的方法

| Thread类, 和锁没关系.                                        | Object作为锁的时候(synchronized)                             | LockSupport本质是Unsafe.park()  | Condition       |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------- | --------------- |
| static sleep() 阻塞当前线程.                                 | 锁对象.wait() 释放锁并阻塞, 等待别的线程调用notify()         | LockSupport.park() 阻塞自己.    | await()阻塞     |
| static yield() 将当前线程让出CPU, 进入就绪态                 | 锁对象.notify() 不释放锁, 仅将一个wait()的线程变为就绪态     | LockSupport.unpark(t1)将t1唤醒. | signal()        |
| join() 线程A执行 线程B.join() 使A阻塞直到B执行结束. 原理就是synchronized的wait()和notify() | 锁对象.notifyAll() 不释放锁, 仅将所有wait()的线程变为就绪态. |                                 | signalAll()唤醒 |



# Thread类

| Thread类               |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| static currentThread() | 返回当前正在执行的线程对象                                   |
| static interrupted()   | 返回当前线程是否已经中断, 并将中断状态设为false              |
| interrupt()            | 如果线程强制停止可能会出现错误, 所以一般调用t1.interrupt()告诉t1线程可以中断了, 由t1线程的内部逻辑决定什么时候停止. |
| isInterrupted()        |                                                              |
| 属性1: name            | 线程名                                                       |
| 属性2: priority        | 线程优先级                                                   |
| 属性3: boolean daemon  | 是否是守护线程, 如果主线程挂了, 守护线程也挂.                |
| 属性4: ThreadLocalMap  | 存放当前线程的所有ThreadLocal                                |



创建线程类的方式: 本质上都是Runnable

1. `extends Thread`,  Thread类实现了Runnable接口.

   1. ```java
      Thread thread = new Thread(() -> {System.out.println("Thread is running...");});
      ```

2. `implements Runnable`: 推荐! 避免了类的单继承的局限性; 更适合处理共享数据问题; 实现了代码和数据的分离

   -    前两种方法重写的run方法不能有返回值也不能抛出异常

3. `implements Callable<>`,  可以有返回值, FutureTask的get()获取线程返回结果时会阻塞当前调用它的线程

4. 线程池



Lock锁通过两个方法控制需要被同步的代码, 更灵活; Lock作为接口, 适合更多更复杂的场景, 效率更高



| 进程和线程                                                   |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 进程是==资源分配==的基本单位, 线程是==资源调度==的基本单位   |                                                              |
| 为什么要有多个线程? 为了提高CPU的利用率, 现在CPU都是多核, 如果只用单线程, 其他核心就空闲了 | Tomcat每受到一个请求, 就会从线程池中用一个线程去处理; 连接数据库时用到的连接池Druid |
| 多线程的案例:                                                | 有个定时任务, 该任务执行时间非常长, 就需要用一个线程池将该定时任务的请求进行处理, 好处就是可以及时返回结果给调用方, 提高系统吞吐量. |



## 线程中断

Thread.stop()会直接强制停止线程, 导致必要的操作没有做完.

线程中断用来将线程停止. 在一个可以中断的位置判断中断标识位, 如果需要中断则结束线程.

Thread.interrupt()将线程中断标记位设为true, 告诉线程可以停止了. 如果线程处在waiting或者time_waiting状态下会被唤醒. (还有个阻塞状态是blocked, synchronized没拿到锁会blocked).



# 线程同步

| 解决线程安全问题的思路 |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| 操作的原子性           | 考虑atomic包                                                 |
| 操作的可见性           | 考虑volatile关键字                                           |
| 对线程的控制           | 比如一次能使用多少个线程, 当前线程触发的条件是否依赖其他线程的结果. 考虑CountDownLatch/Semaphore |
| 集合                   | 考虑java.util.concurrent包下的集合类                         |
| 如果synchronized不行   | 考虑lock包下的类                                             |



## 基于CAS的自旋锁(Spinlock)

Test-And-Set, 忙等. 适合自旋比调度的成本低的情, 不把耗费时间在线程调度上, 只需要忙等很短的时间就能获得自旋锁. 

==内核态线程==很容易满足这些条件, 因为运行在内核态的中断处理函数里可以通过关闭调度, 从而避免CPU被抢占, 而且有些内核态线程调用的处理函数不能阻塞, 只能使用自旋锁.

用户态线程推荐阻塞锁, 因为尽管访问临界区时间很短, 当没有来得及释放锁, 操作系统将CPU调度给其他线程, 这样持有锁的时间就会很长.

Linux系统优化过后的mutex实现，在加锁的时候会先做有限次数的自旋，只有有限次自旋失败后，才会进入阻塞让出CPU，所以，实际使用中，它的性能也足够好。此外，自旋锁必须在多CPU或者多Core架构下，试想如果只有一个核，那么它执行自旋逻辑的时候，别的线程没有办法运行，也就没有机会释放锁。



基于加锁的方式有个问题, 如果持有锁的线程出了问题永远释放不了锁了, 其他线程就没办法执行了, 这不符合lock-free要求.

lock-free: 如果一个线程持有了锁, 其他线程也能继续执行. CAS可以满足lock-free.

Compare And Swap, CAS操作对共享的变量进行修改要用到(变量, 期望值, 新值) 如果变量的当前值与期望值不相等(Compare过程), 则更改期望值并且自旋.

CAS的过程是用系统指令cmpxchg实现的原子操作, 但是加载期望值, 也就是 expectValue = 变量的值 和 CAS这是两步, 不是原子的. 所以需要循环.



| CAS本质上是调用Unsafe.compareAndSetXXX()                     |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 哪里用到CAS?                                                 | JUC的atomic包中的类都是用CAS保证线程安全的                   |
| CAS存在的ABA现象, 线程1对A进行修改, 写回的时候, 线程2已经将A改为B又改为A了, 此时应该怎么处理? | 需要对资源设置版本号, CAS 操作需同时匹配值和版本号. AtomicStampedReference |
| 无法锁住一段代码.                                            |                                                              |
| LongAdder                                                    | LongAdder采用分段锁的思想也就是把总任务拆分为多个子任务, 每个线程对自己的Cell的 value 进行CAS, 降低了失败的次数(也就是减少了自旋次数) |

AtomicLong做累加的时候实际上就是多个线程操作同一个目标资源, 但是只有一个线程是执行成功的, 其他的线程都会不断自旋. 



## synchronized原理

如果修饰的是实例方法, 则锁是对象实例;

如果修饰的是静态方法, 则锁是当前类的Class实例; 

修饰的是代码块, 则锁是传入synchronized的对象



| Synchronize                                |                                                              |
| ------------------------------------------ | ------------------------------------------------------------ |
| 用来当作锁的对象需要加`final`的原因是什么? | 锁更改后并发就出现问题                                       |
| Synchronized将对象作为锁是怎么实现的?      | 每个对象都有一个8B的Markword, 它里面记录了锁的信息(重量级还是轻量级) |
| Synchronized为什么保证了可见性?            | 可见性指的是一个线程修改共享资源后, 其他线程能立即看到修改后的值. 解锁时, 将工作内存中的修改刷新到主内存. |
| 程序出现异常, Synchronized锁怎么办?        | 锁会被释放                                                   |
| Synchronize锁升级                          | 1.偏向锁: Markword记录这个线程ID, 认为不会有第二个线程来抢; 2.一发生争抢, 偏向锁升级为自旋锁, 第二个线程占用cpu等待第一个线程; 3.第二个线程旋10次, 升级为重量级系统锁, 重量级锁去操作系统申请锁资源 |



| synchronized重量级锁时ObjectMonitor里面的实现 |                                                              |
| --------------------------------------------- | ------------------------------------------------------------ |
| waitSet                                       | wait()释放锁后等待拿到锁的线程执行notify()                   |
| cxq, 单链表                                   | 存放等待拿锁的线程, 当锁竞争激烈时, 锁放到cxq中阻塞.         |
| EntryList, 双向链表                           | 存放等待拿锁的线程, 为了环节大量线程放入cxq的压力. 当线程在重量级锁时, 也会CAS拿锁, 拿锁失败后先放入EntryList. |



Synchronized锁升级



## AQS

AQS(AbstractQueuedSynchronizer)是并发编程的核心类, 很多共性的功能向上抽取为AQS类, CopyOnWriteArrayList.

ReentrantLock, ReentrantReadWriteLock, ThreadPoolExecutor, ==CountDownLatch==, Semaphore都是基于AQS实现的.

ConcurrentHashMap, ==CyclicBarrier==用了ReentrantLock

| AQS的核心内容                   |                                                              |
| ------------------------------- | ------------------------------------------------------------ |
| state 表示锁的状态              | ReentrantLock state>0代表锁被占用; Semaphore表示有几个资源; CountDown()减的就是state. |
| ConditionNode双向链表 等待队列  | 用于实现排队机制. 如果获取不到锁, 将当前线程封装为Node入队等待. 释放资源后, 唤醒队首结点. releaseShared()释放资源的时候会从Node链表中LockSupport.unpark(t1)唤醒一个线程. |
| ConditionObject 单链表 条件队列 | 线程主动Condition.await()释放锁, 入队, 等待持有锁线程Condition.signal()唤醒自己, 醒来进入等待队列. |



### Lock

| Lock接口            |                                                            |
| ------------------- | ---------------------------------------------------------- |
| lock()              | 一直等                                                     |
| tryLock()           | CAS获取一次锁                                              |
| tryLock(time)       | CAS获取time时间的锁, 时间范围内可被中断, 如果中断则抛异常. |
| lockInterruptibly() | 一直等, 如果被中断则抛出异常.                              |

Lock接口的实现有 ReentrantLock, ReentrantReadWriteLock.ReadLock(WriteLock), StampedLock.ReadLockView(WriteLockView).



锁的各种特性: 乐观悲观, 可否重入, 公平非公平, 互斥非互斥.



### ReentrantLock

有了synchronized, 为什么还要有ReentrantLock?

1. ReentrantLock可以自定义公平非公平;
2. ReentrantLock等待锁的线程可以放弃等待, 也就是带超时的获取锁;
3. ReentrantLock可以响应中断请求, 当持有锁的线程被中断时, 锁会释放, 并且抛出中断异常;
4. ReentrantLock加锁可以判断是否有线程正在排队获取锁;
5. 竞争激烈情况下synchronized性能会下降, ReentrantLock性能不变.
6. 但是ReentrantLock释放锁必须要手动, synchronized的锁会由JVM释放. 



### JUC核心类

| JUC核心类                                                    |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ConcurrentHashMap                                            |                                                              |
| ThreadPoolExecutor                                           |                                                              |
| ReentrantLock                                                | lock() :CAS修改state, 失败则执行AQS的acquire(), 自旋几圈就LockSupport.park()阻塞; unlock() 执行AQS的release(), CAS修改state, 成功则LockSupport.unpark()唤醒. |
| ReentrantReadWriteLock                                       | ReentrantLock是互斥锁, 如果读操作多, 写操作少, 效率较低. 为了提高ReentrantLock的效率, 引入了读写锁, 是共享的. |
| Semaphore                                                    |                                                              |
| CountDownLatch 一个线程等待n个线程完成                       | 汇总线程CountDownLatch.await()自旋几圈就LockSupport.park()阻塞, n个线程任务完成后执行CountDownLatch.countDown() 进行state--, 如果state为0, 则LockSupport.unpark()唤醒汇总线程. |
| CyclicBarrier 给多个线程分阶段, 全部执行完第一阶段, 才执行第二阶段. | 依赖了ReentrantLock中的Condition, n个线程执行CyclicBarrier.await() 如果线程未全部阻塞则Condition.await()阻塞; 如果全部阻塞则Condition.signalAll(). |



## 死锁问题

| 死锁                                                       |      |
| ---------------------------------------------------------- | ---- |
| 不设计让线程需要两把锁                                     |      |
| 顺序: 固定加锁的顺序, 比如可以用Hash值的大小确定加锁的先后 |      |
| 释放: 尝试获取锁失败, 则释放自己持有的锁                   |      |
| 尽可能缩减加锁的范围                                       |      |



# 线程池ThreadPoolExecutor

为什么要线程池? 类似于JDBC连接池的连接的创建与销毁, 线程的创建与销毁比较浪费资源. 线程池可以固定线程的数量并且复用这些线程.

JDK中提供了Executors, 提供了很多封装好了的线程池, 为什么不用而是去自己构建线程池? 自己new线程池是阿里的规范, 因为要手动的调整参数使得线程池的性能达到最优.

| 线程池核心参数 |                                                              |
| -------------- | ------------------------------------------------------------ |
| 核心线程数     | 核心线程空闲时, 在干嘛? 在take()阻塞队列, 等新任务.          |
| 最大线程数     | 当任务队列满的时候, 生成几个非核心线程, 堵在队列外面的任务插队分配给非核心线程 |
| 生存时间       | 临时线程执行完任务后, 阻塞一个生存时间再去阻塞队列获取, 如果获取不到, 则线程结束. |
| 阻塞队列       | 阻塞队列与队列有什么区别? 阻塞队列额外提供两个方法, 如果offer()时队列已满, 则当前线程会阻塞直到不满; 如果poll()时队列为空, 则当前线程阻塞直到不空. |
| 拒绝策略       | 策略1: 抛异常; 策略2: 给主线程执行; 策略3: 阻塞队列最前面的任务扔掉; 策略4: 什么都不干, 也就是把这个任务扔掉 |
| 线程工厂       | 线程工厂负责new线程, 在线程池中, Thread被封装为Work. 为什么要自己写个线程工厂? 给线程设置一些属性和参数, 从而满足业务需求, 也方便在排查问题的时候找到问题; 指定如何处理未捕捉的异常. |

如果线程池执行的线程抛出了异常, 线程会将异常抛出, 线程结束. 因为线程工厂可以配置未捕捉异常的处理, 如果线程任务本身没有捕捉异常, 可以由线程工厂处理.



阻塞队列: 如果任务中网络IO较多, 则考虑将阻塞队列扩大一些作为缓冲.
- ArrayBlockingQueue: 有界
- LinkedBlockingQueue: 可以无界可以有界
- PriorityBlockingQueue: 有顺序, 无界
- SynchronousQueue
- DelayQueue: 无界, 队列中的元素是 `Delayed` 接口的实现
- LinkedTransferQueue



execute()过程

1. 判断 工作线程数是否>=核心线程数
   - 否: 创建线程并执行任务
   - 是: 尝试将任务放入阻塞队列
2. 判断阻塞队列是否满了
   - 否: 放入阻塞队列
   - 是: 尝试创建线程
3. 判断线程数是否大于maximumPoolSize
   - 否: 创建线程并执行任务
   - 是: 按照拒绝策略拒绝



如果线程抛出了异常, 则线程池会将这个线程销毁, 再重新创建线程.



线程池的Worker继承了AQS, 

是不可重入锁, 因为如果任务中获取到重入锁, 调用了某些方法可能会中断.

lock()获取了锁说明线程在执行任务中;

如果在执行任务则不应该中断线程; 如果线程没在处理任务时可以中断;

线程池结束的时候会调用interruptIdleWorkers()中断空闲的线程.



## 线程池的线程数设置策略

CPU密集 n+1, 线程数 > CPU核心数时, 线程数多少都一样

IO密集, 其实线程数多少都无所谓 (2*n的说法是Java并发编程这本书中给出的公式, 假定计算时间等于等待时间算出的2n)

Tomcat线程池线程数达到200后入队, 队满触发拒绝策略.



线程数其实可以设置很高, 但为什么大家不敢设置很高? 说是上下文切换会带来损耗? 这个说法有些过时, 当代CPU上下文切换1-3微秒.



总结: CPU密集型>n就行; IO密集型随便, 主要取决于外部.



# ThreadLocal

| ThreadLocal的应用场景                                        |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 线程不安全的对象, 不想每次调用都创建一次, 就放入ThreadLocal. | DateUtils用SimpleDateFormat对时间进行格式化, 但是SimpleDateFormat不是线程安全的, 可以在方法上创建SimpleDateFormat对象, 但每调用一次就创建一次比较浪费, 所以就用ThreadLocal让每个线程装载着自己的SimpleDateFormat对象, 以确保线程安全. |
| Spring事务                                                   | 一次事务的多个操作需要在同一个数据库连接上, 连接是由连接池分配, 同一个线程的多个操作可能会被分配不同的连接, Spring用ThreadLocal来解决. 每次获取连接的时候看看ThreadLocal中是否已经有了连接, 有了就不用再去找连接池了. ThreadLocal中存放一个Map<DataSource, Connection>(有多数据源的情况, 所以是一个Map), 保证了同一个线程获取同一个Connection对象 |
| Spring AOP失效问题                                           | 增强方法A调用增强方法B, B没被增强. @EnableAspectJAutoProxy(exposeProxy=true)设置在线程中暴露代理对象, 有了这个注解, Spring就会将代理对象存到ThreadLocal中, 通过AopContext.currentProxy()拿到当前正在调用的代理对象. |
| SpringMVC                                                    | HttpSession, HttpServletRequest, HttpServletResponse都放在ThreadLocal中, 因为Servlet是单例的, 而SpringMVC允许在Controller中配置request, response和requestContext. 底层是通过ThreadLocal才实现线程安全. |



ThreadLocal的原理:

如果定义一个Map<key: Thread, value>, key是当前线程, 这样就可以通过当前线程获取到属于当前线程的变量. 问题1: 但是一个线程是可以拥有多个私有变量, 还需要对value处理来唯一标识每个私有变量; 问题2: 并发量足够大时, 所有的线程都去操作同一个Map, Map体积有可能会膨胀, 导致性能下降; 问题3: 这个Map维护着所有的线程的私有变量, 不知道什么时候可以销毁.



JDK实现: 

每个Thread有一个属性: ThreadLocalMap<key: ThreadLocal, value.>, 我们可以定义多个ThreadLocal, 都存在这一个ThreadLocalMap中, ThreadLoca.set()其实就是向ThreadLocalMap中存放了一个Entry<threadLocal1, val>. 

threadLocal1.get() 其实是 ThreadLocalMap<>.get(threadLocal1)



<u>ThreadLocal内存泄露问题</u>

线程池的线程使用ThreadLocal时, 如果线程没有结束, 而是去阻塞队列继续获取任务时, 因为ThreadLocal中的value是==强引用==, 就不回被垃圾回收, 引发了内存泄露. 所以我们需要在用完ThreadLocal后调用ThreadLocal.remove()



一个ThreadLocal对象有两个引用指向它, 一个是ThreadLocal tl = new ThreadLocal<>()的tl强引用, 另一个是Thread.ThreadLocalMap.Entry.Key弱引用. 当tl = null, 还剩个弱引用, 就JVM就会将其当作垃圾回收. 那value怎么回收? ThreadLocal.remove().



<u>java的四种引用</u>

| java的四种引用 |                                                              |
| -------------- | ------------------------------------------------------------ |
| 强引用         |                                                              |
| 软引用         | SoftReference, GC在JVM堆内存不够时, 马上要OOM了, 才会清除软引用. 主要用作缓存, 指向非必须对象. |
| ==弱引用==     | WeakReference, 每次GC都会清理. ThreadLocal中有弱引用.        |
| 虚引用         | PhantomReference                                             |



# volatile

**volatile有什么作用?** 

- 保证==可见性==
  - 原理是happen-before规则实现可见性: 前面一个操作的结果对后续操作必须是可见的
  - 一个线程对一个共享资源修改, 另一个线程不知道, 加了`volatile`, 就能保证一个线程修改, 另一个线程知道, 这就是可见性. 用CPU的缓存一致性协议MESI来实现
- 保证==有序性==, 禁止指令重排
  - DCL(Double Check Lock): 1.申请内存空间; 2.给内存空间赋值, 3.指针指向内存空间. 2,3顺序可能调换, 另一个线程单例`getInstance()`得到null
  - volatile「前后」加上「CPU内存屏障」, 使得编译器和CPU无法进行重排序

但是不能保证原子性, 所以volatile修饰的变量不是并发安全的.



# Java内存模型

为了解决CPU和内存速度不一样的问题, 多核CPU的每个核心都有高速缓存, L1和L2缓存一般是「每个核心独占」一份的.

为了提高CPU利用率, 处理器可能会对输入的代码进行「乱序执行」, 也就是所谓的「指令重排序」

一次对数值的修改操作往往是非原子性的, 例如i++实际上在计算机执行时就会分成多条指令

在单线程情况下, 编译器/runtime/处理器都必须遵守as-if-serial语义, 不会对「数据依赖关系的操作」做重排序, 没什么问题.

在多线程情况下, 就会存在线程安全问题:

- 缓存数据不一致, 多个线程同时修改共享变量, 每个缓存是不共享的, 多个缓存与内存的数据同步应该怎么做?
  - 解决方式1: 总线锁, 某个核心在修改数据的过程中, 其他核心均无法修改内存中的数据. 类似于独占内存, 只要有一个核心在修改, 那别的核心就得等待当前核心释放
  - 解决方式2: 缓存一致性协议(MESI协议, Modified Exclusive Share Invalid), 缓存锁, 对缓存行(缓存的最小单位)加锁. 
    - MESI, 某个核心在对数据进行修改时, 需要「同步」通知其他CPU, 表示这个数据被我修改了, 你们不能用了.
    - MESI原理是当每个CPU读取共享变量之前, 会先识别数据的「对象状态」(是修改、还是共享、还是独占、还是无效), 根据「对象状态」做不同的策略.
    - 问题: 当某个核心修改数据时, 需要「同步」告诉其他的核心, 待其他核心响应接收到invalid(无效)后, 它才能将高速缓存数据写到主存, 同步意味着啥也不干就等着, 所以要同步 变成 异步, 会出现CPU乱序执行的问题

- CPU指令重排序在多线程下会导致代码在非预期下执行, 最终会导致结果存在错误的情况



乱序问题(可见性问题): 修改完没有及时同步到其他的CPU核心

「内存屏障」其实就是为了解决「异步优化」导致「CPU乱序执行」/「缓存不及时可见」的问题, 就是把「异步优化」给禁用

内存屏障可以分为三种类型：写屏障，读屏障以及全能屏障(包含了读写屏障). 在操作数据的时候, 往数据插入一条”特殊的指令”, 只要遇到这条指令, 那前面的操作都得「完成」.

写(读)操作可见性: CPU当发现写(读)屏障的指令时, 会把该指令「之前」存在于「store Buffer」所有写(读)指令刷入高速缓存. 就可以让CPU修改的数据可以马上暴露给其他CPU.





由于不同CPU架构的缓存体系不一样, 缓存一致性协议不一样, 重排序的策略不一样, 所提供的内存屏障指令也有差异, 为了简化Java开发人员的工作. Java封装了一套规范, 这套规范就是「Java内存模型」. 「Java内存模型」希望屏蔽各种硬件和操作系统的访问差异, 保证了Java程序在各种平台下对内存的访问都能得到一致效果.



并发问题的三个根源:

1. 原子性: 一行需要多条CPU 指令完成(i++), 由于操作系统的线程切换很可能导致i++ 操作未完成, 其他线程中途操作了共享变量 i , 导致最终结果错误
2. 有序性: 
   1. 编译器优化导致重排序(编译器可以在不改变单线程程序语义的情况下，可以对代码语句顺序进行调整重新排序)
   2. 指令集并行重排序(CPU原生就有可能将指令进行重排)
   3. 内存系统重排序(CPU架构下很可能有store buffer /invalid queue 缓冲区，这种「异步」很可能会导致指令重排)
3. 可见性(缓存一致性): CPU架构下存在高速缓存, 每个核心下的L1/L2高速缓存不共享(不可见)



<u>java逻辑上的内存结构</u>

分主内存和本地内存, 线程之间的「共享变量」存储在「主内存」中, 线程都有自己私有的「本地内存」, 「本地内存」存储了该线程以读/写「共享变量的副本」. 

线程对变量的所有操作都必须在「本地内存」进行，「不能直接读写主内存」的变量

Java内存模型定义了8种操作来完成 变量如何从主内存到本地内存 和 变量如何从本地内存到主内存.



# ConcurrentHashMap

CopyOnWrite系列, 写操作的时候要复制一个副本, 在副本中做各种操作. 应用场景: WebSocket存储客户端的session, session需要存储, 同时可能会客户端并发连接, 而且session体量不太大, 当时选择的CopyOnWriteArraySet.

| ConcurrentHashMap           |                                                              |
| --------------------------- | ------------------------------------------------------------ |
| CHM用来作为JVM缓存干啥      | 如果是集群部署, 缓存数据出现不一致的情况怎么解决? SpringCloud的bus组件, 基于MQ实现广播, 让多个服务端数据同步, 或者将JVM缓存的生存时间设置短一些, 既可以保证可用, 还能减少系统之间的耦合. |
| CHM的初始化                 | CHM的用DCL懒加载创建数组, 锁是CAS.                           |
| LongAdder保证计数器线程安全 | 计数器记录元素个数, 只需要++和--, 但是要保证线程安全; 如果Atomic原子类, 基于CAS实现, 多个线程等待一个baseCount都在浪费CPU. CHM用LongAdder除了baseCount, 还提供了CounterCell数组, 最后将多个CounterCell加到一起. |
| 1.7保证读写并发安全         | 一个Segment锁HashMap的一部分; 获取元素时二次hash, 第一次定位到哪个Segment, 第二次定位到数组的哪个位置. Segment分段锁继承了ReentrantLock, 并发度为segment个数, 可以通过构造函数指定 |
| 1.8保证读写并发安全         | 每个位置一把锁; 写操作: 数据要写在数组上时, 采用CAS; 数据要写在链表或者红黑树上时, 会synchronized加锁; 读操作: Node的val和next使用volatile修饰; 数组用volatile修饰, 保证扩容时被读线程感知. 如果数据已经迁移, 查询新数组; 如果是红黑树: 1.如果有写操作活跃, 则查询转换红黑树时保留的双向链表; 2.在红黑树上查找. |
| 1.7扩容机制                 | 基于Segment分段锁实现的, 每个Segment相对于⼀个⼩型的HashMap,  每个Segment内部会进⾏扩容, 数组扩容不会影响其他的segment, 扩容的判断也是每个Segment内部单独判断的, 判断是否超过阈值. |
| 1.8扩容机制                 | 扩容时, 读操作继续读老数组; 写操作时, 写线程帮忙扩容.        |


# 异步编程

ListenableFuture




