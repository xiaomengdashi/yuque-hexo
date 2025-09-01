---
title: 高级 C++ 开发综合岗面试题，能挑战否？
date: '2025-01-11 18:22:57'
updated: '2025-01-11 18:22:58'
---
> <font style="color:black;background-color:rgb(251, 249, 253);">说明：</font>
>
> <font style="color:black;background-color:rgb(251, 249, 253);">1. 此试题是小方的训练营的期中考试题；</font>
>
> <font style="color:black;background-color:rgb(251, 249, 253);">2. 每题 5 分，满 80 分及格（工作标准）。</font>
>

![](/images/cbff824d191939e24d7f1b9993a248dd.png)

<font style="color:rgb(1, 1, 1);">1. 在Visual Studio中解决方案a.sln下有三个工程b.vcxproj, c.vcxproj, d.vcxproj，目录结构如下：</font>

```plain
workspace/a.sln
workspace/b.vcxproj
workspace/T/c.vcxproj
workspace/T/d.vcxproj
```

<font style="color:black;">现在d.vcxproj需要引入some.lib，我们将some.lib放入目录workspace/中，现在给d.vcxproj配置lib目录，则新增的lib路径是：</font>

```plain
选项1: ./
选项2: ../
选项3: ../../
选项4: ./T/
```

<font style="color:black;">2. 高大强同学刚入职了一个新公司，接触到某 Linux C++ 后端项目 BigTea，代码可以正常编译调试，高大强可以使用 gdb 调试法快速搞清楚程序在技术上的主要执行流，他该怎么做，请贴出这个过程中使用到的主要 GDB 调试命令。</font>

<font style="color:black;">3. A 公司使用 B 的 C++ 库开发自己的 Windows 和 Linux 项目，A 公司使用非静态库隐式加载的方式使用 B 公司的库，则 B 公司一般需要提供哪几种扩展名类型的文件？</font>

<font style="color:black;">4. 罗舜宇在公司负责一个 Linux C++ 后台项目，最近测试同学祝英台在自己的功能环境（该环境存在 gdb 软件，可以用于调试）测试出一个程序应答不符合预期的行为，且程序重启后，再次刻意复现几率不大，但祝英台测试过程中偶尔再次复现，且罗舜宇同学在自己的开发环境始终无法复现该 bug，那么罗舜宇同学应该如何定位该 bug？请列出该过程中可能会用到的 gdb 命令。</font>

<font style="color:black;">5. 刘双江同学所在公司最近接到一个新的合作需求，开发一款安全助手，该软件会向服务端发起TCP联网请求，并主动上报用户电脑的软件使用频率，由于客户的电脑存在安全策略，仅开放了若干个著名端口和4000～4500范围的端口，请问刘双江同学编码时，该如何解决端口限制问题？</font>

<font style="color:black;">6. 贾宝玉同学公司最近引进了一批新的嵌入式设备，该设备上只能运行 C 程序，且不存在判断内置字节序函数，贾宝玉同学决定自己实现一个，请问贾宝玉该怎么实现？请写出实现代码。</font>

<font style="color:black;">7. 乔峰同学入职新公司，负责 Linux 下某个接收消息推送的 SDK 开发，SDK 连上服务端后，服务端会立即主动推送一条欢迎消息，乔峰同学选择连接逻辑使用非阻塞的connect方法，请写出伪代码。</font>

<font style="color:black;">8. 朱正纯同学最近负责接手开发一款 Windows C++ 安全软件，该软件每次只允许用户启动一个实例，请写出实现代码。</font>

<font style="color:black;">9. 请说出在 Windows 完成端口模型中，AcceptEx 和 WSASend、WSARecv 三个函数的作用，完成端口模型中，当存在多个客户端连接时，给某个客户端发完业务数据后，如何区分是哪个客户端的哪次操作？</font>

<font style="color:black;">10. 莫荣福同学最近负责实现公司的协议解析逻辑，他实现的伪代码如下：</font>

```plain
//socket读事件触发收到数据后，调用下面函数
void XXSession::onRead()
{
    //1. 判断收取的数据是否够一个包头大小，不够，退出，足够，执行下一步
    //2. 从接收缓冲区中取出包头数据，解析得到包体大小，判断缓冲区中是否够一个包体大小，不够，退出，足够，执行下一步
    //3. 解析包体得到业务数据，进行业务逻辑处理
    //4. 业务逻辑处理完后，退出
}
```

<font style="color:black;">上述逻辑是否有问题，如果有问题，存在什么问题？</font>

<font style="color:black;">11. 鲁智深同学最近接手一个项目的开发，负责与客户端同学联调，在过程中发现客户端传过来的报文格式不符合约定，由于客户端代码已经封板，无法给加新的日志代码打印报文，该报文是纯文本协议，由于是第一次合作，客户端同学也怀疑是服务端收包逻辑有问题，导致报文不完整，鲁智深同学如何快速地定位出到底是哪一方处理报文有问题？</font>

<font style="color:black;">12. 小王同学最近在开发一个相机协议采集程序，拿到相机采集的图片数据后，开启200个线程，每个线程函数的逻辑如下：</font>

```plain
1. 每200ms中收到数据；
2. 创建socket；
3. 发起连接；
4. 连接成功后发送数据（数据较大）一直到成功或者出错；
5. 调用recv函数等待接收对端响应，收到响应，关闭连接。
```

<font style="color:black;">请问，上述代码是否存在问题？</font>

<font style="color:black;">13. 某同学开发了一个 Linux Server 程序，该程序使用了 epoll 模型的边缘触发模式，该服务使用的协议固定10k长，该同学在 socket 读事件出发后，每次固定收满 5k 大小数据，收数据的函数才返回。请问该逻辑是否存在问题？该同学发数据时，如果数据发不出去，将数据存在发送缓冲区的首部，然后注册监听写事件，当写事件触发时，继续发数据，数据发完之后，也不移除对写事件的监听，请问这样是否有问题。</font>

<font style="color:black;">14. 请使用 select 函数模拟一个 sleep 函数，写出实现代码。</font>

<font style="color:black;">15. 假设要开发一个 Linux C++ Server，请写出你对 SIGPIPE 信号的处理方法。</font>

<font style="color:black;">16. 某同学要一次性创建 10 路连接，在连接期间，需要执行另外一段逻辑用于连接成功后的发送数据的准备，请设计一个合理的程序结构。</font>

<font style="color:black;">17. 请设计一个 HttpClient 和 HttpServer，阐述程序的基本结构和数据流向。</font>

<font style="color:black;">18. 某个 HTTP 客户端发送给 HTTPServer 使用 chunk 技术传输，请写出 HTTPServer 端 chunk 处理逻辑。</font>

<font style="color:black;">19. 小明的程序使用 Linux C/C++ 开发，在预发布环境出现了死锁，该环境存在 gdb 工具，但是不可以对该程序进行调试，请问小明应该如何定位该死锁问题。</font>

<font style="color:black;">20. 小方公司正在开发一款即时通讯软件，服务端需要：同一个账号在不同设备登录后会将前一个账号踢下线然后关闭连接。请写出伪代码逻辑流程。</font>

<font style="color:rgb(51, 51, 51);">所有问题答案，会在训练营中讲解，训练营报名点击</font>[这里](http://mp.weixin.qq.com/s?__biz=Mzk0MjUwNDE2OA==&mid=2247499222&idx=2&sn=d44789c463a8362d2ae1548dd8f76d00&chksm=c2c09c51f5b7154783abe1adc7c91b9bcfcf498dcc0c46537a9b977e1edfa95241ec30c555be&scene=21#wechat_redirect)<font style="color:rgb(51, 51, 51);">。</font>

  
  


> 来自: [高级 C++ 开发综合岗面试题，能挑战否？](https://mp.weixin.qq.com/s?__biz=Mzk0MjUwNDE2OA==&mid=2247499559&idx=1&sn=5f60c7cfa07c6e6a52c3d2cf8dcbcffe&chksm=c2c09ea0f5b717b6449581cf1565deccdce295b9d142b5b048166265408d5a74e3dd3b5f0559&scene=21#wechat_redirect)
>

