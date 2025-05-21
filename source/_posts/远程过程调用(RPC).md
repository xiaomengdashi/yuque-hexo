---
title: 远程过程调用(RPC)
date: '2025-01-05 17:44:29'
updated: '2025-01-05 17:45:40'
---
**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">RPC</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);"> 是一种通信机制，它允许程序通过网络调用位于远程机器上的过程（函数或方法）。RPC 使得远程调用像调用本地函数一样简单，开发者无需关注底层的网络细节。RPC 是分布式计算中常见的通信方式，广泛用于客户端与服务器之间的通信。</font>

#### <font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">特点：</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">作用范围</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：适用于分布式系统，支持跨机器通信。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">方式</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：通过网络协议（如 HTTP、gRPC、Thrift、JSON-RPC 等）调用远程机器上的服务。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">透明性</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：使远程调用透明化，调用方像调用本地方法一样调用远程方法，底层的网络和序列化过程对用户透明。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">性能</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：一般来说，RPC 的性能受到网络延迟的影响，虽然有一些优化技术（如 gRPC）可以提供低延迟的通信。</font>
+ **<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">配置</font>**<font style="color:rgb(36, 41, 47);background-color:rgb(244, 246, 248);">：RPC 通常涉及服务端和客户端的配置，包括接口定义、序列化协议、网络设置等。</font>

  
 

