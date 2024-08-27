1、nginx是多进程，每个进程只有一个线程
2、nginx的进程模型
master进程：管理worker进程，读取配置文件，启动worker进程，平滑重启
worker进程：处理客户端请求，worker进程的数量可以配置
3、nginx的优势：
   1）高并发
   2）内存消耗少
   3）稳定性高
   4）配置简单
   5）支持热部署
   6）支持平滑升级
   7）支持模块化
   8）支持事件驱动
   9）支持epoll模型
   10）支持keepalive
   11）支持gzip压缩
   12）支持负载均衡
   13）支持虚拟主机
   14）支持反向代理
   15）支持缓存
   16）支持限流

4、由于操作系统分为内核空间和用户空间，用户空间不能直接访问内核空间，所以需要通过系统调用或者消息队列等方式来访问内核空间。这就涉及到同步、异步和阻塞、非阻塞的概念。
同步和异步：同步指的是调用者等待被调用者返回结果，异步指的是调用者不等待被调用者返回结果，而是继续执行其他操作，被调用者通过事件、回调等方式通知调用者结果。
阻塞和非阻塞：阻塞指的是调用者被挂起，直到被调用者返回结果，非阻塞指的是调用者不会被挂起，而是继续执行其他操作，被调用者通过轮询或者通知等方式通知调用者结果。


5、limit_req_zone指令用于限制每个客户端的请求速率。
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
limit_req zone=one burst=5 nodelay;  #nodelay不延时

6、limit_conn_zone指令用于限制每个客户端的连接数。
limit_conn_zone $binary_remote_addr zone=one:10m;
limit_conn one 10;  #限制每个客户端最多同时建立10个连接

7、limit_req_zone指令和limit_conn_zone指令可以同时使用，以达到更复杂的限制效果。
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
limit_conn one 10;

8、nginx访问控制：
allow指令：允许访问的IP地址或网络段 取值可以是：
    1）单个IP地址，如：allow 192.168.1.1;
    2）网段，如：allow 192.168.1.0/24 (CIDR表示方法，后面的24表示子网掩码，24表示前24位为网络地址，后8位为主机地址,想当于255.255.255.0);
    3）子网掩码，如：allow 192.168.1.0/255.255.255.0;
    4）通配符，如：allow 192.168.*.*;
deny指令：禁止访问的IP地址或网络段


