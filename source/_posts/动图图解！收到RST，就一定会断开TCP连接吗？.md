---
title: 动图图解！收到RST，就一定会断开TCP连接吗？
date: '2024-12-04 22:24:18'
updated: '2024-12-23 23:19:59'
---
CppGuide _2024年11月14日 14:30_ _上海_

<font style="color:rgba(0, 0, 0, 0.3);">以下文章来源于小白debug ，作者小白</font>

想必大家已经知道我的niao性，搞个标题，就是不喜欢立马回答。

就是要搞一大堆**<font style="color:rgb(41, 98, 255);">原理性</font>**的东西，再回答标题的问题。

说这个是因为我这次会把问题的答案就放到开头吗？

不！

**<font style="color:rgb(41, 98, 255);">我就不！</font>**

但是大家可以直接根据目录看自己感兴趣的部分。

之所以要先铺垫一些原理，还是希望大家能先看些基础的，再慢慢循序渐进，**<font style="color:rgb(41, 98, 255);">这样有利于建立知识体系</font>**。多一点上下文，少一点`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">gap</font>`。

好了，进入正题。

下面是这篇文章的目录。

![](/images/de7672e264c9ca5ddfdc52654c1c5c89.png)

<font style="color:rgb(153, 153, 153);">收到RST就一定会断开连接吗</font>

  


### <font style="color:rgb(255, 255, 255);background-color:rgb(21, 101, 192);">什么是RST</font>
我们都知道TCP正常情况下断开连接是用四次挥手，那是**<font style="color:rgb(41, 98, 255);">正常时候</font>**的优雅做法。

但**<font style="color:rgb(41, 98, 255);">异常情况</font>**下，收发双方都不一定正常，连挥手这件事本身都可能做不到，所以就需要一个机制去强行关闭连接。

**<font style="color:rgb(41, 98, 255);">RST</font>** 就是用于这种情况，一般用来**<font style="color:rgb(41, 98, 255);">异常地</font>**关闭一个连接。它是一个TCP包头中的**<font style="color:rgb(41, 98, 255);">标志位</font>**。

**<font style="color:rgb(41, 98, 255);">正常情况下</font>**，不管是**<font style="color:rgb(41, 98, 255);">发出</font>**，还是**<font style="color:rgb(41, 98, 255);">收到</font>**置了这个标志位的数据包，相应的内存、端口等连接资源都会被释放。从效果上来看就是TCP连接被关闭了。

而接收到 RST的一方，一般会看到一个 `<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">connection reset</font>` 或  `<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">connection refused</font>` 的报错。

![](/images/dfe6c4438a53bf587967d54b42e5a204.png)

<font style="color:rgb(153, 153, 153);">TCP报头RST位</font>

  


### <font style="color:rgb(255, 255, 255);background-color:rgb(21, 101, 192);">怎么知道收到RST了？</font>
我们知道**<font style="color:rgb(41, 98, 255);">内核</font>**跟**<font style="color:rgb(41, 98, 255);">应用层</font>**是分开的两层，网络通信功能在内核，我们的客户端或服务端属于应用层。应用层**<font style="color:rgb(41, 98, 255);">只能</font>**通过 `<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">send/recv</font>` 与内核交互，才能感知到内核是不是收到了`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`。

当本端收到远端发来的`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`后，**<font style="color:rgb(41, 98, 255);">内核</font>**已经认为此链接已经关闭。

此时如果本端**<font style="color:rgb(41, 98, 255);">应用层</font>**尝试去执行 **<font style="color:rgb(41, 98, 255);">读数据</font>**操作，比如`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">recv</font>`，应用层就会收到 **<font style="color:rgb(41, 98, 255);">Connection reset by peer</font>** 的报错，意思是**<font style="color:rgb(41, 98, 255);">远端已经关闭连接</font>**。

![](/images/bb4107377a711a1a8b86401720811e57.gif)

<font style="color:rgb(153, 153, 153);">ResetByPeer</font>

如果本端**<font style="color:rgb(41, 98, 255);">应用层</font>**尝试去执行**<font style="color:rgb(41, 98, 255);">写数据</font>**操作，比如`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">send</font>`，那么应用层就会收到 **<font style="color:rgb(41, 98, 255);">Broken pipe</font>** 的报错，意思是发送通道已经坏了。

![](/images/5e904def72805985e153a876346a1b31.gif)

<font style="color:rgb(153, 153, 153);">BrokenPipe</font>

这两个是开发过程中很经常遇到的报错，感觉大家可以**<font style="color:rgb(41, 98, 255);">把这篇文章放进收藏夹吃灰</font>**了，等遇到这个问题了，再打开来擦擦灰，说不定对你会有帮助。

  


### <font style="color:rgb(255, 255, 255);background-color:rgb(21, 101, 192);">出现RST的场景有哪些</font>
**<font style="color:rgb(41, 98, 255);">RST</font>**一般出现于异常情况，归类为 **<font style="color:rgb(41, 98, 255);">对端的端口不可用</font>** 和 **<font style="color:rgb(41, 98, 255);">socket提前关闭</font>**。

  


#### 端口不可用
端口不可用分为两种情况。要么是这个端口从来就没有"可用"过，比如根本就没监听**<font style="color:rgb(41, 98, 255);">（listen）</font>**过；要么就是曾经"可用"，但现在"不可用"了，比如服务**<font style="color:rgb(41, 98, 255);">突然崩</font>**了。

##### 端口未监听
![](/images/0becd3dd69ee6e02d29ddf2e5249fbe2.png)

<font style="color:rgb(153, 153, 153);">TCP连接未监听的端口</font>

服务端`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">listen</font>` 方法会创建一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`放入到全局的`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">哈希表</font>`中。

此时客户端发起一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">connect</font>`请求到服务端。服务端在收到数据包之后，第一时间会根据IP和端口从哈希表里去获取`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`。

![](/images/2addfbde216a5e50dc58f577f7a2fd17.png)

<font style="color:rgb(153, 153, 153);">全局hash表</font>

如果服务端执行过`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">listen</font>`，就能从`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">全局哈希表</font>`里拿到`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`。

但如果服务端没有执行过`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">listen</font>`，那`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">哈希表</font>`里也就不会有对应的`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`，结果当然是拿不到。此时，**<font style="color:rgb(41, 98, 255);">正常情况下</font>**服务端会发`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`给客户端。

  


###### 端口未监听就一定会发RST吗？
**<font style="color:rgb(41, 98, 255);">不一定</font>**。上面提到，发RST的前提是**<font style="color:rgb(41, 98, 255);">正常情况下</font>**，我们看下源码。

```plain
// net/ipv4/tcp_ipv4.c  
// 代码经过删减
int tcp_v4_rcv(struct sk_buff *skb)
{
    // 根据ip、端口等信息 获取sock。
    sk = __inet_lookup_skb(&tcp_hashinfo, skb, th->source, th->dest);
    if (!sk)
        goto no_tcp_socket;

no_tcp_socket:
    // 检查数据包有没有出错
    if (skb->len < (th->doff << 2) || tcp_checksum_complete(skb)) {
        // 错误记录
    } else {
        // 发送RST
        tcp_v4_send_reset(NULL, skb);
    }
}
```

内核在收到数据后会从物理层、数据链路层、网络层、传输层、应用层，一层一层往上传递。到传输层的时候，根据当前数据包的协议是**<font style="color:rgb(41, 98, 255);">TCP还是UDP</font>**走不一样的函数方法。可以简单认为，**<font style="color:rgb(41, 98, 255);">TCP</font>**数据包都会走到 `<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">tcp_v4_rcv()</font>`。这个方法会从`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">全局哈希表</font>`里获取 `<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`，如果此时服务端没有`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">listen()</font>`过 , 那肯定获取不了`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`，会跳转到`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">no_tcp_socket</font>`的逻辑。

注意这里会先走一个 `<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">tcp_checksum_complete()</font>`，目的是看看数据包的**<font style="color:rgb(41, 98, 255);">校验和(Checksum)</font>**是否合法。

**<font style="color:rgb(41, 98, 255);background-color:rgb(242, 247, 251);">校验和</font>**<font style="background-color:rgb(242, 247, 251);">可以验证数据从端到端的传输中是否出现异常。由发送端计算，然后由接收端验证。计算范围覆盖数据包里的TCP首部和TCP数据。</font>

如果在发送端到接收端传输过程中，数据发生**<font style="color:rgb(41, 98, 255);">任何改动</font>**，比如被第三方篡改，那么接收方能检测到校验和有差错，此时TCP段会被直接丢弃。如果校验和没问题，那才会发RST。  


所以，**<font style="color:rgb(41, 98, 255);">只有在数据包没问题的情况下，比如校验和没问题，才会发RST包给对端。</font>**

  


###### 为什么数据包异常的情况下，不发RST？
一个数据包连校验都不能通过，那这个包，**<font style="color:rgb(41, 98, 255);">多半有问题</font>**。

![](/images/a67fcd6990cc9558f322ee5e3b83343b.jpeg)

有可能是在发送的过程中被篡改了，又或者，可能只是一个**<font style="color:rgb(41, 98, 255);">胡乱伪造</font>**的数据包。

**<font style="color:rgb(41, 98, 255);">五层网络，不管是哪一层</font>**，只要遇到了这种数据，**<font style="color:rgb(41, 98, 255);">推荐的做法都是默默扔掉</font>**，**<font style="color:rgb(41, 98, 255);">而不是</font>**去回复一个消息告诉对方数据有问题。

如果对方用的是TCP，是可靠传输协议，发现很久没有`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">ACK</font>`响应，自己就会重传。

如果对方用的是UDP，说明发送端已经接受了“不可靠会丢包”的事实，那丢了就丢了。

因此，数据包异常的情况下，默默扔掉，不发`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`，非常合理。

  


![](/images/4ad9446cf986b6aacf92f87718f3ab3d.jpeg)

还是不能理解？那我**<font style="color:rgb(41, 98, 255);">再举个例子</font>**。

正常人喷你，他说话**<font style="color:rgb(41, 98, 255);">条理清晰，主谓宾分明</font>**。此时你喷回去，那你是个充满热情，正直，富有判断力的好人。

而此时一个憨憨也想喷你，但他**<font style="color:rgb(41, 98, 255);">思维混乱，连话都说不清楚，一直阿巴阿巴</font>**的，你虽然听不懂，但**<font style="color:rgb(41, 98, 255);">大受震撼</font>**，此时你会？

+ A：跟他激情互喷
+ B：不跟他一般见识，就当没听过

一般来说**<font style="color:rgb(41, 98, 255);">最优选择是B</font>**，毕竟你理他，他反而来劲。

这下，应该就懂了。

  


##### 程序启动了但是崩了
端口不可用的场景里，除了端口未监听以外，还有可能是从前监听了，但服务端机器上做监听操作的**<font style="color:rgb(41, 98, 255);">应用程序突然崩了</font>**，此时客户端还像往常一样正常发送消息，服务器内核协议栈收到消息后，则会**<font style="color:rgb(41, 98, 255);">回一个RST</font>**。在开发过程中，**<font style="color:rgb(41, 98, 255);">这种情况是最常见的</font>**。

比如你的服务端应用程序里，弄了个**<font style="color:rgb(41, 98, 255);">空指针</font>**，或者**<font style="color:rgb(41, 98, 255);">数组越界</font>**啥的，程序立马就崩了。

![](/images/a2032b91e51fb048663430826f627e87.png)

<font style="color:rgb(153, 153, 153);">TCP监听了但崩了</font>

这种情况跟**<font style="color:rgb(41, 98, 255);">端口未监听</font>**本质上类似，在服务端的应用程序**<font style="color:rgb(41, 98, 255);">崩溃后</font>**，原来监听的端口资源就被释放了，从效果上来看，类似于处于`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">CLOSED</font>`状态。

此时服务端又收到了客户端发来的消息，内核协议栈会根据**<font style="color:rgb(41, 98, 255);">IP端口</font>**，从全局哈希表里查找`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`，结果当然是拿不到对应的`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">sock</font>`数据，于是走了跟上面**<font style="color:rgb(41, 98, 255);">"端口未监听"</font>**时一样的逻辑，回了个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`。客户端在收到RST后也**<font style="color:rgb(41, 98, 255);">释放了sock资源</font>**，从效果上来看，就是**<font style="color:rgb(41, 98, 255);">连接断了</font>**。

###### RST和502的关系
上面这张图，服务端程序崩溃后，如果客户端再有数据发送，会出现`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`。但如果在客户端和服务端中间再加一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">nginx</font>`，就像下图一样。

![](/images/2b36c61f138c3d8b146398ce6feb367f.png)

<font style="color:rgb(153, 153, 153);">RST与502</font>

`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">nginx</font>`会作为客户端和服务端之间的"中间人角色"，负责**<font style="color:rgb(41, 98, 255);">转发</font>**请求和响应结果。但当服务端程序**<font style="color:rgb(41, 98, 255);">崩溃</font>**，比如出现**<font style="color:rgb(41, 98, 255);">野指针或者OOM</font>**的问题，那转发到服务器的请求，必然得不到响应，后端服务端还会返回一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`给`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">nginx</font>`。`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">nginx</font>`在收到这个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`后会断开与服务端的连接，同时返回客户端一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">502</font>`错误码。

所以，出现502问题，一般情况下都是因为后端程序崩了，基于这一点假设，去看看监控是不是发生了OOM或者日志是否有空指针等报错信息。

  


#### socket提前关闭
这种情况分为**<font style="color:rgb(41, 98, 255);">本端</font>**提前关闭，和**<font style="color:rgb(41, 98, 255);">远端</font>**提前关闭。

##### 本端提前关闭
如果本端`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">socket</font>`接收缓冲区**<font style="color:rgb(41, 98, 255);">还有数据未读</font>**，此时**<font style="color:rgb(41, 98, 255);">提前</font>**`**<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">close()</font>**`**<font style="color:rgb(41, 98, 255);"> </font>****<font style="color:rgb(41, 98, 255);">socket</font>**。那么本端会先把接收缓冲区的数据清空，然后给远端发一个RST。

![](/images/a0e152765920e6b07958aee314d18573.gif)

<font style="color:rgb(153, 153, 153);">recvbuf非空</font>

  


##### 远端提前关闭
远端已经`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">close()</font>`了`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">socket</font>`，此时本端还尝试发数据给远端。那么远端就会回一个RST。

![](/images/64acde216718a84a120095dbcc4a33ed.png)

<font style="color:rgb(153, 153, 153);">close()触发TCP四次挥手</font>

大家知道，TCP是**<font style="color:rgb(41, 98, 255);">全双工通信</font>**，意思是发送数据的同时，还可以接收数据。

`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">Close()</font>`的含义是，此时要同时**<font style="color:rgb(41, 98, 255);">关闭发送和接收</font>**消息的功能。

客户端执行`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">close()</font>`， 正常情况下，会发出**<font style="color:rgb(41, 98, 255);">第一次</font>**挥手FIN，然后服务端回**<font style="color:rgb(41, 98, 255);">第二次</font>**挥手ACK。如果在**<font style="color:rgb(41, 98, 255);">第二次和第三次挥手之间</font>**，如果服务方还尝试传数据给客户端，那么客户端不仅不收这个消息，还会发一个RST消息到服务端。直接结束掉这次连接。

  


### <font style="color:rgb(255, 255, 255);background-color:rgb(21, 101, 192);">对方没收到RST，会怎么样？</font>
我们知道TCP是可靠传输，意味着本端发一个数据，远端在收到这个数据后就会回一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">ACK</font>`，意思是"我收到这个包了"。

**<font style="color:rgb(41, 98, 255);">而RST，不需要ACK确认包</font>**。

因为`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">RST</font>`本来就是设计来处理异常情况的，既然都已经在异常情况下了，还指望对方能正常回你一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">ACK</font>`吗？**<font style="color:rgb(41, 98, 255);">可以幻想，不要妄想。</font>**

但**<font style="color:rgb(41, 98, 255);">问题又来了</font>**，网络环境这么复杂，丢包也是分分钟的事情，既然RST包不需要ACK来确认，那万一对方就是没收到RST，会怎么样？

![](/images/c483f6b3887dfb1d8a87802285935449.png)

<font style="color:rgb(153, 153, 153);">RST丢失</font>

RST丢了，问题不大。比方说上图服务端，发了RST之后，服务端就认为连接不可用了。

如果客户端之前**<font style="color:rgb(41, 98, 255);">发送了数据</font>**，一直没等到这个数据的确认ACK，就会重发，重发的时候，自然就会触发一个新的RST包。

而如果客户端之前**<font style="color:rgb(41, 98, 255);">没有发数据</font>**，但服务端的RST丢了，TCP有个keepalive机制，会定期发送探活包，这种数据包到了服务端，也会重新触发一个RST。

![](/images/1087a5ffd47a90745ded90d900f21b24.png)

<font style="color:rgb(153, 153, 153);">RST丢失后keepalive</font>

  


### <font style="color:rgb(255, 255, 255);background-color:rgb(21, 101, 192);">收到RST就一定会断开连接吗?</font>
先说结论，**<font style="color:rgb(41, 98, 255);">不一定会断开</font>**。我们看下源码。

```plain
// net/ipv4/tcp_input.c
static bool tcp_validate_incoming()
{
    // 获取sock
    struct tcp_sock *tp = tcp_sk(sk);

    // step 1：先判断seq是否合法（是否在合法接收窗口范围内）
    if (!tcp_sequence(tp, TCP_SKB_CB(skb)->seq, TCP_SKB_CB(skb)->end_seq)) {
        goto discard;
    }

    // step 2：执行收到 RST 后该干的事情
    if (th->rst) {
        if (TCP_SKB_CB(skb)->seq == tp->rcv_nxt)
            tcp_reset(sk);
        else
            tcp_send_challenge_ack(sk);
        goto discard;
    }
}
```

收到RST包，第一步会通过`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">tcp_sequence</font>`先看下这个seq是否合法，其实主要是看下这个seq是否在合法**<font style="color:rgb(41, 98, 255);">接收窗口</font>**范围内。**<font style="color:rgb(41, 98, 255);">如果不在范围内，这个RST包就会被丢弃。</font>**

至于接收窗口是个啥，我们先看下面这个图。

![](/images/58ba7a693d71389b5b46e8565d0316ae.png)

<font style="color:rgb(153, 153, 153);">接收窗口</font>

这里**<font style="color:rgb(41, 98, 255);">黄色的部分</font>**，就是指接收窗口，只要RST包的seq不在这个窗口范围内，那就会被丢弃。

  


#### 为什么要校验是否在窗口范围内
正常情况下客户端服务端双方可以通过RST来断开连接。假设不做seq校验，如果这时候有不怀好意的第三方介入，构造了一个RST包，且在TCP和IP等报头都填上客户端的信息，发到服务端，那么服务端就会断开这个连接。同理也可以伪造服务端的包发给客户端。这就叫**RST攻击**。

![](/images/8b727a72644d04326f275e83701590fc.png)

<font style="color:rgb(153, 153, 153);">RST攻击</font>

受到RST攻击时，从现象上看，客户端老感觉服务端崩了，这非常影响用户体验。

如果这是个游戏，我相信多崩几次，第二天大家就不来玩了。

实际消息发送过程中，接收窗口是不断移动的，seq也是在飞快的变动中，此时第三方是**<font style="color:rgb(41, 98, 255);">比较难</font>**构造出合法seq的RST包的，那么通过这个seq校验，就可以拦下了很多不合法的消息。

  


#### 加了窗口校验就不能用RST攻击了吗
**<font style="color:rgb(41, 98, 255);">不是，只是增加了攻击的成本。</font>**但如果想搞，还是可搞的。

以下是**<font style="color:rgb(41, 98, 255);">面向监狱编程</font>**的环节。

希望大家只**<font style="color:rgb(41, 98, 255);">了解原理</font>**就好了，**<font style="color:rgb(41, 98, 255);">不建议使用</font>**。

相信大家都不喜欢穿着蓝白条纹的衣服，拍**<font style="color:rgb(41, 98, 255);">纯狱风</font>**的照片。

从上面可以知道，不是每一个RST包都会导致连接重置的，要求是这个RST包的seq要在窗口范围内，所以，问题就变成了，**<font style="color:rgb(41, 98, 255);">我们怎么样才能构造出合法的seq</font>**。

  


##### 盲猜seq
窗口数值seq本质上只是个uint32类型。

```plain
struct tcp_skb_cb {
    __u32       seq;        /* Starting sequence number */
}
```

如果在这个范围内疯狂猜测seq数值，并构造对应的包，发到目的机器，虽然概率低，但是总是能被试出来，从而实现**<font style="color:rgb(41, 98, 255);">RST攻击</font>**。这种乱棍打死老师傅的方式，就是所谓的**<font style="color:rgb(41, 98, 255);">合法窗口盲打（blind in-window attacks）</font>**。

觉得这种方式比较**<font style="color:rgb(41, 98, 255);">笨</font>**？那有没有聪明点的方式，还真有，但是在这之前需要先看下面的这个问题。

  


##### 已连接状态下收到第一次握手包会怎么样？
我们需要了解一个问题，比如服务端在已连接（`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">ESTABLISHED</font>`）状态下，如果收到客户端发来的第一次握手包（`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">SYN</font>`），会怎么样？

以前我以为**<font style="color:rgb(41, 98, 255);">服务单会认为客户端憨憨了，直接RST连接。</font>**

**<font style="color:rgb(41, 98, 255);">但实际，并不是</font>**。

```plain
static bool tcp_validate_incoming()
{
    struct tcp_sock *tp = tcp_sk(sk);

    /* 判断seq是否在合法窗口内 */
    if (!tcp_sequence(tp, TCP_SKB_CB(skb)->seq, TCP_SKB_CB(skb)->end_seq)) {
        if (!th->rst) {
            // 收到一个不在合法窗口内的SYN包
            if (th->syn)
                goto syn_challenge;
        }
    }

    /* 
     * RFC 5691 4.2 : 发送 challenge ack
     */
    if (th->syn) {
syn_challenge:
        tcp_send_challenge_ack(sk);
    }
}
```

当客户端发出一个不在合法窗口内的SYN包的时候，服务端会发一个带有正确的seq数据ACK包出来，这个ACK包叫 `<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">challenge ack</font>`。

![](/images/0fd4de80f9c396006d995bd11b33b240.png)

<font style="color:rgb(153, 153, 153);">challenge ack抓包</font>

上图是抓包的结果，用`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">scapy</font>`随便伪造一个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">seq=5</font>`的包发到服务端（`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">端口9090</font>`），服务端回复一个带有正确seq值的`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">challenge ack</font>`包给客户端（`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">端口8888</font>`）。

  


##### 利用challenge ack获取seq
上面提到的**<font style="color:rgb(41, 98, 255);">这个challenge ack ，仿佛为盲猜seq的老哥们打开了一个新世界。</font>**

在获得这个`<font style="color:rgb(255, 82, 82);background-color:rgb(248, 248, 248);">challenge ack</font>`后，攻击程序就可以以ack值为基础，在一定范围内设置seq，这样造成RST攻击的几率就大大增加了。

![](/images/1780717c2ccd5df6598209feba624bad.png)

<font style="color:rgb(153, 153, 153);">利用ChallengeACK的RST攻击</font>

  


### <font style="color:rgb(255, 255, 255);background-color:rgb(21, 101, 192);">总结</font>
+ RST其实是TCP包头里的一个标志位，目的是为了在**<font style="color:rgb(41, 98, 255);">异常情况</font>**下关闭连接。
+ 内核收到RST后，应用层只能通过调用读/写操作来感知，此时会对应获得 **<font style="color:rgb(41, 98, 255);">Connection reset by peer</font>** 和**<font style="color:rgb(41, 98, 255);">Broken pipe</font>** 报错。
+ 发出RST后不需要得到对方的ACK确认包，因此RST丢失后对方不能立刻感知，但是通过下一次**<font style="color:rgb(41, 98, 255);">重传</font>**数据或keepalive**<font style="color:rgb(41, 98, 255);">心跳包</font>**可以导致RST重传。
+ **<font style="color:rgb(41, 98, 255);">收到RST包，不一定会断开连接，seq不在合法窗口范围内的数据包会被默默丢弃。</font>**通过构造合法窗口范围内seq，可以造成RST攻击，**<font style="color:rgb(41, 98, 255);">这一点大家了解就好，千万别学！</font>**

  


### <font style="color:rgb(255, 255, 255);background-color:rgb(21, 101, 192);">参考资料</font>
TCP旁路攻击分析与重现 - https://www.cxyzjd.com/article/qq_27446553/52416369<font style="color:rgb(255, 76, 65);"></font>

<font style="color:rgba(0, 0, 0, 0.9);">  
</font>

