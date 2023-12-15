# MYSQL慢查询原因，如何排查，如何解决

- 没有加索引,或者没用到索引

- 死锁
  - 事务执行的顺序不合理导致出现死锁
- 查询大量数据
  - 占用大量io资源
- 硬件资源不足
  - 硬件资源不支持复杂查询
- 查询语句复杂
  - 将复杂语句拆分为简单语句，在业务逻辑组合过滤数据
- 大量数据操作
  - 大量增删改数据导致b+树频繁修改结构
- 内存不足
  - 导致大量读取磁盘内容
- 返回不需要的列

-----------

由于慢查询涉及到索引，sql语句， 网络，磁盘io,数据库架构等方面，所以需要先判断这个慢查询的语句需不需要解决，如果很久才出现一次慢查询，可能是由于网络波动，或者redolog满了，需要刷盘。

如果某个sql语句每次运行都特别慢，就要对这个sql排查。

首先是获取慢查询日志。

通过慢查询日志找到慢sql。

---

使用`explain`对sql分析，主要关注：`possible_key` `key` `ken_lens` `extra`四个字段

`possible_key`表明哪个索引可能使用

`key`表明真正使用的索引

`ken_lens`表明索引长度

`extra `一些额外信息，例如using index 使用索引 ,using filesort 读取数据到内存排序

如果explain表明索引没用到，或者key_len很小，证明可以优化。

------

对于慢sql，可以从索引，改写sql,表结构优化下手。

对于索引方面，可以在where order by 用的多的字段并且区分度很高的字段添加索引

对于改写sql,可以将or改为union或者union all。可以将深分页改为滚动分页或者子查询

例如查询id是 10000到10020

从`select * from p where p.start_time_ >xxx  limit 10000, 20`

改成

`select * from p where id >= (select id from pppdr2 where p.start_time_ >xxx limit 100000,1) limit 20`

----

对于数据库架构方面，将频繁使用的字段放在中间表，可以水平分表或垂直分表