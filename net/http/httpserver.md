### HTTP Server 

#### HTTP Server 的作用
对于一款HTTP Server 其主要作用有：

* 处理http请求，返回数据给浏览器 
* 负载均衡/反向代理/健康检查
* 日志(access log)
* URL Rewrite
* gzip 
* cache-control（expire,etag,last-modified） 
* keep-alive（长连接、短连接）

#### Nginx 分流
Nginx 可以通过很多种方式进行分流。