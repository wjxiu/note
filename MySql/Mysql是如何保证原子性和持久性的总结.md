# Mysql是如何保证原子性和持久性的总结

原子性：undo log

持久性：redo log

一致性：a+c+d

隔离性：mvcc+undo log