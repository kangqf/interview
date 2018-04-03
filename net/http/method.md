### HTTP 方法

#### GET

1. get 用于从服务器获取资源
2. get 的数据添加再url后面，因此get的数据长度受到url的长度的限制，而url的长度限制与实际用的浏览器和服务器有关，chrome 是8k。
3. get 请求的url可能会被日志记录从而导致不安全。
4. get 对请求的url 有编码的要求，必须是ascii码。

#### POST
1. post 用于改变服务器的资源
2. post 的数据通常是位于http报文的主体部分，而其数据量的大小限制受到服务器的配置影响，对于nginx 默认是8m。
3. post 的请求不会被日志记录，较为安全。
4. post 刷新页面会重复提交数据。

#### PUT
PUT 通过不带验证的方式向服务器传输文件，一般开放于REST类型的网站

#### DELETE
与PUT相反用户删除服务器中的文件，也是不带验证的一种方法

#### OPTIONS 
询问服务端对指定URI资源所支持的方法，例如：`OPTIONS * http/1.1` 服务端会返回200 + Allow:GET,POST 等支持的方法列表

例如 百度的options 响应是 `Allow:GET,HEAD,POST,OPTIONS,TRACE`

#### HEAD
只返回报文首部，不返回报文主体，用于获取报文的一些信息（确认uri有效性和资源更新日期等）以确定下一步要进行的操作

#### CONNECT TRACE
CONNECT 要求与代理服务器通信时建立隧道
TRACE 让服务器将之前的请求 的通信链路 返回给客户端