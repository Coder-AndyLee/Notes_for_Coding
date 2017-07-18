# socket()
- #include<sys/socket.h>  
  int socket(int family, int type, int protocol)  
  返回：非负描述字──成功， -1──出错
- 参数
    - famliy：协议簇/协议域
        -  AF_INET：IPv4
        -  AF_INET6：IPv6
        -  AF_LOCAL：UNIX 
    - type：套接口的类型
        - SOCK_STREAM：字节流
        - SOCK_DGRAM：数据报
        - SOCK_SEQPACKET：有序分组
        - SOCK_RAW：原始套接口
        - 类型与协议簇要匹配
    - protocol：
        - 指定相应的传输协议，也就是诸如TCP或UDP协议等等
        - 系统针对每一个协议簇与类型提供了一个默认的协议，我们通过把protocol设置为0来使用这个默认的值。
        - 常见的协议有TCP、UDP、SCTP，要指定它们分别使用宏IPPROTO_TCP、IPPROTO_UPD、IPPROTO_SCTP来指定
    - family是网络层协议，protocol是传输层协议

# getsockopt()/setsockopt()
- 获取或者设置与某个套接字关联的选项


用法： 
```
#include <sys/types.h>
#include <sys/socket.h>

int getsockopt(int sock, int level, int optname, void *optval, socklen_t *optlen);

int setsockopt(int sock, int level, int optname, const void *optval, socklen_t optlen);
```

- 参数：   
  sock：将要被设置或者获取选项的套接字。  
  level：选项所在的协议层。  
  optname：需要访问的选项名。  
  optval：对于getsockopt()，指向返回选项值的缓冲。对于setsockopt()，指向包含新选项值的缓冲。  
  optlen：对于getsockopt()，作为入口参数时，选项值的最大长度。作为出口参数时，选项值的实际长度。对于setsockopt()，现选项的长度。

- 返回说明：   
  成功执行时，返回0。失败返回-1，errno被设为以下的某个值     
  EBADF：sock不是有效的文件描述词  
  EFAULT：optval指向的内存并非有效的进程空间  
  EINVAL：在调用setsockopt()时，optlen无效  
  ENOPROTOOPT：指定的协议层不能识别选项  
  ENOTSOCK：sock描述的不是套接字  

# bind()
```
#include<sys/socket.h>
int bind(int sockfd, struct sockaddr* addr, socklen_t addrlen)
```
- 返回：0 - 成功，-1 - 失败
- 在套接口中，一个套接字只是用户程序与内核交互信息的枢纽，它自身没有太多的信息，也没有网络协议地址和端口号等信息，在进行网络通信的时候，必须把一个套接字与一个地址相关联，这个过程就是地址绑定的过程。
- sockfd：指定地址与哪个套接字绑定，这是一个由之前的socket函数调用返回的套接字。
- addr：Points to a sockaddr structure containing the address to be bound to the socket. The length and format of the address depend on the address family of the socket.  
```
server_addr.sin_family      = AF_INET;
server_addr.sin_port        = htons(SERVER_PORT);
server_addr.sin_addr.s_addr = INADDR_ANY;
```
- addrlen：sizeof( addr )
- 只有用户进程想与一个具体的地址或端口相关联的时候才需要调用这个函数
- 如果用户进程没有这个需要，那么程序可以依赖内核的自动的选址机制来完成自动地址选择
- 在一般情况下，对于服务器进程问题需要调用bind函数，对于客户进程则不需要调用bind函数。

# listen()
```
#include <sys/socket.h>
int listen(int socket, int backlog);
```
- backlog是侦听队列的长度，在内核函数中，首先对backlog作检查，如果大于128，则强制使其等于128。  
  接下来要检查结构体struct sock的成员sk_state，即当前socket的状态，如果不为TCP_LISTEN，则开始启动端口侦听。

# accept()
```
#include <sys/types.h>
#include <sys/socket.h>
int accept(int sockfd,struct sockaddr *addr,socklen_t *addrlen);
```
- accept()系统调用主要用在基于连接的套接字类型，比如SOCK_STREAM 和 SOCK_SEQPACKET。  
  它提取出所监听套接字的等待连接队列中第一个连接请求，创建一个新的套接字，并返回指向该套接字的文件描述符。新建立的套接字不在监听状态，原来所监听的套接字也不受该系统调用的影响。
- 如果队列中没有等待的连接，套接字也没有被标记为Non-blocking，accept()会阻塞调用函数直到连接出现；如果套接字被标记为Non-blocking，队列中也没有等待的连接，accept()返回错误EAGAIN或EWOULDBLOCK。
- 一般来说，实现时accept()为阻塞函数，当监听socket调用accept()时，它先到自己的receive_buf中查看是否有连接数据包；若有，把数据拷贝出来，删掉接收到的数据包，创建新的socket与客户发来的地址建立连接；

若没有，就阻塞等待；

# fork()
- fork()函数通过系统调用创建一个与原来进程几乎完全相同的进程
- fork调用的一个奇妙之处就是它仅仅被调用一次，却能够返回两次，它可能有三种不同的返回值：  
    1）在 **父进程** 中，fork返回新创建子进程的 **进程ID**；  
    2）在 **子进程** 中，fork返回 **0**；  
    3）如果出现错误，fork返回一 个**负值**；  
- 其实就相当于链表，进程形成了链表，父进程的fpid(p意味point)指向子进程的进程id,因为子进程没有子进程，所以其fpid为0.
- **fork出错**可能有两种原因：  
    1）当前的进程数已经达到了系统规定的上限，这时errno的值被设置为EAGAIN。  
    2）系统内存不足，这时errno的值被设置为ENOMEM。
- 创建新进程成功后，系统中出现两个基本完全相同的进程，这两个进程执行没有固定的先后顺序，哪个进程先执行要看**系统的进程调度策略**。