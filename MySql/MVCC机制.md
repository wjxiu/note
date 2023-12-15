# MVCC机制

MVCC机制在读已提交级别下生效的。为了避免加锁导致的读写冲突导致性能下降的另外一套事务隔离机制。

MVCC需要提及以下概念

- 行的隐藏字段 

  - trx_id：保存着对该条记录进行修改操作的事务id
  - roll_ptr:指向历史版本的指针。（历史版本是指许多事务修改前的数据，类似链表的结构）

- read view的四个字段

  - create_trx_id:创建read_view的事务id
  - m_ids:保存着活跃但是未提交的事务id列表
  - min_id:保存着m_id的最小值
  - max_id:保存数据库事务的下一个id

  <img src="https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost4@main/mysql/事务隔离/事务ab的视图2.png" style="zoom:80%;" />

> 注意：readview是保存在事务里边的，不是保存在行记录的

  ## 如何判断事务数据是否可见

![mysql行的可见性判断](https://raw.githubusercontent.com/wjxiu/photo/5aed42c1749723295bee8afd9e9471b811c6a6b1/202312082034284.png)  当事务访问某一行的时候，判断当前行是否是自己创建的。trx_id存在于行里，是指修改该记录的事务

- 是，没有任何问题，可以修改读取

- 否，检查trx_id和自己的read_view的min_id比较
  - 小于等于：说明修改记录的事务在当前事务开始前已经提交了（m_id保存着活跃但未提交的事务id），可以读取
  - 大于：是在当前事务开始前就存在的事务。
- 如果trx_id>=max_id，说明说明修改记录的事务在当前事务开始后创建的事务，不可见。
- 如果trx_id在m_id,说明修改记录的事务还在活跃，不能读取。
- 如果trx_id不再在m_id,说明修改记录的事务没有活跃已经提交，可以读取。
### 总结：
- trx_id<=min_id可见
- trx_id>=max_id不可见
- trx_id在m_ids不可见
- trx_id不在m_ids可见

如果不可见，会顺着roll_ptr一直寻找下一条记录，重复比较，直到某条记录可见。

![s](https://raw.githubusercontent.com/wjxiu/photo/main/202309241338663.png)

## MVCC在读已提交和可重复读区别

区别在于生成read_view的时机。

读已提交：每次读取每一行数据之前都会生成新的read_view

可重复读:事务创建时生成read_view，之后一直使用read_view，不会继续创建
