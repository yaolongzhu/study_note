## SQL调优

### exlain

|     字段      |                    |      |
| :-----------: | :----------------: | :--: |
|      id       |                    |      |
|  Select_type  |      操作类型      |      |
|     table     |    作用在哪个表    |      |
|  partitions   |                    |      |
|     type      |                    |      |
| Possible_keys |  可能使用哪些索引  |      |
|      key      | 实际使用了哪些索引 |      |
|    Key_len    |                    |      |
|      ref      |                    |      |
|     rows      |    扫描了多少行    |      |
|   filtered    |                    |      |
|     Extra     |                    |      |

### 调优步骤

1. 用explain+sql_no_cache，在实际生产环境跑一下这条sql。使用sql_no_cache的原因是防止缓存的干扰
2. 关注点在possible_keys、keys以及rows上面，看下是否用上了期望中的索引
3. show index from 看下这个表的索引，关注下cardinality这列，这列能体现这个数据的多样性
4. 有些时候选错索引，是因为mysql的统计出了问题，可以用analyze table来修正
5. 如果还是不行，可能是因为mysql觉得现在的选择是最优的，这是因为有时候是否需要回表等因素
6. 可以使用下force index语句来指明用的哪个索引来验证使用这个索引的想法
7. 各个索引
8. 避免索引失效

### Join优化

+ 算法

  + Index Nested-Loop Join(NLJ)

    如果被驱动表的join字段有索引，就会使用NLJ。流程如下：

    1. 驱动表进行全表扫描，循环取出join字段的值
    2. 使用1中取到的值在被驱动表进行树搜索

    所以在INL算法里面，应该使用小表来作为驱动表，减少全表扫码的数量。

  + Block Nested-Loop Join(BNL)

    如果被驱动表的join字段没有索引，就会使用BNL。流程如下：

    1. 把驱动表的数据读入到线程内存join_buffer
    2. 扫描被驱动表，循环获取每一行和join_buffer中的数据对比，满足join条件的就作为结果集返回

    join_buffer_size足够大的情况下，大表和小表做驱动都无所谓。但是不够大的情况下，用小表做驱动表。

    所以增大join_buffer_size也是调优的一个手段

+ 优化算法

  + Multi-range read(MRR)

    通过二级索引来进行查询，在覆盖索引没法起效的时候，会有回表的动作，这个回表必须是一行一行的进行的。如果对二级索引进行多返回值的查询

  + batched key access(BKA)

+ 

