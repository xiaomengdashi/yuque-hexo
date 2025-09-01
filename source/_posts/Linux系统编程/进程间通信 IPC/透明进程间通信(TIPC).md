---
title: 透明进程间通信(TIPC)
date: '2025-01-05 17:42:47'
updated: '2025-01-05 17:46:43'
---
**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC（透明进程间通信）</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);"> 是一种特别的进程间通信机制，它最初由 </font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">Linux</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);"> 开发，并且特别设计用于分布式环境中的进程间通信。TIPC 允许不同节点之间的进程进行通信，甚至在跨越不同物理机器时，也能像在同一台机器上的进程通信一样，保持透明性和一致性。</font>

**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC 的特点</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：</font>

+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">分布式系统</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：TIPC 主要设计目标是提供一种高效的、透明的分布式进程间通信机制。它的主要优势在于，应用程序不需要了解物理网络的存在，就可以进行跨机器的进程间通信。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">透明性</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：TIPC 让应用程序能够像在本地进行 IPC 一样，在分布式环境中进行通信，而不需要直接处理底层的网络和分布式问题。开发者只需要像使用常规 IPC 一样使用 TIPC。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">高效性</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：TIPC 提供了高效的消息传递和低延迟通信，适用于高性能计算和大规模分布式系统。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">消息传递模型</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：TIPC 使用消息队列和广播机制，它支持可靠的、面向消息的通信。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">通信地址</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：TIPC 使用一个类似于网络协议的地址结构，支持节点间的动态连接与断开，自动选择最佳路径。</font>

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC 是通过专门的协议和机制来实现的，通常用于需要高效、低延迟通信的分布式系统。</font>



### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">IPC 与 TIPC 的主要区别</font>
| <font style="color:rgb(36, 41, 47);">特性</font> | <font style="color:rgb(36, 41, 47);">IPC</font> | <font style="color:rgb(36, 41, 47);">TIPC</font> |
| --- | --- | --- |
| **<font style="color:rgb(36, 41, 47);">应用场景</font>** | <font style="color:rgb(36, 41, 47);">主要用于同一台机器的进程间通信。</font> | <font style="color:rgb(36, 41, 47);">主要用于分布式环境，跨多个物理节点的进程间通信。</font> |
| **<font style="color:rgb(36, 41, 47);">透明性</font>** | <font style="color:rgb(36, 41, 47);">不透明，通信通常依赖操作系统提供的低级 API。</font> | <font style="color:rgb(36, 41, 47);">透明，应用程序无需关心底层网络和物理节点，像使用本地 IPC 一样。</font> |
| **<font style="color:rgb(36, 41, 47);">通信方式</font>** | <font style="color:rgb(36, 41, 47);">多种方式：管道、消息队列、共享内存、信号、套接字等。</font> | <font style="color:rgb(36, 41, 47);">基于专门的协议，使用消息传递机制，支持广播、点对点通信。</font> |
| **<font style="color:rgb(36, 41, 47);">性能</font>** | <font style="color:rgb(36, 41, 47);">性能受限于操作系统和硬件配置。</font> | <font style="color:rgb(36, 41, 47);">优化了分布式系统中的通信，低延迟、高效。</font> |
| **<font style="color:rgb(36, 41, 47);">配置和使用复杂度</font>** | <font style="color:rgb(36, 41, 47);">相对简单，基于操作系统提供的常见 API。</font> | <font style="color:rgb(36, 41, 47);">相对复杂，配置需要支持 TIPC 协议的网络和服务。</font> |
| **<font style="color:rgb(36, 41, 47);">可靠性</font>** | <font style="color:rgb(36, 41, 47);">可靠性由具体的 IPC 机制（如信号、消息队列）决定。</font> | <font style="color:rgb(36, 41, 47);">提供内置的可靠性机制，支持消息传递的可靠性。</font> |


### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">总结</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">IPC</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);"> </font><font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">是一种通用的进程间通信方式，适用于同一台机器上的进程，常见的方式有管道、消息队列、共享内存等。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);"> </font><font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">是专为分布式环境设计的进程间通信机制，能够透明地在不同节点的进程之间传递消息，支持高效的、低延迟的通信。</font>

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC 适合大规模分布式系统中的高性能通信，而 IPC 更适合单机环境下的进程间通信。如果你的应用需要跨多台机器进行进程间通信，TIPC 是一个理想的选择。</font>

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);"></font>

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC（Transparent Inter-Process Communication）是 Linux 系统上用于分布式通信的协议。它允许不同节点上的进程之间进行高效、透明的消息传递。为了实现一个简单的 TIPC 示例，我们需要使用 TIPC 协议栈，通常通过 </font>`<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">libtipc</font>`<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);"> 来编程。</font>

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">在此代码示例中，我将展示如何在 TIPC 上创建一个简单的客户端和服务器模型：</font>

### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">安装 TIPC 支持（如果没有安装）</font>
<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">在某些 Linux 发行版中，TIPC 支持可能默认没有安装。你可以通过以下命令安装它：</font>

```bash
apt-get install libtipc-dev
```

### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">示例：TIPC 客户端和服务器</font>
#### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC 服务器</font>
<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">这个服务器将监听一个端口，等待来自客户端的消息，并返回一个响应。</font>

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <tipc/socket.h>
#include <sys/types.h>
#include <sys/socket.h>

#define SERVER_PORT 4321
#define SERVER_NODE 1  // 假设服务器节点为1

int main() {
    int sockfd;
    struct sockaddr_tipc addr;
    char buffer[256];
    int len;

    // 创建一个 TIPC 套接字
    sockfd = socket(AF_TIPC, SOCK_RDM, 0);
    if (sockfd < 0) {
        perror("socket");
        exit(1);
    }

    // 配置服务器地址
    memset(&addr, 0, sizeof(addr));
    addr.family = AF_TIPC;
    addr.addrtype = TIPC_ADDR_NAMESEQ;
    addr.addrtype.name.seq = SERVER_PORT;
    addr.addrname.name.node = SERVER_NODE;

    // 绑定套接字到指定的地址
    if (bind(sockfd, (struct sockaddr *)&addr, sizeof(addr)) < 0) {
        perror("bind");
        close(sockfd);
        exit(1);
    }

    printf("Server listening on node %d, port %d...\n", SERVER_NODE, SERVER_PORT);

    // 接收客户端消息
    while (1) {
        len = recv(sockfd, buffer, sizeof(buffer), 0);
        if (len < 0) {
            perror("recv");
            close(sockfd);
            exit(1);
        }

        buffer[len] = '\0'; // 确保消息以 null 结尾
        printf("Received message: %s\n", buffer);

        // 向客户端发送响应
        if (send(sockfd, "Message received!", 18, 0) < 0) {
            perror("send");
            close(sockfd);
            exit(1);
        }
    }

    close(sockfd);
    return 0;
}
```

#### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC 客户端</font>
<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">客户端将连接到服务器，发送一个消息，并接收服务器的响应。</font>

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <tipc/socket.h>
#include <sys/types.h>
#include <sys/socket.h>

#define SERVER_PORT 4321
#define SERVER_NODE 1  // 假设服务器节点为1

int main() {
    int sockfd;
    struct sockaddr_tipc addr;
    char buffer[256];
    int len;

    // 创建一个 TIPC 套接字
    sockfd = socket(AF_TIPC, SOCK_RDM, 0);
    if (sockfd < 0) {
        perror("socket");
        exit(1);
    }

    // 配置服务器地址
    memset(&addr, 0, sizeof(addr));
    addr.family = AF_TIPC;
    addr.addrtype = TIPC_ADDR_NAMESEQ;
    addr.addrtype.name.seq = SERVER_PORT;
    addr.addrname.name.node = SERVER_NODE;

    // 连接到服务器
    if (connect(sockfd, (struct sockaddr *)&addr, sizeof(addr)) < 0) {
        perror("connect");
        close(sockfd);
        exit(1);
    }

    // 向服务器发送消息
    strcpy(buffer, "Hello from client!");
    if (send(sockfd, buffer, strlen(buffer), 0) < 0) {
        perror("send");
        close(sockfd);
        exit(1);
    }

    // 接收服务器响应
    len = recv(sockfd, buffer, sizeof(buffer), 0);
    if (len < 0) {
        perror("recv");
        close(sockfd);
        exit(1);
    }

    buffer[len] = '\0'; // 确保消息以 null 结尾
    printf("Received from server: %s\n", buffer);

    close(sockfd);
    return 0;
}
```

### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">编译和运行代码</font>
<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">首先，确保你的 Linux 系统支持 TIPC，并且已经加载了 TIPC 协议模块。</font>

1. **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">编译 TIPC 服务器和客户端：</font>**

```c
gcc -o tipc_server tipc_server.c -ltipc
gcc -o tipc_client tipc_client.c -ltipc
```

1. **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">运行 TIPC 服务器：</font>**

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">在一台机器上运行 TIPC 服务器，使用指定的节点号和端口。</font>

```c
sudo ./tipc_server
```

1. **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">运行 TIPC 客户端：</font>**

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">在同一台机器或另一台机器上，运行 TIPC 客户端并发送消息。</font>

```c
./tipc_client
```

### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">重要注意事项</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">节点编号</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：TIPC 使用节点编号来区分不同机器。上面的示例中假设节点号是 </font>`<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">1</font>`<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">。如果你在多台机器上运行，你需要确保每台机器上的 TIPC 节点号不同。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">TIPC 配置</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：你需要确保内核支持 TIPC，并且已经启用 TIPC 协议。你可以检查和启用 TIPC 协议：</font>

```c
sudo modprobe tipc
```

+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">网络环境</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：如果你的 TIPC 客户端和服务器不在同一台机器上，你还需要配置网络，使它们能够相互通信。通常，这需要配置 TIPC 协议的多播组等高级配置。</font>

<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">这只是一个简单的 TIPC 示例，展示了如何创建基本的客户端和服务器之间的通信。在实际应用中，可能还需要处理更多的错误检查、连接管理和消息处理等细节。</font>

