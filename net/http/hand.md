### TCP 的握手与挥手

#### 三次握手

三次握手的流程为： 
SYN -> SYN/ACK -> ACK 

详细过程见图：

![](/assest/img/http200.png)

##### SYN Flood 攻击
在这里有一个细节，如果客户端 发送 SYN 后就不管了（不发送ACK），立马发送下一个 SYN 就会引发 `SYN Flood` 攻击。
 
一旦接收到Client发来的Syn报文，服务器就需要为该请求分配一个TCB（Transmission Control Block），通常一个TCB至少需要280个字节，在某些操作系统中TCB甚至需要1300个字节，并返回一个SYN ACK命令，立即转为SYN-RECEIVED即半开连接状态，而某些操作系统在SOCK的实现上最多可开启512个半开连接（如Linux2.4.20 内核）。
    
###### SYN 泛洪攻击的方式
1. Direct Attack 攻击方使用固定的源地址发起攻击，这种方法对攻击方的消耗最小
2. Spoofing Attack 攻击方使用变化的源地址发起攻击，这种方法需要攻击方不停地修改源地址，实际上消耗也不大
3. Distributed Direct Attack 这种攻击主要是使用僵尸网络进行固定源地址的攻击
    
###### SYN 泛洪防护的方式
1. 对于第一种攻击的防范可以使用比较简单的方法，即对SYN包进行监视，如果发现某个IP发起了较多的攻击报文，直接将这个IP列入黑名单即可。
2. 无效连接监视释放。这种方法不停监视系统的半开连接和不活动连接，当达到一定阈值时拆除这些连接，从而释放系统资源。这种方法对于所有的连接一视同仁，而且由于SYN Flood造成的半开连接数量很大，正常连接请求也被淹没在其中被这种方式误释放掉，因此这种方法属于入门级的SYN Flood方法。
3. 延缓TCB分配方法。从前面SYN Flood原理可以看到，消耗服务器资源主要是因为当SYN数据报文一到达，系统立即分配TCB，从而占用了资源。而SYN Flood由于很难建立起正常连接，因此，当正常连接建立起来后再分配TCB则可以有效地减轻服务器资源的消耗。常见的方法是使用Syn Cache和Syn Cookie技术。
4. 使用SYN Proxy防火墙。很多防火墙中都提供一种 SYN代理的功能，其主要原理是对试图穿越的SYN请求进行验证后才放行。

详细信息参考：[SYN Flood攻击及防御方法](http://www.cnblogs.com/hubavyn/p/4477883.html)
   
    
#### 四次挥手

四次挥手的流程为：
FIN/ACK -> ACK ... FIN/ACK -> ACK

##### TIME_WAIT
值得注意的是 最后一个ACK发送完后，发送端会把状态 变成 TIME_WAIT 而不是立马关闭连接，而是在此状态停留 两倍的 MSL 时长，原因：
如果最后一个 ACK 丢失，那么被动端会重发 FIN/ACK，那么如果主动端不是 TIME_WAIT 状态就会
* 回应rst 而不是ACK 或 
* 主动端已经建立了新的连接，就会断开新连接造成错误。

而经历了2个MSL后就会使得所有的FIN包都已经失效了。

MSL指的是报文段的最大生存时间，如果报文段在网络活动了MSL时间，还没有被接收，那么会被丢弃。


参考 [再叙TIME_WAIT](https://huoding.com/2013/12/31/316)

###### TIME_WAIT过多的危害
在生产过程中，如果服务器使用短连接，那么完成一次请求后会主动断开连接，就会造成大量``time_wait`状态。因此我们常常在系统中会采用长连接，减少建立连接的消耗，同时也减少`TIME_WAIT`的产生，但实际上即使使用长连接配置不当时，当`TIME_WAIT`的生产速度远大于其消耗速度时，系统仍然会累计大量的TIME_WAIT状态的连接。

`TIME_WAIT`状态连接过多就会造成一些问题。如果客户端的`TIME_WAIT`连接过多，同时它还在不断产生，将会导致客户端端口耗尽，新的端口分配不出来，出现错误。如果服务器端的`TIME_WAIT`连接过多，可能会导致客户端的请求连接失败。
[如何控制TIME_WAIT过多](https://www.cnblogs.com/lgqboke/p/5917355.html)

#### 正常通信
正如上图所示，对于一个正常的通信流程一般是这样的：
request -> ACK ... response -> ACK

即使是 返回 404 的状态码，那么，对于一个 request 也应该回应 一个ACK
例如 Nginx:

![](/assest/img/http404.png) 

而很多自行设计的 webserver 就会忽略这一点：

![](/assest/img/myserver404.png)





