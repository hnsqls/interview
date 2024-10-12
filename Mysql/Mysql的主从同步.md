## Mysql的中从同步原理

主从的模式

![image-20241012180058533](images/Mysql的主从同步.assets/image-20241012180058533.png)

主从同步的原理

![image-20241012180152092](images/Mysql的主从同步.assets/image-20241012180152092.png)



核心就是binlog日志，Mysql的主库在事务提交时会有个binlog日志记录数据库的变化，从库有个线程IOthread去读去主库的binlog日志写入到从库的replylog日志中。然后从库的线程SqlThread执行relylog日志实现主从同步。

