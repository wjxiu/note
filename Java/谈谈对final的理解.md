# 谈谈对final的理解

final是java的关键字
可以加在变量,方法,类上

|      | 作用              |
| ---- | ----------------- |
| 变量 | 只能赋值,不能更改 |
| 方法 | 不可以被重写      |
| 类   | 不可以被继承      |

## final域重排序规则
### 变量是基本数据类型

#### 写final域重排序规则

如果final变量在构造器内赋值,会在写final变量指令后 构造函数结束前加入storestore屏障

#### 读final域重排序规则

在一个线程中，初次读对象引用和初次读该对象包含的final域，JMM会禁止这两个操作的重排序.通过在两次读之间插入loadload屏障.

### 变量是引用类型

额外添加:禁止在构造函数的引用赋值和其他地方使用final的变量之间重排序

>  注意,上面的写final域重排序规则还是存在,这个是额外添加的,而且是在变量是引用类型添加的

## 关于内存屏障的说明

>https://www.cnblogs.com/yifanSJ/p/16314331.html
>
>**3、Java里面的四种内存屏障**
>
>  LoadLoad屏障：举例语句是**Load1; LoadLoad; Load2** (这句里面的LoadLoad里面的第一个Load对应Load1加载代码，然后LoadLoad里面的第二个Load对应Load2加载代码)，**此时的意思就是，在Load2及后续读取操作从内存读取数据到CPU前，保证Load1从主内存里要读取的数据读取完毕。**
>
>  StoreStore屏障：举例语句是 **Store1; StoreStore; Store2** (这句里面的StoreStore里面的第一个Store对应Store1存储代码，然后StoreStore里面的第二个Store对应Store2存储代码)。**此时的意思就是在Store2及后续写入操作执行前，保证Store1的写入操作已经把数据写入到主内存里面，确认Store1的写入操作对其它处理器可见。**
>
>  LoadStore屏障：举例语句是 **Load1; LoadStore; Store2** (这句里面的LoadStore里面的Load对应Load1加载代码，然后LoadStore里面的Store对应Store2存储代码)，**此时的意思就是在Store2及后续代码写入操作执行前，保证Load1从主内存里要读取的数据读取完毕。**
>
>  StoreLoad屏障：举例语句是 **Store1; StoreLoad; Load2** (这句里面的StoreLoad里面的Store对应Store1存储代码，然后StoreLoad里面的Load对应Load2加载代码)，**在Load2及后续读取操作从内存读取数据到CPU前，保证Store1的写入操作已经把数据写入到主内存里，确认Store1的写入操作对其它处理器可见。**