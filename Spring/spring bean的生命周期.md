# spring bean的生命周期

主要分为四个阶段，实例化，属性注入，初始化，销毁。
主要的流程集中在初始化和销毁两个阶段。

1. 实例化
2. 属性注入
3. 初始化
	1. aware接口的对应方法
	2. init-method方法（@PostConstruct）
	3. initialingBean接口的方法
	4. BeanPostProcessor的前置方法
	5. BeanPostProcessor的后置方法
4. 销毁
	DisposableBean的distory方法

测试对象
```java
@Component
public class TestController implements BeanNameAware,InitializingBean, BeanPostProcessor,  DisposableBean {
    @Override
    public void setBeanName(String s) {
        System.out.println("setBeanName   "+s);
    }
    @PostConstruct
    public void init() {
        System.out.println("init");
        // ...
    }
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("afterPropertiesSet");
    }
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessBeforeInitialization");
        return BeanPostProcessor.super.postProcessBeforeInitialization(bean, beanName);
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessAfterInitialization");
        return BeanPostProcessor.super.postProcessAfterInitialization(bean, beanName);
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("destroy");
    }
}
```
结果
```
setBeanName   testController
init
afterPropertiesSet
postProcessBeforeInitialization
postProcessAfterInitialization
重复出现postProcessBeforeInitialization，postProcessAfterInitialization
destroy
````