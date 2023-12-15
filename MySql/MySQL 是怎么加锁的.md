# MySQL 是怎么加锁的

## 什么 SQL 语句会加行级锁？

普通select不会加锁，除非是串行化隔离等级。但是`select ...for update`或者`select ...lock in share mode`加锁，统称为锁定读。还有update delete语句也会加锁

## 行级锁有哪些种类？

三种，分别是record lock,gap lock,next-key lock

record lock :只对某一行加锁

- record分为s和x
  - s:共享锁，和s共享，和x互斥
  - x:排他锁，和s,x都互斥

gap lock:对某个范围加锁

next-key lock:对某个返回和某一行加锁

行级锁分为共享锁（s）和排他锁（x）

## MySQL 是怎么加行级锁的？

加锁的对象是索引，加锁的基本单位是next key lock.

如果next key lock可以在其他情况下可以避免幻读，就退化为gap 或者record

### 唯一索引等值查询

- 当查询的记录是「存在」的，在索引树上定位到这一条记录后，将该记录的索引中的 next-key lock 会**退化成「record」**。
- 当查询的记录是「不存在」的，在索引树找到第一条大于该查询记录的记录后，将该记录的索引中的 next-key lock 会**退化成「gap lock」**。(锁是加在行上的,不能加在不存在的行)

###  有什么命令可以分析加了什么锁？

通过命令`select * from performance_schema.data_locks\G;`\G表示每个字段一行

通过LOCK_MODE 可以确认是 next-key 锁，还是间隙锁，还是记录锁：

- 如果 LOCK_MODE 为 `X`，说明是 next-key 锁；
- 如果 LOCK_MODE 为 `X, REC_NOT_GAP`，说明是记录锁；
- 如果 LOCK_MODE 为 `X, GAP`，说明是间隙锁；

> 为什么唯一索引等值查询并且查询记录存在的场景下，该记录的索引中的 next-key lock 会退化成记录锁？

查到记录，如果插入会因为主键相同报错，不会改变记录

如果修改删除操作可以通过记录锁来阻塞，也不会改变记录

如果查询的值不存在，例如表的id字段有1,2,3,5如果查询4的话，会退化为gap锁。

在5上加了gap就可以避免幻读问题。首先范围是（3，5），只需要保证4不被插入，其他的数据有没有改变都可以，因为其他数据变了也不影响查询4的结果，语句只查询id=4的。

> 剩下的区域以后再探索吧

- [唯一索引范围查询](https://xiaolincoding.com/mysql/lock/how_to_lock.html#唯一索引范围查询)
- [非唯一索引等值查询](https://xiaolincoding.com/mysql/lock/how_to_lock.html#非唯一索引等值查询)
- [非唯一索引范围查询](https://xiaolincoding.com/mysql/lock/how_to_lock.html#非唯一索引范围查询)
- [没有加索引的查询](https://xiaolincoding.com/mysql/lock/how_to_lock.html#没有加索引的查询)