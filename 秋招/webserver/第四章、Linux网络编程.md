
- [lesson01-07 网络基础](#lesson01-07-网络基础)
  - [一、网络结构模式](#一网络结构模式)
    - [1、C/S结构](#1cs结构)
    - [2、B/S结构\*\*](#2bs结构)
  - [二、MAC 地址](#二mac-地址)
  - [三、IP 地址](#三ip-地址)
    - [1、IP 地址编址方式](#1ip-地址编址方式)
    - [2、特殊的网址](#2特殊的网址)
    - [3、子网掩码](#3子网掩码)
  - [四、端口](#四端口)
    - [端口类型](#端口类型)
  - [五、网络模型](#五网络模型)
    - [1、OSI 七层参考模型](#1osi-七层参考模型)
    - [2、TCP/IP 四层模型](#2tcpip-四层模型)
  - [六、协议](#六协议)
    - [1、常见协议](#1常见协议)
    - [2、封装](#2封装)
    - [3、分用](#3分用)
    - [4、封装与分用](#4封装与分用)
- [lesson08-12 socket通信基础](#lesson08-12-socket通信基础)
  - [一、socket介绍](#一socket介绍)
  - [二、字节序](#二字节序)
    - [案例： 通过代码检测当前主机的字节序](#案例-通过代码检测当前主机的字节序)
    - [1、字节序转换函数](#1字节序转换函数)
  - [三、socket 地址](#三socket-地址)
    - [1、通用 socket 地址 `sockaddr`](#1通用-socket-地址-sockaddr)
    - [2、专用 socket 地址](#2专用-socket-地址)
      - [UNIX 本地域协议族使用如下专用的 socket 地址结构体](#unix-本地域协议族使用如下专用的-socket-地址结构体)
      - [TCP/IP 协议族](#tcpip-协议族)
  - [四、IP地址转换](#四ip地址转换)
  - [五、传输层协议 TCP 和 UDP 的对比](#五传输层协议-tcp-和-udp-的对比)
- [lesson13-24 TCP通信](#lesson13-24-tcp通信)
  - [一、TCP 通信的流程](#一tcp-通信的流程)
  - [二、套接字函数](#二套接字函数)
  - [三、简单的TCP通信例子](#三简单的tcp通信例子)
  - [四、TCP 三次握手](#四tcp-三次握手)
    - [详细过程](#详细过程)
  - [五、TCP 滑动窗口](#五tcp-滑动窗口)
    - [一次简单的连接过程](#一次简单的连接过程)
  - [六、TCP 四次挥手](#六tcp-四次挥手)
  - [七、TCP 通信并发](#七tcp-通信并发)
    - [TCP 通信多进程并发案例](#tcp-通信多进程并发案例)
    - [TCP 通信多线程并发案例](#tcp-通信多线程并发案例)
  - [八、TCP 状态转变](#八tcp-状态转变)
  - [九、端口复用](#九端口复用)
- [lesson25-31 I/O多路复用（I/O多路转接）](#lesson25-31-io多路复用io多路转接)
  - [一、I/O多路复用概述](#一io多路复用概述)
    - [1、传统的输入输出](#1传统的输入输出)
    - [2、阻塞等待模型(BIO模型)](#2阻塞等待模型bio模型)
    - [3、非阻塞, 忙轮询模型(NIO模型)](#3非阻塞-忙轮询模型nio模型)
  - [二、IO多路复用技术](#二io多路复用技术)
    - [1、select](#1select)
      - [案例： select()实现TCP通信](#案例-select实现tcp通信)
    - [二、poll](#二poll)
      - [poll实现TCP通信案例](#poll实现tcp通信案例)
    - [三、epoll](#三epoll)
      - [epoll 实现TCP通信案例](#epoll-实现tcp通信案例)
      - [Epoll 工作模式](#epoll-工作模式)
        - [1. LT 模式（水平触发）](#1-lt-模式水平触发)
        - [2. ET模式（边沿触发）](#2-et模式边沿触发)
- [lesson32-34 UDP通信](#lesson32-34-udp通信)
  - [一、UDP通信流程](#一udp通信流程)
    - [UDP 通信的简单案例](#udp-通信的简单案例)
  - [二、广播](#二广播)
    - [UDP 广播的简单案例](#udp-广播的简单案例)
  - [三、组播（多播）](#三组播多播)
    - [UDP组播通信案例](#udp组播通信案例)
- [lesson35 本地套接字](#lesson35-本地套接字)
  - [一、本地套接字通信的流程 - tcp](#一本地套接字通信的流程---tcp)
    - [本地套接字IPC案例](#本地套接字ipc案例)

# lesson01-07 网络基础

## 一、网络结构模式

### 1、C/S结构

***优点***
1. 能充分发挥客户端 PC 的处理能力，所以 C/S 结构客户端响应速度快；
2. 操作界面漂亮、形式多样，可以充分满足客户自身的个性化要求；
3. C/S 结构的管理信息系统具有较强的事务处理能力，能实现复杂的业务流程；
4. 安全性较高，C/S 一般面向相对固定的用户群，程序更加注重流程，它可以对权限进行多层次校验，提供了更安全的存取模式，对信息安全的控制能力很强，一般高度机密的信息系统采用 C/S 结构适宜。

***缺点***
1. 客户端需要安装专用的客户端软件，其维护和升级成本非常高 (每一台客户机需要重新安装)
2. 对客户端的操作系统一般也会有限制，不能够跨平台。

### 2、B/S结构**

***优点***
1. 成本低、维护方便、 分布性强、开发简单，客户端零维护，系统的扩展非常容易，只要有一台能上网的电脑就能使用

***缺点***
1. 通信开销大、系统和数据的安全性较难保障;
2. 个性特点明显降低，无法实现具有个性化的功能要求；
3. 协议一般是固定的：http/https
4. 客户端服务器端的交互是请求-响应模式，通常动态刷新页面，响应速度明显降低。

## 二、MAC 地址

网卡是一块被设计用来允许计算机在计算机网络上进行通讯的计算机硬件，又称为网络适配器或网络接口卡NIC。其拥有 MAC 地址，也称为局域网地址、以太网地址、物理地址或硬件地址。**MAC 地址的长度为 48 位（6个字节）**，通常表示为 12 个 16 进制数，如：00-16-EA-AE-3C-40

## 三、IP 地址

IP 地址是 IP协议提供的一种统一的地址格式，它为互联网上的每一个网络和每一台主机分配一个逻辑地址，以此来屏蔽物理地址的差异。IP 地址是一个 **32 位**的二进制数，常用“点分十进制”表示

### 1、IP 地址编址方式

共有5 种 IP 地址类型以适合不同容量的网络，即 A 类~ E 类

### 2、特殊的网址

- 每一个字节都为 0 的地址 (`0.0.0.0`) 对应于当前主机；
- IP 地址中的每一个字节都为 1 的 IP 地址 (`255.255.255.255`) 是当前子网的广播地址；
- IP 地址中凡是以 `11110` 开头的 E 类 IP 地址都保留用于将来和实验使用。
- IP地址中不能以十进制 `127` 作为开头，该类地址中数字 `127.0.0.1` 到 `127.255.255.255` 用于回路测试，如：`127.0.0.1` 可以代表本机IP地址。

### 3、子网掩码

子网掩码是一种用来指明一个 IP 地址的哪些位标识的是主机所在的子网，以及哪些位标识的是主机的位掩码。子网掩码的作用就是将 IP 地址划分成网络地址和主机地址两部分

## 四、端口

端口是设备与外界通讯交流的出口。端口可分为虚拟端口和物理端口

- 虚拟端口特指TCP/IP协议中的端口，是逻辑意义上的端口。例如计算机中的 80 端口、21 端口、23 端口等
- 物理端口又称为接口，是可见端口，计算机背板的 RJ45 网口，交换机路由器集线器等 RJ45 端口。

### 端口类型

1. 周知端口 (0 ~ 1023)：也叫知名端口、公认端口，它们紧密绑定于一些特定的服务。例如 WWW 服务 (80端口),FTP 服务 (21端口) 等等。若网络服务使用的不是默认的 80端口，则应输入“ 网址:8080 ”  (如使用 “8080” 作为 WWW服务的端口)

2. 注册端口 (1024 ~ 49151)：它们松散地绑定于一些服务，分配给用户进程或应用程序，这些进程主要是用户选择安装的一些应用程序，而不是已经分配好了公认端口的常用程序。

3. 动态端口 / 私有端口 (49152 ~ 65535)：它一般不固定分配某种服务，而是 动态分配。

## 五、网络模型

### 1、OSI 七层参考模型

1. 物理层：主要定义物理设备标准，如网线的接口类型、各种传输介质的传输速率等。主要作用是传输比特流（数模转换与模数转换），这一层的数据叫做**比特**

2. 数据链路层：建立逻辑连接、进行硬件地址寻址、差错校验等功能。定义了如何让格式化数据以**帧**为单位进行传输

3. 网络层：进行逻辑地址寻址，在位于不同地理位置的网络中的两个主机系统之间提供连接和路径选择。

4. 传输层：定义了一些传输数据的协议和端口号（ WWW 端口 80, TCP协议等）。 主要是将从下层接收的数据进行分段和传输，到达目的地址后再进行重组。常常把这一层数据叫做**段**。

5. 会话层：通过传输层（端口号：传输端口与接收端口）建立数据传输的通路

6. 表示层：数据的表示、安全、压缩。主要是进行对接收的数据进行解释、加密与解密、压缩与解压缩等（也就是把计算机能够识别的东西转换成人能够能识别的东西）

7. 应用层：为用户的应用程序（例如电子邮件、文件传输和终端仿真）提供网络服务

### 2、TCP/IP 四层模型

1. 应用层：直接为应用进程提供服务的
    - 对不同种类的应用程序它们会根据自己的需要来使用应用层的不同协议，万维网应用使用了 HTTP 协议、远程登录服务应用使用了有 TELNET 协议
    - 应用层还能加密、解密、格式化数据
    - 应用层可以建立或解除与其他节点的联系，这样可以充分节省网络资源。

2. 传输层：传输层在整个 TCP/IP 协议中起到了中流砥柱的作用。且在传输层中， TCP 和 UDP 也同样起到了中流砥柱的作用

3. 网络层：在 TCP/IP 协议中网络层可以进行网络连接的建立和终止以及 IP 地址的寻找等功能。

4. 网络接口层：网络接口层兼并了物理层和数据链路层，所以它既是传输数据的物理媒介，也可以为网络层提供一条准确无误的线路。

![TCP、IP四层模型](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/TCP%E3%80%81IP%E5%9B%9B%E5%B1%82%E6%A8%A1%E5%9E%8B.png?raw=true)

## 六、协议

是通信计算机双方必须共同遵从的一组约定。如怎么样建立连接、怎么样互相识别等。三要素是：语法、语义、时序

### 1、常见协议
1. 应用层常见的协议有：FTP协议（File Transfer Protocol 文件传输协议）、HTTP协议（Hyper TextTransfer Protocol 超文本传输协议）、NFS（Network File System 网络文件系统）

2. 传输层常见协议有：TCP协议（Transmission Control Protocol 传输控制协议）、UDP协议（User Datagram Protocol 用户数据报协议）

3. 网络层常见协议有：IP 协议（Internet Protocol 因特网互联协议）、ICMP 协议（Internet Control Message Protocol 因特网控制报文协议）、IGMP 协议（Internet Group Management Protocol 因特网组管理协议）

4. 网络接口层常见协议有：ARP协议（Address Resolution Protocol 地址解析协议）、RARP协议（Reverse Address Resolution Protocol 反向地址解析协议）。

***TCP协议***

![TCP协议](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/TCP%E5%8D%8F%E8%AE%AE.png?raw=true)

***UDP协议***

![UDP协议](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/UDP%E5%8D%8F%E8%AE%AE.png?raw=true)

***IP协议***

![IP协议](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/IP%E5%8D%8F%E8%AE%AE.png?raw=true)


### 2、封装

上层协议通过封装使用下层协议提供的服务。应用程序数据在发送到物理网络上之前，将沿着协议栈从上往下依次传递。每层协议都将在上层数据的基础上加上自己的头部信息（有时还包括尾部信息），以实现该层的功能，这个过程就称为封装。

![封装](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/%E5%B0%81%E8%A3%85.png?raw=true)

### 3、分用

当帧到达目的主机时，将沿着协议栈自底向上依次传递。各层协议依次处理帧中本层负责的头部数据，以获取所需的信息，并最终将处理后的帧交给目标应用程序。这个过程称为分用。分用是依靠头部信息中的类型字段实现的。

![分用](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/%E5%88%86%E7%94%A8.png?raw=true)

### 4、封装与分用

![封装与分用](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/%E5%B0%81%E8%A3%85%E4%B8%8E%E5%88%86%E7%94%A8.png?raw=true)

# lesson08-12 socket通信基础

## 一、socket介绍

socket（套接字），就是对网络中不同主机上的应用进程之间进行双向通信的端点的抽象。一个套接字就是网络上进程通信的一端，提供了应用层进程利用网络协议交换数据的机制

套接字通信分两部分：
- 服务器端：被动接受连接，一般不会主动发起连接
- 客户端：主动向服务器发起连接

socket是一套通信的接口，Linux 和 Windows 都有，但是有一些细微的差别。

## 二、字节序

字节序：字节在内存中存储的顺序。就是大于一个字节类型的数据在内存中的存放顺序(一个字节的数据当然就无需谈顺序的问题了)

- 小端字节序：数据的高位字节存储在内存的高位地址，低位字节存储在内存的低位地址
- 大端字节序：数据的低位字节存储在内存的高位地址，高位字节存储在内存的低位地址

### 案例： 通过代码检测当前主机的字节序
```c++
int main() {
    union {
        short value;    // 2字节
        char bytes[sizeof(short)];  // char[2]
    } test;
    test.value = 0x0102;
    if((test.bytes[0] == 1) && (test.bytes[1] == 2)) printf("大端字节序\n");
    else if((test.bytes[0] == 2) && (test.bytes[1] == 1)) printf("小端字节序\n");
    else printf("未知\n");
    return 0;
}
```

### 1、字节序转换函数

网络通信时，需要将**主机字节序**转换成**网络字节序（大端）**，另外一端获取到数据以后根据情况将网络字节序转换成主机字节序。

```c++
#include <arpa/inet.h>

// 转换端口 (short unsigned short)
uint16_t htons(uint16_t hostshort); // 主机字节序 - 网络字节序
uint16_t ntohs(uint16_t netshort); // 网络字节序 - 主机字节序 

// 转IP (long unsigned int)
uint32_t htonl(uint32_t hostlong); // 主机字节序 - 网络字节序
uint32_t ntohl(uint32_t netlong); // 网络字节序 - 主机字节序

字符含义：
    h - host 主机，主机字节序
    n - network 网络字节序
    s - short (unsigned short)
    l - long (unsigned int)

例子：
// htons 转换端口
unsigned short a = 0x0102;
printf("a : %x\n", a); //本机小端，0x0102
unsigned short b = htons(a);
printf("b : %x\n", b); //大端，0x0201
printf("=======================\n");

// htonl  转换IP
char buf[4] = {192, 168, 1, 100};
int num = *(int *)buf; //将它看成一个整数
int sum = htonl(num);
unsigned char *p = (char *)&sum;
printf("%d %d %d %d\n", *p, *(p+1), *(p+2), *(p+3)); //100 1 168 192
printf("=======================\n");

// ntohl, ntohs，同上
```

## 三、socket 地址

socket地址其实是一个结构体，封装端口号和IP等信息

### 1、通用 socket 地址 `sockaddr`
```c++
#include <bits/socket.h>
struct sockaddr {
    sa_family_t sa_family;
    char sa_data[14];
};
typedef unsigned short int sa_family_t;
```
`sa_family` 成员是地址族类型 `sa_family_t` 的变量。地址族类型通常与协议族类型对应

| 协议族  |  地址族 | 描述 |
| :--:  |  :--: | :--: |                  
| `PF_UNIX`  |  `AF_UNIX`   | `UNIX`本地域协议族 |
| `PF_INET`  |  `AF_INET`   | `TCP/IPv4`协议族 |
| `PF_INET6` |  `AF_INET6`  | `TCP/IPv6`协议族 |

宏 `PF_*` 和 `AF_*` 都定义在 `bits/socket.h` 头文件中，且后者与前者有完全相同的值，所以二者通常混用。

`sa_data` 成员用于存放 socket 地址值。但是，不同的协议族的地址值具有不同的含义和长度：

| 协议族   |  地址值含义和长度 |
| :--:  |  :--: |            
|`PF_UNIX`  | 文件的路径名，长度可达到108字节 |
|`PF_INET`  | 16 bit 端口号和 32 bit IPv4 地址，共 6 字节 |
|`PF_INET6` | 16 bit 端口号，32 bit 流标识，128 bit IPv6 地址，32 bit 范围 ID，共 26 字节 |

所以14 字节的 `sa_data` 根本无法容纳多数协议族的地址值。因此，Linux 定义了下面这个新的通用的 socket 地址结构体，这个结构体不仅提供了足够大的空间用于存放地址值，而且是内存对齐的

```c++
// 不常用
#include <bits/socket.h>
struct sockaddr_storage
{
    sa_family_t sa_family;
    unsigned long int __ss_align;
    char __ss_padding[ 128 - sizeof(__ss_align) ];
};
typedef unsigned short int sa_family_t;
```

### 2、专用 socket 地址

为了向前兼容，现在 sockaddr 退化成了（void *）的作用，传递一个地址给函数，至于这个函数是 `sockaddr_in` 还是 `sockaddr_in6`，由地址族确定，然后函数内部再强制类型转化为所需的地址类型。

#### UNIX 本地域协议族使用如下专用的 socket 地址结构体
```c++
#include <sys/un.h>
struct sockaddr_un
{
    sa_family_t sin_family;
    char sun_path[108];
};
```
#### TCP/IP 协议族

TCP/IP 协议族有 sockaddr_in 和 sockaddr_in6 两个专用的 socket 地址结构体，它们分别用于 IPv4 和IPv6
```c++
#include <netinet/in.h>
struct sockaddr_in
{
    sa_family_t sin_family; /* __SOCKADDR_COMMON(sin_) */
    in_port_t sin_port; /* Port number. */
    struct in_addr sin_addr; /* Internet address. */
    /* Pad to size of `struct sockaddr'. */
    unsigned char sin_zero[sizeof (struct sockaddr) - __SOCKADDR_COMMON_SIZE -
    sizeof (in_port_t) - sizeof (struct in_addr)];
};

struct in_addr
{
    in_addr_t s_addr;
};

struct sockaddr_in6
{
    sa_family_t sin6_family;
    in_port_t sin6_port; /* Transport layer port # */
    uint32_t sin6_flowinfo; /* IPv6 flow information */
    struct in6_addr sin6_addr; /* IPv6 address */
    uint32_t sin6_scope_id; /* IPv6 scope-id */
};
typedef unsigned short uint16_t;
typedef unsigned int uint32_t;
typedef uint16_t in_port_t;
typedef uint32_t in_addr_t;
#define __SOCKADDR_COMMON_SIZE (sizeof (unsigned short int))
```

所有专用 socket 地址类型的变量在实际使用时都需要转化为 `sockaddr`（强制转化即可），因为所有 socket 编程接口使用的地址参数类型都是 `sockaddr`

## 四、IP地址转换

- 字符串 Ip - 整数 
- 主机、网络字节序的转换

通常，人们习惯用可读性好的字符串来表示 IP 地址，比如用点分十进制字符串表示 IPv4 地址，以及用十六进制字符串表示 IPv6 地址。但编程中我们需要先把它们转化为整数（二进制数）方能使用。而记录日志时则相反，我们要把整数表示的 IP 地址转化为可读的字符串。因此需要转换函数，下面的函数同时适用于 IPv4 和 IPv6

```c++
#include <arpa/inet.h>
// p:点分十进制的IP字符串，n:表示network，网络字节序的整数

// 字符串到整数的转换
int inet_pton(int af, const char *src, void *dst);
    af: 地址族： AF_INET  AF_INET6
    src: 需要转换的点分十进制的IP字符串
    dst: 转换后的结果保存在这个里面

// 将网络字节序的整数，转换成点分十进制的IP地址字符串
const char *inet_ntop(int af, const void *src, char *dst, socklen_t size);
    af: 地址族： AF_INET  AF_INET6
    src: 要转换的Ip的整数的地址
    dst: 转换成IP地址字符串保存的地方
    size：第三个参数的大小（数组的大小）

    返回值：返回转换后的数据的地址（字符串），和 dst 是一样的

例子：
// 创建一个ip字符串,点分十进制的IP地址字符串
char buf[] = "192.168.1.4";
unsigned int num = 0;
// 将点分十进制的IP字符串转换成网络字节序的整数
inet_pton(AF_INET, buf, &num);
unsigned char * p = (unsigned char *)&num;
printf("%d %d %d %d\n", *p, *(p+1), *(p+2), *(p+3)); //按字节打印
// 将网络字节序的IP整数转换成点分十进制的IP字符串
char ip[16] = "";
const char * str =  inet_ntop(AF_INET, &num, ip, 16);
printf("ip : %s\n", str);
```
## 五、传输层协议 TCP 和 UDP 的对比

- UDP:用户数据报协议，面向无连接，可以单播，多播，广播， 面向数据报，不可靠
- TCP:传输控制协议，面向连接的，可靠的，基于字节流，仅支持单播传输

|       对比      |      UDP       |    TCP     |
|       :--:      |      :--:     |     :--:    |
|   是否创建连接    |   无连接      |   面向连接 |
|       是否可靠     |     不可靠   |   可靠的      |
|   连接的对象个数 |  一对一、一对多、多对一、多对多 | 仅支持一对一|
|   传输的方式   |  面向数据报      |    面向字节流 |
|   首部开销    |   8个字节          |    最少20个字节 |
|   适用场景    |   实时应用（视频会议，直播） | 可靠性高的应用（文件传输） |

# lesson13-24 TCP通信

## 一、TCP 通信的流程

![TCP通信流程](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/TCP%E9%80%9A%E4%BF%A1%E6%B5%81%E7%A8%8B.png?raw=true)

***服务器端 （被动接受连接的角色）***
1. 创建一个用于监听的套接字
    - 监听：监听有客户端的连接
    - 套接字：这个套接字其实就是一个文件描述符
2. 将这个监听文件描述符和本地的IP和端口绑定（IP和端口就是服务器的地址信息）
    - 客户端连接服务器的时候使用的就是这个IP和端口
3. 设置监听，监听的fd开始工作
4. 阻塞等待，当有客户端发起连接，解除阻塞，接受客户端的连接，会得到一个和客户端通信的套接字（fd'，监听的fd仅负责监听，fd'负责通信）
5. 通信
    - 接收数据
    - 发送数据
6. 通信结束，断开连接

***客户端***
1. 创建一个用于通信的套接字（fd）
2. 连接服务器，需要指定连接的服务器的 IP 和 端口
3. 连接成功了，客户端可以直接和服务器通信
    - 接收数据
    - 发送数据
4. 通信结束，断开连接


## 二、套接字函数

```c++
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h> // 包含了这个头文件，上面两个就可以省略

// 创建一个套接字
int socket(int domain, int type, int protocol);
    - 参数：
        - domain: 协议族
            AF_INET : ipv4
            AF_INET6 : ipv6
            AF_UNIX, AF_LOCAL : 本地套接字通信（进程间通信）
        - type: 通信过程中使用的协议类型
            SOCK_STREAM : 流式协议
            SOCK_DGRAM : 报式协议
        - protocol : 具体的一个协议。一般写0
            若type为SOCK_STREAM : 流式协议默认使用 TCP
            若type为SOCK_DGRAM : 报式协议默认使用 UDP
    - 返回值：
        成功则返回文件描述符，操作的就是内核缓冲区。 失败返回-1

// 绑定，将fd 和本地的IP + 端口进行绑定
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen); 
    - 参数：
        - sockfd : 通过socket函数得到的文件描述符
        - addr : 需要绑定的socket地址，这个地址封装了ip和端口号的信息
        - addrlen : 第二个参数结构体占的内存大小

// 监听这个socket上的连接
int listen(int sockfd, int backlog); // /proc/sys/net/core/somaxconn
    - 参数：
        - sockfd : 通过socket()函数得到的文件描述符
        - backlog : 未连接的和已经连接的和的最大值，/proc/sys/net/core/somaxconn中规定的最大值为4096
    - 返回值：成功 0， 失败 -1

// 接收客户端连接，默认是一个阻塞的函数，阻塞等待客户端连接
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
    - 参数：
        - sockfd : 用于监听的文件描述符
        - addr : 传出参数，记录了连接成功后客户端的地址信息（ip，port）
        - addrlen : 指定第二个参数的对应的内存大小
    - 返回值：
        成功则用于通信的文件描述符。 失败返回-1

// 客户端连接服务器
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
    - 参数：
        - sockfd : 用于通信的文件描述符
        - addr : 客户端要连接的服务器的地址信息
        - addrlen : 第二个参数的内存大小
    - 返回值：成功 0， 失败 -1

ssize_t write(int fd, const void *buf, size_t count); // 写数据
ssize_t read(int fd, void *buf, size_t count); // 读数据
```

## 三、简单的TCP通信例子

***TCP 通信的服务器端***
```c++
#include <netinet/in.h>
#include <stdio.h>
#include <stdlib.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>

int main()
{
    // 1.创建socket(用于监听的套接字)
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if (lfd == -1){
        perror("socket");
        exit(-1);
    }
    // 2.绑定
    struct sockaddr_in saddr;
    saddr.sin_family = AF_INET;
    //inet_pton(AF_INET, "192.168.190.128", saddr.sin_addr.s_addr);
    saddr.sin_addr.s_addr = INADDR_ANY; //上面的是指定了一个网卡的ip，写INADDR_ANY (即0) 可以指定服务器所有网卡的ip
    saddr.sin_port = htons(9999); //随便指定一个端口，注意应转为网络字节序
    int ret = bind(lfd, (struct sockaddr *)&saddr, sizeof(saddr));
    if (ret == -1){
        perror("bind");
        exit(-1);
    }
    // 3.监听
    ret = listen(lfd, 8);
    if (ret == -1){
        perror("listen");
        exit(-1);
    }
    // 4.接收客户端连接
    struct sockaddr_in client_addr;
    int len = sizeof(client_addr);
    int cfd = accept(lfd, (struct sockaddr *)&client_addr, &len);
    if (cfd == -1){
        perror("accept");
        exit(-1);
    }    
    // 输出客户端的信息
    char client_ip[16];
    inet_ntop(AF_INET, &client_addr.sin_addr.s_addr, client_ip, sizeof(client_ip));
    unsigned short client_port = ntohs(client_addr.sin_port);
    printf("client ip is : %s, port is: %d\n", client_ip, client_port);
    // 5.通信
    char recv_buf[1024] = {0};
    while (1)
    {
        // 获取客户端的数据
        int num = read(cfd, recv_buf, sizeof(recv_buf));
        if (num == -1){
            perror("read");
            exit(-1);
        } else if (num > 0){
            printf("recv client data: %s\n", recv_buf);
        }
        else if (num == 0){ // 表示客户端断开连接
            printf("client closed...\n");
            break;
        }
        // 给客户端发送数据
        char *data = "hi, i am server";
        write(cfd, data, strlen(data));
    }
    // 关闭文件描述符
    close(lfd);
    close(cfd);
    return 0;
}
```

***TCP通信的客户端***

```c++
int main()
{
    // 1. 创建套接字
    int cfd = socket(AF_INET, SOCK_STREAM, 0);
    if (cfd == -1){
        perror("socket");
        exit(-1);
    }
    // 2. 连接服务器端
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    inet_pton(AF_INET, "192.168.190.128", &server_addr.sin_addr.s_addr);
    server_addr.sin_port = htons(9999);
    int ret = connect(cfd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    if (ret == -1){
        perror("connect");
        exit(-1);
    }
    // 3. 通信
    char recv_buf[1024] = {0};
    char buf[128];
    while (1)
    {
        memset(buf, 0, 128);
        fgets(buf, 128, stdin);
        write(cfd, buf, strlen(buf));
        sleep(1);
        int len = read(cfd, recv_buf, sizeof(recv_buf));
        if (len == -1){
            perror("read");
            exit(-1);
        } else if (len > 0){
            printf("recv server data: %s\n", recv_buf);
        } else if(len == 0){
            printf("server closed...\n");
            break;
        } 
    }
    // 4. 关闭连接
    close(cfd);
    return 0;
}
```

## 四、TCP 三次握手

TCP 是一种面向连接的单播协议，在发送数据前，通信双方必须在彼此间建立一条连接。TCP 可以看成是一种字节流，它会处理 IP 层或以下的层的丢包、重复以及错误问题。在连接的建立过程中，双方需要交换一些连接的参数。这些参数可以放在 TCP 头部。

TCP 提供了一种可靠、面向连接、字节流、传输层的服务，采用三次握手建立一个连接。采用四次挥手来关闭一个连接。

三次握手发生在客户端连接的时候，当调用 `connect()` 时，底层会通过 TCP 协议进行三次握手 (利用标志位)，三次握手可以帮助客户端以及服务端确认自己以及对方都既能发送数据又能接受数据，要保证可靠的连接至少需要三次握手！！

- (1) 客户端 SYN = 1 (客户端确定自己能发, 服务端确定自己能收)
- (2) 服务端 ACK = 1, SYN = 1 (服务端能确定自己能发并且服务端确定客户端能发; 客户端确定自己能收并且客户端确定服务端能发能收。即此时客户端确认完毕，但服务端不能确定客户端能否收)
- (3) 客户端 ACK = 1 (服务端确定客户端能收，两端都知道自己以及对方能发能收)

TCP头部中的序号 `seq` 以及确认号 `ack` 可以确保最终收到的数据完整以及保证顺序

### 详细过程

第一次握手：
- (1) 客户端向服务器端发送连接请求: SYN = 1
- (2) 生成一个随机的32位的序号 seq = J, 不能携带通信数据

第二次握手：
- (1) 服务器端接收客户端的连接: ACK = 1
- (2) 服务器回发一个确认序号: ack = J + SYN/FIN (1个字节)
- (3) 服务器端向客户端发起连接请求: SYN = 1
- (4) 服务器生成一个随机序号: seq = K

第三次握手：
- (1) 客户单应答服务器的连接请求: ACK = 1，可以携带通信数据
- (2) 客户端回复收到了服务器端的数据: ack = K + 数据长度 + SYN/FIN (1个字节)

***TCP 具体头部结构***

![TCP协议](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/TCP%E5%8D%8F%E8%AE%AE.png?raw=true)

## 五、TCP 滑动窗口

滑动窗口（Sliding window）是一种流量控制技术。TCP 中采用滑动窗口来进行传输控制，滑动窗口的大小意味着接收方还有多大的缓冲区可以用于接收数据。发送方可以通过滑动窗口的大小来确定应该发送多少字节的数据。当滑动窗口为 0 时，发送方一般不能再发送数据报。

滑动窗口是 TCP 中实现诸如 ACK 确认、流量控制、拥塞控制的承载结构，窗口理解为缓冲区的大小
滑动窗口的大小会随着发送数据和接收数据而变化，通信的双方都有发送缓冲区和接收数据的缓冲区

**发送方的发送缓冲区**
- (1) 空闲的空间
- (2) 已经发送了但还没被接收的数据
- (3) 已经发送的数据

**接收方的接收缓冲区**
- (1) 空闲的空间
- (2) 已经接收的数据

`mss`: `Maximum Segment Size` (一条数据的最大的数据量)
`win`: 滑动窗口

### 一次简单的连接过程
1. 客户端向服务器发起连接，win = 4096，mss = 1460
2. 服务器接收连接情况，告诉客户端服务器的 win = 6144，mss = 1024
3. 第三次握手
4. 第4-9次, 客户端连续给服务器发送了 6k 的数据，每次发送 1k
5. 第10次，服务器告诉客户端接收到6k数据 (ack)，缓冲区数据已经处理了2k, win = 2048
6. 第11次，服务器告诉客户端接收到6k数据 (ack)，缓冲区数据已经处理了4k, win = 4096
7. 第12次，客户端给服务器发送了1k的数据
8. 第13次，客户端主动请求和服务器断开连接，并且给服务器发送了1k的数据
9. 第14次，服务器回复 ack, a:同意断开连接的请求 b:告诉客户端已经接受到方才发的2k的数据
10. 第15、16次，通知客户端滑动窗口的大小
11. 第17次，第三次挥手，服务器端给客户端发送FIN,请求断开连接
12. 第18次，第四次挥手，客户端同意了服务器端的断开请求

## 六、TCP 四次挥手

四次挥手发生在断开连接的时候，在程序中当调用了`close()`会使用TCP协议进行四次挥手。因为在TCP连接的时候，采用三次握手建立的的连接是双向的，在断开的时候需要双向断开（四次）

客户端和服务器端都可以主动发起断开连接，谁先调用close()谁就是发起者

1. 客户端请求断开连接 `FIN = M` 
2. 服务端同意断开连接 `ACK = 1, ack = M + 1`
3. 服务端请求断开连接 `FIN = N` 
4. 客户端同意断开连接 `ACK = 1, ack = N + 1`

注意第一次握手时还未建立连接，所以发送方不能携带数据，但第三次握手可以携带数据；接收方必须三次握手之后才能发送数据

四次挥手是因为服务端在(2)回复 ACK 时只是同意客户端断开连接，但服务端可能仍有数据要发送给客户端 (服务端并不想断开)，所以服务端的 ACK 和 FIN 分两次发送

**TCP 四次挥手过程中状态的改变**

![四次挥手状态的改变](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B%E7%8A%B6%E6%80%81%E7%9A%84%E6%94%B9%E5%8F%98.png?raw=true)

当 TCP 连接主动关闭方接收到被动关闭方发送的 FIN 和最终的 ACK 后，连接的主动关闭方必须处于`TIME_WAIT` 状态并持续 `2MSL(Maximum Segment Lifetime)` 时间，这样就能够让 TCP 连接的主动关闭方在它发送的 ACK 丢失的情况下重新发送最终的 ACK。**只有主动关闭的一方才会进入`TIME_WAIT` 状态**

被断开连接的一方第三次挥手后若没收到主动断开连接一方的第四次挥手中的 ACK 会重发 FIN，2MSL可以确保这个 FIN 在抵达主动方时主动方仍可接收到数据，进而发送 ACK

半关闭状态：当 TCP 连接中 A 向 B 发送 FIN 请求关闭，另一端 B 回应 ACK 之后（A 端进入 FIN_WAIT_2 状态），并没有立即发送 FIN 给 A，A 方处于半连接状态（半关闭），此时 A 可以接收 B 发送的数据，但是 A 已经不能再向 B 发送数据。

## 七、TCP 通信并发

要实现TCP通信服务器处理并发的任务，使用多线程或者多进程来解决，总体思路：
1. 一个父进程，多个子进程
2. 父进程负责等待并接受客户端的连接
3. 子进程：完成通信，接受一个客户端连接，就创建一个子进程用于通信

### TCP 通信多进程并发案例

***TCP 通信并发客户端***
```c++
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

int main()
{
    // 1.创建套接字
    int cfd = socket(AF_INET, SOCK_STREAM, 0);
    if (cfd == -1) {
        printf("socket");
        exit(-1);
    }
    // 2.连接服务器端
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(9999);
    inet_pton(AF_INET, "192.168.190.128", &server_addr.sin_addr.s_addr);
    int ret = connect(cfd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    if (ret == -1) {
        perror("connect");
        exit(-1);
    }
    // 3. 通信
    int i = 1;
    char recv_data[1024] = {0};
    while (1)
    {
        memset(recv_data, 0, sizeof(recv_data));
        sprintf(recv_data, "data : %d\n", i++);
        write(cfd, recv_data, strlen(recv_data));
        int len = read(cfd, recv_data, sizeof(recv_data));
        if (len == -1) {
            perror("read");
            exit(-1);
        } else if (len > 0) {
            printf("recv server : %s\n", recv_data);
        }else if (len == 0) {
            printf("server closed...\n");
            break;
        }
        sleep(1);
    }
    // 关闭连接
    close(cfd);
    return 0;
}
```

***TCP 通信并发服务端***

```c++
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <unistd.h>
#include <string.h>
#include <signal.h>
#include <wait.h>

void recycleChild(int arg)
{
    while (1) //可能同时有多个子进程结束
    {
        int ret = waitpid(-1, NULL, WNOHANG); //回收所有进程
        if(ret == -1) { // 所有的子进程都回收了
            break;
        } else if(ret == 0) { // 还有子进程活着
            break;
        } else if(ret > 0){ // 子进程被回收了
            printf("子进程 %d 被回收了\n", ret);
        }
    }
}

int main()
{
    // 注册信号捕捉，回收子进程
    struct sigaction act;
    act.sa_flags = 0;
    sigemptyset(&act.sa_mask);
    act.sa_handler = recycleChild;
    sigaction(SIGCHLD, &act, NULL);
    // 创建socket
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if (lfd == -1) {
        perror("socket");
        exit(-1);
    }
    // 绑定
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = INADDR_ANY;
    server_addr.sin_port = htons(9999);
    int ret = bind(lfd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    if (ret == -1) {
        perror("bind");
        exit(-1);
    }
    // 监听
    ret = listen(lfd, 8);
    if (ret == -1) {
        perror("bind");
        exit(-1);
    }
    // 不断循环等待客户端连接
    while (1)
    {
        // 接受连接
        struct sockaddr_in client_addr;
        int client_addr_size = sizeof(client_addr);
        int cfd = accept(lfd, (struct sockaddr *)&client_addr, &client_addr_size);
        if (cfd == -1) {
            if (errno == EINTR) continue; // 没有这个的话在阻塞等待客户端连接过程中，accept会被信号捕捉打断，直接exit(-1)
            perror("accept");
            exit(-1);
        }
        // 每一个连接进来，创建一个子进程跟客户端通信
        pid_t pid = fork();
        if (pid == 0) // 子进程
        {
            // 获取客户端的信息
            char client_ip[16];
            inet_ntop(AF_INET, &client_addr.sin_addr.s_addr, client_ip, sizeof(client_ip));
            unsigned short client_port = ntohs(client_addr.sin_port);
            printf("client ip : %s, port : %d\n", client_ip, client_port);
            // 通信
            char recv_buf[1024] = {0};
            while (1)
            {
                int len = read(cfd, recv_buf, sizeof(recv_buf));
                if (len == -1) {
                    perror("read");
                    exit(-1);
                } else if (len > 0) {
                    printf("recv client : %s\n", recv_buf);
                } else if (len == 0) {
                    printf("client closed...\n");
                    break;
                }
                write(cfd, recv_buf, strlen(recv_buf));
            }
            close(cfd);
            exit(0); // 退出当前子进程
        }
    }
    close(lfd);
    return 0;
}
```
### TCP 通信多线程并发案例

***客户端同上，TCP 通信多线程并发服务器端***

```c++
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <unistd.h>
#include <string.h>
#include <pthread.h>

struct sockInfo //传递给子线程的信息
{
    int fd; // 通信的文件描述符
    pthread_t tid; // 线程号
    struct sockaddr_in addr; // 客户端的信息
    int id; //保存在数组中的id，用于断开连接后将该结构体重新初始化
};

// 限制最大连接的客户端数目为128个。若在while(1)中定义临时的结构体变量的话，创建完子线程后主线程重新开始循环的时候会回收该资源
struct sockInfo sock_infos[128]; 

// 子线程创建后的工作函数，可以传递结构体保存通信所需的信息
void * working(void * arg)
{
    struct sockInfo * pinfo = (struct sockInfo *) arg;
    // 获取客户端的信息
    char client_ip[16];
    inet_ntop(AF_INET, &pinfo->addr.sin_addr.s_addr, client_ip, sizeof(client_ip));
    unsigned short client_port = ntohs(pinfo->addr.sin_port);
    printf("client ip : %s, port : %d\n", client_ip, client_port);
    // 通信
    char recv_buf[1024] = {0};
    while (1)
    {
        int len = read(pinfo->fd, recv_buf, sizeof(recv_buf));
        if (len == -1) {
            perror("read");
            exit(-1);
        } else if (len > 0) {
            printf("recv client : %s\n", recv_buf);
        } else if (len == 0) { // 服务器断开连接，重新初始化数据
            bzero(&sock_infos[pinfo->id].addr, sizeof(sock_infos[pinfo->id].addr));
            sock_infos[pinfo->id].fd = -1;
            sock_infos[pinfo->id].tid = -1;
            printf("client closed...\n");
            break;
        }
        write(pinfo->fd, recv_buf, strlen(recv_buf));
    }
    close(pinfo->fd);
    return NULL;
}

int main()
{
    // 创建socket
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    if (lfd == -1) {
        perror("socket");
        exit(-1);
    }
    // 绑定
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = INADDR_ANY;
    server_addr.sin_port = htons(9999);
    int ret = bind(lfd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    // 监听
    ret = listen(lfd, 8);
    // 初始化通信所用的结构体信息
    int max_num = sizeof(sock_infos) / sizeof(sock_infos[0]);
    for (int i = 0; i < max_num; ++i)
    {
        bzero(&sock_infos[i], sizeof(sock_infos[i])); //整个结构体的信息置0
        sock_infos[i].fd = -1;
        sock_infos[i].tid = -1;
        sock_infos[i].id = i;
    }
    // 不断循环等待客户端连接，一旦一个客户端连接进来，就创建一个子线程进行通信
    while (1)
    {
        // 接受连接
        struct sockaddr_in client_addr;
        int client_addr_size = sizeof(client_addr);
        int cfd = accept(lfd, (struct sockaddr *)&client_addr, &client_addr_size);
        if (cfd == -1) {
            if (errno == EINTR) continue; // 没有这个的话在阻塞等待客户端连接过程中，accept会被信号捕捉打断，直接exit(-1)
            perror("accept");
            exit(-1);
        }
        
        struct sockInfo* pinfo;
        // 从这个数组中找到一个可以用的sockInfo元素
        for (int i = 0; i < max_num; ++i)
        {
            if (sock_infos[i].fd == -1)
            {
                pinfo = &sock_infos[i];
                break;
            }
            if (i == max_num - 1) // 已经达到了最大的连接数目，休眠1s，然后再从头开始找
            {
                sleep(1);
                i = -1;
            }
        }
        pinfo->fd = cfd;
        memcpy(&pinfo->addr, &client_addr, client_addr_size); //整个拷贝，不然需要两个结构体的成员依次赋值
        // 创建子线程通信
        pthread_create(&pinfo->tid, NULL, working, pinfo);
        pthread_detach(pinfo->tid); //让子线程自己回收资源，join会阻塞
    }
    close(lfd);
    return 0;
}
```
## 八、TCP 状态转变

![TCP状态转变](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/TCP%E7%8A%B6%E6%80%81%E8%BD%AC%E5%8F%98.png?raw=true)

半关闭状态：一端可以继续接收数据但不能再发送数据的状态.

若调用close()进行关闭的话会不能读也不能写，利用API来控制实现半连接状态

```c++
#include <sys/socket.h>
int shutdown(int sockfd, int how);
    sockfd: 需要关闭的socket的描述符
    how: 允许为shutdown操作选择以下几种方式:
        - SHUT_RD(0): 关闭sockfd上的读功能，此选项将不允许sockfd进行读操作。该套接字不再接收数据，任何当前在套接字接受缓冲区的数据将被无声的丢弃掉。
        - SHUT_WR(1): 关闭sockfd的写功能，此选项将不允许sockfd进行写操作。进程不能再对此套接字发出写操作。
        - SHUT_RDWR(2): 关闭sockfd的读写功能。相当于调用shutdown两次：首先是以SHUT_RD,然后以SHUT_WR。
```
注意：
1. 使用 close 中止一个连接，但它只是减少描述符的引用计数，并不直接关闭连接，只有当描述符的引用计数为 0 时才关闭连接。shutdown 不考虑描述符的引用计数，直接关闭描述符。也可选择中止一个方向的连接，只中止读或只中止写
2. 如果有多个进程共享一个套接字，close 每被调用一次，计数减 1 ，直到计数为 0 时，也就是所用进程都调用了 close，套接字将被释放。
3. 在多进程中如果一个进程调用了 shutdown(sfd, SHUT_RDWR) 后，其它的进程将无法进行通信。但如果一个进程 close(sfd) 将不会影响到其它进程。

## 九、端口复用

端口复用最常用的用途是:
1. 防止服务器重启时之前绑定的端口还未释放
2. 程序突然退出而系统没有释放端口

```c++        
#include <sys/types.h>
#include <sys/socket.h>

// 设置套接字的属性（不仅仅能设置端口复用）
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
    参数：
        - sockfd : 要操作的文件描述符
        - level : 级别 - SOL_SOCKET (端口复用的级别)
        - optname : 选项的名称
            - SO_REUSEADDR
            - SO_REUSEPORT
        - optval : 端口复用的值（整形）
            - 1 : 可以复用
            - 0 : 不可以复用
        - optlen : optval参数的大小
    端口复用，设置的时机是在服务器绑定端口之前。

例子：
    int optval = 1;
    setsockopt(lfd, SOL_SOCKET, SO_REUSEPORT, &optval, sizeof(optval));

常看网络相关信息的命令
netstat
    - a 所有的socket
    - p 显示正在使用socket的程序的名称
    - n 直接使用IP地址，而不通过域名服务器
```
# lesson25-31 I/O多路复用（I/O多路转接）

## 一、I/O多路复用概述

### 1、传统的输入输出
    
输入指把文件中的内容写到内存；输出指把内存中的内容读到文件

socket通信中，通信双方都有一个socket(在linux中就类似文件描述符)，对应于内核中的一块缓冲区。I/O就是对读缓冲区和写缓冲区的操作

若不是多线程多进程的话，需要保存所有客户端连接进来的文件描述符，每次需要循环检查所有文件描述符对应的缓冲区是否有数据

I/O 多路复用使得程序能**同时**监听多个文件描述符，能够提高程序的性能，Linux 下实现 I/O 多路复用的系统调用主要有 select、poll 和 **epoll**。通过将工作委托给内核，我们每次仅需检查一次即可

### 2、阻塞等待模型(BIO模型)
如read()读数据时若此时缓冲区内无数据则会一直等待直到有数据写入

- 优点: 不占用CPU宝贵的时间片
- 缺点：同一时刻只能处理一个操作, 效率低

解决方法：多线程或者多进程解决，然仍存在下列缺点
1. 线程或者进程会消耗资源
2. 线程或进程调度消耗CPU资源

### 3、非阻塞, 忙轮询模型(NIO模型)
如设置read(), accept()不阻塞，不断循环检测是否有数据写入

- 优点: 提高了程序的执行效率
- 缺点: 需要占用更多的CPU和系统资源，每次循环需要检测所有文件描述符 ( O(n)次系统调用 )

解决方法：使用IO多路转接技术 `select / poll / epoll`

## 二、IO多路复用技术

第一种： `select / poll` 技术概述
- 将检查读缓冲区是否有数据的工作委托给内核，内核会告诉你有几个缓冲区中有数据，然后程序挨个遍历确定是哪些 fd 对应的缓冲区

第二种： `epoll` 技术概述
- 同样是将检查读缓冲区是否有数据的工作委托给内核，它不仅会告诉你有几个 fd 对应的缓冲区中有数据还会告诉你是哪些 fd

### 1、select

**主旨思想：**
1. 首先要构造一个关于文件描述符的列表，将要监听的文件描述符添加到该列表中。
2. 调用一个系统函数，监听该列表中的文件描述符，直到这些描述符中的一个或者多个进行I/O操作时，该函数才返回。
    - 这个函数是阻塞
    - 函数对文件描述符的检测的操作是由内核完成的
3. 在返回时，它会告诉进程有多少描述符要进行I/O操作，程序自己遍历检测标志位。

```c++
// sizeof(fd_set) = 128B 1024b
#include <sys/time.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/select.h>

int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
    - 参数：
        - nfds : 委托内核检测的最大的文件描述符的值 + 1 (文件描述符表是从0开始的)

        - readfds : 要检测的文件描述符的读的集合，委托内核检测哪些文件描述符的读的属性
            - 一般检测读操作
            - 对应的是对方发送过来的数据，因为读是被动的接收数据，检测的就是读缓冲区
            - 是一个传入传出参数

        - writefds : 要检测的文件描述符的写的集合，委托内核检测哪些文件描述符的写的属性
            - 委托内核检测写缓冲区是不是还可以写数据（不满的就可以写）

        - exceptfds : 检测发生异常的文件描述符的集合
        - timeout : 设置的超时时间
            - NULL : 永久阻塞，直到检测到了文件描述符有变化
            - tv_sec = 0, tv_usec = 0， 不阻塞
            - tv_sec > 0, tv_usec > 0， 阻塞对应的时间
            struct timeval {
                long tv_sec; /* seconds */
                long tv_usec; /* microseconds */
            };

    - 返回值 :
        - -1 : 失败
        - 0 ：只有设置了超时时间并且在该时间内没有检测到变化时才会返回0
        - n (n > 0): 检测的集合中有n个文件描述符发生了变化

// 将参数文件描述符fd对应的标志位设置为0
void FD_CLR(int fd, fd_set *set);

// 判断fd对应的标志位是0还是1， 返回值: fd对应的标志位的值
int FD_ISSET(int fd, fd_set *set);

// 将参数文件描述符 fd 对应的标志位设置为1
void FD_SET(int fd, fd_set *set);

// fd_set 一共有1024 bit, 全部初始化为0
void FD_ZERO(fd_set *set);
```
#### 案例： select()实现TCP通信

```c++
#include <stdio.h>
#include <arpa/inet.h>
#include <stdlib.h>
#include <sys/select.h>
#include <unistd.h>
#include <string.h>

int main()
{
    // 创建套接字
    int lfd = socket(AF_INET, SOCK_STREAM, 0);
    // 绑定
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(9999);
    server_addr.sin_addr.s_addr = INADDR_ANY;
    bind(lfd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    // 监听
    listen(lfd, 5);
    // 创建一个 fd_set 的集合，存放需要检测读缓冲区的文件描述符
    fd_set rdset, tmp;
    FD_ZERO(&rdset);
    FD_SET(lfd, &rdset);
    int max_fd = lfd; //lfd为第一个创建出来的fd，必定就是最大值

    while (1)
    {
        tmp = rdset; //rdset会被内核修改，先保存
        // 调用select系统函数，让内核帮检测哪些文件描述符对应的读缓冲区有数据，阻塞方式
        int ret = select(max_fd + 1, &tmp, NULL, NULL, NULL);
        if (ret == -1){
            perror("select");
            exit(-1);
        } else if (ret == 0){ //未设置超时时间，不会进入
            continue;
        } else {
            // 先检测是否有新的客户端连接到服务器
            if (FD_ISSET(lfd, &tmp))
            {
                struct sockaddr_in client_addr;
                int len = sizeof(client_addr);
                int cfd = accept(lfd, (struct sockaddr *) &client_addr, &len);
                FD_SET(cfd, &rdset); //将新的客户端的fd加入到集合中
                max_fd = max_fd > cfd ? max_fd : cfd;
            }
            //lfd 是第一个文件描述符，是最小的那个
            for (int i = lfd + 1; i <= max_fd; ++i)
            {
                if (FD_ISSET(i, &tmp)) //i对应的客户端发送了数据
                {
                    char buf[1024] = {0};
                    int size = read(i, buf, sizeof(buf));
                    if (size == -1) {
                        perror("read");
                        exit(-1);
                    } else if (size == 0) { //客户端断开连接
                        printf("client closed...\n");
                        close(i);
                        FD_CLR(i, &rdset);
                    } else if (size > 0) {
                        printf("server recv info : %s", buf);
                        write(i, buf, strlen(buf) + 1);
                    }
                }
            }
        }
    }
    close(lfd);
    return 0;
}
```

### 二、poll

`select`多路复用的缺点
1. 每次调用select，都需要把fd集合从用户态拷贝到内核态，这个开销在fd很多时会很大
2. 同时每次调用select都需要在内核遍历传递进来的所有fd，这个开销在fd很多时也很大
3. select支持的文件描述符数量太小了，默认是1024
4. fds集合不能重用，每次都需要重置

`poll` 就是对 `select` 的改进，改进了第3,4个缺点；但是它仍然只能返回有几个文件描述符对应的缓冲区的状态发生了改变，主程序还是需要一次遍历一遍所有的文件描述符

```c++
#include <poll.h>
struct pollfd {
    int fd; /* 委托内核检测的文件描述符 */
    short events; /* 委托内核检测文件描述符的什么事件 */
    short revents; /* 文件描述符实际发生的事件 */
};

常用的事件：读事件 (POLLIN), 写事件 (POLLOUT), 若想描述读写则用 POLLIN | POLLOUT

int poll(struct pollfd *fds, nfds_t nfds, int timeout);
    - 参数：
        - fds : 是一个struct pollfd 结构体数组，这是一个需要检测的文件描述符的集合
        - nfds : 这个是第一个参数数组中最后一个有效元素的下标 + 1
        - timeout : 阻塞时长
            0 : 不阻塞
            -1 : 阻塞，当检测到需要检测的文件描述符有变化，解除阻塞
            > 0 : 阻塞的时长
    - 返回值：
        -1 : 失败
        n (n > 0): 检测的集合中有n个文件描述符发生了变化
```

#### poll实现TCP通信案例

```C++
#include <arpa/inet.h>
#include <poll.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    // 绑定
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(9999);
    server_addr.sin_addr.s_addr = INADDR_ANY;
    bind(lfd, (struct sockaddr *) &server_addr, sizeof(server_addr));
    // 监听
    listen(lfd, 5);
    // 创建检测客户端文件描述符的数组并初始化
    struct pollfd fds[1024];
    for (int i = 0; i < 1024; ++i)
    {
        fds[i].fd = -1;
        fds[i].events = POLLIN; //监听读
    }
    fds[0].fd = lfd;
    int max_fd_index = 0;
    while (1)
    {
        // 调用poll系统函数，让内核帮检测哪些文件描述符有数据
        int n = poll(fds, max_fd_index + 1, -1);
        if (n == -1) {
            perror("poll");
            exit(-1);
        } else if (n == 0) {
            continue;
        }else if (n > 0) {
            // 检查是否有新的客户端连接进来
            if (fds[0].revents & POLLIN) // 注意必须用位运算而不能用 ==,因为可能事件为 POLLIN | POLLOUT
            {
                struct sockaddr_in client_addr;
                int addr_size = sizeof(client_addr);
                int cfd = accept(fds[0].fd, (struct sockaddr *)&client_addr, &addr_size);
                for (int i = 1; i < 1024; ++i) // 遍历找第一个空下标
                {
                    if (fds[i].fd == -1)
                    {
                        fds[i].fd = cfd;
                        fds[i].events = POLLIN;
                        max_fd_index = max_fd_index > i ? max_fd_index : i;
                        break;
                    }
                }
            }
            for (int i = 1; i < max_fd_index + 1; ++i) //遍历fd对应的读缓冲区是否有数据
            {
                if (fds[i].revents & POLLIN)
                {
                    char buf[1024] = {0};
                    int len = read(fds[i].fd, buf, sizeof(buf));
                    if (len == -1) {
                        perror("read");
                        exit(-1);
                    } else if (len == 0) {
                        printf("client closed...\n");
                        close(fds[i].fd);
                        fds[i].fd = -1;
                    } else if (len > 0) {
                        printf("server recv info : %s", buf);
                        write(fds[i].fd, buf, strlen(buf));
                    }
                }
            }
        }
    }
    close(lfd);
    return 0;
}
```

### 三、epoll

`epoll` 不同于 `select` 和 `poll`，它直接在内核中创建数据，可以减少用户态到内核态之间的转变。此外，`epoll` 创建的结构体中直接表明了是哪些文件描述符对应的信息发生了改变，无需主函数中再对所有元素进行遍历

注意，`epoll` 结构体中遍历检查所有文件描述符的结构为红黑树，保存哪些 fd 改变了用的双向链表，内核遍历处理的速度更快

```c++
#include <sys/epoll.h>
// 创建一个新的epoll实例。在内核中创建了一个数据，这个数据中有两个比较重要的数据，一个是需要检的文件描述符的信息（红黑树），还有一个是就绪列表，存放检测到数据发送改变的文件描述符信息（双向链表）

int epoll_create(int size);
    - 参数 size : 目前没有意义了。随便写一个数，但必须大于0

    - 返回值：
        > 0 : 文件描述符，用于操作epoll实例；-1 : 失败

// union 就选一个
typedef union epoll_data { 
    void *ptr;
    int fd;
    uint32_t u32;
    uint64_t u64;
} epoll_data_t;

struct epoll_event {
    uint32_t events; /* Epoll events */
    epoll_data_t data; /* User data variable */
};

常见的Epoll检测事件（events）：
    - EPOLLIN
    - EPOLLOUT
    - EPOLLERR
    - EPOLLET 设置边沿触发模式

// 对epoll实例进行管理：添加文件描述符信息，删除信息，修改信息
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
    - 参数：
        - epfd : epoll实例对应的文件描述符

        - op : 要进行什么操作
            EPOLL_CTL_ADD: 添加
            EPOLL_CTL_MOD: 修改
            EPOLL_CTL_DEL: 删除

        - fd : 要检测的文件描述符
        - event : 检测文件描述符什么事情
    
// 检测函数
int epoll_wait(int epfd, struct epoll_event *events, int maxevents, inttimeout);
    - 参数：
        - epfd : epoll实例对应的文件描述符
        - events : 传出参数，保存了发送了变化的文件描述符的信息
        - maxevents : 第二个参数结构体数组的大小
        - timeout : 阻塞时间
            - 0 : 不阻塞
            - -1 : 阻塞，直到检测到fd数据发生变化，解除阻塞
            - > 0 : 阻塞的时长（毫秒）
    - 返回值：
        成功，返回发送变化的文件描述符的个数 ( > 0)；失败 -1
```

#### epoll 实现TCP通信案例

```c++
#include <arpa/inet.h>
#include <sys/epoll.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>

int main()
{
    // 创建socket
    int lfd = socket(PF_INET, SOCK_STREAM, 0);
    // 绑定
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(9999);
    server_addr.sin_addr.s_addr = INADDR_ANY;
    bind(lfd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    // 监听
    listen(lfd, 5);
    // 调用epoll_create()创建一个epoll实例
    int epfd = epoll_create(1);
    // 将监听的文件描述符相关的检测信息添加到 epoll 实例中 
    struct epoll_event epev;
    epev.data.fd = lfd;
    epev.events = EPOLLIN;
    epoll_ctl(epfd, EPOLL_CTL_ADD, lfd, &epev);
    struct epoll_event epevs[1024]; //用于保存文件描述符状态改变的结果
    while (1)
    {
        int ret = epoll_wait(epfd, epevs, 1024, -1);
        if (ret == -1) {
            perror("epoll_wait");
            exit(-1);
        } 
        printf("ret = %d\n", ret);
        for (int i = 0; i < ret; ++i)
        {
            int curfd = epevs[i].data.fd;
            if (curfd == lfd) // 监听的文件描述符有数据达到，有客户端连接
            {
                struct sockaddr_in client_addr;
                int addr_size = sizeof(client_addr);
                int cfd = accept(curfd, (struct sockaddr *)&client_addr, &addr_size);
                epev.data.fd = cfd;
                epev.events = EPOLLIN;
                epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &epev);
            } else if (epevs[i].events & EPOLLIN) {
                char buf[1024] = {0};
                int len = read(curfd, buf, sizeof(buf));
                if (len == -1) {
                    perror("read");
                    exit(-1);
                } else if (len == 0) { // 断开连接
                    printf("client closed...\n");
                    close(curfd);
                    epoll_ctl(epfd, EPOLL_CTL_DEL, curfd, NULL);
                } else if (len > 0) {
                    printf("server recv info : %s", buf);
                    write(curfd, buf, strlen(buf));
                }
            }
        }
    }
    close(lfd);
    close(epfd);
    return 0;
}
```

#### Epoll 工作模式

##### 1. LT 模式（水平触发）

假设委托内核检测读事件 -> 检测fd的读缓冲区
读缓冲区有数据 - > epoll检测到了会给用户通知

- 用户不读数据，数据一直在缓冲区，epoll 会一直通知
- 用户只读了一部分数据，epoll会通知
- 缓冲区的数据读完了，不通知

LT（level - triggered）是缺省的工作方式，并且同时支持 block 和 no-block socket。在这种做法中，内核告诉你一个文件描述符是否就绪了，然后你可以对这个就绪的 fd 进行 IO 操作。如果你不作任何操作，内核还是会继续通知你的。

##### 2. ET模式（边沿触发）

假设委托内核检测读事件 -> 检测fd的读缓冲区
读缓冲区有数据 - > epoll检测到了会给用户通知
- 用户不读数据，数据一直在缓冲区，epoll下次检测的时候就不通知了
- 用户只读了一部分数据，epoll不通知
- 缓冲区的数据读完了，不通知

ET（edge - triggered）是高速工作方式，只支持 no-block socket。在这种模式下，当描述符从未就绪变为就绪时，内核通过epoll告诉你。然后它会假设你知道文件描述符已经就绪，并且不会再为那个文件描述符发送更多的就绪通知，直到你做了某些操作导致那个文件描述符不再为就绪状态了。但是请注意，如果一直不对这个 fd 作 IO 操作（从而导致它再次变成未就绪），内核不会发送更多的通知（only once）。

ET 模式在很大程度上减少了 epoll 事件被重复触发的次数，因此效率要比 LT 模式高。epoll工作在 ET 模式的时候，**必须使用非阻塞套接口**，以避免由于一个文件句柄的阻塞读/阻塞写操作把处理多个文件描述符的任务饿死。

epoll()默认水平触发工作模式，若想将客户端对应的文件描述符设置为边沿触发模式需在添加fd时在event里添加EPOLLET

```c++
// 之前的epoll() IO多路复用服务端检测并读数据部分简写，注意修改buf大小为5来避免一次性读完所有数据
while (1)
{
    int ret = epoll_wait(epfd, epevs, 1024, -1);
    printf("ret = %d\n", ret);
    for (int i = 0; i < ret; ++i)
    {
        int curfd = epevs[i].data.fd;
        if (curfd == lfd)
        {
            struct sockaddr_in client_addr;
            int addr_size = sizeof(client_addr);
            int cfd = accept(curfd, (struct sockaddr *)&client_addr, &addr_size);
            epev.data.fd = cfd;
            epev.events = EPOLLIN | EPOLLET; // 将检测客户端缓冲区的epoll工作模式设为ET模式
            epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &epev);
        } else if (epevs[i].events & EPOLLIN) {
            char buf[5] = {0};
            int len = read(curfd, buf, sizeof(buf));
            printf("server recv info : %s", buf);
        }
    }
}

// 默认的 LT 模式，假设客户端发送 hello, i am a client，结果为
ret = 1
server recv info : hello
ret = 1
server recv info : , i a
ret = 1
server recv info : m a c
ret = 1
server recv info : lient
ret = 1
即使客户端没有发送新数据，只要读缓冲区内仍有数据qpoll()就会一直通知，会一直读


// 设置ET模式，结果为
ret = 1
server recv info : hello
// 当客户端没有写入新数据时，epoll()不会再通知服务端，假设此时客户端又发送 are you ok? 结果为
ret = 1
server recv info : , i a
从上一次未读的地方开始读，因此采用ET模式最好一次性循环全部读完


// 正确使用 ET 模式
if (curfd == lfd)
{
    struct sockaddr_in client_addr;
    int addr_size = sizeof(client_addr);
    int cfd = accept(curfd, (struct sockaddr *)&client_addr, &addr_size);
    // 设置 cfd 的read() 为非阻塞
    int flag = fcntl(cfd, F_GETFL);
    flag = flag | O_NONBLOCK;
    fcntl(cfd, F_SETFL, flag);
    epev.data.fd = cfd;
    epev.events = EPOLLIN | EPOLLET; // 将检测客户端缓冲区的epoll工作模式设为ET模式
    epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &epev);
} else if (epevs[i].events & EPOLLIN) {
    // 循环读数据
    char buf[5] = {0};
    int len = 0;
    while ( (len = read(curfd, buf, sizeof(buf))) > 0)
    {
        // 打印数据
        printf("server recv info : %s", buf);
        write(curfd, buf, len);
    }
    if (len == 0) {
        printf("client closed....");
    } else if (len == -1) {
        if (errno == EAGAIN) {
            printf("data over.....");
        } else {
            perror("read");
            exit(-1);
        }
    }
}

```

# lesson32-34 UDP通信

## 一、UDP通信流程

![](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E5%9B%9B%E7%AB%A0%E3%80%81Linux%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B/UDP%E9%80%9A%E4%BF%A1%E6%B5%81%E7%A8%8B.png?raw=true)

**`sendto(), recvfrom()` 函数**
```c++
#include <sys/types.h>
#include <sys/socket.h>

ssize_t sendto(int sockfd, const void *buf, size_t len, int flags, const struct sockaddr *dest_addr, socklen_t addrlen);
    - 参数：
        - sockfd : 通信的fd
        - buf : 要发送的数据
        - len : 发送数据的长度
        - flags : 0
        - dest_addr : 通信的另外一端的地址信息
        - addrlen : 地址的内存大小
    - 返回值：
        成功：实际发送的字节数，失败：-1

ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);
    - 参数：
        - sockfd : 通信的fd
        - buf : 接收数据的数组
        - len : 数组的大小
        - flags : 0
        - src_addr : 用来保存另外一端的地址信息，不需要可以指定为NULL
        - addrlen : 地址的内存大小
    - 返回值
        成功：实际接受到的字符数，失败：-1
```
### UDP 通信的简单案例

***服务器端***
```c++
#include <stdio.h>
#include <stdlib.h>
#include <arpa/inet.h>
#include <string.h>
#include <unistd.h>

int main()
{
    // 1、创建套接字
    int fd = socket(PF_INET, SOCK_DGRAM, 0); // 注意UDP是数据报
    // 2、绑定
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(9999);
    server_addr.sin_addr.s_addr = INADDR_ANY;
    bind(fd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    // 3、通信
    while (1)
    {
        char recv_buf[128], IP_buf[16];
        struct sockaddr_in client_addr;
        int addr_size = sizeof(client_addr);
        // 接收收据
        int len = recvfrom(fd, recv_buf, sizeof(recv_buf), 0, (struct sockaddr *)&client_addr, &addr_size);
        printf("client IP : %s, port : %d", 
        inet_ntop(AF_INET, &client_addr.sin_addr.s_addr, IP_buf, sizeof(IP_buf)), ntohs(client_addr.sin_port));
        printf("client say : %s\n", recv_buf);
        // 发送数据
        sendto(fd, recv_buf, strlen(recv_buf) + 1, 0, (struct sockaddr *)&client_addr, sizeof(client_addr)); 
    }
    close(fd);
    return 0;
}
```
***客户端***

```c++
#include <stdio.h>
#include <stdlib.h>
#include <arpa/inet.h>
#include <string.h>
#include <unistd.h>

int main()
{
    // 创建socket
    int fd = socket(PF_INET, SOCK_DGRAM, 0);
    // 服务器的信息
    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(9999);
    inet_pton(AF_INET, "127.0.0.1", &server_addr.sin_addr.s_addr);
    // 通信
    int num = 0, addr_size = sizeof(server_addr);
    while (1)
    {
        char send_buf[128];
        sprintf(send_buf, "send %d info\n", num++);
        // 发送数据
        sendto(fd, send_buf, strlen(send_buf) + 1, 0, (struct sockaddr *)&server_addr, sizeof(server_addr));
        // 接收数据
        memset(send_buf, 0, sizeof(send_buf));
        int num = recvfrom(fd, send_buf, sizeof(send_buf), 0, (struct sockaddr *)&server_addr, &addr_size);
        printf("server say : %s", send_buf);
        sleep(1);
    }
    close(fd);
    return 0;
}
```

## 二、广播

向子网中多台计算机发送消息，并且子网中所有的计算机都可以接收到发送方发送的消息，每个广播消息都包含一个特殊的IP地址，这个IP中子网内主机标志部分的二进制全部为1
- 只能在局域网中使用。
- 客户端需要绑定服务器广播使用的端口，才可以接收到广播消息。

```c++
// 设置广播属性的函数
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t
optlen);
    - sockfd : 文件描述符
    - level : SOL_SOCKET
    - optname : SO_BROADCAST
    - optval : int类型的值，为1表示允许广播
    - optlen : optval的大小
```

### UDP 广播的简单案例

***服务器端***
```c++
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main()
{
    // 创建socket()
    int fd = socket(PF_INET, SOCK_DGRAM, 0);
    // 设置广播属性
    int op = 1;
    setsockopt(fd, SOL_SOCKET, SO_BROADCAST, &op, sizeof(op));
    // 创建一个广播的地址
    struct sockaddr_in client_addr;
    client_addr.sin_family = AF_INET;
    client_addr.sin_port = htons(9999); //客户端用于接受广播信息的端口，客户端需要与这个端口绑定监听
    inet_pton(AF_INET, "192.168.190.255", &client_addr.sin_addr.s_addr);
    // 广播信息
    int num = 0;
    while (1)
    {
        char send_buf[1024] = {0};
        sprintf(send_buf, "hello, %d info\n", num++);
        sendto(fd, send_buf, strlen(send_buf) + 1, 0, (struct sockaddr *)&client_addr, sizeof(client_addr));
        printf("广播的信息：%s", send_buf);
        sleep(1);
    }
    close(fd);
    return 0;
}
```

***客户端***

```c++
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

int main()
{
    // 创建socket
    int fd = socket(PF_INET, SOCK_DGRAM, 0);
    // 指定一个端口并绑定，服务器向这个端口发送广播数据
    struct sockaddr_in client_addr;
    client_addr.sin_family = AF_INET;
    client_addr.sin_port = htons(9999);
    client_addr.sin_addr.s_addr = INADDR_ANY;
    int ret = bind(fd, (struct sockaddr *)&client_addr, sizeof(client_addr));
    // 接受广播信息
    while (1)
    {
        char recv_buf[128] = {0};
        recvfrom(fd, recv_buf, sizeof(recv_buf), 0, NULL, NULL);
        printf("server say : %s", recv_buf);
    }
    close(fd);
    return 0;
}
```

## 三、组播（多播）

单播和广播要么单个要么全部，多播数据报则由运行相应多播会话应用系统的主机上的接口接收。另外，广播一般局限于局域网内使用，而多播则既可以用于局域网，也可以跨广域网使用。
- 组播既可以用于局域网，也可以用于广域网
- 客户端需要加入多播组，才能接收到多播的数据

组播地址

IP 多播通信必须依赖于 IP 多播地址，在 IPv4 中它的范围从 224.0.0.0 到 239.255.255.255，并被划分为局部链接多播地址、预留多播地址和管理权限多播地址三类

***设置组播函数***
```c++
int setsockopt(int sockfd, int level, int optname,const void *optval, socklen_t optlen);
    // 服务器设置多播的信息，外出接口
    - level : IPPROTO_IP
    - optname : IP_MULTICAST_IF
    - optval : struct in_addr

    // 客户端加入到多播组：
    - level : IPPROTO_IP
    - optname : IP_ADD_MEMBERSHIP
    - optval : struct ip_mreq

struct ip_mreq
{
    /* IP multicast address of group. */
    struct in_addr imr_multiaddr; // 组播的IP地址
    /* Local IP address of interface. */
    struct in_addr imr_interface; // 本地的IP地址
};

typedef uint32_t in_addr_t;
struct in_addr
{
    in_addr_t s_addr;
};
```

多播通信代码跟广播通信的区别
- 服务器设置多播的属性，设置外出接口
- 服务器需要初始化客户端的地址信息（多播地址）
- 客户端需要加入到多播地址

### UDP组播通信案例
***服务器端***
```c++
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main()
{
    // 1、创建socket()
    int fd = socket(PF_INET, SOCK_DGRAM, 0);
    // 2、设置组播的属性，设置外出接口
    // 初始化组播地址
    struct in_addr op;
    inet_pton(AF_INET, "239.0.0.10", &op.s_addr);
    setsockopt(fd, IPPROTO_IP, IP_MULTICAST_IF, &op, sizeof(op));
    // 3、初始化客户端的地址信息
    struct sockaddr_in client_addr;
    client_addr.sin_family = AF_INET;
    client_addr.sin_port = htons(9999); //客户端需要与这个端口绑定监听
    inet_pton(AF_INET, "239.0.0.10", &client_addr.sin_addr.s_addr);
    // 4、组播信息
    int num = 0;
    while (1)
    {
        char send_buf[1024] = {0};
        sprintf(send_buf, "hello, %d info\n", num++);
        sendto(fd, send_buf, strlen(send_buf) + 1, 0, (struct sockaddr *)&client_addr, sizeof(client_addr));
        printf("组播的信息：%s", send_buf);
        sleep(1);
    }
    close(fd);
    return 0;
}
```
***客户端***
```c++
#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

int main()
{
    // 1、创建socket
    int fd = socket(PF_INET, SOCK_DGRAM, 0);
    // 2.客户端绑定本地的IP和端口
    struct sockaddr_in client_addr;
    client_addr.sin_family = AF_INET;
    client_addr.sin_port = htons(9999);
    client_addr.sin_addr.s_addr = INADDR_ANY;
    int ret = bind(fd, (struct sockaddr *)&client_addr, sizeof(client_addr));
    if (ret == -1) {
        perror("bind");
        exit(-1);
    }
    // 3、客户端加入到多播组
    struct ip_mreq op;
    inet_pton(AF_INET, "239.0.0.10", &op.imr_multiaddr.s_addr);
    op.imr_interface.s_addr = INADDR_ANY;
    setsockopt(fd, IPPROTO_IP, IP_ADD_MEMBERSHIP, &op, sizeof(op));
    // 4、接受多播信息
    while (1)
    {
        char recv_buf[128] = {0};
        recvfrom(fd, recv_buf, sizeof(recv_buf), 0, NULL, NULL);
        printf("server say : %s", recv_buf);
    }
    close(fd);
    return 0;
}
```
# lesson35 本地套接字

本地套接字的作用：本地的进程间通信 (本地套接字IPC)
- 有关系的进程间的通信
- 没有关系的进程间的通信

本地套接字实现流程和网络套接字类似，一般采用TCP的通信流程。

```c++
#include <sys/un.h>
#define UNIX_PATH_MAX 108
struct sockaddr_un {
    sa_family_t sun_family; // 地址族协议 af_local
    char sun_path[UNIX_PATH_MAX]; // 套接字文件的路径, 这是一个伪文件, 大小永远=0
};

注意伪文件若已经存在的话会 bind() 失败，最好先删除，调用 unlink()
```

## 一、本地套接字通信的流程 - tcp
```c++
// 服务器端
1. 创建监听的套接字
    int lfd = socket(AF_UNIX/AF_LOCAL, SOCK_STREAM, 0);
2. 监听的套接字绑定本地的套接字文件 -> server端
    struct sockaddr_un addr;
    // 绑定成功之后，指定的sun_path中的套接字文件会自动生成。
    bind(lfd, addr, len);
3. 监听
    listen(lfd, 100);
4. 等待并接受连接请求
    struct sockaddr_un cliaddr;
    int cfd = accept(lfd, &cliaddr, len);
5. 通信
    接收数据：read/recv
    发送数据：write/send
6. 关闭连接
    close();

// 客户端的流程
1. 创建通信的套接字
    int fd = socket(AF_UNIX/AF_LOCAL, SOCK_STREAM, 0);
2. 绑定本地的套接字文件 -> client 端
    struct sockaddr_un addr;
    // 绑定成功之后，指定的sun_path中的套接字文件会自动生成。
    bind(lfd, addr, len);
3. 连接服务器，会用服务端的本地套接字文件
    struct sockaddr_un serveraddr;
    connect(fd, &serveraddr, sizeof(serveraddr));
4. 通信
    接收数据：read/recv
    发送数据：write/send
5. 关闭连接
    close();
```

### 本地套接字IPC案例
***服务端***
```c++
#include <stdlib.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/un.h>
#include <string.h>
#include <stdio.h>

int main()
{
    unlink("server.sock");
    // 1、创建监听的套接字
    int lfd = socket(AF_LOCAL, SOCK_STREAM, 0);
    // 2、绑定本地套接字文件
    struct sockaddr_un addr;
    addr.sun_family = AF_LOCAL;
    strcpy(addr.sun_path, "server.sock");
    int ret = bind(lfd, (struct sockaddr *)&addr, sizeof(addr));
    // 3、监听
    listen(lfd, 5);
    // 4、等待客户端连接
    struct sockaddr_un client_addr;
    int addr_size = sizeof(client_addr);
    int cfd = accept(lfd, (struct sockaddr *)&client_addr, &addr_size);
    printf("client socket filename : %s\n", client_addr.sun_path);
    // 5、通信
    while (1)
    {
        char recv_buf[128] = {0};
        int len = recv(cfd, recv_buf, sizeof(recv_buf), 0);
        if (len == -1) {
            perror("recv");
            exit(-1);
        } else if (len == 0) {
            printf("client closed...\n");
            break;
        } else if (len > 0) {
            printf("client say : %s\n", recv_buf);
            send(cfd, recv_buf, strlen(recv_buf), 0);
        }
    }
    close(cfd);
    close(lfd);
    return 0;
}
```
***客户端***
```c++
#include <stdlib.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/un.h>
#include <string.h>
#include <stdio.h>

int main()
{
    unlink("client.sock");
    // 1、创建套接字
    int fd = socket(AF_LOCAL, SOCK_STREAM, 0);
    // 2、绑定本地套接字文件
    struct sockaddr_un client_addr;
    client_addr.sun_family = AF_LOCAL;
    strcpy(client_addr.sun_path, "client.sock");
    bind(fd, (struct sockaddr *)&client_addr, sizeof(client_addr));
    // 3、连接服务器
    struct sockaddr_un server_addr;
    server_addr.sun_family = AF_LOCAL;
    strcpy(server_addr.sun_path, "server.sock");
    int ret = connect(fd, (struct sockaddr *)&server_addr, sizeof(server_addr));
    // 4、通信
    int num = 0;
    while (1)
    {
        char send_buf[128] = {0};
        sprintf(send_buf, "send %d info", num++);
        send(fd, send_buf, strlen(send_buf), 0);
        int len = recv(fd, send_buf, sizeof(send_buf), 0);
        if(len == -1) {
            perror("recv");
            exit(-1);
        } else if(len == 0) {
            printf("server closed....\n");
            break;
        } else if(len > 0) {
            printf("server say : %s\n", send_buf);
        }
        sleep(1);
    }
    close(fd);
    return 0;
}
```
