# Java正则表达式的使用

## 正则表达式相关类

- `Pattern` 构建pattern对象,构造器参数为正则表达式
- `Matcher`  根据pattern匹配字符串

## 常见方法

- ### Pattern类
	
	- 静态方法`compile(String reg)` 生成一个pattern对象,匹配规则是reg
	
	- `matcher(seq);`根据匹配规则生成对应`Matcher`对象
	
- ### Matcher类
	
	- `find()`如果找到第一个符合正则表达式的字符串,返回true,否则false
  - `group()`返回符合正则表达式的字符串,如果找不到返回控空字符串
  
  ### 例子
  
  ```java
  String reg= "[a-z]";
          String seq="azdfgv765";
          Pattern compile = Pattern.compile(reg);
          Matcher matcher = compile.matcher(seq);
          while (matcher.find()) {
              System.out.print(matcher.group()+" ");
          }
  //返回:a z d f g v 
  ```
  

# 语法

## 基本

- 区分大小写
- 转义字符:`\\`,其他语言为`\`
  - 匹配`(`,正确:`\\()`,错误:`()`
- 普通字符,直接写
  - 匹配 `abc`,需要都有`abc`才符合
- 任意一个小写字符
  - `[a-z]`
- 任意一个大写字符
  - `	[A-Z]`
- 一部分大写,一部分小写
  - `a(?i)bC`:匹配`aBC`,`aBc`
  - `(?i)`表示其后的字符忽略的大小写
  - `a((?i)bCD)FG`:表示忽略`((?i)xxx)`部分字符大小写,其他不受影响
- 一个数字
  - `[1-9]`或者`\\d`
- 取反
  - `^`,如果放在外面就是开头的意思,放在`[]`里边是取反
  - `[^a-z]`表示匹配非a-z的字符
  - `[^A-Z]`表示匹配非A-Z的字符
- 匹配个数
  - `{num}`
  - `[a-z]{3}`匹配连续三个小写字母

- 任意一个字符
  - 这里需要特别注意，如果特殊字符`.` ` \ `    `^` 等需要表示原本含义，在中括号`[]`不用转义,其他地方表示原本含义需要转义
  - `[abcd]`匹配一个a或者一个b或者一个c或者一个d
  
- 匹配多个组合
  - `a|bbb|deq`匹配a或者bbb或者deq
  - 和`[]`不同,`[]`指的是里边的任意一个都符合,而`|`需要整个符合

## 特殊的转义字符

- `\\d`任意一个数字字符
- `\\D`任意一个非数字字符
- `\\w`任意一个字母数字,相当于`[a-zA-Z0-9]`
- `\\W`任意一个非字母数字,相当于`[^a-zA-Z0-9]`
- `\\s`任意一个空白字符,相当于`[\\t\\n ]`<-----有一个空格在]的旁边
- `\\S`任意一个非空白字符,相当于`[^\\t\\n ]`
- `.`任意一个除了`\n`的字符

## 限定符

用于限定字符出现的次数

- `*`[0-n]次
  - `a*bc` 0-n次的a和bc拼接
  - bc,abc, aabc都符合
- `+`[1-n]次
  - `a+bc` 1-n次的a和bc拼接
  - abc, aabc都符合
- `?`[0-1]次
  - `a?bc`0或者1个a和bc结合
  - bc,abc符合
- `{n}`限定n次
  - `a{2}bc`0或者1个a和bc结合
  - aabc符合,其他都不符合
  - [abcd]{2},由a b c d组成,长度只能为2
  - [abcd]{5}, 由a b c d组成,长度只能为5
- {n,}  n-无限次(包括n)
  - `a{2,}bc`至少两个a和bc结合
  - aabc,aaabc,aaabc都符合
- {n,m}  n-m(包括n,m)
  - `a{2,3}bc`至少2个a和bc结合,至多3个a和bc结合
  - aabc,aaabc,aaabc都符合,aaaabc不符合

> {n,m} 会尽可能先匹配多的(贪婪匹配)
>
> `+` `?` `*`都遵循贪婪匹配
>
> 比如 `a{1,3}`, 字符串为`aaaaaaaa`,会先匹配三个a的,接着是2个,最后才是一个

## 捕获分组

- 匿名分组
- `(\\d{2})(\\d{2})`这里就是两个组，使用`()`表示一个组
  - 第0组是整个字串的结果，剩下的就是指定使用（）定义的组
  - 待匹配的串为`123456789`
  - 第1次find() 第0组是`1234`第一组`12`,第二组`34`
  - 第2次find() 第0组是`5678`第一组`56`,第二组`78`
- 非匿名分组,给组命名
- `(?<g1>\\d{2})(?<g2>\\d{2})`
  - 这里的g1和g2就是两个组的名字
  - `System.out.println(matcher.group("g1")+" group 1")`就可以得到第一次find()的第一组为`12`

## 非捕获分组

这个写法是为了简化`|`的语法，比如想要找到`windows 2000|windows 98|windows 10|windows NT`等等具有相同内容的结果是就可以用非捕获分组来简化

这个分组并不会保存到结果中，而是并入到之前的结果中

一共三种写法

- `(?:windows (?:2000|98|NT)`并入相邻的结果中
- `(?=windows (?:2000|98|NT)`只需要重复的结果，不需要`000|98|NT`的内容
- `(?!windows (?:2000|98|NT)`取反,只要不符合条件的结果，结果中并不存在`2000|98|NT)`

例子：

```java
String seq="awindows 2000 is good  bwindows 98 very good cwindows 10 super good dwindows NT extremely bad";
String reg="[a-d]windows (?:2000|98|NT)";
```

返回的结果如下

| 正则 | 结果 |
| ---- | ---- |
|   `(?:windows (?:2000|98|NT)`   | awindows 2000， bwindows 98，  cwindows 10 ，dwindows NT |
|  `(?=windows (?:2000|98|NT)`    | awindows   bwindows dwindows |
|   `(?!windows (?:2000|98|NT)`   | cwindows 10 |

## 非贪婪模式

以最少的方式匹配，只需要在符号前后使用？即可

```
 public static void main(String[] args) {
        String seq="123";
        String reg="\\d+?"; //非贪婪
        TEST(seq,reg);
        reg="\\d+"; //贪婪
        TEST(seq,reg);

    }
    static void TEST(String seq,String reg){
        Pattern compile = Pattern.compile(reg);
        Matcher matcher = compile.matcher(seq);
        while (matcher.find()) {
            System.out.println(matcher.group()+" "+"total");
        }
        System.out.println("-----------------------");
    }
```

```java
1 total
2 total
3 total
-----------------------(非贪婪的结果)
123 total
-----------------------（贪婪的结果）
```

## 定位符号

- `^`指定开始字符
  - `^[1-9]+[a-z]+`表示必须是1-9开头，并且至少有一个小写字母字符
- `$`指定结束字符
  - `^[1-9]+\\-[a-z]+$`表示开头至少一个数字1-9，中间有`-`并且结尾至少有一个小写字母字符

## 反向引用

正则表达式可以在外部获取分组的结果，这个就是反向引用

下面的n代表着第几组，第0组是整个结果

- `\\n` 内部引用，在和分组同一个表达式里边引用
- `$n`外部引用，不在和分组同一个表达式里边引用，而是在获取结果处使用

例子：

获取两个连续相同的数字（11,22,33....）

```
reg="(\\d)\\1"
```

获取类似1221 3883，获取4位数字，第一位和最后一位相同，第2，3位相同

```
reg="(\\d)(\\d)\\2\\1"
```

获取前五位是数字，中间使用`-`风格，连续3个相同数字，-后一共9位

如 12345-111222333，22224-666999333

```
reg="\\d{5}-(\\d)\\1{2}(\\d)\\2{2}(\\d)\\3{2}"
```

# 作业

## 匹配汉字

```
^[\\u4e00-\\u9FA5]+$
```

## 匹配邮箱

```
(([+-]?[1-9])?\\d*|0?)(\\.\\d+)?;
```

## 匹配url

```
^https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#()?&//=]*)$
```

## 去除结巴说的重复字符

```
    public static void main(String[] args) {
        String context="我我我...要学学学....变变变...程！！！";
        context=Pattern.compile("\\.").matcher(context).replaceAll("");
        System.out.println(context);
        String reg="(.)\\1+";
        Pattern compile = Pattern.compile(reg);
        Matcher matcher = compile.matcher(context);
//        while (matcher.find()) {
//            System.out.println("找到了");
//            System.out.println(matcher.group());
//        }
        System.out.println(matcher.replaceAll("$1"));
    }
```

## 匹配正数 负数 小数

```
(([+-]?[1-9])?\\d*|0?)(\\.\\d+)?
```

# 注意事项

## 区分大小写

## 如果特殊字符`.` ` \ `    `^` 等需要表示原本含义，在中括号`[]`不用转义,其他地方表示原本含义需要转义

