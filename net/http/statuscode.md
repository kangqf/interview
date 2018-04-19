### HTTP 状态码

首先 2xx 系列标识正常， 3xx 系列标识重定向，4xx标识客户端请求错误，5xx标识服务器响应错误。

* 1XX 
Informational（信息性状态码） 接收的请求正在处理
* 2XX 
Success（成功状态码） 请求正常处理完毕
* 3XX 
Redirection（重定向状态码）  需要进行附加操作以完成请求
* 4XX 
Client Error(客户端错误状态码）  服务器无法处理请求
* 5XX 
Server Error（服务器错误状态码）  服务器处理请求出错


1. 200 OK
  正常响应
2. 304 Not Modified
  内容未修改，直接读取本地的缓存
  ![](/assest/img/http304.png)
3. 401 Unauthorized
  未鉴权
4. 403 Forbidden
  未授权
5. 404 Not Found
  内容未找到    
6. 204 No Content
  请求执行成功，但是无数据返回，例如将一个文件PUT到服务器成功后就返回204以表征文件放置成功。使用   DELETE  删除一个文件也是一样的道理
7. 206 Partial Content 
  部分内容，当客户端进行了范围请求的时候服务器就会以该响应作为应答并在报文中附加 Content-Range 的首部指定当前返回的内容所属的范围
1. 301 Moved Permanently
  永久性重定向，客户端应该更新自己的书签
1. 302  Found
  临时性重定向，只是希望用户知道该状况
1. 303  See Other
  告知客户端存在里一个资源的URI，客户端应该通过GET方法向该URI请求数据
1. 307 Temporary Redirect
  也是临时重定向，区别在于302不允许将POST变换成GET 307允许
1. 400 Bad Request
  报文中有语法错误
1. 500 Internal Server Error
  服务器在响应请求时发生错误
1. 503 Service Unavailable 
  服务不可用，服务器可能在维护
1. 101 Switching Protocols
 服务器将遵从客户的请求转换到另外一种协议，例如从http/1.0 到1.1 或 https
1. 502：作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。nginx无法与php-fpm进行连接，nginx在连接php-fpm一段时间后发现与php-fpm的连接被断开。
1. 504：作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器（URI标识出的服务器，例如HTTP、FTP、LDAP）或者辅助服务器（例如DNS）收到响应。504即nginx超过了自己设置的超时时间，不等待php-fpm的返回结果，直接给客户端返回504错误



### Ajax 状态码

201——提示知道新文件的URL

更多ajax状态码相关:[https://www.jianshu.com/p/a01cfd612b69](https://www.jianshu.com/p/a01cfd612b69)