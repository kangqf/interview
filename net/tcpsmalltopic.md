### TCP 相关的小知识点

#### TCP Fast Open (TFO)

TCP快速打开是对TCP的一种简化握手手续的拓展，用于提高两端点间连接的打开速度。简而言之，就是在TCP的三次握手过程中传输实际有用的数据。

它通过握手开始时的SYN包中的TFO cookie来验证一个之前连接过的客户端。如果验证成功，它可以在三次握手最终的ACK包收到之前就开始发送数据，这样便跳过了一个绕路的行为，更在传输开始时就降低了延迟。这个加密的Cookie被存储在客户端，在一开始的连接时被设定好。然后每当客户端连接时，这个Cookie被重复返回。

![示意图](/assest/img/devconf-2014-kernel-networking-walkthrough-16-638.jpg)

[参考](https://chenjx.cn/linux-tfo/)