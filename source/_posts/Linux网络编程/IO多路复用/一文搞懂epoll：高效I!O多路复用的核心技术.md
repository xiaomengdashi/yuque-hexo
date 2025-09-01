---
title: 一文搞懂epoll：高效I/O多路复用的核心技术
date: '2025-01-22 21:30:10'
updated: '2025-06-19 02:10:23'
---
![](/images/7ee1a9de98b3c7ca685eccbe05754032.gif)

<font style="color:rgb(25, 27, 31);">在 Linux 系统中，处理 I/O 操作有多种方式，像我们熟知的 select 和 poll 等。在连接数较少的情况下，它们或许还能应付自如，但一旦面对大量的并发连接，它们的性能就会大打折扣，就像小马拉大车一样，显得力不从心。然而，有一个技术却能在这种高并发的场景下脱颖而出，它就是 epoll。</font>

<font style="color:rgb(25, 27, 31);">epoll 作为一种高效的 I/O 多路复用技术，与传统的 select 和 poll 相比，具有许多独特的优势和强大的性能。它就像是一把专门为高并发场景打造的利器，能够让我们的程序在处理大量连接时更加高效、稳定。那么，epoll 到底是如何做到的呢？让我们一起深入理解 epoll，探寻它的奥秘吧。</font>

<font style="color:rgb(25, 27, 31);">epoll是Linux内核为处理大批量文件描述符而作了改进的poll，是Linux下多路复用IO接口select/poll的增强版本，它能显著提高程序在大量并发连接中只有少量活跃的情况下的系统CPU利用率。另一点原因就是获取事件的时候，它无须遍历整个被侦听的描述符集，只要遍历那些被内核IO事件异步唤醒而加入Ready队列的描述符集合就行了。</font>

<font style="color:rgb(25, 27, 31);">epoll除了提供select/poll那种IO事件的水平触发（Level Triggered）外，还提供了边缘触发（Edge Triggered），这就使得用户空间程序有可能缓存IO状态，减少epoll_wait/epoll_pwait的调用，提高应用程序效率。</font>

### <font style="color:rgb(25, 27, 31);">1.1epoll 初印象</font>
<font style="color:rgb(25, 27, 31);">epoll 可是 Linux 下多路复用 I/O 接口的 “超级增强版”，专为应对高并发而生。与传统的 select 和 poll 相比，那优势可不是一星半点。select 在处理大量并发连接时，就像个没头苍蝇，每次都得把所有文件描述符集合一股脑从用户态拷贝到内核态，开销巨大，而且还得在内核里线性遍历这些描述符，看看哪个 “有事”，效率低得感人，关键它还有个致命弱点，默认最多只能处理 1024 个文件描述符，稍微多点连接就应付不来。</font>

<font style="color:rgb(25, 27, 31);">poll 虽说在一些方面改进了 select，比如不需要计算最大文件描述符加一的大小，对大批文件描述符处理速度稍快，基于链表存储没了最大连接数限制，但本质上还是得遍历所有描述符找就绪的，大量无谓的遍历让它在高并发下也力不从心。</font>

<font style="color:rgb(25, 27, 31);">epoll 就不一样了，它采用全新的设计理念。当创建一个 epoll 实例后，在内核中有个精心构建的数据结构，像是用红黑树来高效管理所有要监听的文件描述符，添加、删除操作那叫一个快，时间复杂度仅 O (log n)；还有个就绪列表，通常用双向链表实现，专门存放已经就绪、有事件发生的文件描述符。</font>

<font style="color:rgb(25, 27, 31);">当调用 epoll_wait 时，压根不用像 select、poll 那样大海捞针般遍历所有描述符，只需瞅瞅这个就绪列表就行，轻松定位到 “有事” 的连接，大大节省了 CPU 时间。就好比在一个大型仓库里找几件特定物品，select 和 poll 是逐个货架、逐件货物查看，epoll 则是有个智能清单，直接指引到目标货物所在货架，效率高下立判。这使得 epoll 在面对海量并发连接时，系统资源开销小，响应迅速，成为众多高性能网络应用的坚实后盾。</font>

<font style="color:rgb(25, 27, 31);">epoll除了提供select/poll那种IO事件的电平触发 （Level Triggered）外，还提供了边沿触发（Edge Triggered），这就使得用户空间程序有可能缓存IO状态，减少epoll_wait/epoll_pwait的调用，提高应用程序效率。Linux2.6内核中对/dev/epoll设备的访问的封装（system epoll）。这个使我们开发网络应用程序更加简单，并且更加高效。</font>

### <font style="color:rgb(25, 27, 31);">1.2为什么要使用epoll？</font>
<font style="color:rgb(25, 27, 31);">同样，我们在linux系统下，影响效率的依然是I/O操作，linux提供给我们select/poll/epoll等多路复用I/O方式(kqueue暂时没研究过)，为什么我们对epoll情有独钟呢？原因如下：</font>

**<font style="color:rgb(25, 27, 31);">⑴文件描述符数量的对比</font>**

<font style="color:rgb(25, 27, 31);">epoll并没有fd(文件描述符)的上限，它只跟系统内存有关，我的2G的ubuntu下查看是20480个，轻松支持20W个fd。可使用如下命令查看：</font>

```cpp
cat /proc/sys/fs/file-max
```

<font style="color:rgb(25, 27, 31);">再来看select/poll，有一个限定的fd的数量，linux/posix_types.h头文件中</font>

```cpp
#define __FD_SETSIZE    1024
```

**<font style="color:rgb(25, 27, 31);">⑵效率对比</font>**

<font style="color:rgb(25, 27, 31);">当然了，你可以修改上述值，然后重新编译内核，然后再次写代码，这也是没问题的，不过我先说说select/poll的机制，估计你马上会作废上面修改枚举值的想法。</font>

<font style="color:rgb(25, 27, 31);">select/poll会因为监听fd的数量而导致效率低下，因为它是轮询所有fd，有数据就处理，没数据就跳过，所以fd的数量会降低效率；而epoll只处理就绪的fd，它有一个就绪设备的队列，每次只轮询该队列的数据，然后进行处理。</font>

**<font style="color:rgb(25, 27, 31);">⑶内存处理方式对比</font>**

<font style="color:rgb(25, 27, 31);">不管是哪种I/O机制，都无法避免fd在操作过程中拷贝的问题，而epoll使用了mmap(是指文件/对象的内存映射，被映射到多个内存页上)，所以同一块内存就可以避免这个问题。</font>

<font style="color:rgb(25, 27, 31);">btw:TCP/IP协议栈使用内存池管理sk_buff结构，你还可以通过修改内存池pool的大小，毕竟linux支持各种微调内核。</font>

## **<font style="color:rgb(255, 255, 255);">二、epoll核心原理</font>**
### <font style="color:rgb(25, 27, 31);">2.1epoll的工作方式</font>
**<font style="color:rgb(25, 27, 31);">epoll分为两种工作方式LT和ET：</font>**

<font style="color:rgb(25, 27, 31);">LT(level triggered) 是默认/缺省的工作方式，同时支持 block和no_block socket。这种工作方式下，内核会通知你一个fd是否就绪，然后才可以对这个就绪的fd进行I/O操作。就算你没有任何操作，系统还是会继续提示fd已经就绪，不过这种工作方式出错会比较小，传统的select/poll就是这种工作方式的代表。</font>

<font style="color:rgb(25, 27, 31);">ET(edge-triggered) 是高速工作方式，仅支持no_block socket，这种工作方式下，当fd从未就绪变为就绪时，内核会通知fd已经就绪，并且内核认为你知道该fd已经就绪，不会再次通知了，除非因为某些操作导致fd就绪状态发生变化。如果一直不对这个fd进行I/O操作，导致fd变为未就绪时，内核同样不会发送更多的通知，因为only once。所以这种方式下，出错率比较高，需要增加一些检测程序。</font>

<font style="color:rgb(25, 27, 31);">LT可以理解为水平触发，只要有数据可以读，不管怎样都会通知。而ET为边缘触发，只有状态发生变化时才会通知，可以理解为电平变化。</font>

### <font style="color:rgb(25, 27, 31);">2.2如何使用epoll？</font>
<font style="color:rgb(25, 27, 31);">使用epoll很简单，只需要：</font>

```cpp
#include <sys/epoll.h>
```

<font style="color:rgb(25, 27, 31);">有三个关键函数：</font>

```cpp
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_events* event);
int epoll_wait(int epfd, struct epoll_event* events, int maxevents, int timeout);
```

<font style="color:rgb(25, 27, 31);">当然了，不要忘记关闭函数。</font>

**<font style="color:rgb(25, 27, 31);">epoll和select</font>**

<font style="color:rgb(25, 27, 31);">epoll 和 select 的主要区别是：</font>

<font style="color:rgb(25, 27, 31);">epoll 监听的 fd（file descriptor）集合是常驻内核的，它有 3 个系统调用 （</font>_<font style="color:rgb(25, 27, 31);">epoll_create</font>_<font style="color:rgb(25, 27, 31);">, </font>_<font style="color:rgb(25, 27, 31);">epoll_wait</font>_<font style="color:rgb(25, 27, 31);">, </font>_<font style="color:rgb(25, 27, 31);">epoll_ctl</font>_<font style="color:rgb(25, 27, 31);">），通过 </font>_<font style="color:rgb(25, 27, 31);">epoll_wait</font>_<font style="color:rgb(25, 27, 31);"> 可以多次监听同一个 fd 集合，只返回可读写那部分</font>

<font style="color:rgb(25, 27, 31);">select 只有一个系统调用，每次要监听都要将其从用户态传到内核，有事件时返回整个集合。</font>

<font style="color:rgb(25, 27, 31);">从性能上看，如果 fd 集合很大，用户态和内核态之间数据复制的花销是很大的，所以 select 一般限制 fd 集合最大1024。</font>

<font style="color:rgb(25, 27, 31);">从使用上看，epoll 返回的是可用的 fd 子集，select 返回的是全部，哪些可用需要用户遍历判断。</font>

<font style="color:rgb(25, 27, 31);">尽管如此，epoll 的性能并不必然比 select 高，对于 fd 数量较少并且 fd IO 都非常繁忙的情况 select 在性能上有优势。</font>

### <font style="color:rgb(25, 27, 31);">2.3epoll原理</font>
**<font style="color:rgb(25, 27, 31);">⑴为什么要 I/O 多路复用</font>**

<font style="color:rgb(25, 27, 31);">epoll 是一个优秀的 I/O 多路复用方式。所以，在讲解 epoll 之前，我们先来看一下为什么需要 I/O 多路复用。</font>

**<font style="color:rgb(25, 27, 31);">阻塞 OR 非阻塞</font>**

<font style="color:rgb(25, 27, 31);">我们知道，对于 linux 来说，I/O 设备为特殊的文件，读写和文件是差不多的，但是 I/O 设备因为读写与内存读写相比，速度差距非常大。与 cpu 读写速度更是没法比，所以相比于对内存的读写，I/O 操作总是拖后腿的那个。网络 I/O 更是如此，我们很多时候不知道网络 I/O 什么时候到来，就好比我们点了一份外卖，不知道外卖小哥们什么时候送过来，这个时候有两个处理办法：</font>

+ 第一个是我们可以先去睡觉，外卖小哥送到楼下了自然会给我们打电话，这个时候我们在醒来取外卖就可以了。
+ 第二个是我们可以每隔一段时间就给外卖小哥打个电话，这样就能实时掌握外卖的动态信息了。

<font style="color:rgb(25, 27, 31);">第一种方式对应的就是阻塞的 I/O 处理方式，进程在进行 I/O 操作的时候，进入睡眠，如果有 I/O 时间到达，就唤醒这个进程。第二种方式对应的是非阻塞轮询的方式，进程在进行 I/O 操作后，每隔一段时间向内核询问是否有 I/O 事件到达，如果有就立刻处理。</font>

**<font style="color:rgb(25, 27, 31);">线程池OR轮询</font>**

<font style="color:rgb(25, 27, 31);">在现实中，我们当然选择第一种方式，但是在计算机中，情况就要复杂一些。我们知道，在 linux 中，不管是线程还是进程都会占用一定的资源，也就是说，系统总的线程和进程数是一定的。如果有许多的线程或者进程被挂起，无疑是白白消耗了系统的资源。而且，线程或者进程的切换也是需要一定的成本的，需要上下文切换，如果频繁的进行上下文切换，系统会损失很大的性能。一个网络服务器经常需要连接成千上万个客户端，而它能创建的线程可能之后几百个，线程耗光就不能对外提供服务了。这些都是我们在选择 I/O 机制的时候需要考虑的。这种阻塞的 I/O 模式下，一个线程只能处理一个流的 I/O 事件，这是问题的根源。</font>

<font style="color:rgb(25, 27, 31);">这个时候我们首先想到的是采用线程池的方式限制同时访问的线程数，这样就能够解决线程不足的问题了。但是这又会有第二个问题了，多余的任务会通过队列的方式存储在内存只能够，这样很容易在客户端过多的情况下出现内存不足的情况。</font>

<font style="color:rgb(25, 27, 31);">还有一种方式是采用轮询的方式，我们只要不停的把所有流从头到尾问一遍，又从头开始。这样就可以处理多个流了。</font>

**<font style="color:rgb(25, 27, 31);">代理</font>**

<font style="color:rgb(25, 27, 31);">采用轮询的方式虽然能够处理多个 I/O 事件，但是也有一个明显的缺点，那就是会导致 CPU 空转。试想一下，如果所有的流中都没有数据，那么 CPU 时间就被白白的浪费了。</font>

<font style="color:rgb(25, 27, 31);">为了避免CPU空转，可以引进了一个代理。这个代理比较厉害，可以同时观察许多流的I/O事件，在空闲的时候，会把当前线程阻塞掉，当有一个或多个流有I/O事件时，就从阻塞态中醒来，于是我们的程序就会轮询一遍所有的流。这就是 select 与 poll 所做的事情，可见，采用 I/O 复用极大的提高了系统的效率。</font>

**<font style="color:rgb(25, 27, 31);">(2)核心原理大揭秘</font>**

**<font style="color:rgb(25, 27, 31);">红黑树 —— 精准管理的 “魔法树”</font>**

<font style="color:rgb(25, 27, 31);">红黑树在 epoll 里可是扮演着 “大管家” 的关键角色，它专门负责存储和管理海量的文件描述符。这棵树有着独特的 “魔力”，它是一种自平衡的二叉搜索树，意味着无论插入、删除还是查找操作，时间复杂度都能稳稳地保持在 O (log n)。</font>

<font style="color:rgb(25, 27, 31);">想象一下，在高并发场景下，每秒有成千上万个新连接涌入，每个连接对应一个文件描述符。要是没有红黑树，查找一个特定的文件描述符就如同大海捞针，效率极其低下。而有了红黑树，就好比给每个文件描述符都安排了一个专属的智能导航。当需要添加新连接（即新文件描述符）时，它能快速指引插入位置；要关闭某个连接删除对应描述符，也能迅速定位并移除，丝毫不乱。举个例子，在大型在线游戏服务器里，同时在线玩家众多，网络连接频繁变动，红黑树就能高效管理这些连接，确保游戏运行顺畅，玩家操作即时响应，不会因连接管理混乱而卡顿。</font>

**<font style="color:rgb(25, 27, 31);">就绪链表 —— 即时响应的 “情报站”</font>**

<font style="color:rgb(25, 27, 31);">就绪链表就像是 epoll 的 “情报收集站”。当某个被监听的文件描述符状态发生变化，比如有数据可读、可写，内核立马知晓，并通过回调机制，闪电般地将这个就绪的文件描述符添加到就绪链表中。这个链表通常是用双向链表实现，插入和删除操作那叫一个快，时间复杂度仅 O (1)。</font>

<font style="color:rgb(25, 27, 31);">打个比方，这就好比快递驿站收到了你的包裹（数据就绪），立马把你的取件码（文件描述符）放到一个专门的 “待取货架”（就绪链表）上，你一来就能快速拿到包裹。对于应用程序而言，当调用 epoll_wait 时，根本不用费时费力去遍历所有文件描述符，直接到这个 “情报站”—— 就绪链表瞅一眼，就能瞬间获取所有已就绪的文件描述符，第一时间进行数据读写操作，大大提升了响应速度，让数据处理快如闪电。</font>

**<font style="color:rgb(25, 27, 31);">mmap—— 高效传输的 “隐形桥梁”</font>**

<font style="color:rgb(25, 27, 31);">mmap 堪称 epoll 实现高效的幕后英雄，它搭建起了内核空间与用户空间的 “隐形桥梁”—— 共享内存。在传统的 I/O 操作里，数据从内核缓冲区拷贝到用户缓冲区，这个过程就像搬运工来回搬货，费时费力，还增加系统开销。</font>

<font style="color:rgb(25, 27, 31);">但有了 mmap 就不一样了，它直接让内核空间和用户空间共享同一块内存区域，数据来了，双方都能直接访问，减少了数据拷贝次数。这就好比图书馆有个公共书架，管理员（内核）和读者（用户程序）都能直接在上面取放书籍（数据），无需来回搬运。像是视频流处理应用，大量视频数据频繁传输，mmap 使得数据能快速从内核流向用户空间，减少传输延迟，让视频播放流畅无卡顿，极大提升了系统整体性能。</font>

### <font style="color:rgb(25, 27, 31);">2.4epoll优缺点</font>
**<font style="color:rgb(25, 27, 31);">select 与 poll 的缺陷</font>**

<font style="color:rgb(25, 27, 31);">上文中我们发现，实现一个代理来帮助我们处理 I/O 时间能够极大的提高工作效率，select 与 poll 就是这样的代理。但是它们也不是完美的，从上文中我们可以发现，我们能够从 select 中知道是只是有 I/O 事件发生了。但是我们不知道那一个事件发生，每一个 I/O 事件发生的时候，都需要轮询所有的流，这样的时间复杂度 O（N）。但是很多情况下，发生 I/O 时间的只是少数的几个。通过轮询所有的找出少数的几个发生 I/O 的流显然效率非常低下，因此 select 和 epoll 通常只能处理几千个并发连接。</font>

**<font style="color:rgb(25, 27, 31);">epoll 的优势</font>**

<font style="color:rgb(25, 27, 31);">select的缺点之一就是在网络IO流到来的时候，线程会轮询监控文件数组，并且是线性扫描，还有最大值的限制。相比select，epoll则无需如此。服务器主线程创建了epoll对象，并且注册socket和文件事件即可。当数据抵达的时候，也就是对于事件发生，则会调用此前注册的那个io文件。</font>

<font style="color:rgb(25, 27, 31);">先看一个python的epoll例子，采用了网络上一段著名的code：</font>

```python
import socket
import select

EOL1 = b'\n\n'
EOL2 = b'\n\r\n'
response = b'HTTP/1.0 200 OK\r\nDate: Mon, 1 Jan 1996 01:01:01 GMT\r\n'
response += b'Content-Type: text/plain\r\nContent-Length: 13\r\n\r\n'
response += b'Hello, world!'

# 创建套接字对象并绑定监听端口
serversocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serversocket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
serversocket.bind(('0.0.0.0', 8080))
serversocket.listen(1)
serversocket.setblocking(0)

# 创建epoll对象，并注册socket对象的 epoll可读事件
epoll = select.epoll()
epoll.register(serversocket.fileno(), select.EPOLLIN)

try:
    connections = {}
    requests = {}
    responses = {}
    while True:
        # 主循环，epoll的系统调用，一旦有网络IO事件发生，poll调用返回。这是和select系统调用的关键区别
        events = epoll.poll(1)
        # 通过事件通知获得监听的文件描述符，进而处理
        for fileno, event in events:
            # 注册监听的socket对象可读，获取连接，并注册连接的可读事件
            if fileno == serversocket.fileno():
                connection, address = serversocket.accept()
                connection.setblocking(0)
                epoll.register(connection.fileno(), select.EPOLLIN)
                connections[connection.fileno()] = connection
                requests[connection.fileno()] = b''
                responses[connection.fileno()] = response
            elif event & select.EPOLLIN:
                # 连接对象可读，处理客户端发生的信息，并注册连接对象可写
                requests[fileno] += connections[fileno].recv(1024)
                if EOL1 in requests[fileno] or EOL2 in requests[fileno]:
                    epoll.modify(fileno, select.EPOLLOUT)
                    print('-' * 40 + '\n' + requests[fileno].decode()[:-2])
            elif event & select.EPOLLOUT:
                # 连接对象可写事件发生，发送数据到客户端
                byteswritten = connections[fileno].send(responses[fileno])
                responses[fileno] = responses[fileno][byteswritten:]
                if len(responses[fileno]) == 0:
                    epoll.modify(fileno, 0)
                    connections[fileno].shutdown(socket.SHUT_RDWR)
            elif event & select.EPOLLHUP:
                epoll.unregister(fileno)
                connections[fileno].close()
                del connections[fileno]
finally:
    epoll.unregister(serversocket.fileno())
    epoll.close()
    serversocket.close()
```

<font style="color:rgb(25, 27, 31);">可见epoll使用也很简单，并没有过多复杂的逻辑，当然主要是在系统层面封装的好。至于Epoll的原理，也不是三言两语可以解释清楚，作为开发者，先学会如何使用API。</font>

**<font style="color:rgb(25, 27, 31);">epoll与tornado</font>**

<font style="color:rgb(25, 27, 31);">既然epoll是一种高性能的网络io模型，很多web框架也采取epoll模型。大名鼎鼎tornado是python框架中一个高性能的异步框架，其底层也是来者epoll的IO模型。</font>

<font style="color:rgb(25, 27, 31);">当然，tornado是跨平台的，因此他的网络io，在linux下是epoll，unix下则是kqueue。幸好tornado都做了封装，对于开发者及其友好，下面看一个tornado写的回显例子。</font>

```python
import errno
import functools
import tornado.ioloop
import socket


def handle_connection(connection, address):
""" 处理请求，返回数据给客户端 """
    data = connection.recv(2014)
    print data
connection.send(data)


    def connection_ready(sock, fd, events):
""" 事件回调函数，主要用于socket可读事件，用于获取socket的链接 """
        while True:
    try:
        connection, address = sock.accept()
        except socket.error as e:
if e.args[0] not in (errno.EWOULDBLOCK, errno.EAGAIN):
raise
return
        connection.setblocking(0)
        handle_connection(connection, address)


if __name__ == '__main__':
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM, 0)
        sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        sock.setblocking(0)
        sock.bind(("", 5000))
        sock.listen(128)
        # 使用tornado封装好的epoll接口，即IOLoop对象
io_loop = tornado.ioloop.IOLoop.current()
callback = functools.partial(connection_ready, sock)
        # io_loop对象注册网络io文件描述符和回调函数与io事件的绑定
        io_loop.add_handler(sock.fileno(), callback, io_loop.READ)
        io_loop.start()
```

<font style="color:rgb(25, 27, 31);">上面的代码来者tornado的模块IOLoop源码的文档，很简明的介绍了在tornado中如何使用网络IO。当然具体的封装实现，可以参考tornado源码获知，在此不做介绍了。</font>

<font style="color:rgb(25, 27, 31);">说了这么多，总算引出了我们的主人公 epoll 了。不同于忙轮询和无差别轮询，epoll 会把哪个流发生了怎样的 I/O 事件通知我们。此时我们对这些流的操作都是有意义的。（复杂度降低到了O(k)，k为产生 I/O 事件的流的个数。</font>

## **<font style="color:rgb(255, 255, 255);">三、epoll实现原理</font>**
### <font style="color:rgb(25, 27, 31);">3.1epoll 操作</font>
<font style="color:rgb(25, 27, 31);">epoll 在 linux 内核中申请了一个简易的文件系统，把原先的一个 select 或者 poll 调用分为了三个部分：调用 epoll_create 建立一个 epoll 对象（在 epoll 文件系统中给这个句柄分配资源）、调用 epoll_ctl 向 epoll 对象中添加连接的套接字、调用 epoll_wait 收集发生事件的连接。</font>

<font style="color:rgb(25, 27, 31);">这样只需要在进程启动的时候建立一个 epoll 对象，并在需要的时候向它添加或者删除连接就可以了，因此，在实际收集的时候，epoll_wait 的效率会非常高，因为调用的时候只是传递了发生 IO 事件的连接。</font>

### <font style="color:rgb(25, 27, 31);">3.2epoll 实现</font>
<font style="color:rgb(25, 27, 31);">我们以 linux 内核 2.6 为例，说明一下 epoll 是如何高效的处理事件的。当某一个进程调用 epoll_create 方法的时候，Linux 内核会创建一个 eventpoll 结构体，这个结构体中有两个重要的成员。</font>

+ 第一个是 rb_root rbr，这是红黑树的根节点，存储着所有添加到 epoll 中的事件，也就是这个 epoll 监控的事件。
+ 第二个是 list_head rdllist 这是一个双向链表，保存着将要通过 epoll_wait 返回给用户的、满足条件的事件。

<font style="color:rgb(25, 27, 31);">每一个 epoll 对象都有一个独立的 eventpoll 结构体，这个结构体会在内核空间中创造独立的内存，用于存储使用 epoll_ctl 方法向 epoll 对象中添加进来的事件。这些事件都会挂到 rbr 红黑树中，这样就能够高效的识别重复添加的节点。</font>

<font style="color:rgb(25, 27, 31);">所有添加到 epoll 中的事件都会与设备（如网卡等）驱动程序建立回调关系，也就是说，相应的事件发生时会调用这里的方法。这个回调方法在内核中叫做 ep_poll_callback，它把这样的事件放到 rdllist 双向链表中。在 epoll 中，对于每一个事件都会建立一个 epitem 结构体。</font>

<font style="color:rgb(25, 27, 31);">当调用 epoll_wait 检查是否有发生事件的连接时，只需要检查 eventpoll 对象中的 rdllist 双向链表中是否有 epitem 元素，如果 rdllist 链表不为空，则把这里的事件复制到用户态内存中的同时，将事件数量返回给用户。通过这种方法，epoll_wait 的效率非常高。epoll-ctl 在向 epoll 对象中添加、修改、删除事件时，从 rbr 红黑树中查找事件也非常快。这样，epoll 就能够轻易的处理百万级的并发连接。</font>

**<font style="color:rgb(25, 27, 31);">⑴pollable</font>**

<font style="color:rgb(25, 27, 31);">首先，linux 的 file 有个 pollable 的概念，只有 pollable 的 file 才可以加入到 epoll 和 select 中。一个 file 是 pollable 的当且仅当其定义了 file->f_op->poll。file->f_op->poll 的形式如下：</font>

```cpp
__poll_t poll(struct file *fp, poll_table *wait)
```

**<font style="color:rgb(25, 27, 31);">不同类型的 file 实现不同，但做的事情都差不多：</font>**

+ 通过 fp 拿到其对应的 waitqueue
+ 通过 wait 拿到外部设置的 callback[[1]]
+ 执行 callback(fp, waitqueue, wait)，在 callback 中会将另外一个 callback2[[2]] 注册到 waitqueue[[3]]中，此后 fp 有触发事件就会调用 callback2

<font style="color:rgb(25, 27, 31);">waitqueue 是事件驱动的，与驱动程序密切相关，简单来说 poll 函数在 file 的触发队列中注册了个 callback， 有事件发生时就调用callback。感兴趣可以根据文后 [[4]] 的提示看看 socket 的 poll 实现</font>

<font style="color:rgb(25, 27, 31);">了解了 pollable 我们看看 epoll 的三个系统调用</font>**<font style="color:rgb(25, 27, 31);"> epoll_create,，epoll_ctl,，epoll_wait：</font>**

**<font style="color:rgb(25, 27, 31);">①epoll_create：开启 epoll 之门</font>**

<font style="color:rgb(25, 27, 31);">epoll_create 宛如一把神奇的钥匙，用来开启 epoll 的大门。它的使命是创建一个 epoll 实例，函数原型为 “int epoll_create (int size);”，这里的 size 参数在早期 Linux 内核版本里，像是给内核的一个 “小提示”，暗示预计要监听的文件描述符数量，好让内核提前规划资源。不过在后续版本，特别是 Linux 2.6.8 及以后，内核变得更加智能，这个参数就没那么关键了，只要传入大于 0 的值就行，通常我们就简单传个 1。函数执行成功，会返回一个文件描述符（fd），这可是后续操作 epoll 实例的关键 “入口”，有了它，才能进行添加、删除监听事件等一系列操作，要是创建失败，就会返回 -1，同时 errno 会被设置成相应错误码，告诉你问题出在哪，就像出门找不到钥匙，后续啥事都干不了，所以得小心检查错误。</font>

**<font style="color:rgb(25, 27, 31);">②epoll_ctl：掌控监控大权</font>**

<font style="color:rgb(25, 27, 31);">epoll_ctl 则是掌控监控大权的 “指挥官”，负责向 epoll 实例添加、修改或删除文件描述符，原型是 “int epoll_ctl (int epfd, int op, int fd, struct epoll_event *event);”。其中 epfd 就是前面 epoll_create 返回的 epoll 实例文件描述符，op 指明操作类型，有三个 “指令” 可选：EPOLL_CTL_ADD 如同招募新兵，把新的 fd 及其关注事件注册到 epoll 实例中；EPOLL_CTL_MOD 类似给士兵换岗，修改已注册 fd 的监听事件；EPOLL_CTL_DEL 则是让士兵退役，从 epoll 实例里移除指定 fd。fd 就是要操作的文件描述符，而 event 是个关键的结构体指针，结构体里的 events 成员能指定要监听的事件类型，像 EPOLLIN（可读）、EPOLLOUT（可写）、EPOLLERR（出错）等，data 成员可以存放些自定义数据，方便识别 fd，比如存个指向结构体的指针，里面包含 fd 相关的业务信息。操作成功返回 0，失败返回 -1，还会设置 errno，让你知晓 “指挥” 哪里出了岔子。</font>

**<font style="color:rgb(25, 27, 31);">epoll_ctl 的主要操作在 ep_insert, 它做了以下事情：</font>**

+ 初始化一个 epitem，里面包含 fd，监听的事件，就绪链表，关联的 epoll_fd 等信息
+ 调用 ep_item_poll(epitem, ep_ptable_queue_proc[[1]])。ep_item_poll 会调用 vfs_poll， vfs_poll 会调用上面说的 file->f_op->poll 将 ep_poll_callback[[2]] 注册到 waitqueue
+ 调用 ep_rbtree_insert(eventpoll, epitem) 将 epitem 插入 evenpoll 对象的红黑树，方便后续查找

**<font style="color:rgb(25, 27, 31);">ep_poll_callback</font>**

<font style="color:rgb(25, 27, 31);">在了解 epoll_wait 之前我们还需要知道 ep_poll_callback 做了哪些操作</font>

+ ep_poll_callback 被调用，说明 epoll 中某个 file 有了新事件
+ eventpoll 对象有一个 rdllist 字段，用链表存着当前就绪的所有 epitem
+ ep_poll_callback 被调用的时候将 file 对应的 epitem 加到 rdllist 里（不重复）
+ 如果当前用户正在 epoll_wait 阻塞状态 ep_poll_callback 还会通过 wake_up_locked 将 epoll_wait 唤醒

**<font style="color:rgb(25, 27, 31);">③epoll_wait：等待事件降临</font>**

<font style="color:rgb(25, 27, 31);">epoll_wait 就像个耐心的 “守望者”，阻塞等待文件描述符上的事件发生。函数定义为 “int epoll_wait (int epfd, struct epoll_event *events, int maxevents, int timeout);”，epfd 还是那个熟悉的 epoll 实例文件描述符，events 是个传出参数，是个数组，用来存放就绪事件的详细信息，就像个 “收件箱”，内核把发生的事件详情投递进来。maxevents 规定了这个 “收件箱” 的最大容量，也就是最多能接收多少个就绪事件，要注意不能超过创建 epoll 实例时的 size 值。timeout 是超时时间，单位毫秒，-1 表示永远等待，直到有事件发生；0 则是急性子，立马返回，不管有没有事件；正数就设定个等待期限。当有事件就绪或者超时，函数就会返回就绪事件的数量，如果返回 -1，那就是遇到错误，errno 会记录错误原因，等待结束后，就能从 events 数组里依次取出就绪事件，按业务需求处理，开启数据的读写之旅。</font>**<font style="color:rgb(25, 27, 31);">epoll_wait 主要做了以下操作：</font>**

+ 检查 rdllist，如果不为空则去到 7，如果为空则去到 2
+ 设置 timeout
+ 开始无限循环
+ 设置线程状态为 TASK_INTERRUPTIBLE [参看 Sleeping in the Kernal](Kernel Korner - Sleeping in the Kernel)
+ 检查 rdllist 如果不为空去到 7， 否则去到 6
+ 调用 schedule_hrtimeout_range 睡到 timeout，中途有可能被 ep_poll_callback 唤醒回到 4，如果真的 timeout 则 break 去到 7
+ 设置线程状态为 TASK_RUNNING，rdllist如果不为空时退出循环，否则继续循环
+ 调用 ep_send_events 将 rdllist 返回给用户态

<font style="color:rgb(25, 27, 31);">epoll 的原理基本上就这些，还有很多细节如红黑树在哪里用，怎样实现 level-triggered 和 edge-triggered... 我还没看。</font>

<font style="color:rgb(25, 27, 31);">PS. 普通文件不是 pollable 的，详情请看 epoll_does_not_work_with_file</font>

### <font style="color:rgb(25, 27, 31);">3.3epoll 工作模式</font>
**<font style="color:rgb(25, 27, 31);">①水平触发（LT）：持续通知的 “贴心管家”</font>**

<font style="color:rgb(25, 27, 31);">水平触发（LT）可是 epoll 的默认工作模式，就像一位贴心管家，时刻关注着文件描述符的状态。当某个文件描述符处于就绪状态，比如有数据可读或者可写，内核就会通知应用程序。要是应用程序这次没处理完数据，或者没来得及处理，别担心，下次调用 epoll_wait 时，内核依旧会不厌其烦地再次通知，直到数据被处理完或者缓冲区里没数据可读、可写了为止。</font>

<font style="color:rgb(25, 27, 31);">举个例子，在处理 HTTP 报文时，数据可能是一段段陆续到达的。使用 LT 模式，只要缓冲区还有没读完的报文片段，每次 epoll_wait 都会把对应的文件描述符事件返回，让应用程序可以分次从容地解析报文，不用担心错过任何数据，大大降低了编程复杂度，对新手程序员那是相当友好，就像有个老师在旁边，不停提醒你还有作业没做完呢。</font>

**<font style="color:rgb(25, 27, 31);">②边缘触发（ET）：高效灵敏的 “情报员”</font>**

<font style="color:rgb(25, 27, 31);">边缘触发（ET）模式则像一位高效灵敏的情报员，奉行 “只报新事” 原则。只有在文件描述符的状态发生改变时，比如从无数据变为有数据可读，或者从不可写变为可写，内核才会触发事件通知应用程序。一旦通知了，它就默认你知晓此事，后续除非状态再次改变，否则不会重复通知。这意味着应用程序得打起十二分精神，在收到通知后，必须立刻、马上处理数据，而且要尽可能把当前就绪的数据一次性处理完。</font>

<font style="color:rgb(25, 27, 31);">比如说读取大型文件，使用 ET 模式，一旦检测到文件描述符可读，就得赶紧用 while 循环一股脑把数据全读完，不然下次 epoll_wait 可不会再提醒你还有剩余数据。要是读数据时遇到 EAGAIN 或 EWOULDBLOCK 错误，那就说明这次数据真读完了。这种模式虽然编程难度稍高，需要精细处理数据，但减少了不必要的唤醒次数，系统开销小，在追求极致性能的场景下，那可是 “利器”，能让数据如闪电般高效流转。</font>

<font style="color:rgb(25, 27, 31);">当然，在 LT 模式下开发基于 epoll 的应用要简单一些，不太容易出错，而在 ET 模式下事件发生时，如果没有彻底地将缓冲区的数据处理完，则会导致缓冲区的用户请求得不到响应。注意，默认情况下 Nginx 采用 ET 模式使用 epoll 的。</font>

## **<font style="color:rgb(255, 255, 255);">四、epoll内核源码详解</font>**
<font style="color:rgb(25, 27, 31);">网上很多博客说epoll使用了共享内存,这个是完全错误的 ,可以阅读源码,会发现完全没有使用共享内存的任何api，而是 使用了copy_from_user跟__put_user进行内核跟用户虚拟空间数据交互。</font>

```cpp
/*
 *  fs/eventpoll.c (Efficient event retrieval implementation)
 *  Copyright (C) 2001,...,2009	 Davide Libenzi
 *
 *  This program is free software; you can redistribute it and/or modify
 *  it under the terms of the GNU General Public License as published by
 *  the Free Software Foundation; either version 2 of the License, or
 *  (at your option) any later version.
 *
 *  Davide Libenzi <davidel@xmailserver.org>
 *
 */
/*
 * 在深入了解epoll的实现之前, 先来了解内核的3个方面.
 * 1. 等待队列 waitqueue
 * 我们简单解释一下等待队列:
 * 队列头(wait_queue_head_t)往往是资源生产者,
 * 队列成员(wait_queue_t)往往是资源消费者,
 * 当头的资源ready后, 会逐个执行每个成员指定的回调函数,
 * 来通知它们资源已经ready了, 等待队列大致就这个意思.
 * 2. 内核的poll机制
 * 被Poll的fd, 必须在实现上支持内核的Poll技术,
 * 比如fd是某个字符设备,或者是个socket, 它必须实现
 * file_operations中的poll操作, 给自己分配有一个等待队列头.
 * 主动poll fd的某个进程必须分配一个等待队列成员, 添加到
 * fd的对待队列里面去, 并指定资源ready时的回调函数.
 * 用socket做例子, 它必须有实现一个poll操作, 这个Poll是
 * 发起轮询的代码必须主动调用的, 该函数中必须调用poll_wait(),
 * poll_wait会将发起者作为等待队列成员加入到socket的等待队列中去.
 * 这样socket发生状态变化时可以通过队列头逐个通知所有关心它的进程.
 * 这一点必须很清楚的理解, 否则会想不明白epoll是如何
 * 得知fd的状态发生变化的.
 * 3. epollfd本身也是个fd, 所以它本身也可以被epoll,
 * 可以猜测一下它是不是可以无限嵌套epoll下去... 
 *
 * epoll基本上就是使用了上面的1,2点来完成.
 * 可见epoll本身并没有给内核引入什么特别复杂或者高深的技术,
 * 只不过是已有功能的重新组合, 达到了超过select的效果.
 */
/* 
 * 相关的其它内核知识:
 * 1. fd我们知道是文件描述符, 在内核态, 与之对应的是struct file结构,
 * 可以看作是内核态的文件描述符.
 * 2. spinlock, 自旋锁, 必须要非常小心使用的锁,
 * 尤其是调用spin_lock_irqsave()的时候, 中断关闭, 不会发生进程调度,
 * 被保护的资源其它CPU也无法访问. 这个锁是很强力的, 所以只能锁一些
 * 非常轻量级的操作.
 * 3. 引用计数在内核中是非常重要的概念,
 * 内核代码里面经常有些release, free释放资源的函数几乎不加任何锁,
 * 这是因为这些函数往往是在对象的引用计数变成0时被调用,
 * 既然没有进程在使用在这些对象, 自然也不需要加锁.
 * struct file 是持有引用计数的.
 */
/* --- epoll相关的数据结构 --- */
/*
 * This structure is stored inside the "private_data" member of the file
 * structure and rapresent the main data sructure for the eventpoll
 * interface.
 */
/* 每创建一个epollfd, 内核就会分配一个eventpoll与之对应, 可以说是
 * 内核态的epollfd. */
struct eventpoll {
	/* Protect the this structure access */
	spinlock_t lock;
	/*
	 * This mutex is used to ensure that files are not removed
	 * while epoll is using them. This is held during the event
	 * collection loop, the file cleanup path, the epoll file exit
	 * code and the ctl operations.
	 */
	/* 添加, 修改或者删除监听fd的时候, 以及epoll_wait返回, 向用户空间
	 * 传递数据时都会持有这个互斥锁, 所以在用户空间可以放心的在多个线程
	 * 中同时执行epoll相关的操作, 内核级已经做了保护. */
	struct mutex mtx;
	/* Wait queue used by sys_epoll_wait() */
	/* 调用epoll_wait()时, 我们就是"睡"在了这个等待队列上... */
	wait_queue_head_t wq;
	/* Wait queue used by file->poll() */
	/* 这个用于epollfd本事被poll的时候... */
	wait_queue_head_t poll_wait;
	/* List of ready file descriptors */
	/* 所有已经ready的epitem都在这个链表里面 */
	struct list_head rdllist;
	/* RB tree root used to store monitored fd structs */
	/* 所有要监听的epitem都在这里 */
	struct rb_root rbr;
	/*
		这是一个单链表链接着所有的struct epitem当event转移到用户空间时
	 */
	 * This is a single linked list that chains all the "struct epitem" that
	 * happened while transfering ready events to userspace w/out
	 * holding ->lock.
	 */
	struct epitem *ovflist;
	/* The user that created the eventpoll descriptor */
	/* 这里保存了一些用户变量, 比如fd监听数量的最大值等等 */
	struct user_struct *user;
};
/*
 * Each file descriptor added to the eventpoll interface will
 * have an entry of this type linked to the "rbr" RB tree.
 */
/* epitem 表示一个被监听的fd */
struct epitem {
	/* RB tree node used to link this structure to the eventpoll RB tree */
	/* rb_node, 当使用epoll_ctl()将一批fds加入到某个epollfd时, 内核会分配
	 * 一批的epitem与fds们对应, 而且它们以rb_tree的形式组织起来, tree的root
	 * 保存在epollfd, 也就是struct eventpoll中. 
	 * 在这里使用rb_tree的原因我认为是提高查找,插入以及删除的速度.
	 * rb_tree对以上3个操作都具有O(lgN)的时间复杂度 */
	struct rb_node rbn;
	/* List header used to link this structure to the eventpoll ready list */
	/* 链表节点, 所有已经ready的epitem都会被链到eventpoll的rdllist中 */
	struct list_head rdllink;
	/*
	 * Works together "struct eventpoll"->ovflist in keeping the
	 * single linked chain of items.
	 */
	/* 这个在代码中再解释... */
	struct epitem *next;
	/* The file descriptor information this item refers to */
	/* epitem对应的fd和struct file */
	struct epoll_filefd ffd;
	/* Number of active wait queue attached to poll operations */
	int nwait;
	/* List containing poll wait queues */
	struct list_head pwqlist;
	/* The "container" of this item */
	/* 当前epitem属于哪个eventpoll */
	struct eventpoll *ep;
	/* List header used to link this item to the "struct file" items list */
	struct list_head fllink;
	/* The structure that describe the interested events and the source fd */
	/* 当前的epitem关系哪些events, 这个数据是调用epoll_ctl时从用户态传递过来 */
	struct epoll_event event;
};
struct epoll_filefd {
	struct file *file;
	int fd;
};
/* poll所用到的钩子Wait structure used by the poll hooks */
struct eppoll_entry {
	/* List header used to link this structure to the "struct epitem" */
	struct list_head llink;
	/* The "base" pointer is set to the container "struct epitem" */
	struct epitem *base;
	/*
	 * Wait queue item that will be linked to the target file wait
	 * queue head.
	 */
	wait_queue_t wait;
	/* The wait queue head that linked the "wait" wait queue item */
	wait_queue_head_t *whead;
};
/* Wrapper struct used by poll queueing */
struct ep_pqueue {
	poll_table pt;
	struct epitem *epi;
};
/* Used by the ep_send_events() function as callback private data */
struct ep_send_events_data {
	int maxevents;
	struct epoll_event __user *events;
};

/* --- 代码注释 --- */
/* 你没看错, 这就是epoll_create()的真身, 基本啥也不干直接调用epoll_create1了,
 * 另外你也可以发现, size这个参数其实是没有任何用处的... */
SYSCALL_DEFINE1(epoll_create, int, size)
{
        if (size <= 0)
                return -EINVAL;
        return sys_epoll_create1(0);
}
/* 这才是真正的epoll_create啊~~ */
SYSCALL_DEFINE1(epoll_create1, int, flags)
{
	int error;
	struct eventpoll *ep = NULL;//主描述符
	/* Check the EPOLL_* constant for consistency.  */
	/* 这句没啥用处... */
	BUILD_BUG_ON(EPOLL_CLOEXEC != O_CLOEXEC);
	/* 对于epoll来讲, 目前唯一有效的FLAG就是CLOEXEC */
	if (flags & ~EPOLL_CLOEXEC)
		return -EINVAL;
	/*
	 * Create the internal data structure ("struct eventpoll").
	 */
	/* 分配一个struct eventpoll, 分配和初始化细节我们随后深聊~ */
	error = ep_alloc(&ep);
	if (error < 0)
		return error;
	/*
	 * Creates all the items needed to setup an eventpoll file. That is,
	 * a file structure and a free file descriptor.
	 */
	/* 这里是创建一个匿名fd, 说起来就话长了...长话短说:
	 * epollfd本身并不存在一个真正的文件与之对应, 所以内核需要创建一个
	 * "虚拟"的文件, 并为之分配真正的struct file结构, 而且有真正的fd.
	 * 这里2个参数比较关键:
	 * eventpoll_fops, fops就是file operations, 就是当你对这个文件(这里是虚拟的)进行操作(比如读)时,
	 * fops里面的函数指针指向真正的操作实现, 类似C++里面虚函数和子类的概念.
	 * epoll只实现了poll和release(就是close)操作, 其它文件系统操作都有VFS全权处理了.
	 * ep, ep就是struct epollevent, 它会作为一个私有数据保存在struct file的private指针里面.
	 * 其实说白了, 就是为了能通过fd找到struct file, 通过struct file能找到eventpoll结构.
	 * 如果懂一点Linux下字符设备驱动开发, 这里应该是很好理解的,
	 * 推荐阅读 <Linux device driver 3rd>
	 */
	error = anon_inode_getfd("[eventpoll]", &eventpoll_fops, ep,
				 O_RDWR | (flags & O_CLOEXEC));
	if (error < 0)
		ep_free(ep);
	return error;
}
/* 
* 创建好epollfd后, 接下来我们要往里面添加fd咯
* 来看epoll_ctl
* epfd 就是epollfd
* op ADD,MOD,DEL
* fd 需要监听的描述符
* event 我们关心的events
*/
SYSCALL_DEFINE4(epoll_ctl, int, epfd, int, op, int, fd,
		struct epoll_event __user *, event)
{
	int error;
	struct file *file, *tfile;
	struct eventpoll *ep;
	struct epitem *epi;
	struct epoll_event epds;
	error = -EFAULT;
	/* 
	 * 错误处理以及从用户空间将epoll_event结构copy到内核空间.
	 */
	if (ep_op_has_event(op) &&
	    copy_from_user(&epds, event, sizeof(struct epoll_event)))
		goto error_return;
	/* Get the "struct file *" for the eventpoll file */
	/* 取得struct file结构, epfd既然是真正的fd, 那么内核空间
	 * 就会有与之对于的一个struct file结构
	 * 这个结构在epoll_create1()中, 由函数anon_inode_getfd()分配 */
	error = -EBADF;
	file = fget(epfd);
	if (!file)
		goto error_return;
	/* Get the "struct file *" for the target file */
	/* 我们需要监听的fd, 它当然也有个struct file结构, 上下2个不要搞混了哦 */
	tfile = fget(fd);
	if (!tfile)
		goto error_fput;
	/* The target file descriptor must support poll */
	error = -EPERM;
	/* 如果监听的文件不支持poll, 那就没辙了.
	 * 你知道什么情况下, 文件会不支持poll吗?
	 */
	if (!tfile->f_op || !tfile->f_op->poll)
		goto error_tgt_fput;
	/*
	 * We have to check that the file structure underneath the file descriptor
	 * the user passed to us _is_ an eventpoll file. And also we do not permit
	 * adding an epoll file descriptor inside itself.
	 */
	error = -EINVAL;
	/* epoll不能自己监听自己... */
	if (file == tfile || !is_file_epoll(file))
		goto error_tgt_fput;
	/*
	 * At this point it is safe to assume that the "private_data" contains
	 * our own data structure.
	 */
	/* 取到我们的eventpoll结构, 来自与epoll_create1()中的分配 */
	ep = file->private_data;
	/* 接下来的操作有可能修改数据结构内容, 锁之~ */
	mutex_lock(&ep->mtx);
	/*
	 * Try to lookup the file inside our RB tree, Since we grabbed "mtx"
	 * above, we can be sure to be able to use the item looked up by
	 * ep_find() till we release the mutex.
	 */
	/* 对于每一个监听的fd, 内核都有分配一个epitem结构,
	 * 而且我们也知道, epoll是不允许重复添加fd的,
	 * 所以我们首先查找该fd是不是已经存在了.
	 * ep_find()其实就是RBTREE查找, 跟C++STL的map差不多一回事, O(lgn)的时间复杂度.
	 */
	epi = ep_find(ep, tfile, fd);
	error = -EINVAL;
	switch (op) {
		/* 首先我们关心添加 */
	case EPOLL_CTL_ADD:
		if (!epi) {
			/* 之前的find没有找到有效的epitem, 证明是第一次插入, 接受!
			 * 这里我们可以知道, POLLERR和POLLHUP事件内核总是会关心的
			 * */
			epds.events |= POLLERR | POLLHUP;
			/* rbtree插入, 详情见ep_insert()的分析
			 * 其实我觉得这里有insert的话, 之前的find应该
			 * 是可以省掉的... */
			error = ep_insert(ep, &epds, tfile, fd);
		} else
			/* 找到了!? 重复添加! */
			error = -EEXIST;
		break;
		/* 删除和修改操作都比较简单 */
	case EPOLL_CTL_DEL:
		if (epi)
			error = ep_remove(ep, epi);
		else
			error = -ENOENT;
		break;
	case EPOLL_CTL_MOD:
		if (epi) {
			epds.events |= POLLERR | POLLHUP;
			error = ep_modify(ep, epi, &epds);
		} else
			error = -ENOENT;
		break;
	}
	mutex_unlock(&ep->mtx);
error_tgt_fput:
	fput(tfile);
error_fput:
	fput(file);
error_return:
	return error;
}
/* 分配一个eventpoll结构 */
static int ep_alloc(struct eventpoll **pep)
{
	int error;
	struct user_struct *user;
	struct eventpoll *ep;
	/* 获取当前用户的一些信息, 比如是不是root啦, 最大监听fd数目啦 */
	user = get_current_user();
	error = -ENOMEM;
	ep = kzalloc(sizeof(*ep), GFP_KERNEL);
	if (unlikely(!ep))
		goto free_uid;
	/* 这些都是初始化啦 */
	spin_lock_init(&ep->lock);
	mutex_init(&ep->mtx);
	init_waitqueue_head(&ep->wq);//初始化自己睡在的等待队列
	init_waitqueue_head(&ep->poll_wait);//初始化
	INIT_LIST_HEAD(&ep->rdllist);//初始化就绪链表
	ep->rbr = RB_ROOT;
	ep->ovflist = EP_UNACTIVE_PTR;
	ep->user = user;
	*pep = ep;
	return 0;
free_uid:
	free_uid(user);
	return error;
}
/*
 * Must be called with "mtx" held.
 */
/* 
 * ep_insert()在epoll_ctl()中被调用, 完成往epollfd里面添加一个监听fd的工作
 * tfile是fd在内核态的struct file结构
 */
static int ep_insert(struct eventpoll *ep, struct epoll_event *event,
		     struct file *tfile, int fd)
{
	int error, revents, pwake = 0;
	unsigned long flags;
	struct epitem *epi;
	struct ep_pqueue epq;
	/* 查看是否达到当前用户的最大监听数 */
	if (unlikely(atomic_read(&ep->user->epoll_watches) >=
		     max_user_watches))
		return -ENOSPC;
	/* 从著名的slab中分配一个epitem */
	if (!(epi = kmem_***_alloc(epi_***, GFP_KERNEL)))
		return -ENOMEM;
	/* Item initialization follow here ... */
	/* 这些都是相关成员的初始化... */
	INIT_LIST_HEAD(&epi->rdllink);
	INIT_LIST_HEAD(&epi->fllink);
	INIT_LIST_HEAD(&epi->pwqlist);
	epi->ep = ep;
	/* 这里保存了我们需要监听的文件fd和它的file结构 */
	ep_set_ffd(&epi->ffd, tfile, fd);
	epi->event = *event;
	epi->nwait = 0;
	/* 这个指针的初值不是NULL哦... */
	epi->next = EP_UNACTIVE_PTR;
	/* Initialize the poll table using the queue callback */
	/* 好, 我们终于要进入到poll的正题了 */
	epq.epi = epi;
	/* 初始化一个poll_table
	 * 其实就是指定调用poll_wait(注意不是epoll_wait!!!)时的回调函数,和我们关心哪些events,
	 * ep_ptable_queue_proc()就是我们的回调啦, 初值是所有event都关心 */
	init_poll_funcptr(&epq.pt, ep_ptable_queue_proc);
	/*
	 * Attach the item to the poll hooks and get current event bits.
	 * We can safely use the file* here because its usage count has
	 * been increased by the caller of this function. Note that after
	 * this operation completes, the poll callback can start hitting
	 * the new item.
	 */
	/* 这一部很关键, 也比较难懂, 完全是内核的poll机制导致的...
	 * 首先, f_op->poll()一般来说只是个wrapper, 它会调用真正的poll实现,
	 * 拿UDP的socket来举例, 这里就是这样的调用流程: f_op->poll(), sock_poll(),
	 * udp_poll(), datagram_poll(), sock_poll_wait(), 最后调用到我们上面指定的
	 * ep_ptable_queue_proc()这个回调函数...(好深的调用路径...).
	 * 完成这一步, 我们的epitem就跟这个socket关联起来了, 当它有状态变化时,
	 * 会通过ep_poll_callback()来通知.
	 * 最后, 这个函数还会查询当前的fd是不是已经有啥event已经ready了, 有的话
	 * 会将event返回. */
	revents = tfile->f_op->poll(tfile, &epq.pt);
	/*
	 * We have to check if something went wrong during the poll wait queue
	 * install process. Namely an allocation for a wait queue failed due
	 * high memory pressure.
	 */
	error = -ENOMEM;
	if (epi->nwait < 0)
		goto error_unregister;
	/* Add the current item to the list of active epoll hook for this file */
	/* 这个就是每个文件会将所有监听自己的epitem链起来 */
	spin_lock(&tfile->f_lock);
	list_add_tail(&epi->fllink, &tfile->f_ep_links);
	spin_unlock(&tfile->f_lock);
	/*
	 * Add the current item to the RB tree. All RB tree operations are
	 * protected by "mtx", and ep_insert() is called with "mtx" held.
	 */
	/* 都搞定后, 将epitem插入到对应的eventpoll中去 */
	ep_rbtree_insert(ep, epi);
	/* We have to drop the new item inside our item list to keep track of it */
	spin_lock_irqsave(&ep->lock, flags);
	/* If the file is already "ready" we drop it inside the ready list */
	/* 到达这里后, 如果我们监听的fd已经有事件发生, 那就要处理一下 */
	if ((revents & event->events) && !ep_is_linked(&epi->rdllink)) {
		/* 将当前的epitem加入到ready list中去 */
		list_add_tail(&epi->rdllink, &ep->rdllist);
		/* Notify waiting tasks that events are available */
		/* 谁在epoll_wait, 就唤醒它... */
		if (waitqueue_active(&ep->wq))
			wake_up_locked(&ep->wq);
		/* 谁在epoll当前的epollfd, 也唤醒它... */
		if (waitqueue_active(&ep->poll_wait))
			pwake++;
	}
	spin_unlock_irqrestore(&ep->lock, flags);
	atomic_inc(&ep->user->epoll_watches);
	/* We have to call this outside the lock */
	if (pwake)
		ep_poll_safewake(&ep->poll_wait);
	return 0;
error_unregister:
	ep_unregister_pollwait(ep, epi);
	/*
	 * We need to do this because an event could have been arrived on some
	 * allocated wait queue. Note that we don't care about the ep->ovflist
	 * list, since that is used/cleaned only inside a section bound by "mtx".
	 * And ep_insert() is called with "mtx" held.
	 */
	spin_lock_irqsave(&ep->lock, flags);
	if (ep_is_linked(&epi->rdllink))
		list_del_init(&epi->rdllink);
	spin_unlock_irqrestore(&ep->lock, flags);
	kmem_***_free(epi_***, epi);
	return error;
}
/*
 * This is the callback that is used to add our wait queue to the
 * target file wakeup lists.
 */
/* 
 * 该函数在调用f_op->poll()时会被调用.
 * 也就是epoll主动poll某个fd时, 用来将epitem与指定的fd关联起来的.
 * 关联的办法就是使用等待队列(waitqueue)
 */
static void ep_ptable_queue_proc(struct file *file, wait_queue_head_t *whead,
				 poll_table *pt)
{
	struct epitem *epi = ep_item_from_epqueue(pt);
	struct eppoll_entry *pwq;
	if (epi->nwait >= 0 && (pwq = kmem_***_alloc(pwq_***, GFP_KERNEL))) {
		/* 初始化等待队列, 指定ep_poll_callback为唤醒时的回调函数,
		 * 当我们监听的fd发生状态改变时, 也就是队列头被唤醒时,
		 * 指定的回调函数将会被调用. */
		init_waitqueue_func_entry(&pwq->wait, ep_poll_callback);
		pwq->whead = whead;
		pwq->base = epi;
		/* 将刚分配的等待队列成员加入到头中, 头是由fd持有的 */
		add_wait_queue(whead, &pwq->wait);
		list_add_tail(&pwq->llink, &epi->pwqlist);
		/* nwait记录了当前epitem加入到了多少个等待队列中,
		 * 我认为这个值最大也只会是1... */
		epi->nwait++;
	} else {
		/* We have to signal that an error occurred */
		epi->nwait = -1;
	}
}
/*
 * This is the callback that is passed to the wait queue wakeup
 * machanism. It is called by the stored file descriptors when they
 * have events to report.
 */
/* 
 * 这个是关键性的回调函数, 当我们监听的fd发生状态改变时, 它会被调用.
 * 参数key被当作一个unsigned long整数使用, 携带的是events.
 */
static int ep_poll_callback(wait_queue_t *wait, unsigned mode, int sync, void *key)
{
	int pwake = 0;
	unsigned long flags;
	struct epitem *epi = ep_item_from_wait(wait);//从等待队列获取epitem.需要知道哪个进程挂载到这个设备
	struct eventpoll *ep = epi->ep;//获取
	spin_lock_irqsave(&ep->lock, flags);
	/*
	 * If the event mask does not contain any poll(2) event, we consider the
	 * descriptor to be disabled. This condition is likely the effect of the
	 * EPOLLONESHOT bit that disables the descriptor when an event is received,
	 * until the next EPOLL_CTL_MOD will be issued.
	 */
	if (!(epi->event.events & ~EP_PRIVATE_BITS))
		goto out_unlock;
	/*
	 * Check the events coming with the callback. At this stage, not
	 * every device reports the events in the "key" parameter of the
	 * callback. We need to be able to handle both cases here, hence the
	 * test for "key" != NULL before the event match test.
	 */
	/* 没有我们关心的event... */
	if (key && !((unsigned long) key & epi->event.events))
		goto out_unlock;
	/*
	 * If we are trasfering events to userspace, we can hold no locks
	 * (because we're accessing user memory, and because of linux f_op->poll()
	 * semantics). All the events that happens during that period of time are
	 * chained in ep->ovflist and requeued later on.
	 */
	/* 
	 * 这里看起来可能有点费解, 其实干的事情比较简单:
	 * 如果该callback被调用的同时, epoll_wait()已经返回了,
	 * 也就是说, 此刻应用程序有可能已经在循环获取events,
	 * 这种情况下, 内核将此刻发生event的epitem用一个单独的链表
	 * 链起来, 不发给应用程序, 也不丢弃, 而是在下一次epoll_wait
	 * 时返回给用户.
	 */
	if (unlikely(ep->ovflist != EP_UNACTIVE_PTR)) {
		if (epi->next == EP_UNACTIVE_PTR) {
			epi->next = ep->ovflist;
			ep->ovflist = epi;
		}
		goto out_unlock;
	}
	/* If this file is already in the ready list we exit soon */
	/* 将当前的epitem放入ready list */
	if (!ep_is_linked(&epi->rdllink))
		list_add_tail(&epi->rdllink, &ep->rdllist);
	/*
	 * Wake up ( if active ) both the eventpoll wait list and the ->poll()
	 * wait list.
	 */
	/* 唤醒epoll_wait... */
	if (waitqueue_active(&ep->wq))
		wake_up_locked(&ep->wq);
	/* 如果epollfd也在被poll, 那就唤醒队列里面的所有成员. */
	if (waitqueue_active(&ep->poll_wait))
		pwake++;
out_unlock:
	spin_unlock_irqrestore(&ep->lock, flags);
	/* We have to call this outside the lock */
	if (pwake)
		ep_poll_safewake(&ep->poll_wait);
	return 1;
}
/*
 * Implement the event wait interface for the eventpoll file. It is the kernel
 * part of the user space epoll_wait(2).
 */
SYSCALL_DEFINE4(epoll_wait, int, epfd, struct epoll_event __user *, events,
		int, maxevents, int, timeout)
{
	int error;
	struct file *file;
	struct eventpoll *ep;
	/* The maximum number of event must be greater than zero */
	if (maxevents <= 0 || maxevents > EP_MAX_EVENTS)
		return -EINVAL;
	/* Verify that the area passed by the user is writeable */
	/* 这个地方有必要说明一下:
	 * 内核对应用程序采取的策略是"绝对不信任",
	 * 所以内核跟应用程序之间的数据交互大都是copy, 不允许(也时候也是不能...)指针引用.
	 * epoll_wait()需要内核返回数据给用户空间, 内存由用户程序提供,
	 * 所以内核会用一些手段来验证这一段内存空间是不是有效的.
	 */
	if (!access_ok(VERIFY_WRITE, events, maxevents * sizeof(struct epoll_event))) {
		error = -EFAULT;
		goto error_return;
	}
	/* Get the "struct file *" for the eventpoll file */
	error = -EBADF;
	/* 获取epollfd的struct file, epollfd也是文件嘛 */
	file = fget(epfd);
	if (!file)
		goto error_return;
	/*
	 * We have to check that the file structure underneath the fd
	 * the user passed to us _is_ an eventpoll file.
	 */
	error = -EINVAL;
	/* 检查一下它是不是一个真正的epollfd... */
	if (!is_file_epoll(file))
		goto error_fput;
	/*
	 * At this point it is safe to assume that the "private_data" contains
	 * our own data structure.
	 */
	/* 获取eventpoll结构 */
	ep = file->private_data;
	/* Time to fish for events ... */
	/* OK, 睡觉, 等待事件到来~~ */
	error = ep_poll(ep, events, maxevents, timeout);
error_fput:
	fput(file);
error_return:
	return error;
}
/* 这个函数真正将执行epoll_wait的进程带入睡眠状态... */
static int ep_poll(struct eventpoll *ep, struct epoll_event __user *events,
		   int maxevents, long timeout)
{
	int res, eavail;
	unsigned long flags;
	long jtimeout;
	wait_queue_t wait;//等待队列
	/*
	 * Calculate the timeout by checking for the "infinite" value (-1)
	 * and the overflow condition. The passed timeout is in milliseconds,
	 * that why (t * HZ) / 1000.
	 */
	/* 计算睡觉时间, 毫秒要转换为HZ */
	jtimeout = (timeout < 0 || timeout >= EP_MAX_MSTIMEO) ?
		MAX_SCHEDULE_TIMEOUT : (timeout * HZ + 999) / 1000;
retry:
	spin_lock_irqsave(&ep->lock, flags);
	res = 0;
	/* 如果ready list不为空, 就不睡了, 直接干活... */
	if (list_empty(&ep->rdllist)) {
		/*
		 * We don't have any available event to return to the caller.
		 * We need to sleep here, and we will be wake up by
		 * ep_poll_callback() when events will become available.
		 */
		/* OK, 初始化一个等待队列, 准备直接把自己挂起,
		 * 注意current是一个宏, 代表当前进程 */
		init_waitqueue_entry(&wait, current);//初始化等待队列,wait表示当前进程
		__add_wait_queue_exclusive(&ep->wq, &wait);//挂载到ep结构的等待队列
		for (;;) {
			/*
			 * We don't want to sleep if the ep_poll_callback() sends us
			 * a wakeup in between. That's why we set the task state
			 * to TASK_INTERRUPTIBLE before doing the checks.
			 */
			/* 将当前进程设置位睡眠, 但是可以被信号唤醒的状态,
			 * 注意这个设置是"将来时", 我们此刻还没睡! */
			set_current_state(TASK_INTERRUPTIBLE);
			/* 如果这个时候, ready list里面有成员了,
			 * 或者睡眠时间已经过了, 就直接不睡了... */
			if (!list_empty(&ep->rdllist) || !jtimeout)
				break;
			/* 如果有信号产生, 也起床... */
			if (signal_pending(current)) {
				res = -EINTR;
				break;
			}
			/* 啥事都没有,解锁, 睡觉... */
			spin_unlock_irqrestore(&ep->lock, flags);
			/* jtimeout这个时间后, 会被唤醒,
			 * ep_poll_callback()如果此时被调用,
			 * 那么我们就会直接被唤醒, 不用等时间了... 
			 * 再次强调一下ep_poll_callback()的调用时机是由被监听的fd
			 * 的具体实现, 比如socket或者某个设备驱动来决定的,
			 * 因为等待队列头是他们持有的, epoll和当前进程
			 * 只是单纯的等待...
			 **/
			jtimeout = schedule_timeout(jtimeout);//睡觉
			spin_lock_irqsave(&ep->lock, flags);
		}
		__remove_wait_queue(&ep->wq, &wait);
		/* OK 我们醒来了... */
		set_current_state(TASK_RUNNING);
	}
	/* Is it worth to try to dig for events ? */
	eavail = !list_empty(&ep->rdllist) || ep->ovflist != EP_UNACTIVE_PTR;
	spin_unlock_irqrestore(&ep->lock, flags);
	/*
	 * Try to transfer events to user space. In case we get 0 events and
	 * there's still timeout left over, we go trying again in search of
	 * more luck.
	 */
	/* 如果一切正常, 有event发生, 就开始准备数据copy给用户空间了... */
	if (!res && eavail &&
	    !(res = ep_send_events(ep, events, maxevents)) && jtimeout)
		goto retry;
	return res;
}
/* 这个简单, 我们直奔下一个... */
static int ep_send_events(struct eventpoll *ep,
			  struct epoll_event __user *events, int maxevents)
{
	struct ep_send_events_data esed;
	esed.maxevents = maxevents;
	esed.events = events;
	return ep_scan_ready_list(ep, ep_send_events_proc, &esed);
}
/**
 * ep_scan_ready_list - Scans the ready list in a way that makes possible for
 *                      the scan code, to call f_op->poll(). Also allows for
 *                      O(NumReady) performance.
 *
 * @ep: Pointer to the epoll private data structure.
 * @sproc: Pointer to the scan callback.
 * @priv: Private opaque data passed to the @sproc callback.
 *
 * Returns: The same integer error code returned by the @sproc callback.
 */
static int ep_scan_ready_list(struct eventpoll *ep,
			      int (*sproc)(struct eventpoll *,
					   struct list_head *, void *),
			      void *priv)
{
	int error, pwake = 0;
	unsigned long flags;
	struct epitem *epi, *nepi;
	LIST_HEAD(txlist);
	/*
	 * We need to lock this because we could be hit by
	 * eventpoll_release_file() and epoll_ctl().
	 */
	mutex_lock(&ep->mtx);
	/*
	 * Steal the ready list, and re-init the original one to the
	 * empty list. Also, set ep->ovflist to NULL so that events
	 * happening while looping w/out locks, are not lost. We cannot
	 * have the poll callback to queue directly on ep->rdllist,
	 * because we want the "sproc" callback to be able to do it
	 * in a lockless way.
	 */
	spin_lock_irqsave(&ep->lock, flags);
	/* 这一步要注意, 首先, 所有监听到events的epitem都链到rdllist上了,
	 * 但是这一步之后, 所有的epitem都转移到了txlist上, 而rdllist被清空了,
	 * 要注意哦, rdllist已经被清空了! */
	list_splice_init(&ep->rdllist, &txlist);
	/* ovflist, 在ep_poll_callback()里面我解释过, 此时此刻我们不希望
	 * 有新的event加入到ready list中了, 保存后下次再处理... */
	ep->ovflist = NULL;
	spin_unlock_irqrestore(&ep->lock, flags);
	/*
	 * Now call the callback function.
	 */
	/* 在这个回调函数里面处理每个epitem
	 * sproc 就是 ep_send_events_proc, 下面会注释到. */
	error = (*sproc)(ep, &txlist, priv);
	spin_lock_irqsave(&ep->lock, flags);
	/*
	 * During the time we spent inside the "sproc" callback, some
	 * other events might have been queued by the poll callback.
	 * We re-insert them inside the main ready-list here.
	 */
	/* 现在我们来处理ovflist, 这些epitem都是我们在传递数据给用户空间时
	 * 监听到了事件. */
	for (nepi = ep->ovflist; (epi = nepi) != NULL;
	     nepi = epi->next, epi->next = EP_UNACTIVE_PTR) {
		/*
		 * We need to check if the item is already in the list.
		 * During the "sproc" callback execution time, items are
		 * queued into ->ovflist but the "txlist" might already
		 * contain them, and the list_splice() below takes care of them.
		 */
		/* 将这些直接放入readylist */
		if (!ep_is_linked(&epi->rdllink))
			list_add_tail(&epi->rdllink, &ep->rdllist);
	}
	/*
	 * We need to set back ep->ovflist to EP_UNACTIVE_PTR, so that after
	 * releasing the lock, events will be queued in the normal way inside
	 * ep->rdllist.
	 */
	ep->ovflist = EP_UNACTIVE_PTR;
	/*
	 * Quickly re-inject items left on "txlist".
	 */
	/* 上一次没有处理完的epitem, 重新插入到ready list */
	list_splice(&txlist, &ep->rdllist);
	/* ready list不为空, 直接唤醒... */
	if (!list_empty(&ep->rdllist)) {
		/*
		 * Wake up (if active) both the eventpoll wait list and
		 * the ->poll() wait list (delayed after we release the lock).
		 */
		if (waitqueue_active(&ep->wq))
			wake_up_locked(&ep->wq);
		if (waitqueue_active(&ep->poll_wait))
			pwake++;
	}
	spin_unlock_irqrestore(&ep->lock, flags);
	mutex_unlock(&ep->mtx);
	/* We have to call this outside the lock */
	if (pwake)
		ep_poll_safewake(&ep->poll_wait);
	return error;
}
/* 该函数作为callbakc在ep_scan_ready_list()中被调用
 * head是一个链表, 包含了已经ready的epitem,
 * 这个不是eventpoll里面的ready list, 而是上面函数中的txlist.
 */
static int ep_send_events_proc(struct eventpoll *ep, struct list_head *head,
			       void *priv)
{
	struct ep_send_events_data *esed = priv;
	int eventcnt;
	unsigned int revents;
	struct epitem *epi;
	struct epoll_event __user *uevent;
	/*
	 * We can loop without lock because we are passed a task private list.
	 * Items cannot vanish during the loop because ep_scan_ready_list() is
	 * holding "mtx" during this call.
	 */
	/* 扫描整个链表... */
	for (eventcnt = 0, uevent = esed->events;
	     !list_empty(head) && eventcnt < esed->maxevents;) {
		/* 取出第一个成员 */
		epi = list_first_entry(head, struct epitem, rdllink);
		/* 然后从链表里面移除 */
		list_del_init(&epi->rdllink);
		/* 读取events, 
		 * 注意events我们ep_poll_callback()里面已经取过一次了, 为啥还要再取?
		 * 1. 我们当然希望能拿到此刻的最新数据, events是会变的~
		 * 2. 不是所有的poll实现, 都通过等待队列传递了events, 有可能某些驱动压根没传
		 * 必须主动去读取. */
		revents = epi->ffd.file->f_op->poll(epi->ffd.file, NULL) &
			epi->event.events;
		if (revents) {
			/* 将当前的事件和用户传入的数据都copy给用户空间,
			 * 就是epoll_wait()后应用程序能读到的那一堆数据. */
			if (__put_user(revents, &uevent->events) ||
			    __put_user(epi->event.data, &uevent->data)) {
				list_add(&epi->rdllink, head);
				return eventcnt ? eventcnt : -EFAULT;
			}
			eventcnt++;
			uevent++;
			if (epi->event.events & EPOLLONESHOT)
				epi->event.events &= EP_PRIVATE_BITS;
			else if (!(epi->event.events & EPOLLET)) {
				/* 嘿嘿, EPOLLET和非ET的区别就在这一步之差呀~
				 * 如果是ET, epitem是不会再进入到readly list,
				 * 除非fd再次发生了状态改变, ep_poll_callback被调用.
				 * 如果是非ET, 不管你还有没有有效的事件或者数据,
				 * 都会被重新插入到ready list, 再下一次epoll_wait
				 * 时, 会立即返回, 并通知给用户空间. 当然如果这个
				 * 被监听的fds确实没事件也没数据了, epoll_wait会返回一个0,
				 * 空转一次.
				 */
				list_add_tail(&epi->rdllink, &ep->rdllist);
			}
		}
	}
	return eventcnt;
}
/* ep_free在epollfd被close时调用,
 * 释放一些资源而已, 比较简单 */
static void ep_free(struct eventpoll *ep)
{
	struct rb_node *rbp;
	struct epitem *epi;
	/* We need to release all tasks waiting for these file */
	if (waitqueue_active(&ep->poll_wait))
		ep_poll_safewake(&ep->poll_wait);
	/*
	 * We need to lock this because we could be hit by
	 * eventpoll_release_file() while we're freeing the "struct eventpoll".
	 * We do not need to hold "ep->mtx" here because the epoll file
	 * is on the way to be removed and no one has references to it
	 * anymore. The only hit might come from eventpoll_release_file() but
	 * holding "epmutex" is sufficent here.
	 */
	mutex_lock(&epmutex);
	/*
	 * Walks through the whole tree by unregistering poll callbacks.
	 */
	for (rbp = rb_first(&ep->rbr); rbp; rbp = rb_next(rbp)) {
		epi = rb_entry(rbp, struct epitem, rbn);
		ep_unregister_pollwait(ep, epi);
	}
	/*
	 * Walks through the whole tree by freeing each "struct epitem". At this
	 * point we are sure no poll callbacks will be lingering around, and also by
	 * holding "epmutex" we can be sure that no file cleanup code will hit
	 * us during this operation. So we can avoid the lock on "ep->lock".
	 */
	/* 之所以在关闭epollfd之前不需要调用epoll_ctl移除已经添加的fd,
	 * 是因为这里已经做了... */
	while ((rbp = rb_first(&ep->rbr)) != NULL) {
		epi = rb_entry(rbp, struct epitem, rbn);
		ep_remove(ep, epi);
	}
	mutex_unlock(&epmutex);
	mutex_destroy(&ep->mtx);
	free_uid(ep->user);
	kfree(ep);
}
/* File callbacks that implement the eventpoll file behaviour */
static const struct file_operations eventpoll_fops = {
	.release	= ep_eventpoll_release,
	.poll		= ep_eventpoll_poll
};
/* Fast test to see if the file is an evenpoll file */
static inline int is_file_epoll(struct file *f)
{
	return f->f_op == &eventpoll_fops;
}
/* OK, eventpoll我认为比较重要的函数都注释完了... */
```

### <font style="color:rgb(25, 27, 31);">4.1epoll_create</font>
<font style="color:rgb(25, 27, 31);">从slab缓存中创建一个eventpoll对象,并且创建一个匿名的fd跟fd对应的file对象, 而eventpoll对象保存在struct file结构的private指针中,并且返回, 该fd对应的file operations只是实现了poll跟release操作。</font>

<font style="color:rgb(25, 27, 31);">创建eventpoll对象的初始化操作，获取当前用户信息,是不是root,最大监听fd数目等并且保存到eventpoll对象中 初始化等待队列,初始化就绪链表,初始化红黑树的头结点。</font>

### <font style="color:rgb(25, 27, 31);">4.2epoll_ctl操作</font>
<font style="color:rgb(25, 27, 31);">将epoll_event结构拷贝到内核空间中，并且判断加入的fd是否支持poll结构(epoll,poll,selectI/O多路复用必须支持poll操作)，并且从epfd->file->privatedata获取event_poll对象,根据op区分是添加删除还是修改, 首先在eventpoll结构中的红黑树查找是否已经存在了相对应的fd,没找到就支持插入操作,否则报重复的错误，相对应的修改,删除比较简单就不啰嗦了。</font>

<font style="color:rgb(25, 27, 31);">插入操作时,会创建一个与fd对应的epitem结构,并且初始化相关成员,比如保存监听的fd跟file结构之类的，重要的是指定了调用poll_wait时的回调函数用于数据就绪时唤醒进程,(其内部,初始化设备的等待队列,将该进程注册到等待队列)完成这一步, 我们的epitem就跟这个socket关联起来了, 当它有状态变化时, 会通过ep_poll_callback()来通知，最后调用加入的fd的file operation->poll函数(最后会调用poll_wait操作)用于完成注册操作，最后将epitem结构添加到红黑树中。</font>

### <font style="color:rgb(25, 27, 31);">4.3epoll_wait操作</font>
<font style="color:rgb(25, 27, 31);">计算睡眠时间(如果有),判断eventpoll对象的链表是否为空,不为空那就干活不睡明.并且初始化一个等待队列,把自己挂上去,设置自己的进程状态，为可睡眠状态.判断是否有信号到来(有的话直接被中断醒来,),如果啥事都没有那就调用schedule_timeout进行睡眠，如果超时或者被唤醒,首先从自己初始化的等待队列删除,然后开始拷贝资源给用户空间了，拷贝资源则是先把就绪事件链表转移到中间链表,然后挨个遍历拷贝到用户空间, 并且挨个判断其是否为水平触发,是的话再次插入到就绪链表。</font>

## **<font style="color:rgb(255, 255, 255);">五、epoll使用实例：TCP服务器处理多个客户端请求</font>**
### <font style="color:rgb(25, 27, 31);">5.1epoll创建</font>
```cpp
int epoll_create(int size); //监听个数
```

### <font style="color:rgb(25, 27, 31);">5.2epoll事件设置</font>
```cpp
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event)
```

<font style="color:rgb(25, 27, 31);">第一个参数epfd是epoll_create()的返回值。</font>

<font style="color:rgb(25, 27, 31);">第二个参数op表示动作，用三个宏来表示：</font>

```cpp
EPOLL_CTL_ADD：注册新的fd到epfd中；
EPOLL_CTL_MOD：修改已经注册的fd的监听事件；
EPOLL_CTL_DEL：从epfd中删除一个fd；
```

<font style="color:rgb(25, 27, 31);">第三个参数是需要监听的fd。</font>

<font style="color:rgb(25, 27, 31);">第四个参数是告诉内核需要监听什么事。</font>

<font style="color:rgb(25, 27, 31);">struct epoll_event结构如下：</font>

```cpp
struct epoll_event {  
    __uint32_t events; /* Epoll events */
    epoll_data_t data; /* User data variable */
};
```

**<font style="color:rgb(25, 27, 31);">events可以是以下几个宏的集合：</font>**

+ EPOLLIN ：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）；
+ EPOLLOUT：表示对应的文件描述符可以写；
+ EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）；
+ EPOLLERR：表示对应的文件描述符发生错误；
+ EPOLLHUP：表示对应的文件描述符被挂断；
+ EPOLLET：将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的。
+ EPOLLONESHOT：只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里

### <font style="color:rgb(25, 27, 31);">5.3epoll监听</font>
```cpp
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout)
```

+ 等待事件的产生，类似于select()调用。
+ 参数events用来从内核得到事件的集合，
+ maxevents告之内核这个events有多大，这个maxevents的值不能大于创建epoll_create()时的size，
+ 参数timeout是超时时间（毫秒，0会立即返回，-1将不确定，也有说法说是永久阻塞）。
+ 该函数返回需要处理的事件数目，如返回0表示已超时。

### <font style="color:rgb(25, 27, 31);">5.4编程实例测试</font>
<font style="color:rgb(25, 27, 31);">本次测试在上篇Unix域socket通信代码的基础上进行修改，只使用TCP方式的socket通信进行测试。上篇的测试代码，服务端接收到一个客户端的连接后，就仅对该客户端进行服务，没有再接收其它客户端的处理逻辑，本篇要实现的，就是一个服务端，能够接收多个客户端的数据。</font>

<font style="color:rgb(25, 27, 31);">编程之前，先来看下要实现的程序结构，其中黄色的部分为本篇在上篇例程的基础上，需要增加的部分：</font>

![](/images/02f80065d62c0787b90426d8b9481863.jpeg)

**<font style="color:rgb(25, 27, 31);">①为socket服务端增加epoll监听功能，</font>**<font style="color:rgb(25, 27, 31);">TCP服务端的代码修改后如下，主要的修改在listen之后，创建一个epoll，然后把服务端的socketfd加入epoll进行监听：</font>

<font style="color:rgb(25, 27, 31);">当有新的客户端请求连接时，服务端的socketfd会收到事件，进而epoll会收到服务端socketfd的EPOLLIN事件，此时可以让服务端接受客户端的请求，并把创建的客户端fd也加入到epoll进行监听</font>

<font style="color:rgb(25, 27, 31);">当客户端连接成功并被epoll监听后，客户端再发消息过来，epoll就会收到对应客户端fd的EPOLLIN事件，此时可以让服务端读取客户端的消息</font>

```cpp
#define LISTEN_MAX     5
#define EPOLL_FDSIZE   LISTEN_MAX
#define EPOLL_EVENTS   20
#define CLIENT_NUM     3

void EpollAddEvent(int epollfd, int fd, int event)
{
    PRINT("epollfd:%d add fd:%d(event:%d)\n", epollfd, fd, event);
    struct epoll_event ev;
    ev.events = event;
    ev.data.fd = fd;
    epoll_ctl(epollfd, EPOLL_CTL_ADD, fd, &ev);
}

void TcpServerThread()
{
	//------------socket
	int sockfd = socket(AF_UNIX, SOCK_STREAM, 0);
	if (sockfd < 0)
	{
		PRINT("create socket fail\n");
		return;
	}
	PRINT("create socketfd:%d\n", sockfd);

	struct sockaddr_un addr;
	memset (&addr, 0, sizeof(addr));
	addr.sun_family = AF_UNIX;
	strcpy(addr.sun_path, UNIX_TCP_SOCKET_ADDR);

	//------------bind
	if (bind(sockfd, (struct sockaddr *)&addr, sizeof(addr)))
	{
		PRINT("bind fail\n");
		return;
	}
	PRINT("bind ok\n");

	//------------listen
	if (listen(sockfd, LISTEN_MAX))
	{
		PRINT("listen fail\n");
		return;
	}
	PRINT("listen ok\n");

	//------------epoll---------------
	int epollfd = epoll_create(EPOLL_FDSIZE);
	if (epollfd < 0)
	{
		PRINT("epoll create fail\n");
		return;
	}
	PRINT("epoll create fd:%d\n", epollfd);

	EpollAddEvent(epollfd, sockfd, EPOLLIN);

	struct epoll_event events[EPOLL_EVENTS];
	while(1)
	{
		PRINT("epoll wait...\n");
		int num = epoll_wait(epollfd, events, EPOLL_EVENTS, -1);
		PRINT("epoll wait done, num:%d\n", num);
		for (int i = 0;i < num;i++)
		{
			int fd = events[i].data.fd;
			if (EPOLLIN == events[i].events)
			{
				//接受客户端的连接请求
				if (fd == sockfd)
				{
					//------------accept
					int clientfd = accept(sockfd, NULL, NULL);
					if (clientfd == -1)
					{
						PRINT("accpet error\n");
					}
					else
					{
						PRINT("=====> accept new clientfd:%d\n", clientfd);
						
						EpollAddEvent(epollfd, clientfd, EPOLLIN);
					}
				}
				//读取客户端发来的数据
				else
				{
					char buf[BUF_SIZE] = {0};
					//------------recv
					size_t size = recv(fd, buf, BUF_SIZE, 0);
					//size = read(clientfd, buf, BUF_SIZE);
					if (size > 0)
					{
						PRINT("recv from clientfd:%d, msg:%s\n", fd, buf);
					}
				}
			}
		}
	}

	PRINT("end\n");
}
```

**<font style="color:rgb(25, 27, 31);">⑵启动多个客户端进行测试，</font>**<font style="color:rgb(25, 27, 31);">修改主程序，创建多个客户端线程，产生多个客户端，去连接同一个服务端，来测试epoll监听多个事件的功能。</font>

```cpp
int main()
{
	unlink(UNIX_TCP_SOCKET_ADDR);

	//创建一个服务端
	thread thServer(TcpServerThread);

	//创建多个客户端
	thread thClinet[CLIENT_NUM];
	for (int i=0; i<CLIENT_NUM; i++)
	{
		thClinet[i] = thread(TcpClientThread);
		sleep(1);
	}

	while(1)
	{
		sleep(5);
	}
}
```

<font style="color:rgb(25, 27, 31);">本例中，CLIENT_NUM为3，使用3个客户端来测试epoll功能。</font>

**<font style="color:rgb(25, 27, 31);">⑶测试结果，</font>**<font style="color:rgb(25, 27, 31);">在Ubuntu上编译运行，程序运行时的打印如下：</font>

```cpp
[TcpServerThread] create socketfd:3
[TcpServerThread] bind ok
[TcpClientThread] create socketfd:4
[TcpServerThread] listen ok
[TcpServerThread] epoll create fd:5
[EpollAddEvent] epollfd:5 add fd:3(event:1)
[TcpServerThread] epoll wait...
[TcpClientThread] create socketfd:6
[TcpClientThread] connect ok
[TcpServerThread] epoll wait done, num:1
[TcpServerThread] =====> accept new clientfd:7
[EpollAddEvent] epollfd:5 add fd:7(event:1)
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:1
[TcpServerThread] recv from clientfd:7, msg:helloTCP(fd:4)1
[TcpServerThread] epoll wait...
[TcpClientThread] create socketfd:8
[TcpClientThread] connect ok
[TcpServerThread] epoll wait done, num:2
[TcpServerThread] recv from clientfd:7, msg:helloTCP(fd:4)2
[TcpServerThread] =====> accept new clientfd:9
[EpollAddEvent] epollfd:5 add fd:9(event:1)
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:1
[TcpServerThread] recv from clientfd:9, msg:helloTCP(fd:6)3
[TcpServerThread] epoll wait...
[TcpClientThread] connect ok
[TcpServerThread] epoll wait done, num:3
[TcpServerThread] =====> accept new clientfd:10
[EpollAddEvent] epollfd:5 add fd:10(event:1)
[TcpServerThread] recv from clientfd:7, msg:helloTCP(fd:4)5
[TcpServerThread] recv from clientfd:9, msg:helloTCP(fd:6)6
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:1
[TcpServerThread] recv from clientfd:10, msg:helloTCP(fd:8)4
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:3
[TcpServerThread] recv from clientfd:10, msg:helloTCP(fd:8)7
[TcpServerThread] recv from clientfd:7, msg:helloTCP(fd:4)8
[TcpServerThread] recv from clientfd:9, msg:helloTCP(fd:6)9
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:1
[TcpServerThread] recv from clientfd:7, msg:helloTCP(fd:4)10
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:2
[TcpServerThread] recv from clientfd:9, msg:helloTCP(fd:6)12
[TcpServerThread] recv from clientfd:10, msg:helloTCP(fd:8)11
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:3
[TcpServerThread] recv from clientfd:10, msg:helloTCP(fd:8)14
[TcpServerThread] recv from clientfd:7, msg:helloTCP(fd:4)13
[TcpServerThread] recv from clientfd:9, msg:helloTCP(fd:6)15
[TcpServerThread] epoll wait...
[TcpServerThread] epoll wait done, num:3
[TcpServerThread] recv from clientfd:10, msg:helloTCP(fd:8)16
[TcpServerThread] recv from clientfd:7, msg:helloTCP(fd:4)17
[TcpServerThread] recv from clientfd:9, msg:helloTCP(fd:6)18
[TcpServerThread] epoll wait...
```

**<font style="color:rgb(25, 27, 31);">对结果标注一下，更容易理解程序运行过程：</font>**

![](/images/c1b6169b2c8e962109c3e879fad9a8c2.jpeg)

<font style="color:rgb(25, 27, 31);">可以看到，服务端依次接受了3个客户端的连接请求，然后可以接收3个客户端发来的数据。</font>  


> 来自: [一文搞懂epoll：高效I/O多路复用的核心技术](https://mp.weixin.qq.com/s/qEWE2pRHRG403lr-yqaKPQ)
>

