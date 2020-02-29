# 4S Analysis & User Management System

## **系统分析和设计** 

**\#\#\#\# System Design, OO Design, API Design**

**- HR 通知有这样的面试时要问清具体是什么类型的design**  


**- 系统设计面试的一般评分标准**

  **- 可行解 workable solution 25%**

  **- 特定问题 special case 20%**

  **- 分析能力 analysis 25%**

  **- 权衡 tradeoff 15%**

  **- 知识储备 knowledge base 15%**

**- 4s 分析法**

  **- scenario**

      **- 需要设计哪些功能, 要设计得多牛**

      **- ask features/QPS/DAU/Interfaces**

  **- service**

      **- 将大系统拆分为小服务**

      **- Split application / module**

  **- storage \(核心\)**

      **- 数据如何存储与访问**

      **- Schema / data / SQL / noSQL / File system**

  **- scale**

      **- 解决缺陷, 处理可能遇到的问题**

      **- sharding / Optimize / special case**  


**\#\#\#\# Scenario \(场景\)**

**- Ask 问面试官**

  **- 需要设计哪些功能\(也可以自己想\)**

      **- 第一步 Step 1:Enumerate**

          **- 说人话:把Twitter的功能一个个罗列出来 • Register / Login**

          **- User Profile Display / Edit**

          **- Upload Image / Video \***

          **- Search \***

          **- Post / Share a tweet**

          **- Timeline / News Feed**

          **- Follow / Unfollow a user**

      **- 第二步 Step 2:Sort**

          **- 说人话:选出核心功能，因为你不可能这么短的时间什么都设计**

          **- Post a Tweet**

          **- Timeline**

          **- News Feed**

          **- Follow / Unfollow a user**

          **- Register / Login**

  **- 需要承受多大的访问量**

      **- 日活跃用户\(DAU\)**

      **- Twitter: MAU 313M, DAU ~150M+**

**- Analysis & Predict分析**

  **- 并发用户 Concurrent User**

      **- 日活跃 \* 每个用户平均请求次数 / 一天多少秒 = 150M \* 60 / 86400~ 100k**

      **- 峰值 Peak = Average Concurrent User \* 3 ~ 300k**

          **- 快速增长的产品 Fast Growing**

              **- MAX peak users in 3 months = Peak users \* 2**

  **- 读频率 Read QPS \(Queries Per Second\)**

      **- 300k**

  **- 写频率 Write QPS**

      **- 5k**

  **- QPS**

      **- QPS = 100**

          **- 用你的笔记本做 Web 服务器就好了**

      **- QPS = 1k**

          **- 用一台好点的 Web 服务器就差不多了**

          **- 需要考虑 Single Point Failure**

      **- QPS = 1m**

          **- 需要建设一个1000台 Web 服务器的集群**

          **- 需要考虑如何 Maintainance\(某一台挂了怎么办\)**

      **- QPS和 Web Server \(服务器\) / Database \(数据库\) 之间的关系**

          **- 一台 Web Server 约承受量是 1k 的 QPS \(考虑到逻辑处理时间以及数据库查询的瓶颈\)**

          **- 一台 SQL Database 约承受量是 1k 的 QPS\(如果 JOIN 和 INDEX query比较多的话，这个值会更小\)**

          **- 一台 NoSQL Database \(Cassandra\) 约承受量是 10k 的 QPS**

          **- 一台 NoSQL Database \(Memcached\) 约承受量是 1M 的 QPS**  


**\#\#\#\# Service \(服务\)**

**- 将大系统拆分为小服务**

  **- 第一步 Step 1: Replay**

      **- 重新过一遍每个需求，为每个需求添加一个服务**

  **- 第二步 Step 2: Merge**

      **- 归并相同的服务**

  **- 什么是服务 Service?**

      **- 可以认为是逻辑处理的整合**

      **- 对于同一类问题的逻辑处理归并在一个 Service 中**

      **- 把整个 System 细分为若干个小的 Service**  


**\#\#\#\# Storage \(存储\)**

**- 关系型数据库 SQL Database**

  **- 小调查:Twitter的哪些信息适合放在关系型数据库中?**

  **- 用户信息 User Table**

**- 非关系型数据库 NoSQL Database**

  **- 小调查:Twitter的哪些信息适合放在非关系型数据库中?**

  **- 推文 Tweets**

  **- 社交图谱 Social Graph \(followers\)**

**- 文件系统 File System**

  **- 小调查:Twitter的哪些信息适合放在文件系统中?**

  **- 图片、视频 Media Files**

**- 案例 - 新鲜事系统 News Feed**

  **- 什么是新鲜事 News Feed?**

      **- 你登陆 Facebook / Twitter / 朋友圈 之后看到的信息流**

      **- 你的所有朋友发的信息的集合**

  **- 有哪些典型的新鲜事系统?**

      **- Facebook**

      **- Twitter**

      **- 朋友圈**

      **- 今日头条**

  **- 新鲜事系统的核心因素?**

      **- 关注与被关注**

      **- 每个人看到的新鲜事都是不同的**

**- \*\*Pull Model\*\***

**- 算法**

  **- 在用户查看News Feed时，获取每个好友的前100条Tweets，合并出前100条News Feed**

      **- K路归并算法 Merge K Sorted Arrays**

**- 复杂度分析**

  **- News Feed =&gt; 假如有N个关注对象，则为N次DB Reads的时间 + N路归并时间\(可忽略\)**

      **- 为什么K路归并的时间可以忽略?**

  **- Post a tweet =&gt; 1次DB Write的时间**  


**- \*\*Push Model\*\***

**- 算法**

  **- 为每个用户建一个List存储他的News Feed信息**

  **- 用户发一个Tweet之后，将该推文逐个推送到每个用户的News Feed List中**

      **- 关键词:Fanout**

  **- 用户需要查看News Feed时，只需要从该News Feed List中读取最新的100条即可**

**- 复杂度分析**

  **- News Feed =&gt; 1次DB Read**

  **- Post a tweet =&gt; N个粉丝，需要N次DB Writes**

      **- 好处是可以用异步任务在后台执行，无需用户等待**  


**\#\#\#\# Scale \(扩展\)**

**- 第一步 Step 1: Optimize**

  **- 解决设计缺陷 Solve Problems**

      **- Pull vs Push, Normalize vs De-normalize**

  **- 更多功能设计 More Features**

      **- Edit, Delete, Media, Ads**

  **- 一些特殊用例 Special Cases**

      **- Lady Gaga, Inactive Users**

**- 第二步 Step 2: Maintenance**

  **- 鲁棒性 Robust**

      **- 如果有一台服务器/数据库挂了怎么办**

  **- 扩展性 Scalability**

      **- 如果有流量暴增，如何扩展**

**- 解决Pull的缺陷**

  **- 最慢的部分发生在用户读请求时\(需要耗费用户等待时间\)**

      **- 在DB访问之前加入Cache**

      **- Cache每个用户的Timeline**

          **- N次DB请求 → N次Cache请求 \(N是你关注的好友个数 \)**

          **- Trade off: Cache所有的?Cache最近的1000条?**

      **- Cache 每个用户的 News Feed**

          **- 没有Cache News Feed的用户: 归并N个用户最近的100条Tweets，然后取出结果的前100条**

          **- 有Cache News Feed的用户: 归并N个用户的在某个时间戳之后的所有Tweets**  


**- 解决Push的缺陷**

  **- 浪费更多的存储空间 Disk**

      **- 与Pull模型将News Feed存在内存\(Memory\)中相比**

      **- Push模型将News Feed存在硬盘\(Disk\)里完全不是个事儿**

      **- Disk is cheap**

  **- 不活跃用户 Inactive Users**

      **- 粉丝排序 Rank followers by weight \(for example, last login time\)**

  **- 粉丝数目 followers &gt;&gt; 关注数目 following**

      **- Lady Gaga问题**

      **- 无解?完全切换回Pull?**

      **- Trade off: Pull + Push vs Pull**

  **- Lady Gaga 案例**

      **- 粉丝 Followers 65.5M**

          **- Justin Bieber 80M on Instagram**

          **- 谢娜 90M on Weibo**

          **- 以上数据来自2017年3月**

      **- Push 的挑战**

          **- Fanout 的过程可能需要几个小时!**

      **- 面试时错误的回答方案**

          **- 既然 Push 不行，那我们就切换到 Pull 吧!**

          **- 说起来好容易啊!**

      **- 正确的思路**

          **- 尝试在现有的模型下做最小的改动来优化**

              **- 比如多加几台用于做 Push 任务的机器，Problem Solved!**

          **- 对长期的增长进行估计，并评估是否值得转换整个模型**

      **- Push 结合 Pull 的优化方案**

          **- 普通的用户仍然 Push**

          **- 将 Lady Gaga 这类的用户，标记为明星用户**

          **- 对于明星用户，不 Push 到用户的 News Feed 中**

          **- 当用户需要的时候，来明星用户的 Timeline 里取，并合并到 News Feed 里**

      **- 摇摆问题**

          **- 明星定义**

              **- followers &gt; 1m**

          **- 邓超掉粉**

              **- 邓超某天不停的发帖刷屏，于是大家果取关，一天掉了几十万粉**

          **- 解决方法**

              **- 明星用户发 Tweet 之后，依然继续 Push 他们的 Tweet 到所有用户的 News Feed 里**

                  **- 原来的代码完全不用改了**

              **- 将关注对象中的明星用户的 Timeline 与自己的 News Feed 进行合并后展示**

                  **- 但并不存储进自己的 News Feed 列表，因为 Push 会来负责这个事情。**  


**\#\#\#\# 数据库与缓存**

**- Design User System**

  **- Scenario 场景**

      **- 注册、登录、查询、用户信息修改**

          **- 哪个需求量最大?**

      **- 支持 100M DAU**

      **- 注册，登录，信息修改 QPS 约**

          **- 100M \* 0.1 / 86400 ~ 100**

          **- 0.1 = 平均每个用户每天登录+注册+信息修改**

          **- Peak = 100 \* 3 = 300**

      **- 查询的QPS 约**

          **- 100 M \* 100 / 86400 ~ 100k**

          **- 100 = 平均每个用户每天与查询用户信息相关的操作次数\(查看好友，发信息，更新消息主页\)**

          **- Peak = 100k \* 3 = 300 k**

  **- Service 服务**

      **- 一个 AuthService 负责登录注册**

      **- 一个 UserService 负责用户信息存储与查询**

      **- 一个 FriendshipService 负责好友关系存储**

  **- Storage: QPS 与 常用数据存储系统**

      **- MySQL / PosgreSQL 等 SQL 数据库的性能**

          **- 约 1k QPS 这个级别**

      **- MongoDB / Cassandra 等 硬盘型NoSQL 数据库的性能**

          **- 约 10k QPS 这个级别 \(**

      **- Redis / Memcached 等 内存型NoSQL 数据库的性能**

          **- 100k ~ 1m QPS 这个级别**

      **- 以上数据根据机器性能和硬盘数量及硬盘读写󿰀度会有区别**

  **- 用户系统特点**

      **- 读多写少**

  **- Cache**

      **- Cache 是什么?**

          **- 缓存，把之后可能要查询的东西先存一下**

              **- 下次要的时候，直接从这里拿，无需重新计算和存取数据库等**

          **- 可以理解为一个 Java 中的 HashMap**

          **- key-value 的结构**

      **- 有哪些常用的 Cache 系统/软件?**

          **- Memcached\(不支持数据持久化\)**

          **- Redis\(支持数据持久化\)**

      **- Cache 一定是存在内存中么?**

          **- 不是**

          **- Cache 这个概念，并没有指定存在什么样的存储介质中**

          **- File System 也可以做Cache**

          **- CPU 也有 Cache**

      **- Cache 一定指 Server Cache 么?**

          **- 不是，Frontend / Client / Browser 也可能有客户端的 Cache**

  **- Eventually Consistency**

      **- 最终一致性**

      **- 数据不一致肯定会有，但要控制在一定范围之内**

  **- Cache aside**

      **- web server 分别与DB 和 cache 进行沟通**

      **- DB 和 Cache之间不直接沟通**

      **- e.p. Memcached + MySQL**

  **- Cache through**

      **- 服务器只和 Cache 沟通**

      **- Cache 负责 DB 去沟通，把数据持久化**

      **- 业界典型代表: Redis\(可以理解为 Redis 里包含了一个 Cache 和一个 DB\)**

      **- memory 和 disk的打包型 database**

  **- 数据库选择原则1**

      **- 大部分的情况，用SQL也好，用NoSQL也好，都是可以的**

  **- 数据库选择原则2**

      **- 需要支持 Transaction 的话不能选 NoSQL**

  **- 数据库选择原则3**

      **- 你想不想偷懒很大程度决定了选什么数据库 SQL更成熟帮你做了很多事儿 NoSQL很多事儿都要亲力亲为\(Serialization, Secondary Index\)**

  **- 数据库选择原则4**

      **- 如果想省点服务器获得更高的性能，NoSQL就更好 硬盘型的NoSQL比SQL一般都要快10倍以上**  


**\#\#\#\# Scale \(扩展\)**

**- Single Point Failure**

  **- Sharding 数据拆分**

      **- Vertical Sharding**

      **- Horizontal Sharding \(important\)**

  **- Replica**

**\#\#\#\# Consistent Hashing**

**- 将整个 Hash 区间看做环**

**- 这个环的大小从 0~359 变为 0~264-1**

**- 将机器和数据都看做环上的点**

**- 引入 Micro shards / Virtual nodes 的概念**

  **- 一台实体机器对应 1000 个 Micro shards / Virtual nodes**

      **- 怎么定1000个点的位置？**

          **- 一般用机器名例如 Memcache-01 加.1, 成为Memcache-01.1作为key给hash函数, hash函数会返回给一个环上的数即为位置**

  **- 按照hash算法得出的点落在换上然后顺时针往前找, 碰到的第一个virtual node即为放数据的那台机器**

**- 每个 virtual node 对应 Hash 环上的一个点**

**- 每新加入一台机器，就在环上随机撒 1000 个点作为 virtual nodes**

**- 需要计算某个 key 所在服务器时**

  **- 计算该key的hash值——得到0~264-1的一个数，对应环上一个点**

  **- 顺时针找到第一个virtual node**

  **- 该virtual node 所在机器就是该key所在的数据库服务器**

**- 新加入一台机器做数据迁移时**

  **- 1000 个 virtual nodes 各自向顺时针的一个 virtual node 要数据**

**\#\#\#\# Replica 数据备份**

**- Backup 和 Replica的区别**

  **- Backup**

      **- 一般是周期性的, 比如每天晚上进行一次备份**

      **- 当数据丢失的时候, 通常只能恢复到之前某个时间点**

      **- 不用做在线的数据服务, 不分摊读**

  **- Replica**

      **- 是实时性的, 在数据写入的时候, 就会以复制品\(Replica\)的形式存为多份**

      **- 当数据丢失的时候, 可以马上通过其他的复制品\(Replica\)恢复**

      **- Replica用作在线的数据服务, 分摊读**  


**\#\#\#\# Master - Slave**

**- Master responsible for write / read Slave responsible for read only 对于大部分系统来说基本够用**

  **- 原理 Write Ahead Log**

  **- SQL 数据库的任何操作，都会以 Log 的形式做一份记录 • 比如数据A在B时刻从C改到了D**

  **- Slave 被激活后，告诉master我在了**

  **- Master每次有任何操作就通知 slave 来读log**

  **- 因此Slave上的数据是有“延迟”的**

**- Master 挂了怎么办?**

  **- 将一台 Slave 升级 \(promote\) 为 Master，接受读+写**

  **- 可能会󿰀成一定程度的数据丢失和不一致**  


**\#\#\#\# auto-increment id**

**- 为什么不把email或者user name做id**

  **- 因为允许改动**  

**系统分析和设计**  


**\#\#\#\# System Design, OO Design, API Design**

**- HR 通知有这样的面试时要问清具体是什么类型的design**  


**- 系统设计面试的一般评分标准**

  **- 可行解 work solution 25%**

  **- 特定问题 special case 20%**

  **- 分析能力 analysis 25%**

  **- 权衡 tradeoff 15%**

  **- 知识储备 knowledge base 15%**

**- 4s 分析法**

  **- scenario**

      **- 需要设计哪些功能, 要设计得多牛**

      **- ask features/QPS/DAU/Interfaces**

  **- service**

      **- 将大系统拆分为小服务**

      **- Split application / module**

  **- storage \(核心\)**

      **- 数据如何存储与访问**

      **- Schema / data / SQL / noSQL / File system**

  **- scale**

      **- 解决缺陷, 处理可能遇到的问题**

      **- sharding / Optimize / special case**  


**\#\#\#\# Scenario \(场景\)**

**- Ask 问面试官**

  **- 需要设计哪些功能\(也可以自己想\)**

      **- 第一步 Step 1:Enumerate**

          **- 说人话:把Twitter的功能一个个罗列出来 • Register / Login**

          **- User Profile Display / Edit**

          **- Upload Image / Video \***

          **- Search \***

          **- Post / Share a tweet**

          **- Timeline / News Feed**

          **- Follow / Unfollow a user**

      **- 第二步 Step 2:Sort**

          **- 说人话:选出核心功能，因为你不可能这么短的时间什么都设计**

          **- Post a Tweet**

          **- Timeline**

          **- News Feed**

          **- Follow / Unfollow a user**

          **- Register / Login**

  **- 需要承受多大的访问量**

      **- 日活跃用户\(DAU\)**

      **- Twitter: MAU 313M, DAU ~150M+**

**- Analysis & Predict分析**

  **- 并发用户 Concurrent User**

      **- 日活跃 \* 每个用户平均请求次数 / 一天多少秒 = 150M \* 60 / 86400~ 100k**

      **- 峰值 Peak = Average Concurrent User \* 3 ~ 300k**

          **- 快速增长的产品 Fast Growing**

              **- MAX peak users in 3 months = Peak users \* 2**

  **- 读频率 Read QPS \(Queries Per Second\)**

      **- 300k**

  **- 写频率 Write QPS**

      **- 5k**

  **- QPS**

      **- QPS = 100**

          **- 用你的笔记本做 Web 服务器就好了**

      **- QPS = 1k**

          **- 用一台好点的 Web 服务器就差不多了**

          **- 需要考虑 Single Point Failure**

      **- QPS = 1m**

          **- 需要建设一个1000台 Web 服务器的集群**

          **- 需要考虑如何 Maintainance\(某一台挂了怎么办\)**

      **- QPS和 Web Server \(服务器\) / Database \(数据库\) 之间的关系**

          **- 一台 Web Server 约承受量是 1k 的 QPS \(考虑到逻辑处理时间以及数据库查询的瓶颈\)**

          **- 一台 SQL Database 约承受量是 1k 的 QPS\(如果 JOIN 和 INDEX query比较多的话，这个值会更小\)**

          **- 一台 NoSQL Database \(Cassandra\) 约承受量是 10k 的 QPS**

          **- 一台 NoSQL Database \(Memcached\) 约承受量是 1M 的 QPS**  


**\#\#\#\# Service \(服务\)**

**- 将大系统拆分为小服务**

  **- 第一步 Step 1: Replay**

      **- 重新过一遍每个需求，为每个需求添加一个服务**

  **- 第二步 Step 2: Merge**

      **- 归并相同的服务**

  **- 什么是服务 Service?**

      **- 可以认为是逻辑处理的整合**

      **- 对于同一类问题的逻辑处理归并在一个 Service 中**

      **- 把整个 System 细分为若干个小的 Service**  


**\#\#\#\# Storage \(存储\)**

**- 关系型数据库 SQL Database**

  **- 小调查:Twitter的哪些信息适合放在关系型数据库中?**

  **- 用户信息 User Table**

**- 非关系型数据库 NoSQL Database**

  **- 小调查:Twitter的哪些信息适合放在非关系型数据库中?**

  **- 推文 Tweets**

  **- 社交图谱 Social Graph \(followers\)**

**- 文件系统 File System**

  **- 小调查:Twitter的哪些信息适合放在文件系统中?**

  **- 图片、视频 Media Files**

**- 案例 - 新鲜事系统 News Feed**

  **- 什么是新鲜事 News Feed?**

      **- 你登陆 Facebook / Twitter / 朋友圈 之后看到的信息流**

      **- 你的所有朋友发的信息的集合**

  **- 有哪些典型的新鲜事系统?**

      **- Facebook**

      **- Twitter**

      **- 朋友圈**

      **- 今日头条**

  **- 新鲜事系统的核心因素?**

      **- 关注与被关注**

      **- 每个人看到的新鲜事都是不同的**

**- \*\*Pull Model\*\***

**- 算法**

  **- 在用户查看News Feed时，获取每个好友的前100条Tweets，合并出前100条News Feed**

      **- K路归并算法 Merge K Sorted Arrays**

**- 复杂度分析**

  **- News Feed =&gt; 假如有N个关注对象，则为N次DB Reads的时间 + N路归并时间\(可忽略\)**

      **- 为什么K路归并的时间可以忽略?**

  **- Post a tweet =&gt; 1次DB Write的时间**  


**- \*\*Push Model\*\***

**- 算法**

  **- 为每个用户建一个List存储他的News Feed信息**

  **- 用户发一个Tweet之后，将该推文逐个推送到每个用户的News Feed List中**

      **- 关键词:Fanout**

  **- 用户需要查看News Feed时，只需要从该News Feed List中读取最新的100条即可**

**- 复杂度分析**

  **- News Feed =&gt; 1次DB Read**

  **- Post a tweet =&gt; N个粉丝，需要N次DB Writes**

      **- 好处是可以用异步任务在后台执行，无需用户等待**  


**\#\#\#\# Scale \(扩展\)**

**- 第一步 Step 1: Optimize**

  **- 解决设计缺陷 Solve Problems**

      **- Pull vs Push, Normalize vs De-normalize**

  **- 更多功能设计 More Features**

      **- Edit, Delete, Media, Ads**

  **- 一些特殊用例 Special Cases**

      **- Lady Gaga, Inactive Users**

**- 第二步 Step 2: Maintenance**

  **- 鲁棒性 Robust**

      **- 如果有一台服务器/数据库挂了怎么办**

  **- 扩展性 Scalability**

      **- 如果有流量暴增，如何扩展**

**- 解决Pull的缺陷**

  **- 最慢的部分发生在用户读请求时\(需要耗费用户等待时间\)**

      **- 在DB访问之前加入Cache**

      **- Cache每个用户的Timeline**

          **- N次DB请求 → N次Cache请求 \(N是你关注的好友个数 \)**

          **- Trade off: Cache所有的?Cache最近的1000条?**

      **- Cache 每个用户的 News Feed**

          **- 没有Cache News Feed的用户: 归并N个用户最近的100条Tweets，然后取出结果的前100条**

          **- 有Cache News Feed的用户: 归并N个用户的在某个时间戳之后的所有Tweets**  


**- 解决Push的缺陷**

  **- 浪费更多的存储空间 Disk**

      **- 与Pull模型将News Feed存在内存\(Memory\)中相比**

      **- Push模型将News Feed存在硬盘\(Disk\)里完全不是个事儿**

      **- Disk is cheap**

  **- 不活跃用户 Inactive Users**

      **- 粉丝排序 Rank followers by weight \(for example, last login time\)**

  **- 粉丝数目 followers &gt;&gt; 关注数目 following**

      **- Lady Gaga问题**

      **- 无解?完全切换回Pull?**

      **- Trade off: Pull + Push vs Pull**

  **- Lady Gaga 案例**

      **- 粉丝 Followers 65.5M**

          **- Justin Bieber 80M on Instagram**

          **- 谢娜 90M on Weibo**

          **- 以上数据来自2017年3月**

      **- Push 的挑战**

          **- Fanout 的过程可能需要几个小时!**

      **- 面试时错误的回答方案**

          **- 既然 Push 不行，那我们就切换到 Pull 吧!**

          **- 说起来好容易啊!**

      **- 正确的思路**

          **- 尝试在现有的模型下做最小的改动来优化**

              **- 比如多加几台用于做 Push 任务的机器，Problem Solved!**

          **- 对长期的增长进行估计，并评估是否值得转换整个模型**

      **- Push 结合 Pull 的优化方案**

          **- 普通的用户仍然 Push**

          **- 将 Lady Gaga 这类的用户，标记为明星用户**

          **- 对于明星用户，不 Push 到用户的 News Feed 中**

          **- 当用户需要的时候，来明星用户的 Timeline 里取，并合并到 News Feed 里**

      **- 摇摆问题**

          **- 明星定义**

              **- followers &gt; 1m**

          **- 邓超掉粉**

              **- 邓超某天不停的发帖刷屏，于是大家果取关，一天掉了几十万粉**

          **- 解决方法**

              **- 明星用户发 Tweet 之后，依然继续 Push 他们的 Tweet 到所有用户的 News Feed 里**

                  **- 原来的代码完全不用改了**

              **- 将关注对象中的明星用户的 Timeline 与自己的 News Feed 进行合并后展示**

                  **- 但并不存储进自己的 News Feed 列表，因为 Push 会来负责这个事情。**  


**\#\#\#\# 数据库与缓存**

**- Design User System**

  **- Scenario 场景**

      **- 注册、登录、查询、用户信息修改**

          **- 哪个需求量最大?**

      **- 支持 100M DAU**

      **- 注册，登录，信息修改 QPS 约**

          **- 100M \* 0.1 / 86400 ~ 100**

          **- 0.1 = 平均每个用户每天登录+注册+信息修改**

          **- Peak = 100 \* 3 = 300**

      **- 查询的QPS 约**

          **- 100 M \* 100 / 86400 ~ 100k**

          **- 100 = 平均每个用户每天与查询用户信息相关的操作次数\(查看好友，发信息，更新消息主页\)**

          **- Peak = 100k \* 3 = 300 k**

  **- Service 服务**

      **- 一个 AuthService 负责登录注册**

      **- 一个 UserService 负责用户信息存储与查询**

      **- 一个 FriendshipService 负责好友关系存储**

  **- Storage: QPS 与 常用数据存储系统**

      **- MySQL / PosgreSQL 等 SQL 数据库的性能**

          **- 约 1k QPS 这个级别**

      **- MongoDB / Cassandra 等 硬盘型NoSQL 数据库的性能**

          **- 约 10k QPS 这个级别 \(**

      **- Redis / Memcached 等 内存型NoSQL 数据库的性能**

          **- 100k ~ 1m QPS 这个级别**

      **- 以上数据根据机器性能和硬盘数量及硬盘读写󿰀度会有区别**

  **- 用户系统特点**

      **- 读多写少**

  **- Cache**

      **- Cache 是什么?**

          **- 缓存，把之后可能要查询的东西先存一下**

              **- 下次要的时候，直接从这里拿，无需重新计算和存取数据库等**

          **- 可以理解为一个 Java 中的 HashMap**

          **- key-value 的结构**

      **- 有哪些常用的 Cache 系统/软件?**

          **- Memcached\(不支持数据持久化\)**

          **- Redis\(支持数据持久化\)**

      **- Cache 一定是存在内存中么?**

          **- 不是**

          **- Cache 这个概念，并没有指定存在什么样的存储介质中**

          **- File System 也可以做Cache**

          **- CPU 也有 Cache**

      **- Cache 一定指 Server Cache 么?**

          **- 不是，Frontend / Client / Browser 也可能有客户端的 Cache**

  **- Eventually Consistency**

      **- 最终一致性**

      **- 数据不一致肯定会有，但要控制在一定范围之内**

  **- Cache aside**

      **- web server 分别与DB 和 cache 进行沟通**

      **- DB 和 Cache之间不直接沟通**

      **- e.p. Memcached + MySQL**

  **- Cache through**

      **- 服务器只和 Cache 沟通**

      **- Cache 负责 DB 去沟通，把数据持久化**

      **- 业界典型代表: Redis\(可以理解为 Redis 里包含了一个 Cache 和一个 DB\)**

      **- memory 和 disk的打包型 database**

  **- 数据库选择原则1**

      **- 大部分的情况，用SQL也好，用NoSQL也好，都是可以的**

  **- 数据库选择原则2**

      **- 需要支持 Transaction 的话不能选 NoSQL**

  **- 数据库选择原则3**

      **- 你想不想偷懒很大程度决定了选什么数据库 SQL更成熟帮你做了很多事儿 NoSQL很多事儿都要亲力亲为\(Serialization, Secondary Index\)**

  **- 数据库选择原则4**

      **- 如果想省点服务器获得更高的性能，NoSQL就更好 硬盘型的NoSQL比SQL一般都要快10倍以上**  


**\#\#\#\# Scale \(扩展\)**

**- Single Point Failure**

  **- Sharding 数据拆分**

      **- Vertical Sharding**

      **- Horizontal Sharding \(important\)**

  **- Replica**

**\#\#\#\# Consistent Hashing**

**- 将整个 Hash 区间看做环**

**- 这个环的大小从 0~359 变为 0~264-1**

**- 将机器和数据都看做环上的点**

**- 引入 Micro shards / Virtual nodes 的概念**

  **- 一台实体机器对应 1000 个 Micro shards / Virtual nodes**

      **- 怎么定1000个点的位置？**

          **- 一般用机器名例如 Memcache-01 加.1, 成为Memcache-01.1作为key给hash函数, hash函数会返回给一个环上的数即为位置**

  **- 按照hash算法得出的点落在换上然后顺时针往前找, 碰到的第一个virtual node即为放数据的那台机器**

**- 每个 virtual node 对应 Hash 环上的一个点**

**- 每新加入一台机器，就在环上随机撒 1000 个点作为 virtual nodes**

**- 需要计算某个 key 所在服务器时**

  **- 计算该key的hash值——得到0~264-1的一个数，对应环上一个点**

  **- 顺时针找到第一个virtual node**

  **- 该virtual node 所在机器就是该key所在的数据库服务器**

**- 新加入一台机器做数据迁移时**

  **- 1000 个 virtual nodes 各自向顺时针的一个 virtual node 要数据**

**\#\#\#\# Replica 数据备份**

**- Backup 和 Replica的区别**

  **- Backup**

      **- 一般是周期性的, 比如每天晚上进行一次备份**

      **- 当数据丢失的时候, 通常只能恢复到之前某个时间点**

      **- 不用做在线的数据服务, 不分摊读**

  **- Replica**

      **- 是实时性的, 在数据写入的时候, 就会以复制品\(Replica\)的形式存为多份**

      **- 当数据丢失的时候, 可以马上通过其他的复制品\(Replica\)恢复**

      **- Replica用作在线的数据服务, 分摊读**  


**\#\#\#\# Master - Slave**

**- Master responsible for write / read Slave responsible for read only 对于大部分系统来说基本够用**

  **- 原理 Write Ahead Log**

  **- SQL 数据库的任何操作，都会以 Log 的形式做一份记录 • 比如数据A在B时刻从C改到了D**

  **- Slave 被激活后，告诉master我在了**

  **- Master每次有任何操作就通知 slave 来读log**

  **- 因此Slave上的数据是有“延迟”的**

**- Master 挂了怎么办?**

  **- 将一台 Slave 升级 \(promote\) 为 Master，接受读+写**

  **- 可能会󿰀成一定程度的数据丢失和不一致**  


**\#\#\#\# auto-increment id**

**- 为什么不把email或者user name做id**

  **- 因为允许改动**    


