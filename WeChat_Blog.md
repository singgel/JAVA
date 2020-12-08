## 微信公众号高质量技术贴  
> * 过滤掉对自己感觉没有技术相关性的，或者是那种水贴
> * 对内容进行归类整理
> * 阅读完写下自己的读后感
## LINUX

[面试官：如何写出让 CPU 跑得更快的代码？](https://mp.weixin.qq.com/s/XKYSk-5TO888LP3LIzxd_Q)  

[从无盘启动看 Linux 启动原理](https://mp.weixin.qq.com/s/6vFXphhkPpyYEDSgibl_vA)  

[Linux 机器 CPU 毛刺问题排查](https://mp.weixin.qq.com/s/UFPi8PZ9GcAfaalCrWTmng)  

[Linux 入门必看：如何60秒内分析Linux性能](https://mp.weixin.qq.com/s/HvADkICPYflS2VTuSB16rg)  

[​Linux CPU 性能优化指南](https://mp.weixin.qq.com/s/7HGjAy_R_sdpfckFlFr0cw)  

[Linux I/O 原理和 Zero-copy 技术全面揭秘](https://mp.weixin.qq.com/s/dZNjq05q9jMFYhJrjae_LA)  

[为什么 Linux 默认页大小是 4KB](https://mp.weixin.qq.com/s/EQsdFirbnsi8iiyTYQjWTw)  

[面试官：如何写出让 CPU 跑得更快的代码？](https://mp.weixin.qq.com/s/XKYSk-5TO888LP3LIzxd_Q)  

[彻底搞懂 IO 底层原理](https://mp.weixin.qq.com/s/_Eh7eZLtcUjrp9QiCvLmrw)  

[Go netpoller 网络模型之源码全面解析](https://mp.weixin.qq.com/s/HNPeffn08QovQwtUH1qkbQ)  

## JVM GC JAVA

[Twitter短链服务大bug: 预连到错误域名](https://mp.weixin.qq.com/s/W5G_LPr6cBqHBO8qnm0B3g)    

[内存泄漏事故：一顿debug猛如虎，定睛一看原地杵](https://mp.weixin.qq.com/s/l6prJDzbNDg1QkKc9fWJyg)  

[一文掌握开发利器：正则表达式](https://mp.weixin.qq.com/s/wkCHL_QzAJwWEg9JZaZnCQ)  

[为什么我们选择 Java 语言开发高频交易系统](https://mp.weixin.qq.com/s/YVv3wl9TVUhisq8gZXKscQ)  

[Java内存泄漏、性能优化、宕机死锁的N种姿势](https://mp.weixin.qq.com/s/ASrINfC8gHbYryIWSSwT_A)  

[Java中9种常见的CMS GC问题分析与解决](https://mp.weixin.qq.com/s/BoMAIurKtQ8Wy1Vf_KkyGw)  

[你可能未曾使用的新 Java 特性](https://mp.weixin.qq.com/s/uSEkyQ6AGffxIgEFJzwHvQ)  

[java线程池（newFixedThreadPool）线程消失疑问？](https://www.zhihu.com/question/27474985)  
> 线程池的异常线程会被销毁，然后重新创建新的线程来补位

[分享近期社区的几个经典的JVM问题](https://mp.weixin.qq.com/s/Rbg7OiUByzqq46aLPh1r8A)  

[字节码增强：原理与实战](https://mp.weixin.qq.com/s/vTfnj8SXUx4_0Ayar2NPPw)  

[Java ConcurrentHashMap 高并发安全实现原理解析](https://mp.weixin.qq.com/s/4sz6sTPvBigR_1g8piFxug)  

[java.util.ConcurrentModificationException详解](https://www.jianshu.com/p/c5b52927a61a)  
迭代ArrayList的Iterator中有一个变量expectedModCount，该变量会初始化和modCount相等，但如果接下来如果集合进行修改modCount改变，就会造成expectedModCount!=modCount
Iterator的remove会修改expectedModCount，对单线程有用多线程无用

[Java并发编程：并发容器之CopyOnWriteArrayList](http://www.cnblogs.com/dolphin0520/p/3938914.html)  
CopyOnWrite容器也是一种读写分离的思想，读和写不同的容器
写操作复制出一个新的容器，然后新的容器里添加元素（加锁），读还是会读到旧的数据（不加锁）

[深入分析ConcurrentHashMap](http://ifeve.com/concurrenthashmap/)  
get方法里将要使用的共享变量都定义成volatile，java内存模型的happen before原则，对volatile字段的写入操作先于读操作
put方法里需要对共享变量进行写入操作，所以为了线程安全，在操作共享变量时必须得加锁
count方法先尝试两次不行再加锁，modCount变量，在put , remove和clean方法里操作元素前都会将变量modCount进行加1，那么在统计size前后比较modCount是否发生变化，从而得知容器的大小是否发生变化。

[ARM环境下Java应用卡顿优化案例](https://mp.weixin.qq.com/s/dqHTbLk9ETnBPQh2WrUmTg)  

[Java8之Consumer、Supplier、Predicate和Function攻略](https://www.cnblogs.com/SIHAIloveYAN/p/11288064.html)  

[java8 Stream的实现原理 (从零开始实现一个stream流)  ](https://www.cnblogs.com/xiaoxiongcanguan/p/10511233.html)  

[Java 8 Stream原理解析](https://mp.weixin.qq.com/s/ykN7tCur0b9KXNOJtvqQ-g)  

[Java内存模型深入分析](https://mp.weixin.qq.com/s/0H9yfiYvWGQByjFT-fj-ww)  

[Java 语言中锁的设计与应用](https://mp.weixin.qq.com/s/cGJxllpHskK4castfOJ_gw)  

[源码深度解析 Handler 机制及应用](https://mp.weixin.qq.com/s/71OV_K7YJas7pLtsPY-jeQ)  

[vivo 调用链 Agent 原理及实践](https://mp.weixin.qq.com/s/vPgBLi-2svhU_t1wN-6ZBA)  

[Oracle JDK7 bug 发现、分析与解决实战](https://mp.weixin.qq.com/s/8f34CaTp--Wz5pTHKA0Xeg)  

## OKHttp Protobuf GRPC TOMCAT

[OkHttp3线程池相关之Dispatcher中的ExecutorService](https://github.com/soulrelay/InterviewMemoirs/issues/7)  

[奇怪的知识： okhttp 是如何支持 Http2 的？](https://mp.weixin.qq.com/s/TeQhe4T4wRjdAEPz6Ne45g)  

[gRPC客户端详解](http://liumenghan.github.io/2019/10/07/grpc-in-depth/)  
Thrift的客户端是线程不安全的——这意味着在Spring中无法以单例形式注入到Bean中
gRPC服务端的Response都是异步形式
gRPC的客户端有同步阻塞客户端（blockingStub)  和异步非阻塞客户端(Stub）两种

[grpc原理](https://www.jianshu.com/p/9e57da13b737)  

[深入了解 gRPC：协议](https://mp.weixin.qq.com/s/GEaTUTp_wuILYTVbZvJo7g)  

[巧用 Protobuf 反射来优化代码，拒做 PB Boy](https://mp.weixin.qq.com/s/ALhSKrLwNdA_GkozJQXn5g)  
获取 PB 中所有非空字段：bool has_field = reflection->HasField(message, field)  ;
将字段校验规则放置在 Proto 中：optional string name   =1 [(field_rule)  .length_min = 5, (field_rule)  .id = 1];
基于 PB 反射的前端页面自动生成方案
通用存储系统(根据反射取出K/V)  

[一文讲懂进程间通信的几种方式](https://mp.weixin.qq.com/s/TZJ0N8iDjU3dEoU6W1GctQ)  

[Tomcat 优雅关闭之路](https://mp.weixin.qq.com/s/ZqkmoAR4JEYr0x0Suoq7QQ)  

[Tomcat 9.0.26 高并发场景下DeadLock问题排查与修复](https://mp.weixin.qq.com/s/GvFeFXsINaQjymnbXkOovw)  

[Tomcat 应用中并行流带来的类加载问题](https://mp.weixin.qq.com/s/f-X3n9cvDyU5f5NYH6mhxQ)  

## REDIS ES MYSQL MongoDB

[Redis小功能大用处(1)  -stat_expired_time_cap_reached_count](https://mp.weixin.qq.com/s/3UxXnSus0HTlA0ndpLZPgg)  

[Redis常见客户端异常汇总(Jedis篇)  ](https://mp.weixin.qq.com/s/T-BBlAQ4B5qj0VCTKUMgXA)  

[Redis在Linux系统的配置优化](https://mp.weixin.qq.com/s/eN8qQn9HjeI1BV-MMFcWiw)  

[Redis module功能介绍](https://mp.weixin.qq.com/s/uN1Iyha96TCG7Ff2ZRW1Dw)  

[替你踩过Redis缓存的坑，奉上使用规范和监控方法](https://mp.weixin.qq.com/s/R-slZDV2YNTA_M_sctyvZA)  

[运维：终于不用再背着数万实例的Redis集群了](https://mp.weixin.qq.com/s/F5Wn6OWKzswA4tg2fHrevw)  

[Codis VS Redis Cluster：我该选择哪一个集群方案](https://time.geekbang.org/column/article/85495ce4171a7dc638c424414e229cac/share)  
codis提供了
1.dashboard的fe界面运维简单
2.基于zookeeper的proxy代理slot-mapping映射
3.基于sentinel的主从切换高可用
codis提供了异步的数据迁移方案（其中对大key拆分迁移的原子性方案），对比redis-cluster来说相对应用较早

[Redis 缓存性能实践及总结](https://mp.weixin.qq.com/s/bsAw0VKhP_SYngvKMoByAQ)  

[pika集群水平扩展——让性能容量不再受限](https://mp.weixin.qq.com/s/xksAosZSpLVjb1JuX5XhRQ)  

[超全的数据库建表/SQL/索引规范，适合贴在工位上！](https://mp.weixin.qq.com/s/-_m-OJ_PUXrw8Gey2ZA9aA)  

[我在MySQL原厂的那些年都经历了什么？](https://mp.weixin.qq.com/s/HW7tji_fQBeOa7kr2xsmtg)  

[高并发场景下，百万级订单量系统的分库分表重构经历](https://mp.weixin.qq.com/s/Mp6yyS3VVj2v1D1gxuojwA)  

[3年部署3000套PG实例的架构设计与踩坑经验](https://mp.weixin.qq.com/s/OPQr248yiwFI9Q6vLx9tZw)  

[简述3种CQRS架构模式](https://mp.weixin.qq.com/s/pToMvg5tNJKfRmJdNfXicw)  

[InnoDB 事务加锁分析](https://mp.weixin.qq.com/s/S7MhlsZveBHRSQhq5aTIJA)  

[MySQL 的 crash-safe 原理解析](https://mp.weixin.qq.com/s/5i9wmJs4_Er7RaYfNnETyA)  

[MySQL 8 新特性之Clone Plugin](https://mp.weixin.qq.com/s/_HqRXhQX_6e0boACzcAH8g)  

[干货 | 携程Elasticsearch数据同步实践](https://mp.weixin.qq.com/s/2PRX_vVhi3SygrZydBfG6w)  

[一次看完28个关于ES的性能调优技巧](https://mp.weixin.qq.com/s/nnOazH26pq-Kn8zlGKgvTA)  

[ElasticSearch使用规范beta版](https://mp.weixin.qq.com/s/yCTNH1hMp6iOvHgh9vWg6A)  
非日志型(搜索型、线上业务型)  的shard容量在10~30GB（建议在10G）
日志型的shard容量在30~100GB（建议30G）
单个shard的文档个数不能超过21亿左右(Integer.MAX_VALUE - 128)  

一个节点管理的shard数不要超过200个
routing id不均衡：集群容量和访问不均衡，对于分布式存储是致命的。
拒绝大聚合 ：ES计算都在JVM内存中完成。
拒绝模糊查询：es一大杀手
拒绝深度分页
禁止查询 indexName-*

[干货 | 携程Elasticsearch数据同步实践](https://mp.weixin.qq.com/s/2PRX_vVhi3SygrZydBfG6w)  

[分布式搜索引擎Elasticsearch的架构分析](https://mp.weixin.qq.com/s/N_y7BxbO9pCTrgJbDq4bOA)  

[连环触发！MongoDB核心集群雪崩故障背后竟是……](https://mp.weixin.qq.com/s/3bzKacpe3TD6k0y4pFXHCQ)  

## KAFKA MQ ZOOKEEPER

[运维必备：Zookeeper集群“脑裂”问题处理大全](https://mp.weixin.qq.com/s/1NN62CWrCCRpCMNtKsRMvA)  

[运维必备：Zookeeper集群“脑裂”问题处理大全](https://mp.weixin.qq.com/s/1NN62CWrCCRpCMNtKsRMvA)  

[这样做RabbitMQ高可用，业务流量猛增10倍也不怂](https://mp.weixin.qq.com/s/vLTi_VRjeuW_ekDKzcE5ug)  

[vivo 基于原生 RabbitMQ 的高可用架构实践](https://mp.weixin.qq.com/s/7s9-RsLWgiVvw28U51J0bA)  

[简单理解 Kafka 的消息可靠性策略](https://mp.weixin.qq.com/s/T6gCc8OBgyV-yeAg_MUzPQ)  
ISR : leader 副本保持一定同步的 follower 副本, 包括 leader 副本自己，叫 In Sync Replica
HW: Highwater, 俗称高水位，它表示了一个特定的消息偏移量(offset)  , 在一个 parttion 中 consumer 只能拉取这个 offset 之前的消息(此 offset 跟 consumer offset 不是一个概念)   ；
LEO: LogEndOffset, 日志末端偏移量, 用来表示当前日志文件中下一条写入消息的 offset；
leader HW: 该 Partititon 所有副本的 LEO 最小值；
follower HW: min(follower 自身 LEO 和 leader HW)  ；
Leader HW = 所有副本 LEO 最小值；
Follower HW = min(follower 自身 LEO 和 leader HW)  
Leader 不仅保存了自己的 HW &LEO 还保存了远端副本的 HW &LEO
在kafka配置为AP系统的情况下发生截断发生的概率会大大提升
Kafka Broker 会在内存中为每个分区都缓存 Leader Epoch 数据，同时它还会定期地将这些信息持久化到一个 checkpoint 文件中

[Linux Page Cache调优在Kafka中的应用](https://mp.weixin.qq.com/s/MaeXn-kmgLUah78brglFkg)  

## Hystrix Seata Sentinel

[分钟简述熔断器使用方法](https://mp.weixin.qq.com/s/KelqbAbRCUQdW3H8NjnooA)  

[Sentinel 是如何做限流的](https://mp.weixin.qq.com/s/s2h4dzFHse6LW0l0gudp9A)  

[Seata是什么？一文了解其实现原理](https://mp.weixin.qq.com/s/w-Tq6HFx5PqXpAPFDO_huA)  

[Canal 组件简介与 vivo 帐号实践](https://mp.weixin.qq.com/s/X1OFhjpHZSuIMr5PmxXBRQ)  

[Hystrix 如何解决 ThreadLocal 信息丢失](https://mp.weixin.qq.com/s/nmHcf7yQJIHXBf6W4KtF8w)  

[分布式事务：Saga模式](https://www.jianshu.com/p/e4b662407c66)  

## logging tracing metrics

[构建 Netflix 分布式追踪（tracing）体系](https://mp.weixin.qq.com/s/NmGYfoJ7pw8CfRfUkc6o2Q)  

[报警的哲学](https://mp.weixin.qq.com/s/lJRPt7I0SeUwZ4HhVZn8AQ)  

[基于Prometheus来做微服务监控，有多吃香？](https://mp.weixin.qq.com/s/2xsF_JqGJCIz5i6CZ_ckxg)  

[Prometheus远程写入InfluxDB，遇到OOM引发的错误怎么解？](https://mp.weixin.qq.com/s/SvWg2XpdpSlFKbwGayJV2w)  

[深入浅出开源监控系统Prometheus（上）](https://mp.weixin.qq.com/s/4NC4spF6cEvXuIBKVbIU7A)  

[OpenTSDB 数据存储详解](https://mp.weixin.qq.com/s/qayKiwk5QAIWI7-nyD3FVA)  

## CI CD UnitTest

[如何有效地进行代码 Review？](https://mp.weixin.qq.com/s/uFivYfX53s5zAe6hacznlg)  

[这才是真正的Git——Git实用技巧](https://mp.weixin.qq.com/s/qNqZvjy0RXC0MA5WdSUhAA)  

[万字详文告诉你如何做 Code Review](https://mp.weixin.qq.com/s/c3RApB8a98tWahgC9mahJg)  

[如何有效地进行代码 Review？](https://mp.weixin.qq.com/s/uFivYfX53s5zAe6hacznlg)  

## TCP HTTP STOCKET

[爱奇艺网络协程编写高并发应用实践](https://mp.weixin.qq.com/s/XAEzZAUYuOhuqMOszNFe2A)  

[彻底弄懂TCP协议：从三次握手说起](https://mp.weixin.qq.com/s/6LiZGMt2KRiIoMaLwx-lkQ)  

[一文读懂 HTTP/1HTTP/2HTTP/3](https://mp.weixin.qq.com/s/fy84edOix5tGgcvdFkJi2w)  

[白话http2的多路复用](https://mp.weixin.qq.com/s/-mg4AD4-ea_W3aUFchSQIQ)  

[移动端APM网络监控与优化方案](https://mp.weixin.qq.com/s/MnlxrUwB57lqE1aExr0iGA)  
网络状况较好时，HTTP2.0多路复用，带来了性能上的优势，但在网络不稳定时，HTTP1.1错误率低于HTTP2.0。
1. 多域名连接共享，实现0RTT多路复用。
2. 避免了DNS解析，防止DNS劫持。
3. protobuf编码私有协议，节省流量。

[深入理解web协议(二)  ：DNS、WebSocket](https://mp.weixin.qq.com/s/AkbAN4UZLDf841g1ZLFPBQ)  

[深入理解 web 协议(一)  - http 包体传输](https://mp.weixin.qq.com/s/WlT8070LlrnSODFRDwZsUQ)  

## HIVE HBASE FLINK KYLIN

[万亿数据下的多维实时分析系统，如何做到亚秒级响应](https://mp.weixin.qq.com/s/D1Ta8lZI7bDRhLTFbEuohw)  

[高效大数据开发之 bitmap 思想的应用](https://mp.weixin.qq.com/s/c5HDenoVde11IMmezj1v1Q)  

[滴滴实时数仓逐层剖解：实时与离线数据误差<0.5%](https://mp.weixin.qq.com/s/xEUL5ust9btXqiNiMm5wPQ)  

[数仓引入ClickHouse之后，性能提升了400%！](https://mp.weixin.qq.com/s/mJplgZmU6OZqE30cfJvw7Q)  

[pache Kylin的实践与优化](https://mp.weixin.qq.com/s/0S1ih4SJ7xEu0h557yJmgA)  
美团之前没有好好的看kylin的源码和配置参数，导致线上的CPU、内存、文件数没有规划
引擎升级至：spark（最近是flink了） 数据源采用hive 全局字典依赖配置 

[应对万亿数据上亿并发！字节跳动的图数据库研发实践](https://mp.weixin.qq.com/s/WyOMumZyqs_ouaPJpJouNg)  
自研的图数据库：解决了热点无问题，性能单节点R:200k W:10k
不支持ACID事务、总之还在发展中

[基于Kafka+Flink平台化设计，实时数仓还能这样建](https://mp.weixin.qq.com/s/0wF_C8mpYBb8KFB0KhwodA)  
网易云音乐将实时计算的多个sink下kafkasource的重复消费问题：增加了一个data update标记同一个表计算合并
多个kafka集群部署在一个交换机下，离线计算和实时计算等其他情况造成的交换机带宽问题：拆分机房规划

[HBase数据迁移到Kafka？这种逆向操作你懵逼了吗？](https://mp.weixin.qq.com/s/-J9nQs8IjEOcSj849tYigg)  

## serviceMesh K8S Docker

[Service Mesh在腾讯云中间件团队的实践与思考](https://mp.weixin.qq.com/s/uRgPfHgaiba1MuSxzkj4tQ)  

[后分布式时代: 多数派读写的「少数派」实现](https://mp.weixin.qq.com/s/HxWWd_bp9srImcIlXr6b0Q)  

[你真的知道怎么实现一个延迟队列吗 ？](https://mp.weixin.qq.com/s/DcyXPGxXFYcXCQJII1INpg)  

[微信研发体系下的分布式配置系统设计概要](https://mp.weixin.qq.com/s/zYH5SWVT2JFilhyeCCw-1g)  

[Docker 底层原理浅析](https://mp.weixin.qq.com/s/0jFHlWAeH5avIO2NLpTmGA)  

[越来越复杂，为什么是中台？](https://mp.weixin.qq.com/s/902mLsl17r6ut3qRjvZAtg)  

[Kubernetes Ingress 基于内容的路由](https://mp.weixin.qq.com/s/Bk0Ve5sXAsd1I9F85m8_SQ)  

[高可用架构怎么选？常见多活建设这么一对比就懂了](https://mp.weixin.qq.com/s/nuT38bPo3Y2bQ1h5_sWiWQ)  

[谷歌可靠性工程的“封神”之路：从大规模分布式系统高效故障响应说起](https://mp.weixin.qq.com/s/WRx9ZKumRh-JdjAZuxGwPw)  

[复杂环境下落地Service Mesh的挑战与实践](https://mp.weixin.qq.com/s/Z-Nv7XId7EbPpH8UDjWxCQ)  

## delete later

[漫画：什么是一致性哈希？](https://mp.weixin.qq.com/s/i18-XDh8A9jHbnjJ-KTuOQ)  

[漫画：三种 “奇葩” 的排序算法](https://mp.weixin.qq.com/s/oMyyV1u7HKY2IvKwNG7GpA)  

[原创 | 手把手刷二叉搜索树（第二期）](https://mp.weixin.qq.com/s/SuGAvV9zOi4viaeyjWhzDw)  

[读者问：到底怎么学操作系统和计算机网络呀？](https://mp.weixin.qq.com/s/d11_9UxF-cQ4U1UzEzz68Q)  

[双指针技巧秒杀四道数组/链表题目](https://mp.weixin.qq.com/s/55UPwGL0-Vgdh8wUEPXpMQ)  

[只能用分布式锁，也能搞定每秒上千订单的高并发优化？](https://mp.weixin.qq.com/s/jLJLDhK8Ev4T2648RC_FMA)  

[万字详文阐释程序员修炼之道](https://mp.weixin.qq.com/s/XIwfj_AdZqX_vHM4VIq9EA)  

[SpringBoot 配置类解析](https://mp.weixin.qq.com/s/NvPO5-FWLiOlrsOf4wLaJA)  

[分布式定时任务调度框架实践](https://mp.weixin.qq.com/s/l4vuYpNRjKxQRkRTDhyg2Q)  

[一文了解 Consistent Hash](https://mp.weixin.qq.com/s/LGLqEOlGExKob8xEXXWckQ)  