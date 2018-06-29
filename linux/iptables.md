## iptables 使用

### 概念
* tables  
    1. mangle    内容处理表，对内容进行封包和拆包
    2. raw       原生的数据包
    3. filter    转发表
    4. nat       地址转换表
* chain
    1. PREROUTING  路由前
    2. INPUT       输入主机
    3. FORWARD     转发
    4. OUTPUT      从主机输出
    5. POSTROUTING 路由后
    


* target
    1. ACCEPT
    2. DROP
    3. REGECT
    4. SNAT 
    5. DNAT 
    6. MASQUERADE
    
* match
    1. -i input interface  指定该接口后会限制链的类型为：INPUT PREROUTING FORWARD
    2. -o output interface 指定该接口后会限制链的类型为：OUTPUT POSTROUTING FORWARD
    3. -t table 指定该选项也会限制链的类型 filter(INPUT FORWARD OUTPUT) nat(PREROUTING OUTPUT POSTROUTING) mangle(ALL)
    4. -s src
    5. -d dst
    6. -A Append
    7. -D Delete
    8. -R num replace
    9. -I [num] Insert 插入到第num条处
    10. -L List
    11. -j jump
    12. -g goto
    13. -p protol
    14. 
    
    2. -p -m -g -j -A -D -R -L -v -P -I -F -N --dport --sport

    
    

iptables -F  # 清空所有的防火墙规则
iptables -X  # 删除用户自定义的空链
iptables -Z  # 清空计数

rules reject accept drop snat masquerade dnat 

protol

dport


 sudo iptables -A FORWARD -i ppp0  -p udp --sport 4015  -j DROP
 
 sudo iptables -t nat -A POSTROUTING -s 192.168.6.0/24 -j SNAT --to-source 192.168.11.168