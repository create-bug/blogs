原文： ttps://hit-alibaba.github.io/interview/basic/network/Socket-Programming-Basic.html

Socket 是对 TCP/IP 协议族的一种封装，是应用层与TCP/IP协议族通信的中间软件抽象层。从设计模式的角度看来，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议。

Socket 还可以认为是一种网络间不同计算机上的进程通信的一种方法，利用三元组（ip地址，协议，端口）就可以唯一标识网络中的进程，网络中的进程通信可以利用这个标志与其它进程进行交互。

Socket 起源于 Unix ，Unix/Linux 基本哲学之一就是“一切皆文件”，都可以用“打开(open) –> 读写(write/read) –> 关闭(close)”模式来进行操作。因此 Socket 也被处理为一种特殊的文件。

写一个简易的 WebServer
一个简易的 Server 的流程如下：

1.建立连接，接受一个客户端连接。
2.接受请求，从网络中读取一条 HTTP 请求报文。
3.处理请求，访问资源。
4.构建响应，创建带有 header 的 HTTP 响应报文。
5.发送响应，传给客户端。
省略流程 3，大体的程序与调用的函数逻辑如下：

socket() 创建套接字
bind() 分配套接字地址
listen() 等待连接请求
accept() 允许连接请求
read()/write() 数据交换
close() 关闭连接
代码如下：

#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <unistd.h>
#include <sys/socket.h>
...参见原文
