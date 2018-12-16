# Java I/O

## 基础知识

### Linux 网络I/O模型
* 阻塞I/O模型
* 非阻塞I/O模型
* I/O多路复用模型(select,poll,epoll)
* 信号驱动I/O模型
* 异步I/O模型

参考[简书：聊聊Linux 5种IO模型](https://www.jianshu.com/p/486b0965c296)


### IO多路复用技术

> Java NIO的核心类库多路复用器Selector就是基于epoll多路复用技术实现

* 把多个IO的阻塞复用到同一个select的阻塞上，从而使得系统在单线程的情况下可以同时处理多个客户端请求。
* 最大优势在于系统开销小，系统不需要创建新的进程或线程，也不需要维护这些进程和线程的运行，降低了系统的维护工作量，节省了系统资源
* select/poll/epoll的区别，epoll是增强版本，参考[大话select/poll/epoll](https://cloud.tencent.com/developer/article/1005481)

### Java IO模型的演进

#### 一个请求一个处理线程
线程是JVM昂贵的资源

#### 线程池模型
解决了创建大量线程的问题，但是没有根本上解决IO同步阻塞的问题

### NIO多路复用
能够高效的一个前提是，服务器有海量socket，但是活跃socket较少的情况下才会体现出epoll的高效、高性能。  