# Java集合比较



|          | Arraylist                                                    | LinkedList                     | HashMap                                                      |      |      |
| -------- | ------------------------------------------------------------ | ------------------------------ | ------------------------------------------------------------ | ---- | ---- |
| 容量     | [10-Integer.MAX_VALUE-8]                                     | 0-无穷大                       | [16-2^30]                                                    |      |      |
| 数据结构 | 数组                                                         | 双向链表                       | 数组+链表/红黑树                                             |      |      |
| 优点     | 支持随机访问,动态大小                                        | 插入速度快                     | 增删修查都快                                                 |      |      |
| 缺点     | - 频繁增加删除元素导致性能下降<br />自动装箱拆箱(不支持基本数据类型)<br />可能浪费内存 | 不支持随机访问<br />迭代性能差 | 不保证顺序<br />                                             |      |      |
| 扩容机制 | 如果数组满了,扩容,生成新数组,大小是原来的1.5倍               | 无                             | 如果容量达到容量*负载因子(0.75)就会生成新数组,大小是原来的2倍 |      |      |
|          |                                                              |                                |                                                              |      |      |
| 共同点   | 线程不安全                                                   | 线程不安全                     | 线程不安全                                                   |      |      |
|          |                                                              |                                |                                                              |      |      |

