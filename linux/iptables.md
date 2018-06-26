## iptables 使用

### 概念
* tables  
    1. mangle    内容处理表，对内容进行封包和拆包
    2. raw       原生的数据包
    3. filter    转发表
    4. nat       地址转换表
* chain
    1. PREROUTING
    2. INPUT
    3. FORWARD
    4. OUTPUT
    5. POSTROUTING
    
    
* target
    1. ACCEPT
    2. DROP
    3. REGECT
* match
    1. -i
    2. -p -m -g -j -s -d -o -t -A -D -R -L -v -P -I -F -N --dport --sport

对于filter来讲一般只能做在3个链上：INPUT ，FORWARD ，OUTPUT
对于nat来讲一般也只能做在3个链上：PREROUTING ，OUTPUT ，POSTROUTING
而mangle则是5个链都可以做：PREROUTING，INPUT，FORWARD，OUTPUT，POSTROUTING
    
    

iptables -F  # 清空所有的防火墙规则
iptables -X  # 删除用户自定义的空链
iptables -Z  # 清空计数

rules reject accept drop snat masquerade dnat 

protol

dport


 sudo iptables -A FORWARD -i ppp0  -p udp --sport 4015  -j DROP
 
 sudo iptables -t nat -A POSTROUTING -s 192.168.6.0/24 -j SNAT --to-source 192.168.11.168