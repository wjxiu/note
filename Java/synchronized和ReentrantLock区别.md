- 都是可重入锁，可重入锁指的是线程可以多次获取自己持有的锁
- synchronized (sync)是java的关键字，ReentrantLock (lock)是Java的类
- sync由jvm实现，ReentrantLock 有Java api实现
- sync比较简单，lock比较灵活，适用很多情况
  - 例如：sync不支持中断，lock支持
  - sync不是公平锁，lock支持公平和非公平两种情况

- sync支持重入，lock需要使用reentrantlock类实现

