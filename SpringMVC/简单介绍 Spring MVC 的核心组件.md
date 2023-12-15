## 简单介绍 Spring MVC 的核心组件

DispatcherServlet 和九大组件（按使用顺序排序的）：

| 组件                        | 说明                                                         |
| --------------------------- | ------------------------------------------------------------ |
| DispatcherServlet           | Spring MVC 的核心组件，是请求的入口，负责协调各个组件工作    |
| MultipartResolver           | 内容类型( Content-Type )为 multipart/* 的请求的解析器，例如解析处理文件上传的请求，便于获取参数信息以及上传的文件 |
| HandlerMapping              | 负责为请求找到合适的 HandlerExecutionChain 处理器执行链，包含处理器（handler）和拦截器们（interceptors） |
| HandlerAdapter              | 处理器的适配器。因为处理器 handler 的类型是 Object 类型，需要有一个调用者来实现 handler 是怎么被执行。Spring 中的处理器的实现多变，比如用户处理器可以实现 Controller 接口、HttpRequestHandler 接口，也可以用 @RequestMapping 注解将方法作为一个处理器等，这就导致 Spring MVC 无法直接执行这个处理器。所以这里需要一个处理器适配器，由它去执行处理器 |
| HandlerExceptionResolver    | 处理器异常解析器，将处理器（ handler ）执行时发生的异常，解析( 转换 )成对应的 ModelAndView 结果 |
| RequestToViewNameTranslator | 视图名称转换器，用于解析出请求的默认视图名                   |
| LocaleResolver              | 本地化（国际化）解析器，提供国际化支持                       |
| ThemeResolver               | 主题解析器，提供可设置应用整体样式风格的支持                 |
| ViewResolver                | 视图解析器，根据视图名和国际化，获得最终的视图 View 对象     |
| FlashMapManager             | 负责重定向时，保存参数至临时存储（默认 Session）             |

Spring MVC 对各个组件的职责划分的比较清晰。DispatcherServlet 负责协调，其他组件则各自做分内之事，互不干扰。

![image-20230911221314702](https://raw.githubusercontent.com/wjxiu/photo/main/202309112213786.png)

