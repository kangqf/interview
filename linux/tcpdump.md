## tcpdump

-D 显示可用的设备
-i 指定接口
-A 以ASCII码的形式打印内容
-w <filename> 保存到指定文件
-r <filename> 从指定文件读取
-c 设置捕获指定计数的数据包后就退出

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

* 广播多播
broadcast
multicast

* 主机
host <bostname or CIDR>
port <portnum>
net <CIDR>

* 例子
```
sudo tcpdump dst host rfid2
tcpdump -i eth0 src 192.168.1.1 and dst 192.168.1.2 and dst port 80
tcpdump host 210.27.48.1 and \(210.27.48.2 or 210.27.48.3 \)
tcpdump ether multicast
```

#### 16 进制过滤

```
tcpdump -i eth1 'tcp[(tcp[12]>>2):4] = 0x47455420'  抓 HTTP GET 数据 "GET "的十六进制是 47455420
tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```




