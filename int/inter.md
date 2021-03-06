# 面试题

1. 为什么用Restful
可以为多个客户端提供同意规范化的接口，并且方便进行版本的迭代
也可以实现服务的解耦，前后端的分离，前端页面的修改或是APP的更新完全不用修改后台程序
[参考](http://www.scienjus.com/my-restful-api-best-practices/)
2. 为什么Restful比传统的http好
无状态。在调用一个接口（访问、操作资源）的时候，可以不用考虑上下文，不用考虑当前状态，极大的降低了复杂度
规范了HTTP请求动作（PUT，POST等）的使用
[参考](https://stackoverflow.com/questions/2191049/what-is-the-advantage-of-using-rest-instead-of-non-rest-http)
3. Redis的应用场景

缓存 String
消息队列 list的数据结构实现是双向链表，所以可以非常便捷的应用于消息队列（生产者/消费者模型）

计数器 由于Redis是单线程机制，所以不会出现并发问题，所以可以将其作为：计数器，产生唯一ID，做秒杀系统

[参考](http://blog.csdn.net/u013679744/article/details/79209341)
[参考](http://www.bijishequ.com/detail/588807?p=76-78)

4. 和redis比较为什么不用memcached

Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，zset，hash等数据结构的存储

Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用

Redis可以实现主从复制，实现故障恢复

5. 网站权限是怎么做的

RBAC

[参考](https://www.360us.net/article/13.html)

6. Redis用来缓存哪些信息 如果缓存的数据数据库更新了呢

Session 静态资源或静态文件

先把更新的数据写入到数据库中，成功后让缓存失效

不直接删除缓存再更新数据库的原因：试想，两个并发操作，一个是更新操作，另一个是查询操作，更新操作删除缓存后，查询操作没有命中缓存，先把老数据读出来后放到缓存中，然后更新操作更新了数据库。于是，在缓存中的数据还是老的数据，导致缓存中的数据是脏的，而且还一直这样脏下去了

不直接在更新数据库后删除缓存的原因是：防止两个并发写操作使得数据变得不可预期导致脏数据，然后缓存还被删除了就没法回滚数据了

[参考](https://coolshell.cn/articles/17416.html)


7. 进程与线程的区别 线程里面有什么是独立的 没有线程的进程是什么




8. 协程是什么
9. 同步与互斥是怎么做的





























