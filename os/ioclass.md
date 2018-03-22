## IO 分类

在介绍IO分类之前，首先要明确IO操作的流程，例如对于 一个 网络 IO socket，当一个进程对其发出read 的命令时，会经历下面两步：
1. 内核等待数据准备完成
2. 将数据从内核空间拷贝到用户空间


### 阻塞与非阻塞IO
linux 下 对于socket 都是阻塞的，也就是说当用户进程调用一个 read 的系统调用的时候，用户进程就已经阻塞在这里，然后等待内核收到完整的数据包，再等待系统将数据包从内核空间拷贝到用户进程，直到这里阻塞才会返回。

linux下，可以通过设置socket使其变为non-blocking。对一个 non-blocking 的IO进行读操作的系统调用，即使数据没有准备好，调用也会立即返回。用户进程会得到一个error的返回值，然后继续调用 read 系统调用直到数据准备好了，这时候调用就会拷贝数据到用户进程，注意这里的拷贝依旧是阻塞的。


### 同步IO 异步IO
同步IO的定义是指，调用IO操作会导致进程被阻塞的IO。这样的话blocking IO，non-blocking IO，IO multiplexing都属于同步IO，因为非阻塞IO第二步从内核拷贝数据这一步也是阻塞的，所以也是同步IO 的一种。

异步IO：就是说系统调用后内核会等待数据准备好，然后拷贝到用户进程，最后才通知用户进程IO操作完成了。


### 信号驱动IO
信号驱动IO 与 非阻塞IO 不同的一点就是用户进程不需要不停地取询问内核数据是否准备好，而是通过信号的方式通知用户进程数据已经准备好了，但是从内核拷贝数据到用户进程这一步还是阻塞的。

参考 [IO - 同步，异步，阻塞，非阻塞 （亡羊补牢篇）](http://blog.csdn.net/historyasamirror/article/details/5778378])
参考 [同步、异步与阻塞、非阻塞，UNIX I/O模型](http://strawhatfy.github.io/2015/04/21/synchronous-asynchronous-blocking-nonblocking/)