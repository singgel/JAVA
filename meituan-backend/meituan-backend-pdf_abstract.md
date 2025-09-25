# meituan-backend-pdf_abstract
## [JVM源码](http://hg.openjdk.java.net/jdk8u)

## 2018
### [SLA稳定性理解](https://tech.meituan.com/2018/04/19/trade-high-availability-in-action.html)

### netty堆外内存泄漏（netty-socketio）  
```
1. 一次 Connect 和 Disconnect 为一次连接的建立与关闭  
2. 在 Disconnect事件前后申请的内存并没有释放(DIRECT_MEMORY_COUNTER堆外统计字段)  
3. 断点打在client.send() 这行， 然后关闭客户端连接，之后直接进入到这个方法，有个逻辑 encoder.allocateBuffer申请堆外内存  
4. handleWebsocket ：调用 encoder 分配了一段内存，调用完之后，我们的控制台立马就彪了 256B（怀疑肯定是这里申请的内存没有释放）  
5. encoder.encodePacket() 方法，把 packet 里面一个字段的值转换为一个 char（这里报NPE）  
6. 跟踪到NPE之前的代码，看看为啥没有赋值进来，给附上值 *解决*  
```
### 不可不说的Java“锁”事  
```
1. 乐观锁 VS 悲观锁(synchronized关键字和Lock的实现类都是悲观锁)  
2. 自旋锁 VS 适应性自旋锁(自旋锁的实现原理同样也是CAS，AtomicInteger中调用unsafe进行自增操作的源码中的do-while循环就是一个自旋操作)  
3. 无锁 VS 偏向锁 VS 轻量级锁 VS 重量级锁(Mark Word：默认存储对象的HashCode，分代年龄和锁标志位信息)  
4. 公平锁 VS 非公平锁(AQS AbstractQueuedSynchronizer,hasQueuedPredecessors()）  
5. 可重入锁 VS 非可重入锁(ReentrantLock和synchronized都是可重入锁，NonReentrantLock)  
6. 独享锁 VS 共享锁(JDK中的synchronized和JUC中Lock的实现类就是互斥锁。ReentrantReadWriteLock有两把锁：ReadLock和WriteLock，StampedLock 提供了一种乐观读锁的实现)  
```

## 2019
### Java Unsafe 
```
1. 提升程序 I/O 操作的性能。通常在 I/O 通信过程中，会存在堆内内存到堆外内存的数据拷贝操作，对于需要频繁进行内存间数据拷贝且生命周期较短的暂存数据，都建议存储到堆外内存。  
2. 创 建 DirectByteBuffer 的时候， 通过Unsafe.allocateMemory 分配内存、Unsafe.setMemory 进行内存初始化，而后构建 Cleaner 对象用于跟踪 DirectByteBuffer 对象的垃圾回收，以实现当 DirectByteBuffer 被垃圾回收时，分配的堆外内存一起被释放。（Cleaner 继承自 Java 四大引用类型之一的虚引用 PhantomReference（众所周知，无法通过虚引用获取与之关联的对象实例，且当对象仅被虚引用引用时，在任何发生 GC 的时候，其均可被回收），）  
3. 这部分，包括线程挂起、恢复、锁机制等方法。  
4. allocateInstance 在 java.lang.invoke、Objenesis（提供绕过类构造器的对象生成方式）、Gson（反序列化时用到）中都有相应的应用。  
5. 在 Java 8 中引入，用于定义内存屏障（也称内存栅栏，内存栅障，屏障指令等，是一类同步屏障指令，是 CPU 或编译器在对内存随机访问的操作中的一个同步点，使得此点之前的所有读写操作都执行后才可  
```
### Java动态追踪技术 
```
1. java.lang.instrument.Instrumentation替换已经存在的 class 文件，运行时直接替换类很不安全。比如新的 class 文件引用了一个不存在的类，或者把某个类的一个 field 给删除了等等  
2. 因为有 BTrace 的存在，我们不必自己写一套ASM这样的工具了，BTrace 最终借 Instruments 实现 class 的替换  
```
### 字节码增强技术探索
```
1. 如果每次查看反编译后的字节码都使用 javap 命令的话，好非常繁琐。这里推荐一个 Idea 插件：jclasslib。  
2. 利用 Javassist 实现字节码增强时，可以无须关注字节码刻板的结构，其优点就在于编程简单。  
3. Attach API 的作用是提供 JVM 进程间通信的能力，比如说我们为了让另外一个 JVM 进程把线上服务的线程 Dump 出来，会运行 jstack 或 jmap 的进程，并传递 pid 的参数，告诉它要对哪个进程进行线程 Dump，这就是 Attach API 做的事情  
4. 热部署：不部署服务而对线上服务做修改，可以做打点、增加日志等操作，Mock：测试时候对某些服务做 Mock，性能诊断工具
```
### JVM Profiler技术原理和源码探索
```
1. Instrumentation 方式对几乎所有方法添加了额外的 AOP 逻辑，这会导致对线上服务造成巨额的性能影响，但其优势是：绝对精准的方法调用次数、调用时间统计。  
2. Sampling 方式基于无侵入的额外线程对所有线程的调用栈快照进行固定频率抽样，相对前者来说它的性能开销很低。（典型开源实现有 Async-Profiler 和 Honest-Profiler）  
3. FlameGraph 项目的核心只是一个 Perl 脚本  
```
### Java动态调试技术原理
```
1. Java-debug-tool 的同类产品主要是 greys，其他类似的工具大部分都是基于greys 进行的二次开发，所以直接选择 greys 来和 Java-debug-tool 进行对比。  
```
### 从ReentrantLOck的实现看AQS
```
1. 某个线程获取锁失败的后续流程是什么呢？存在某种排队等候机制，线程继续等待，仍然保留获取锁的可能，获取锁流程仍在继续。  
2. 如果处于排队等候机制中的线程一直无法获取锁，需要一直等待么？：线程所在节点的状态会变成取消状态，取消状态的节点会从队列中释放  
```
### springboot堆外内存排查
```
1. Native Code 所引起，而 Java 层面的工具不便于排查此类问题，只能使用系统层面的工具gperftools去定位问题  
2. 使用命令“strace -f -e”brk,mmap,munmap”-p pid”追踪向 OS 申请内存请求  
3. 想着看看内存中的情况使用命令 gdp -pid pid 进入 GDB 之后，然后使用命令 dump memory mem.bin startAddress endAddressdump 内存
其中 startAddress 和 endAddress 可以从 /proc/pid/smaps 中查找。然后使用 strings mem.bin 查看 dump 的内容  
```

## 2020
### 线程池实现原理
```
1. RUNNING、SHUTDOWN、STOP、TIDYING、TERMINATED  
2. 不用线程池的框架：Disruptor、Actor、协程  
3. 动态化线程池设计：简化线程池配置、参数可动态修改、增加线程池监控
```
### 美团亿万级KV存储
```
1. Squirrel 官方提供的方案，任何一个节点从宕机到被标记为 FAIL 摘除，一般需要经过 30 秒。更新 ZooKeeper，通知客户端、添加新node  
2. 持久化机制，写请求会先写到 DB 里，然后写到内存Backlog，这跟官方是一样的。同时它会把请求发给异步线程，异步线程负责把变更刷到硬盘的 Backlog 里。当硬盘 Backlog 过多时，我们会主动在业务低峰期做一次RDB ，然后把 RDB 之前生成的 Backlog 删除  
3. 如果有热点，监控服务会把热点 Key 所在 Slot 上报到我们的迁移服务。迁移服务这时会把热点主从节点加入到这个集群中，然后把热点 Slot 迁移到这个热点主从上。因为热点主从上只有热点 Slot 的请求，所以热点 Key 的处理能力得到了大幅提升  
4. Cellar 跟阿里开源的 Tair 主要有两个架构上的不同。第一个是 OB，第二个是 ZooKeeper  
5. Cellar 快慢列队，Cellar 智能迁移，Cellar 强一致  
6. 如果这个 Key 是一个热点，那么它会在做集群内复制的同时，还会把这个数据复制有热点区域的节点，同时，存储节点在返回结果给客户端时，会告诉客户端，这个 Key 是热点，这时客户端内会缓存这个热点 Key。  
```
### Java常见的9种CMS GC问题分析和解决
```
1. 分代收集器：ParNew：一款多线程的收集器，采用复制算法，主要工作在 Young 区；CMS：以获取最短回收停顿时间为目标，采用“标记 - 清除”算法
2. 分区收集器：G1：一种服务器端的垃圾收集器，应用在多处理器和大容量内存环境中；ZGC：JDK11 中推出的一款低延迟垃圾回收器，适用于大内存低延迟服务的内存管理和回收；
3. 读懂 GC Cause: System.gc()：手动触发 GC 操作；CMS：CMS GC 在执行过程中的一些动作；Promotion Failure：Old 区没有足够的空间分配给 Young 区；Concurrent Mode Failure：CMS GC 运行期间Old 区预留的空间不足；GCLocker Initiated GC：如果线程执行在 JNI 临界区时，刚好需要进行GC
4. MetaSpace 区 OOM: 经常会出问题的几个点有 Orika的 classMap、JSON 的 ASMSerializer、Groovy 动态加载类  
5. 过早晋升：分配速率接近于晋升速率，对象晋升年龄较小。原因：Young/Eden 区过小，分配速率过大  
6. CMS Old GC 频繁：判 断 当前 Old 区使用率是否大于阈值，则触发 CMS GC，默认为 92%。  
7. 内存泄漏：Dump Diff 和 Leak Suspects 比较直观就
```
### 堆外内存泄漏排查
```
-XX:NativeMemoryTracking=detail JVM 参数后重启项目
jcmd 272662 VM.native_memory detail
如果 total 中的 committed 和 top 中的 RES 相差不大，则应为主动申请的堆外内存
未释放造成的，如果相差较大，则基本可以确定是 JNI 调用造成的

原因一：主动申请未释放: NIO 和 Netty 都会取 -XX:MaxDirectMemorySize 配置的值，来限制申请的堆外内存的大小
原因二：通过 JNI 调用的 Native Code 申请的内存未释放: 通过 Google perftools + Btrace 等工具，帮助我们分析
首先可以使用 NMT + jcmd 分析泄漏的堆外内存是哪里申请，确定原因后，使用不同的手段，进行原因定位。

JNI 引发的 GC 问题: 添加 -XX+PrintJNIGCStalls 参数，可以打印出发生 JNI 调用时的线程，
禁用偏向锁：偏向锁在只有一个线程使用到该锁的时候效率很高，但是在竞争激烈情况会升级成轻量级锁，此时就需要先消除偏向锁，这个过程是STW 的。
```
### ZGC（The Z Garbage Collector）是 JDK 11 中推出的一款低延迟垃圾回收器
```
CMS 新生代的 Young GC、G1 和 ZGC 都基于标记 - 复制算法
标记阶段停顿分析
初始标记阶段：初始标记阶段是指从 GC Roots 出发标记全部直接子节点的过程，该阶段是 STW 的 (就遍历一层，快)
并发标记阶段：并发标记阶段是指从 GC Roots 开始对堆中对象进行可达性分析，找出存活对象 （可达性分析，并发，慢）
再标记阶段：重新标记那些在并发标记阶段发生变化的对象。该阶段是 STW 的 （？？？）

清理阶段停顿分析
清理阶段清点出有存活对象的分区和没有存活对象的分区，该阶段不会清理垃圾对象，也不会执行存活对象的复制。该阶段是 STW 的
复制阶段停顿分析
的转移阶段需要分配新内存和复制对象的成员变量。转移阶段是STW 的，其中内存分配通常耗时非常短，但对象成员变量的复制耗时有可能较长 （这个就跟redis大key似的）
为什么转移阶段不能和标记阶段一样并发执行呢？
主要是 G1 未能解决转移过程中准确定位对象地址的问题。
G1 的 Young GC 和 CMS 的 Young GC，其标记 - 复制全过程 STW

ZGC 在标记、转移和重定位阶段几乎都是并发的，这是 ZGC 实现停顿时间小于 10ms 目标的最关键原因
ZGC 通过着色指针和读屏障技术，解决了转移过程中准确访问对象的问题，实现了并发转移。
ZGC 有多种 GC 触发机制
阻塞内存分配请求触发：当垃圾来不及回收，垃圾将堆占满时，会导致部分线程阻塞。
基于分配速率的自适应算法：最主要的 GC 触发方式
基于固定时间间隔：通过 ZCollectionInterval 控制，适合应对突增流量场景。
主动触发规则：类似于固定间隔规则，但时间间隔不固定，是 ZGC 自行算出来的时机
预热规则：服务刚启动时出现，一般不需要关注
外部触发：代码中显式调用 System.gc() 触发
元数据分配触发：元数据区不足时导致，一般不需要关注

升级JDK11
a. 一 些 类 被 删 除： 比 如“sun.misc.BASE64Encoder”， 找 到 替 换 类 java.util.Base64 即可。
b. 组件依赖版本不兼容 JDK 11 问题：找到对应依赖组件，搜索最新版本，一般都支持 JDK 11。
```
### mybatis构建实现
```
SqlSession：作为 MyBatis 工作的主要顶层 API，表示和数据库交互的会话，完成必要数据库增删改查功能
Executor：MyBatis 执行器，这是 MyBatis 调度的核心，负责 SQL 语句的生成和查询缓存的维护
BoundSql：表示动态生成的 SQL 语句以及相应的参数信息
StatementHandler： 封 装 了 JDBC Statement 操 作， 负 责 对 JDBCstatement 的操作，如设置参数、将 Statement 结果集转换成 List 集合等等
ParameterHandler：负责对用户传递的参数转换成 JDBC Statement 所需要的参数
TypeHandler：负责 Java 数据类型和 JDBC 数据类型之间的映射和转换
```

## 2020阿里
### 如何正确地实现重试(Retry)
```
固定循环次数方式: 不带 backoff 的重试，对于下游来说会在失败发生时进一步遇到更多的请求压力，继而进一步恶化。
带固定 delay 的方式: 
虽然这次带了固定间隔的 backoff，但是每次重试的间隔固定，此时对于下游资源的冲击将会变成间歇性的脉冲；
特别是当集群都遇到类似的问题时，步调一致的脉冲，将会最终对资源造成很大的冲击，并陷入失败的循环中。
带随机 delay 的方式: 
如果依赖的底层服务持续地失败，改方法依然会进行固定次数的尝试，并不能起到很好的保护作用。
对结果是否符合预期，是否需要进行重试依赖于异常。
无法针对异常进行精细化的控制，如只针部分异常进行重试。
可进行细粒度控制的重试:
推荐使用 resilience4j-retr y 或则spring-retry 等库来进行组合

和断路器结合
虽然可以比较好的控制重试策略，但是对于下游资源持续性的失败，依然没有很好的解决。当持续的失败时，对下游也会造成持续性的压力。
常见的有 Hystrix 或 resilience4
```
### 阿里技术专家详解 DDD 系列
#### DDD Domain Primitive
```
Domain Primitive 是一个在特定领域里，拥有精准定义的、可自我验证的、拥有行为的 Value Object
DP 的第一个原则：将隐性的概念显性化  
eg: PhoneNumber 类的一个计算属性 getAreaCode

DP 的第二、三个原则：将隐性的上下文显性化、封装多对象行为
eg: 通过将默认货币这个隐性的上下文概念显性化，并且和金额合并为 Money ，我们可以避免很多当前看不出来，但未来可能会暴雷的 bug
eg: ExchangeRate 汇率对象，通过封装金额计算逻辑以及各种校验逻辑，让原始代码变得极其简单

让隐性的概念显性化
让隐性的上下文显性化
封装多对象行为

常见的 DP 的使用场景包括:
有格式限制的 String：比如 Name，PhoneNumber，OrderNumber，ZipCode，Address 等。
有限制的 Integer：比如 OrderId（>0），Percentage（0-100%），Quantity（>=0）等。
可枚举的 int ：比如 Status（一般不用 Enum 因为反序列化问题）。
Double 或 BigDecimal：一般用到的 Double 或 BigDecimal 都是有业务含义的，比如 Temperature、Money、Amount、ExchangeRate、Rating 等。
复杂的数据结构：比如 Map<String, List<Integer>> 等，尽量能把 Map 的所有操作包装掉，仅暴露必要行为。

所有抽离出来的方法要做到无状态, DP 本身不能带状态，所以一切需要改变状态的代码都不属于 DP 的范畴。
```
#### DDD 应用架构
```
可维护性 = 当依赖变化时，有多少代码需要随之改变
eg：
    数据结构的不稳定性
    依赖库的升级
    第三方服务依赖的不确定性
    第三方服务 API 的接口变化
    中间件更换
可扩展性 = 做新需求或改逻辑时，需要新增/修改多少代码
eg：
    数据来源被固定、数据格式不兼容
    业务逻辑无法复用
    逻辑和数据存储的相互依赖
可测试性 = 运行每个测试用例所花费的时间 * 每个需求所需要增加的测试用例数量
eg：
    设施搭建困难
    运行耗时长
    耦合度高

单一性原则（Single Responsibility Principle）：
依赖反转原则（Dependency Inversion Principle）：
开放封闭原则（Open Closed Principle）：

*重构方案*
抽象数据存储层
Data Object 数据类：，从数据库来的都应该先直接映射到 DO 上，但是代码里应该完全避免直接操作 DO。
Entity 实体类：Domain Primitive 代替，可以避免大量的校验代码等。
Repository 对应的是 Entity 对象读取储存的抽象，在接口层面做统一，不关注底层实现。通过 Builder/Factory 对象实现 AccountDO 到 Account 之间的转化。

抽象第三方服务
Anti-Corruption Layer（防腐层或 ACL）
很多时候我们的系统会去依赖其他的系统，而被依赖的系统可能包含不合理的数据结构、API、协议或技术实现，如果对外部系统强依赖，会导致我们的系统被”腐蚀“。
ACL 不仅仅只是多了一层调用：通过在系统间加入一个防腐层，能够有效的隔离外部依赖和内部逻辑，无论外部如何变更，内部代码可以尽可能的保持不变。
适配器：
缓存：
兜底：
易于测试：
功能开关：

抽象中间件（简单就是KafkaTemplate别直接用）
用 Domain Primitive 封装跟实体无关的无状态计算逻辑
用 Entity 封装单对象的有状态的行为，包括业务校验
用 Domain Service 封装多对象逻辑

*DDD 的六边形架构*
又被称之为 Ports and Adapters（端口和适配器架构）
UI 层、DB 层、和各种中间件层实际上是没有本质上区别的，都只是数据的输入和输出，而不是在传统架构中的最上层和最下层。
```
#### DDD Repository 模式
```
Anemic Domain Model（贫血领域模型）
而 2006 年的 JPA 标准，通过@Entity 等注解，以及 Hibernate 等 ORM 框架的实现，
让很多 Java 开发对 Entity 的理解停留在了数据映射层面，忽略了 Entity 实体的本身行为
eg:
  1. 有大量的 XxxDO 对象
  2. 服务和 Controller 里有大量的业务逻辑
  3. 大量的 Utils 工具类等。
Repository 的价值
  能够隔离我们的软件（业务逻辑）和固件/硬件（DAO、DB），
  让我们的软件变得更加健壮，而这个就是 Repository 的核心价值。
手写 Assembler/Converter 是一件耗时且容易出 bug 的事情
  MapStruct

```
#### DDD 领域层设计规范
```
OOP （Object-Oriented Programming）面对对象程序设计实现
  一个比较简单的实现是通过类的继承关系
  对象继承导致代码强依赖父类逻辑，违反开闭原则 Open-Closed Principle（OCP）最核心的原因就是一个现有逻辑的变更可能会影响一些原有的代码
目前领域事件的缺陷和展望
    和消息队列中间件不同的是，领域事件通常是立即执行的、在同一个进程内、可能是同步或异步。
    但会侵入实体本身，同时也需要比较啰嗦的显性在调用方dispatch 事件，也不是一个好的解决方案。
```

## linux查看哪个进程占用磁盘IO  
$ vmstat 2  
执行vmstat命令，可以看到r值和b值较高，r 表示运行和等待cpu时间片的进程数，如果长期大于1，说明cpu不足，需要增加cpu。  
b 表示在等待资源的进程数，比如正在等待I/O、或者内存交换等。

### [IO等待导致性能下降](https://serverfault.com/questions/363355/io-wait-causing-so-much-slowdown-ext4-jdb2-at-99-io-during-mysql-commit)
$ iotop -oP  
命令的含义：只显示有I/O行为的进程  

$ iostat -dtxNm 2 10  
查看磁盘io状况

$ dstat -r -l -t --top-io  
用dstat命令看下io前几名的进程

$ dstat --top-bio-adv  
找到那个进程占用IO最多

$ pidstat -d 1  
命令的含义：展示I/O统计，每秒更新一次  

### [Linux 挂载管理(mount)](https://www.cnblogs.com/chenmh/p/5097530.html)
$ vim /etc/fstab  
mount挂载分区在系统重启之后需要重新挂载，修改/etc/fstab文件可使挂载永久生效

$ mount -t ext4 /dev/sdb1 /sdb1  
-t:指定文件系统类型

$ mount -o remount,noatime,data=writeback,barrier=0,nobh /dev/sdb  
remount:重新挂载文件系统。
noatime:每次访问文件时不更新文件的访问时间。
async:适用缓存，默认方式。

$ fuser -m /dev/sdb  
查看文件系统占用的进程

$ lsof /dev/sdb  
查看正在被使用的文件，losf命令是list open file的缩写

### [Mac中的一些网络命令](https://tonydeng.github.io/2016/07/07/use-lsof-to-replace-netstat/)
$ lsof -itcp -n
当前用户名下启动的链接数

$ lsof -itcp -stcp:listen
当前用户名下监听的端口

$ netstat -antvp tcp
使用 netstat 命令查看连接数

### [Linux网络监控工具大全](https://baijiahao.baidu.com/s?id=1683499342813958473&wfr=spider&for=pc)

/Library/Java/JavaVirtualMachines/jdk-16.0.2.jdk/Contents/Home
/Library/Java/JavaVirtualMachines/jdk-15.0.2.jdk/Contents/Home
/Library/Java/JavaVirtualMachines/jdk-9.0.4.jdk/Contents/Home

PUT _all/_settings
{
    "index.translog.durability" : "async",
    "index.translog.flush_threshold_ops" : 50000
    "index.translog.flush_threshold_size" : "1024mb",
    "index.translog.sync_interval" : "60s",
    "index.refresh_interval" : "60s"
}

PUT /_cluster/settings
{
  "transient": {
    "cluster": {
      "routing": {
        "allocation.disk.watermark.high": "95%",
        "allocation.disk.watermark.low": "90%"
      }
    }
  }
}

PUT _cluster/settings
{
  "transient" : {
    "cluster.routing.allocation.exclude._ip" : "10.0.0.1"
  }
}

PUT _cluster/settings
{
    "persistent" : {
        "indices.store.throttle.max_bytes_per_sec" : "100mb"
    }
}

curl -XPOST http://127.0.0.1:9200/logstash-2015-06.10/_forcemerge?max_num_segments=1

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 172.16.1.15:9092,172.16.1.16:9092 --topic xueqiu-push-req --from-beginning --property print.key=true|grep 39171676469

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 10.10.106.3:9092,10.10.106.4:9092 --topic usercenter_auth_sep --from-beginning --property print.key=true

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 10.10.163.10:9092,10.10.163.11:9092 --topic xueqiu_push_user_auth_xy --from-beginning --property print.key=true

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 10.10.55.2:9092,10.10.55.3:9092,10.10.56.2:9092,10.10.56.3:9092 --topic snowball_analysis_prod --offset 0  --partition 0 --property print.key=true |grep new_symbol  > partition0.txt
/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 10.10.55.2:9092,10.10.55.3:9092,10.10.56.2:9092,10.10.56.3:9092 --topic snowball_analysis_prod --offset 0  --partition 1 --property print.key=true |grep new_symbol  > partition1.txt
/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 10.10.55.2:9092,10.10.55.3:9092,10.10.56.2:9092,10.10.56.3:9092 --topic snowball_analysis_prod --offset 0  --partition 2 --property print.key=true |grep new_symbol  > partition2.txt
/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 10.10.55.2:9092,10.10.55.3:9092,10.10.56.2:9092,10.10.56.3:9092 --topic snowball_analysis_prod --offset 0  --partition 3 --property print.key=true |grep new_symbol  > partition3.txt

/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server 10.10.22.7:9092,10.10.22.8:9092,10.10.23.7:9092,10.10.23.8:9092 --group mirror-maker --describe

/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server 10.10.55.7:9092,10.10.56.7:9092,10.10.58.7:9092 --group logging_logstash_ES --reset-offsets --all-topics --to-current --execute
/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server 10.10.55.7:9092,10.10.56.7:9092,10.10.58.7:9092 --group logging_logstash_ES --reset-offsets --topic logging_snowflake-usercenter_production --to-latest --execute

/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server 10.10.55.2:9092 --describe --group status_release
/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server 10.10.55.2:9092,10.10.55.3:9092,10.10.56.2:9092,10.10.56.3:9092 --list
/opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server 10.10.55.2:9092,10.10.55.3:9092,10.10.56.2:9092,10.10.56.3:9092 --delete --group rc.screener.option

/home/op/kafka_2.13-2.8.0/bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list 10.10.22.7:9092 --topic stock_view_recently --time -1
/home/op/kafka_2.13-2.8.0/bin/kafka-consumer-offset-checker.sh --zookeeper 10.10.31.9:2181 --topic stock_view_recently  --group stock_follower

cat ./grpc.log |sed -n  '/2021-03-19 14:00:00.*/,/2021-03-19 14:10:00.*/p' |grep prePay | awk -F '|' '{if ($6>2000) print $6}'
jcmd 239312 GC.class_stats|awk '{print$13}'|sed 's/\(.*\)\.\(.*\)/\1/g'|sort |uniq -c|sort -nrk1
