# count(*) 和 count(1) 有什么区别？哪个性能最好？

count(*) 和 count(1)没有区别，因为count(\*)会优化为count(0)

count(字段)的性能是最差的，因为要遍历一遍，并且还要比较是否是null

结论：count(*)=count(1)>count(字段)

## 怎么解决呢

- 使用近似值，使用explain 命令，返回值有rows列，标识表的近似行数目
- 使用额外的计数表存储行数