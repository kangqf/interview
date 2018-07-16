## tcpdump

-D 显示可用的设备
-i 指定接口
-A 以ASCII码的形式打印内容
-w <filename> 保存到指定文件
-r <filename> 从指定文件读取
-c 设置捕获指定计数的数据包后就退出

-nn   表示以ip和port的方式显示来源主机和目的主机，而不是用主机名和服务
-v    当分析和打印的时候, 产生详细的输出，vv 更详细 vvv 更更详细
-X    当分析和打印时, tcpdump 会打印每个包的头部数据
-S    打印TCP 数据包的顺序号时, 使用绝对的顺序号, 而不是相对的顺序号
-e    每行的打印输出中将包括数据包的数据链路层头部信息

#### 操作符过滤

* 逻辑运算
not !
and &&
or  || 

* 方向
src 
dst

* 协议
tcp
udp
ip
arp
icmp
ether
ip proto ospf
ip proto 89  参考/etc/protocols中的协议编号

* 广播多播
broadcast
multicast

* 网络
host <bostname>|<CIDR>
port <portnum>
net <CIDR>

* 例子
```
sudo tcpdump dst host rfid2
tcpdump -i eth0 src 192.168.1.1 and dst 192.168.1.2 and dst port 80
tcpdump -i eth1 '((icmp) and ((ether dst host 00:01:02:03:04:05)))'
tcpdump host 210.27.48.1 and \(210.27.48.2 or 210.27.48.3 \)
tcpdump ether multicast
```

#### 报文特征 

greater|less <length> 
proto [ index : size ]  proto指定要查看的报文头——ip则查看IP头，tcp则查看TCP头，index 是起始下标，size 是使用的字节数

* 例子
```
tcpdump greater 200   只取长度大于200字节的报文
tcpdump 'ip[9] = 6'   十字节的IP头，协议值为6
tcpdump -i eth1 'tcp[(tcp[12]>>2):4] = 0x47455420'  抓 HTTP GET 数据 "GET "的十六进制是 47455420
tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
tcpdump -i eth1 '((port 25) and (tcp[(tcp[12]>>2):4] = 0x4d41494c))'  抓 SMTP 数据
```

#### 特殊标志

tcp[tcpflags]
icmp[icmptype]

```
tcpdump -i eth1 'tcp[tcpflags] = tcp-syn'  只抓 SYN 包
tcpdump 'tcp[tcpflags] & (tcp-syn|tcp-fin) != 0 and not src and dst net localnet'
tcpdump -i eth1 'tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack != 0'  抓 SYN, ACK
tcpdump 'icmp[icmptype] != icmp-echo and icmp[icmptype] != icmp-echoreply'
```

