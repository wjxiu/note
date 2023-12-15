# Spring 依赖注入有几种

## 属性注入

@autowired,默认通过byname查找bean。

有多个同类型的bean就会报错这时候需要通过bytype找到bean，需要配合@quifer注解

@resource,默认通过bytype查找bean

缺点：

- 可能更容易违背单一职责原则，因为可以随便引入对象，导致一个类可能干很多事情
- 不能注入final对象

## Setter注入

```Java
    @Autowired
    public void setUserService(UserService userService) {
        this.userService = userService;
    } 
```

缺点：

- 不能注入final对象
- 麻烦
- 被注入对象可以被修改

## 构造器注入

```
@Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }
```

优点：

- 可以注入final对象
- 注入对象不能修改
- 完全初始化

## 总结

依赖注入的常见实现方式有 3 种：属性注入、Setter 注入和构造方法注入。其中属性注入的写法最简单，所以日常项目中使用的频率最高，但它的通用性不好；而 Spring 官方推荐的是构造方法注入，它可以注入不可变对象，其通用性也更好，如果是注入可变对象，那么可以考虑使用 Setter 注入。