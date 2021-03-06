## 哈希表 或是 散列表

### 定义
哈希表是一种通过哈希函数（hash function），将Key映射到特定值作为存储位置索引index，然后根据index得到value的一种数据结构，它维护键key和值value之间一一对应关系。
### 实现
实现一个哈希表也很容易，主要需要完成的工作只有三点：

* 实现哈希函数 hash function： h(key) -> index
* 冲突的解决
* 操作接口的实现

### 哈希冲突
冲突：在映射到一个较小的空间中时可能会出现两个不同的key映射到同一个index上的情况， 这就是我们所说的出现了冲突。

### 哈希冲突的解决
1. 分离链接法
将散列到同一个值得所有元素保存到一个链表中。

搜索：为了执行一次搜索，我们首先需要通过hash函数得到我们需要遍历的链表，然后在链表中执行一次查找。

插入：如果一个元素是一个新的元素，那么它将插入到链表的前端。这是基于这样的事实：**新插入的元素最有可能不久又被访问**

2. 开放定址法
对于发生冲突的元素，从冲突位置附近尝试另外一些单元，直到找出空的单元为止。

对于这类冲突解决的方法一般会有个 称为**装填因子**的变量，其值应该小于0.5

3. 线性探测法
线性探测法是开放定址法的一种，其尝试策略很简单，就是寻找冲突点的下一个空闲位置，直到找到空闲位置为止。

该方法在表很空的情况下（装填因子很小）也容易产生一些连续的区块，使得插入新的元素的时候需要经过多次试选才能完成，该现象被称为**一次聚集**

4. 平方探测法
这种方法是在寻找空闲单元的时候以二次方作为偏移，虽然该方法排除了一次聚集的现象，但是很明显也会出现**二次聚集**的现象。

该方法还有一个很大的缺陷，如果表的大小不是素数的时候，即使表有超过一半是空的，也有可能会出现插入新元素失败的现象。不过可以证明的是**如果表的大小是素数，而且表至少有一半是空闲的（也就是说装填因子小于0.5）那么总能保证插入一个新的元素**

5. 双散列
双散列的定义就是，在偏移的选择上面也是通过一个散列函数来选择的。

6. 再散列
当一个散列表装得太满了（装填因子太大）导致删除或插入操作耗时太长或是无法完成插入，这时需要重新建立一个大小为原来两倍的表，然后扫描原表的内容，将原表的内容再用另一个hash算法去映射到新表中。

再散列的策略主要有三种：一是：当出现插入失败时才进行；二是：表满到一半就进行；三是：达到一定的装填因子的限制就进行。

### 一致性哈希
上面提到的再散列的方法对于某些业务是致命性的，例如，我们使用散列表维护一个服务器集群，当随着时间的巨轮转动，服务器集群迅速扩展，导致我们维护集群的散列表太满了，这时需要进行重新散列，如果使用上面的方法，那么无疑会出现线上事故。这种情况下我们需要一种新的方案来为系统扩容，这里就涉及到了一种新的方案称为一致性哈希。

consistent hashing 是这样一种 hash 算法，简单的说，在移除 / 添加一个 节点（机器，ip）时，它能够尽可能小的改变已存在 key 映射关系，尽可能的满足单调性映射的要求。

一致性哈希使用hash回环。因为任何的hash值都是固定长度的，所以可以通过一个回环来承载所有的hash值。

删除：删除一个节点的val的时候只会改变之前与该节点映射的key

添加：添加一个节点的时候，只会影响逆时针与新节点相邻的key的映射关系，这使得系统在增加删除节点的时候会更为稳定。

参考[redis 一致性hash算法](https://blog.csdn.net/u013851082/article/details/68063446)

### 多阶哈希
多阶哈希可以简单理解为递归的散列，当一阶散列出现很多冲突元素的时候，我们再内联一个哈希表来存储这些冲突的元素。

衡量多阶哈希表的好坏主要有2个指标，空间利用率和平均查找次数。通过实验可知，阶数越多，处理冲突的能力越强，空间的利用率也高，但平均查找次数越多，可见空间利用率和平均查找次数相互制约，需要根据实际情况进行权衡

参考[分布式存储系统设计（3）—— 存储结构](https://www.cnblogs.com/glacierh/p/5689418.html)

### 布隆过滤器
假设我们出现了下面这样的业务场景，key的数据量是海量，我们需要小心的设计散列函数以使得我们的主存能够放的下我们key hash 后的index。

为此我们使用类似于位图的hash方式来实现该需求，但是位图减少主存消耗的同时也会带来插入或查找的问题。

在这样的背景下面，我们想到了一个合乎常理的方法来减少hash冲突的概率，也就是对于一个key 我们hash 多个值，然后拼接这些值来得到index。这其实就是布隆过滤器的基本概念。

我们hash一个值冲突的概率假设都是百分之一, 则hash两个值都冲突的概率就是万分之一.当然这里考虑两个hash是有顺序(排列)的, 如果没有循序(组合)的话, 概率也不超过万分之5.而且我们hash的个数越多, 冲突概率越小, 当然对应成本可能也越高.

参考[谈谈布隆过滤器(Bloom Filter)](http://github.tiankonguse.com/blog/2017/03/19/bloom_filter.html)

### 常用hash函数
* MD5
它对输入仍以512位分组，其输出是4个32位字的级联
* sha
把消息转换为位字符串

对转换得到的位字符串进行补位操作 

附加长度信息 

使用的常量和函数 

计算消息摘要

02年，NIST分别发布了SHA-256、SHA-384、SHA-512，这些算法统称SHA-2。08年又新增了SHA-224。

* PHP中使用的是称为DJBX33A算法

* Redis 使用 MurmurHash2 算法来计算键的哈希值


### 实际使用的哈希冲突解决方法
Redis 的哈希表使用链地址法（separate chaining）来解决键冲突： 每个哈希表节点都有一个 next 指针， 多个哈希表节点可以用 next 指针构成一个单向链表， 被分配到同一个索引上的多个节点可以用这个单向链表连接起来，这就解决了键冲突的问题。