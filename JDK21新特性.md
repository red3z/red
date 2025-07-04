# 1 语言特性

**1-1 var 关键字**

**1-2 """ """ 多行字符串**



**1-3 有序集合**

新增SequencedCollection接口, 之前的版本不同的集合获取首尾元素的方法不统一, SequencedCollection接口对其做了统一, 有reversed()反转, 获取首尾元素, 增删首尾元素



**1-4 Record**

为了代替传统的POJO

Record的字段天生是final的

Record天生有 getter, setter, equals(), hashCode(), toString()

>getter直接为字段名, 如name(), 而不是~~getName()~~

示例:

``` java
public record User(String name, int age)
```



**API增强**

List.of(), Set.of()

HttpClient: 支持HTTP/2, 和WebSocket

响应式编程



# 2 虚拟线程

平台线程和虚拟线程的比较

|            | 平台线程               | 虚拟线程                                                   |
| ---------- | ---------------------- | ---------------------------------------------------------- |
| 内存占用   | 1MB栈内存              | 1KB 栈内存                                                 |
| 调度方式   | OS调度, 1:1映射        | JVM调度, m:n映射                                           |
| 阻塞代价   | 阻塞时线程挂起         | 阻塞时挂起虚拟线程, 释放平台线程执行其他任务               |
| 上下文切换 | 线程的栈帧存在OS线程栈 | 虚拟线程的栈帧作为对象存在堆, 可被GC回收, 轻量级上下文切换 |

核心点: 在代码 sleep(), IO, 同步 等操作时, JVM将虚拟线程从平台线程上卸载, 平台线程去处理下一个虚拟线程.

实现原理: Runnable 封装在 Continuation, Scheduler将任务提交到具体的载体线程池中执行.

虚拟线程使用建议: synchronized中不能yield; 因为线程太多, 所以少用ThreadLocal; 无需池化.



# 3 ZGC垃圾回收器

http://conf.ctripcorp.com/pages/viewpage.action?pageId=2809077087

## G1

G1当GC时会STW, G1的做法是优先清理回收收益高的Region（其他的之后清理），只清理部分则可以控制停顿时间。在G1中通过-XX:MaxGCPauseMillis=200ms可以配置最长可接受的停顿时间，默认是200ms，G1会尽力做到每次停顿在这个时间内。

Young GC: 主要针对年轻代区域的垃圾回收，包括Eden区和Survivor区。当所有Eden区使用率达到最大阀值（默认60%）或者G1计算出来的回收时间接近用户设定的最大暂停时间时，会触发Young GC，回收Eden区和Survivor区，复制移动到另外的Survivor（年龄+1）或Old区（提前晋升的）

Mixed GC（混合回收）：Mixed GC是G1垃圾回收器独有的，也称混合回收，针对年轻代和部分老年代区域的垃圾回收。当老年代的占有率达到阀值（默认45%）时，会触发一次Mixed GC，回收所有年轻代和一部分老年代区（选取的策略是**垃圾对象最多的老年代区域**，确保释放更多内存空间，即回收价值高的），控制最大暂停时间。



## ZGC









# 4 JVM

类数据共享优化启动速度







