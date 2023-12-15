## 导致full gc情况
- 老年代或者新生代过小
- 内存泄露
- 大对象
## 排查gc

1. 获取heap dump
   - `jmap -dump:file=<dump-file-path> <pid>`
2. 使用工具分析dump日志，找到占用内存比例最高的对象类型
3. 找到对应的线程
4. 通过线程找到代码

## 解决full gc

可能是sql语句缺少查询条件，导致返回大量结果集，进入老年代
