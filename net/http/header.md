### HTTP 首部

#### HTTP 报文的组成
* 请求
```
GET URI HTTP/1.1
Host:hostname
```

* 响应
```
HTTP/1.1 status_code reason_phrase 
Date:date
Content-Length:len
Content-Type:type
```

#### HTTP Cookie 的处理
服务端对于一次新的请求会 使用头部关键字要求客户端设置Cookie，例如:
`Set-Cookie: sid=1342077140226724; path=/; expires=Wed,
10-Oct-12 07:12:20 GMT`

客户端使用这个Cookie 的方式是每次都会带着这个Cookie向服务端发起请求。例如：
`Cookie: sid=1342077140226724`

#### 报文结构
HTTP 报文的首部和主体是通过换行回车符进行分割的，回车符的十六进制是 `0x0d 0x0a`

``` 
首部
0x0d0x0a
主体报文
```

####



