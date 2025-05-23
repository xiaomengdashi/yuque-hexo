---
title: 高性能网络编程之 Reactor 网络模型
date: '2025-01-19 17:02:10'
updated: '2025-01-19 17:58:15'
---
#### 文章目录
+ [前言](https://blog.csdn.net/ldw201510803006/article/details/124365838#_5)
+ [一、网络IO进化史](https://blog.csdn.net/ldw201510803006/article/details/124365838#IO_18)
    - [1.非阻塞IO](https://blog.csdn.net/ldw201510803006/article/details/124365838#1IO_34)
    - [2.事件驱动？](https://blog.csdn.net/ldw201510803006/article/details/124365838#2_55)
    - [3.Reactor 模型？](https://blog.csdn.net/ldw201510803006/article/details/124365838#3Reactor__74)
+ [二、大话 Reactor 模型](https://blog.csdn.net/ldw201510803006/article/details/124365838#_Reactor__86)
    - [1.单线程模型](https://blog.csdn.net/ldw201510803006/article/details/124365838#1_108)
    - [2.多线程模型](https://blog.csdn.net/ldw201510803006/article/details/124365838#2_127)
    - [3.主从多线程模型](https://blog.csdn.net/ldw201510803006/article/details/124365838#3_142)
+ [总结](https://blog.csdn.net/ldw201510803006/article/details/124365838#_163)

---

## 前言
网络框架的设计离不开 I/O 线程模型，线程模型的优劣直接决定了系统的吞吐量、可扩展性、安全性等。

目前主流的网络框架，在网络 IO 处理层面几乎都采用了「I/O [多路复用](https://so.csdn.net/so/search?q=%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8&spm=1001.2101.3001.7020)方案」，这是服务端应对高并发的性能利器。

进一步看，当上升到整个网络模块时，另一个常常听说的模式出现了 ---- 「[Reactor](https://so.csdn.net/so/search?q=Reactor&spm=1001.2101.3001.7020) 模式」，也叫反应器模式，本质是一个事件转发器，是网络模块核心中枢，负责将读写事件分发给对应的读写事件处理者。

大名鼎鼎的 Java 并发包作者 Doug [Lea](https://so.csdn.net/so/search?q=Lea&spm=1001.2101.3001.7020)，在 Scalable I/O in Java 一文中阐述了服务端开发中 I/O 模型的演进过程。netty 中三种 reactor 线程模型也来源于这篇经典文章。

通常情况下，reactor 模式的网络 IO 层会采用经典的 IO 多路复用方案，强强联手，妥妥的高性能的代名词、扛把子！！！是众多开源项目普遍的解决方案。

---

## 一、网络IO进化史
我们先来看看，一个网络请求在服务端经历了哪些阶段：

![](/images/d2306bbb3d0a1402538fa77b05892e4c.png)  
可以看到，网络请求先后经历 服务器网卡、内核、连接建立、数据读取、业务处理、数据写回等一系列过程。

其中，连接建立(accept)、数据读取(read)、数据写回(write)等操作都需要操作系统内核提供的系统调用，最终由内核与网卡进行数据交互，这些 IO 调用消耗一般是比较高的，比如 IO 等待、数据传输等。

最初的处理方式是，每个连接都用独立的一个线程来处理这一系列的操作，即 建立连接、数据读写、业务逻辑处理；这样一来最大的弊端在于，N 个连接就需要 N 个线程资源，消耗巨大。

所以，在网络模型演化过程中，不断的对这几个阶段进行拆分，比如，将建立连接、数据读写、业务逻辑处理等关键阶段分开处理。这样一来，每个阶段都可以考虑使用单线程或者线程池来处理，极大的节约线程资源，又能获得超高性能。

在深入了解之前，非常建议你先了解[四种常见的IO模型](https://blog.csdn.net/ldw201510803006/article/details/119767467?spm=1001.2014.3001.5501)以及[内核提供的select/poll/epoll模型](https://blog.csdn.net/ldw201510803006/article/details/124223086?spm=1001.2014.3001.5501)

### 1.非阻塞IO
**阻塞IO**：通常是用户态线程通过系统调用阻塞读取网卡传递的数据，我们知道，在 TCP 三次握手建立连接之后，真正等待数据的到来需要一定时间；

这个时候，在该模式下用户线程会一直阻塞等待网卡数据准备就绪，直到完成数据读写完成；可以看到，用户线程大部分都在等待 IO 事件就绪，造成资源的急剧浪费。

![](/images/538066e409eb4fb1f8cf48c21cc332dd.png)

**非阻塞IO** ：与阻塞 IO 相反，如果数据未就绪会直接返回，应用层轮询读取/查询，直到成功读取数据。

![](/images/906f2ae4edb9e897867e35bfc7f10417.png)

**I/O多路复用：** 是非阻塞IO的一种特例，也是目前最经典、最常用的高性能IO模型。其具体处理方式是：先查询 IO 事件是否准备就绪，当 IO 事件准备就绪了，则会真正的通过系统调用实现数据读写；

查询操作，不管是否数据准备就绪都会立即返回，即非阻塞；因此，通常情况下，会通过轮训来不断监听 IO 事件是否准备就绪；因为操作是非阻塞的，这个过程中通常只需及少量线程（一般一个线程即可）来处理这个轮训操作，极大的解决阻塞模式下 IO 枯竭问题。

这种一个线程就可以监听所有网络连接的 IO 事件是否准备就绪的模式，就是大名鼎鼎的`IO多路复用`。

![](/images/47f1c218e3e6942da9ff078e621e7e57.png)

### 2.事件驱动？
前面我们提到：将一个正常的请求分成多段来看待，每一段都可以分别进行优化（看场景需要）。

经典的一种切分方法是将「连接」和「业务线程」分开处理，当「连接层」有事件触发时提交给「业务线程」，避免了业务线程因「网络数据处于准备中」导致的长时间等待问题，节省线程资源，这就是大名鼎鼎的`事件驱动模型`。

![](/images/da0f1ec84ba88ada92d247df1477a7f6.png)

事件驱动的核心是，以事件为连接点，当有IO事件准备就绪时，以事件的形式通知相关线程进行数据读写，进而业务线程可以直接处理这些数据，这一过程的后续操作方，都是被动接收通知，看起来有点像回调操作；

这种模式下，IO 读写线程、业务线程工作时，必有数据可操作执行，不会在 IO 等待上浪费资源，这便是`事件驱动`的核心思想。

举个简单例子，10个士兵接到命令，在接下来将执行秘密任务，但具体时间待定；一种方式时，这10个士兵自己掌握主动权，隔一段时间就会自己询问将军是否准备执行任务，这种模式比较低下，因为士兵需要花很多精力自己去确认任务执行时间，同时也会耽搁自己的训练时间。

另一种方式为，士兵接到即将执行秘密任务的通知后，会自己做好准备随时执行，在最终执行命名没下达之前，会继续自己的日常训练；等需要执行任务时，将军会立刻通知士兵们立即行动；很显然，这种模式，士兵们的时间资源并没有浪费。这便是事件驱动的优势所在。

好了，到此相信你已经明白了什么是事件驱动了。`Reactor 模型`的核心便是`事件驱动`，同时，为了让其网络 IO 层拥有了高性能的能力，一般会采用 IO 多路复用处理方案。

### 3.Reactor 模型？
有了上文的基础，理解 Reactor 模型就很容易了，其核心是事件驱动，换句话说，`Reactor 是事件驱动模型的一种实现`。

你可以理解为 Reactor 模型中的反应器角色，类似于事件转发器 ----承接连接建立、IO处理以及事件分发，示例图下：

![](/images/4da0774879baebf519edb46a4a92189b.png)

Reactor 模式由 `Reactor 线程`、`Handlers 处理器`两大角色组成，两大角色的职责分别如下：

+ Reactor 线程的职责：主要负责连接建立、监听IO事件、IO事件读写以及将事件分发到Handlers 处理器。
+ Handlers 处理器（业务处理）的职责：非阻塞的执行业务处理逻辑。

## 二、大话 Reactor 模型
好了，到了这里便开始介绍我们的主角：Reactor 模型。

前面我们提到，网络模型演化过程中，将建立连接、IO等待/读写以及事件转发等操作分阶段处理，然后可以对不同阶段采用相应的优化策略来提高性能。  
也正是如此，Reactor 模型在不同阶段都有相关的优化策略，常见的有以下三种方式呈现：

+ 单线程模型
+ 多线程模型
+ 主从多线程模型

从某些方面来说，其实主要有`单线程`和`多线程`两种模型；其中，多线程模型就包含了多线程模型(Woker线程池)和主从多线程模型。多线程 Reactor 的演进分为两个方面：

+ 升级 Handler。既要使用多线程，又要尽可能高效率，则可以考虑使用线程池。
+ 升级 Reactor。可以考虑引入多个Selector（选择器），提升选择大量通道的能力。

**不过，为了方便表述，还是细分为三种模型来进行表述。**

我们常说的`多线程模型`一般是指在 Worker 端使用多线程来提升业务上的处理能力。

而`主从多线程模型`，将 `建立连接` 和 `IO事件监听/读写以及事件分发` 两部分用不同的线程处理，这样各司其职，能有效利用系统多核资源；同时为提高事件处理的效率，通常可以使用线程池来处理 `IO事件监听/读写以及事件分发`这部分操作。

另外，主从多线程模型通常情况下，Worker 端也会采用线程池来处理业务。这样一看，这三种 Reactor 模型其实是层层递进，不断的提升系统的吞吐量。当然，这一系列变换使用都需要结合实际场景考虑，但终究万变不离其宗。

接下来我们将详细分析这几种模型，继续往下看。

### 1.单线程模型
模型图如下：

![](/images/522da3f0b909b5af4a2938be4679d043.png)  
上图描述了 Reactor 的单线程模型结构，在 Reactor 单线程模型中，所有 I/O 操作（包括连接建立、数据读写、事件分发等）、业务处理，都是由一个线程完成的。单线程模型逻辑简单，缺陷也十分明显：

+ 一个线程支持处理的连接数非常有限，CPU 很容易打满，性能方面有明显瓶颈；
+ 当多个事件被同时触发时，只要有一个事件没有处理完，其他后面的事件就无法执行，这就会造成消息积压及请求超时；
+ 线程在处理 I/O 事件时，Select 无法同时处理连接建立、事件分发等操作；
+ 如果 I/O 线程一直处于满负荷状态，很可能造成服务端节点不可用。

在单线程 Reactor 模式中，Reactor 和 Handler 都在同一条线程中执行。这样，带来了一个问题：当其中某个 Handler 阻塞时，会导致其他所有的 Handler 都得不到执行。

在这种场景下，被阻塞的 Handler 不仅仅负责输入和输出处理的传输处理器，还包括负责新连接监听的 Acceptor 处理器，可能导致服务器无响应。这是一个非常严重的缺陷，导致单线程反应器模型在生产场景中使用得比较少。

### 2.多线程模型
![](/images/4300d689a566e701d91094374baf57dd.png)  
由于单线程模型有性能方面的瓶颈，多线程模型作为解决方案就应运而生了。

Reactor 多线程模型将`业务逻辑`交给多个线程进行处理。除此之外，多线程模型其他的操作与单线程模型是类似的，比如连接建立、IO事件读写以及事件分发等都是由一个线程来完成。

当客户端有数据发送至服务端时，Select 会监听到可读事件，数据读取完毕后提交到业务线程池中并发处理。

一般的请求中，耗时最长的一般是业务处理，所以用一个线程池（worker 线程池）来处理业务操作，在性能上的提升也是非常可观的。

当然，这种模型也有明显缺点，连接建立、IO 事件读取以及事件分发完全有单线程处理；比如当某个连接通过系统调用正在读取数据，此时相对于其他事件来说，完全是阻塞状态，新连接无法处理、其他连接的 IO、查询 IO 读写以及事件分发都无法完成。

对于像 Nginx、Netty 这种对高性能、高并发要求极高的网络框架，这种模式便显得有些吃力了。因为，无法及时处理新连接、就绪的 IO 事件以及事件转发等。

接下来，我们看看主从多线程模型是如何解决这个问题的。

### 3.主从多线程模型
![](/images/fac3c72a1122ef92e34b77674946bc9e.png)

主从 Reactor 模型要想解决这个问题，同样需要从我们前面介绍的几个阶段中的某一个或者多个进行优化处理。

**既然是主从模式，那谁主谁从呢？哪个模块使用主从呢？**

在多线程模型中，我们提到，其主要缺陷在于同一时间无法处理**大量新连接**、**IO就绪事件**；因此，将主从模式应用到这一块，就可以解决这个问题。

主从 Reactor 模式中，分为了主 Reactor 和 从 Reactor，分别处理 `新建立的连接`、`IO读写事件/事件分发`。

+ 一来，主 Reactor 可以解决同一时间大量新连接，将其注册到从 Reactor 上进行IO事件监听处理
+ 二来，IO事件监听相对新连接处理更加耗时，此处我们可以考虑使用线程池来处理。这样能充分利用多核 CPU 的特性，能使更多就绪的IO事件及时处理。

简言之，主从多线程模型由多个 Reactor 线程组成，每个 Reactor 线程都有独立的 Selector 对象。MainReactor 仅负责处理客户端连接的 Accept 事件，连接建立成功后将新创建的连接对象注册至 SubReactor。再由 SubReactor 分配线程池中的 I/O 线程与其连接绑定，它将负责连接生命周期内所有的 I/O 事件。

在海量客户端并发请求的场景下，主从多线程模式甚至可以适当增加 SubReactor 线程的数量，从而利用多核能力提升系统的吞吐量。

---

## 总结
相信到这里，你已经很清楚 Reactor 模型扮演什么样的角色了。其核心是围绕`事件驱动`模型

+ 一方面监听并处理IO事件。
+ 另一方面将这些处理好的事件分发业务线程处理。

而几种 Reactor 模型的演进，不过是在这几个阶段中优化升级、层层递进。

我们再重点回顾多线程模式（`多线程模式`和`主从多线程模式`），其工作模式大致如下：

+ 将负责数据传输处理的 IOHandler 处理器的执行放入独立的线程池中。这样，业务处理线程与负责新连接监听的反应器线程就能相互隔离，避免服务器的连接监听受到阻塞。
+ 如果服务器为多核的 CPU，可以将反应器线程拆分为多个子反应器（SubReactor）线程；同时，引入多个选择器，并且为每一个SubReactor引入一个线程，一个线程负责一个选择器的事件轮询。这样充分释放了系统资源的能力，也大大提升了反应器管理大量连接或者监听大量传输通道的能力。

Reactor（反应器）模式是高性能网络编程在设计和架构层面的基础模式，算是基础的原理性知识。只有彻底了解反应器的原理，才能真正构建好高性能的网络应用、轻松地学习和掌握高并发通信服务器与框架（如Netty框架、Nginx服务器）。

参考文献： 

> + [四种常见的IO模型](https://blog.csdn.net/ldw201510803006/article/details/119767467?spm=1001.2014.3001.5501)
> + [内核提供的select/poll/epoll模型](https://blog.csdn.net/ldw201510803006/article/details/124223086?spm=1001.2014.3001.5501)
> + [doug lea - scalable io in java](http://www.4e00.com/blog/java/2019/03/15/doug-lea-scalable-io-in-java.html)
> + [Understanding Reactor Pattern for Highly Scalable I/O Bound Web Server](https://tianpan.co/blog/2015-01-13-understanding-reactor-pattern-for-highly-scalable-i-o-bound-web-server)
> + <<Java高并发核心编程（卷1）：NIO、Netty、Redis、ZooKeeper>> 尼恩
>

  


> 来自: [高性能网络编程之 Reactor 网络模型（彻底搞懂）_reactor网络模型-CSDN博客](https://blog.csdn.net/ldw201510803006/article/details/124365838)
>

