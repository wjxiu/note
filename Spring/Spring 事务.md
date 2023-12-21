
## Spring 事务
### Spring 事务实现方式有哪些
编程式
 - 优点：灵活
 - 缺点：麻烦，难以维护

声明式
 - 加注解
  - Spring的事务管理有什么优点  
    - 支持声明式事务管理  
    - 提供跨不同事务api的一致事务模型
### Spring事务隔离等级
一共五种
- DEFAULT：根据数据库的隔离等级
- READ_UNCOMMITTED 读未提交
- READ_COMMITTED 读已提交
- REPEATABLE_READ 可重复读
- SERIALIZABLE 串行化
### 传播规则
![image](https://img2023.cnblogs.com/blog/2504876/202309/2504876-20230918163510488-1360224008.png)
推荐阅读

>https://www.cnblogs.com/vipstone/p/16735893.html
>
>https://medium.com/spring-boot/understanding-spring-transaction-propagation-86beeb869527

## Spring事务失效的场景
- 数据库引擎不支持事务
- 不是public方法
- 没有被spring管理
- 自身调用，直接调用@transactional的方法
- 没有配置事务管理器
- 事务的传播等级是不支持事务的，例如never,not_support
- 没有抛出异常
- 抛出异常类型不符合
### 总结
最常见的是:自身调用，抛出异常不合适，不是public方法
```