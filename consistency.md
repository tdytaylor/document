### 分布式一致性

[toc]

分布式系统是一个硬件或软件分布在不同的网络计算机上，彼此之前仅仅通过消息传递进行通信和协调的系统。  

#### 分布式的特点
1. 通过消息传递进行通信和协调的系统
2. 分布性
3. 对等性
4. 并发性
5. 缺乏全局时钟
6. 故障总会发生

#### 分布式环境的各种问题
1. 通信异常
2. 网络分区
3. 三态（分布式系统的每一次请求与响应，存在特有的“三态”概念，即成功、失败与超时）
4. 节点故障


#### 隔离级别对比

隔离级别|脏读|可重复度|幻读
:---:|:---:|:---:|:---:|:--:
未授权读取|存在|不可以|存在
授权读|不存在|不可以|存在
可重复读取|不存在|可以|存在
串行化|不存在|可以|不存在

分布式事务是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于分布式系统的不同节点之上。

#### CAP理论
CAP理论告诉我们，一个分布式系统不可能同时满足一致性、可用性和分区容错性这三个基本要求，最多只能同时满足其中两项。（注意：因为这三个基本要求不是对等关系，在分布式环境中，分区容错性一定存在，所以分布式系统都是CP和AP的方式实现）

在实际工程实践中，最终一致性存在以下五类主要变种
1. 因果一致性
2. 读己之所写
3. 会话一致性
4. 单调读一致性
5. 单调写一致性

#### 一致性协议

##### 2PC（Two-Phase Commit）
2PC，即二阶段提交，是计算机网络尤其是在数据库领域内，为了使基于分布式系统架构下的所有节点在进行事务处理过程中能够保持原子性和一致性而设计的一种算法。
##### 协议说明
两阶段提交协议是将事务的提交过程分成了两个阶段来进行处理，其执行流程如下。
##### 阶段一：提交事务请求
1. 事务询问  
协调者向所有的参与者发送事务内容，询问是否可以执行事务提交操作，并开始等待各参与者的响应。
2. 执行事务  
各参与者节点执行事务操作，并将Undo和Redo信息写入事务日志中。
3. 各参与者向协调者反馈事务询问的响应  
如果参与者成功执行了事务操作，那么就反馈给协调者Yes响应，表示事务可以执行；如果参与者没有成功执行事务，那么就反馈给协调者NO响应，表示事务不可以执行。