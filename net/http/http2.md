### HTTP2.0

#### 从 SPDY/2 说起
HTTP2.0 在谷歌的开发者工具里面简称为h2。首先 h2 来源于 SPDY/2 但是，HTTP/2 跟 SPDY 仍有不同的地方，主要是以下两点：

1. HTTP/2 支持明文 HTTP 传输，而 SPDY 强制使用 HTTPS
2. HTTP/2 消息头的压缩算法采用 HPACK，而非 SPDY 采用的 DEFLATE

#### HTTP/2 相比于 HTTP/1.1 的新特性
1. 二进制传输。HTTP/2 采用二进制格式传输数据，而非 HTTP/1.x 的文本格式。二进制格式在协议的解析和优化扩展上带来更多的优势和可能。
2. 头部压缩。HTTP/2 对消息头采用 HPACK 进行压缩传输，能够节省消息头占用的网络的流量。而 HTTP/1.x 每次请求，都会携带大量冗余头信息，浪费了很多带宽资源。头压缩能够很好的解决该问题。
3. 多路复用。直白的说就是所有的请求都是通过一个 TCP 连接并发完成。HTTP/1.x 虽然通过 pipeline 也能并发请求，但是多个请求之间的响应会被阻塞的，所以 pipeline 至今也没有被普及应用，而 HTTP/2 做到了真正的并发请求。同时，流还支持优先级和流量控制。
4. Server Push。服务端能够更快的把资源推送给客户端。例如服务端可以主动把 JS 和 CSS 文件推送给客户端，而不需要客户端解析 HTML 再发送这些请求。当客户端需要的时候，它已经在客户端了。

参考[http://io.upyun.com/2015/05/13/http2/](http://io.upyun.com/2015/05/13/http2/)

#### 新的 QUIC( quick udp internet connection )
QUIC 和 SPDY 一样又是谷歌开发的一个基于udp 的新协议。QQ空间已经将其用户实践，其拥有的具体优点包括：

1. 减少了 TCP 三次握手及 TLS 握手时间。握手包就可以传输数据。
2. 改进的拥塞控制。
3. 避免队头阻塞的多路复用。数据包可以不安顺序到达
4. 连接迁移。
5. 前向冗余纠错

参考[https://cloud.tencent.com/developer/article/1017235](https://cloud.tencent.com/developer/article/1017235)
