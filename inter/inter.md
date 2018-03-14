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
 
消息队列  list的数据结构实现是双向链表，所以可以非常便捷的应用于消息队列（生产者/消费者模型）

计数器 由于Redis是单线程机制，所以不会出现并发问题，所以可以将其作为：计数器，产生唯一ID，做秒杀系统。

[参考](http://blog.csdn.net/u013679744/article/details/79209341)
[参考](http://www.bijishequ.com/detail/588807?p=76-78)

 4.  