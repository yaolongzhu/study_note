## 高性能

+ 高性能数据库集群

  + 读写分离

    ```mermaid
    graph TD
    	server[应用服务器]
    	subgraph 数据库集群
       		master[主机]
       		slave1[从机1]
       		slave2[从机2]
       		master-.复制.->slave1
       		master-.复制.->slave2
    	end
    	server-.写.->master
    	server-.读.->slave1
    	server-.读.->slave2
    	
    ```

    核心思路是写主库，读从库。这样能分担读取的压力，但是写入的压力还是集中在主机上面。

    带来的问题:

    + 主从复制延迟
    + 分配机制
      + 程序代码封装
      + 中间件封装

  + 分表分库

    + 分库
      + 业务分库
        + Join问题
        + 分布式事务
        + 成本
    + 分表
      + 垂直分表
      + 水平分表
        + 路由
          + 范围路由
          + hash路由
          + 配置路由
          + 
        + join
        + count
        + Order by

+ NoSQL

  + K-V存储(redis)
  + 文档数据库(MongoDB)
  + 列式数据库(Hbase)
  + 全文搜索引擎(elasticsearch)

+ 高性能缓存

+ 单服务高性能

  + PPC(process per connection)
  + TPC(thread per connection)
  + Reactor
  + Proactor

+ 负载均衡

  + 分类

    + DNS负载均衡
    + 硬件负载均衡
    + 软件负载均衡(nginx, LVS)

  + 架构

    三个负载均衡分类回组合使用

    DNS实现地理级别负载均衡

    硬件实现集群级别负载均衡

    软件实现机器级别负载均衡

  + 算法

    + 轮询
    + 加权轮询
    + 负载最低优先
    + 性能最优优先
    + hash

+ 

