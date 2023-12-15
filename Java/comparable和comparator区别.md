# comparable和comparator区别

|                  | comparable       | comparator                                                   |
| ---------------- | ---------------- | ------------------------------------------------------------ |
| 是否修改被比较类 | 是               | 否                                                           |
| 包               | java.lang        | java.util                                                    |
| 方法名字         | compareTo        | compare                                                      |
| 方法参数         | (T o)            | (T o,T o1)                                                   |
| 如何使用         | Collections.sort | Collections.sort(list,comparator实例)或者list.sort(comparator实例) |
|                  | 主要用于简单比较 | 用于复杂比较                                                 |

