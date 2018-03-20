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
Nginx 可以通过很多种方式进行分流。包括cookie URL header等。
具体的配置方式如下：
1. URL 分流
  ```
  # nginx 根据 url 分流
  location /{}
  location /hello {}
  ```

2. 根据 cookie 分流
  ```
  map $COOKIE_abcdexpid $group {
  	~*1$	apache001;
  	~*2$	apache002;
  	default	root;
  }
  ```

3. 根据 header 分流
  ```
  location / {  
    alias d:/httpdemo/;  
    index index.html;  
              
    #测试header转发  
    if ($http_yfflag = testuser_7){  
        rewrite ^(.*)$ /demo7/$1 last;  
    }  
    if ($http_yfflag = testuser_8){  
    rewrite ^(.*)$ /demo8/$1 last;  
    #proxy_pass http://192.168.25.217:8080;  
    }  
  } 
  ```