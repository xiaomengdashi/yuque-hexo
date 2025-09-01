---
title: 【C/C++】 秋招常考面试题最全总结（让你有一种相见恨晚的感觉）_c++面试题 由浅及深-CSDN博客
date: '2025-02-16 21:05:29'
updated: '2025-02-16 21:05:30'
---
**目录**

[1.C++程序编译链接过程](https://xas-sunny.blog.csdn.net/article/details/140565534#1.C%2B%2B%E7%A8%8B%E5%BA%8F%E7%BC%96%E8%AF%91%E9%93%BE%E6%8E%A5%E8%BF%87%E7%A8%8B)

[2.浅拷贝和move有区别吗](https://xas-sunny.blog.csdn.net/article/details/140565534#t0)

[3.深拷贝和浅拷贝的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t1)

[4.空类的大小](https://xas-sunny.blog.csdn.net/article/details/140565534#t2)

[5.类的继承有几种方式，区别是什么？](https://xas-sunny.blog.csdn.net/article/details/140565534#t3)

[六、extern 关键字的作用](https://xas-sunny.blog.csdn.net/article/details/140565534#t4)

[七、static关键字的作用](https://xas-sunny.blog.csdn.net/article/details/140565534#t5)

[八、指针和引用的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t6)

[九、C++内存分配方式](https://xas-sunny.blog.csdn.net/article/details/140565534#t7)

[十、结构体对齐原则](https://xas-sunny.blog.csdn.net/article/details/140565534#t8)

[十一、数据结构中，有了二叉树，为什么要有平衡二叉树？](https://xas-sunny.blog.csdn.net/article/details/140565534#t9)

[平衡二叉树的调节机制](https://xas-sunny.blog.csdn.net/article/details/140565534#t10)

[生活中的类比：重新排队](https://xas-sunny.blog.csdn.net/article/details/140565534#t11)

[AVL 树 vs 红黑树](https://xas-sunny.blog.csdn.net/article/details/140565534#t12)

[现实生活中的类比：不同的队伍管理策略](https://xas-sunny.blog.csdn.net/article/details/140565534#t13)

[十二、哈希表插入元素一般有那几个重要的步骤](https://xas-sunny.blog.csdn.net/article/details/140565534#t14)

[1. 计算哈希值（Hash Function）](https://xas-sunny.blog.csdn.net/article/details/140565534#t15)

[2. 处理冲突（Handling Collisions）](https://xas-sunny.blog.csdn.net/article/details/140565534#t16)

[3. 插入元素（Insert Element）](https://xas-sunny.blog.csdn.net/article/details/140565534#t17)

[4. 检查装载因子（Check Load Factor）](https://xas-sunny.blog.csdn.net/article/details/140565534#t18)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t19)

[十三、但是红黑树 和 哈希表有什么区别呢](https://xas-sunny.blog.csdn.net/article/details/140565534#t20)

[数据结构类型](https://xas-sunny.blog.csdn.net/article/details/140565534#t21)

[2. 数据存储方式](https://xas-sunny.blog.csdn.net/article/details/140565534#t22)

[3. 查找时间复杂度](https://xas-sunny.blog.csdn.net/article/details/140565534#t23)

[4. 顺序性](https://xas-sunny.blog.csdn.net/article/details/140565534#t24)

[5. 内存使用](https://xas-sunny.blog.csdn.net/article/details/140565534#t25)

[6. 插入和删除操作](https://xas-sunny.blog.csdn.net/article/details/140565534#t26)

[7. 适用场景](https://xas-sunny.blog.csdn.net/article/details/140565534#t27)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t28)

[十四、不用第三个变量，如何交换ab](https://xas-sunny.blog.csdn.net/article/details/140565534#t29)

[十五、什么是锁？为什么需要锁？](https://xas-sunny.blog.csdn.net/article/details/140565534#t30)

[十六、什么时互斥锁](https://xas-sunny.blog.csdn.net/article/details/140565534#t31)

[互斥锁的作用](https://xas-sunny.blog.csdn.net/article/details/140565534#t32)

[十七、互斥锁和自旋锁有什么区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t33)

[1. 互斥锁](https://xas-sunny.blog.csdn.net/article/details/140565534#t34)

[2. 自旋锁](https://xas-sunny.blog.csdn.net/article/details/140565534#t35)

[主要区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t36)

[十八、什么是死锁，怎么构成死锁的](https://xas-sunny.blog.csdn.net/article/details/140565534#t37)

[死锁的构成](https://xas-sunny.blog.csdn.net/article/details/140565534#t38)

[例子 1: 共享停车位](https://xas-sunny.blog.csdn.net/article/details/140565534#%E4%BE%8B%E5%AD%90%201%3A%20%E5%85%B1%E4%BA%AB%E5%81%9C%E8%BD%A6%E4%BD%8D)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t39)

[十九、什么是单例模式](https://xas-sunny.blog.csdn.net/article/details/140565534#t40)

[单例模式的最重要目的](https://xas-sunny.blog.csdn.net/article/details/140565534#t41)

[类比解释：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E7%B1%BB%E6%AF%94%E8%A7%A3%E9%87%8A%EF%BC%9A)

[二十、Vector的底层原理和扩容机制](https://xas-sunny.blog.csdn.net/article/details/140565534#t42)

[二十一、红黑树 和 avl 的区别，插入多个数据，选择 avl，还是红黑树](https://xas-sunny.blog.csdn.net/article/details/140565534#t43)

[二十二、C++多态的实现原理](https://xas-sunny.blog.csdn.net/article/details/140565534#t44)

[1. 静态绑定 vs 动态绑定](https://xas-sunny.blog.csdn.net/article/details/140565534#t45)

[2. 虚函数与虚函数表（vtable）](https://xas-sunny.blog.csdn.net/article/details/140565534#t46)

[3. 多态实现的流程](https://xas-sunny.blog.csdn.net/article/details/140565534#t47)

[4. 运行时的表现](https://xas-sunny.blog.csdn.net/article/details/140565534#t48)

[5. 多态的条件](https://xas-sunny.blog.csdn.net/article/details/140565534#t49)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t50)

[二十三、多态分几种类型](https://xas-sunny.blog.csdn.net/article/details/140565534#t51)

[1. 编译时多态（静态多态）](https://xas-sunny.blog.csdn.net/article/details/140565534#t52)

[1.1 函数重载](https://xas-sunny.blog.csdn.net/article/details/140565534#1.1%20%E5%87%BD%E6%95%B0%E9%87%8D%E8%BD%BD)

[1.2 运算符重载](https://xas-sunny.blog.csdn.net/article/details/140565534#1.2%20%E8%BF%90%E7%AE%97%E7%AC%A6%E9%87%8D%E8%BD%BD)

[2. 运行时多态（动态多态）](https://xas-sunny.blog.csdn.net/article/details/140565534#t53)

[2.1 虚函数](https://xas-sunny.blog.csdn.net/article/details/140565534#2.1%20%E8%99%9A%E5%87%BD%E6%95%B0)

[2.2 抽象类与纯虚函数](https://xas-sunny.blog.csdn.net/article/details/140565534#2.2%20%E6%8A%BD%E8%B1%A1%E7%B1%BB%E4%B8%8E%E7%BA%AF%E8%99%9A%E5%87%BD%E6%95%B0)

[3. 总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t54)

[二十四、多态有那些特点](https://xas-sunny.blog.csdn.net/article/details/140565534#t55)

[1. 接口一致性](https://xas-sunny.blog.csdn.net/article/details/140565534#t56)

[2. 动态绑定](https://xas-sunny.blog.csdn.net/article/details/140565534#t57)

[3. 继承和虚函数](https://xas-sunny.blog.csdn.net/article/details/140565534#t58)

[5. 提高代码的复用性](https://xas-sunny.blog.csdn.net/article/details/140565534#t59)

[6. 抽象性](https://xas-sunny.blog.csdn.net/article/details/140565534#t60)

[7. 灵活性](https://xas-sunny.blog.csdn.net/article/details/140565534#t61)

[8. 支持运行时类型识别](https://xas-sunny.blog.csdn.net/article/details/140565534#t62)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t63)

[二十五、什么情况下会导致内存泄漏](https://xas-sunny.blog.csdn.net/article/details/140565534#t64)

[1. 动态分配的内存未释放](https://xas-sunny.blog.csdn.net/article/details/140565534#t65)

[2. 在异常处理时未释放内存](https://xas-sunny.blog.csdn.net/article/details/140565534#t66)

[3. 没有为类的析构函数释放内存](https://xas-sunny.blog.csdn.net/article/details/140565534#t67)

[4. 循环引用](https://xas-sunny.blog.csdn.net/article/details/140565534#t68)

[二十六、什么是智能指针，你在一般什么情况下使用智能指针](https://xas-sunny.blog.csdn.net/article/details/140565534#t69)

[智能指针的类型和使用场景](https://xas-sunny.blog.csdn.net/article/details/140565534#t70)

[1. std::unique_ptr（独占所有权）](https://xas-sunny.blog.csdn.net/article/details/140565534#1.%20std%3A%3Aunique_ptr%EF%BC%88%E7%8B%AC%E5%8D%A0%E6%89%80%E6%9C%89%E6%9D%83%EF%BC%89)

[2.std::shared_ptr（共享所有权）](https://xas-sunny.blog.csdn.net/article/details/140565534#2.std%3A%3Ashared_ptr%EF%BC%88%E5%85%B1%E4%BA%AB%E6%89%80%E6%9C%89%E6%9D%83%EF%BC%89)

[3. std::weak_ptr（弱引用）](https://xas-sunny.blog.csdn.net/article/details/140565534#3.%20std%3A%3Aweak_ptr%EF%BC%88%E5%BC%B1%E5%BC%95%E7%94%A8%EF%BC%89)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t71)

[一般情况下使用智能指针的场景：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E4%B8%80%E8%88%AC%E6%83%85%E5%86%B5%E4%B8%8B%E4%BD%BF%E7%94%A8%E6%99%BA%E8%83%BD%E6%8C%87%E9%92%88%E7%9A%84%E5%9C%BA%E6%99%AF%EF%BC%9A)

[二十七、智能指针的本质是什么？](https://xas-sunny.blog.csdn.net/article/details/140565534#t72)

[二十八、new 和 malloc 的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t73)

[二十九、什么是线程同步，为什么需要线程同步？](https://xas-sunny.blog.csdn.net/article/details/140565534#t74)

[为什么需要线程同步？](https://xas-sunny.blog.csdn.net/article/details/140565534#t75)

[1. 互斥锁（Mutex）](https://xas-sunny.blog.csdn.net/article/details/140565534#1.%20%E4%BA%92%E6%96%A5%E9%94%81%EF%BC%88Mutex%EF%BC%89)

[5. 信号量（Semaphore）](https://xas-sunny.blog.csdn.net/article/details/140565534#5.%20%E4%BF%A1%E5%8F%B7%E9%87%8F%EF%BC%88Semaphore%EF%BC%89)

[4. 自旋锁（Spinlock）](https://xas-sunny.blog.csdn.net/article/details/140565534#4.%20%E8%87%AA%E6%97%8B%E9%94%81%EF%BC%88Spinlock%EF%BC%89)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t76)

[三十、进程的通信有哪些？](https://xas-sunny.blog.csdn.net/article/details/140565534#t77)

[我最熟悉的一种：共享内存（Shared Memory）](https://xas-sunny.blog.csdn.net/article/details/140565534#t78)

[优点：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E4%BC%98%E7%82%B9%EF%BC%9A)

[缺点：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E7%BC%BA%E7%82%B9%EF%BC%9A)

[1. 管道（Pipe）](https://xas-sunny.blog.csdn.net/article/details/140565534#t79)

[2. 消息队列（Message Queue）](https://xas-sunny.blog.csdn.net/article/details/140565534#t80)

[3. 共享内存（Shared Memory）](https://xas-sunny.blog.csdn.net/article/details/140565534#t81)

[4. 信号量（Semaphore）](https://xas-sunny.blog.csdn.net/article/details/140565534#t82)

[6. 套接字（Socket）](https://xas-sunny.blog.csdn.net/article/details/140565534#t83)

[7. 文件（File）](https://xas-sunny.blog.csdn.net/article/details/140565534#t84)

[8. 信号量（Semaphore）和互斥锁（Mutex）](https://xas-sunny.blog.csdn.net/article/details/140565534#t85)

[总结：](https://xas-sunny.blog.csdn.net/article/details/140565534#t86)

[三十一、多进程和多线程的优缺点](https://xas-sunny.blog.csdn.net/article/details/140565534#t87)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t88)

[三十二、进程和线程的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t89)

[1. 定义](https://xas-sunny.blog.csdn.net/article/details/140565534#t90)

[总结：](https://xas-sunny.blog.csdn.net/article/details/140565534#t91)

[三十三、线程的通信方式有哪些，分别有什么不同](https://xas-sunny.blog.csdn.net/article/details/140565534#t92)

[总结：](https://xas-sunny.blog.csdn.net/article/details/140565534#t93)

[三十四、 Linux 如何查看进程状态](https://xas-sunny.blog.csdn.net/article/details/140565534#t94)

[1. ps 命令](https://xas-sunny.blog.csdn.net/article/details/140565534#t95)

[2. top 命令](https://xas-sunny.blog.csdn.net/article/details/140565534#t96)

[三十五、 linux 如何查看线程状态](https://xas-sunny.blog.csdn.net/article/details/140565534#t97)

[1. ps 命令](https://xas-sunny.blog.csdn.net/article/details/140565534#t98)

[三十六、 如何查看网络连接情况？](https://xas-sunny.blog.csdn.net/article/details/140565534#t99)

[1. netstat 命令（已被 ss 替代，但仍然常用）](https://xas-sunny.blog.csdn.net/article/details/140565534#t100)

[2. ss 命令（netstat 的现代替代工具）](https://xas-sunny.blog.csdn.net/article/details/140565534#t101)

[三十七、TCP 和 UDP 的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t102)

[1. 连接性](https://xas-sunny.blog.csdn.net/article/details/140565534#t103)

[2. 可靠性](https://xas-sunny.blog.csdn.net/article/details/140565534#t104)

[5. 数据传输效率](https://xas-sunny.blog.csdn.net/article/details/140565534#t105)

[8. 传输单位](https://xas-sunny.blog.csdn.net/article/details/140565534#t106)

[应用建议](https://xas-sunny.blog.csdn.net/article/details/140565534#t107)

[三十八、 左值和右值引用？](https://xas-sunny.blog.csdn.net/article/details/140565534#t108)

[1. 左值 (Lvalue) 与 右值 (Rvalue)](https://xas-sunny.blog.csdn.net/article/details/140565534#t109)

[5. 移动语义和右值引用的应用场景（重点！！）](https://xas-sunny.blog.csdn.net/article/details/140565534#t110)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t111)

[三十九、虚函数指针是什么时候初始化的](https://xas-sunny.blog.csdn.net/article/details/140565534#t112)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t113)

[四十、析构函数不是虚函数会有什么问题](https://xas-sunny.blog.csdn.net/article/details/140565534#t114)

[如何解决](https://xas-sunny.blog.csdn.net/article/details/140565534#t115)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t116)

[四十一、map使用find查找和[]查找的区别？为啥要有find](https://xas-sunny.blog.csdn.net/article/details/140565534#t117)

[1. 为什么需要 find？](https://xas-sunny.blog.csdn.net/article/details/140565534#t118)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t119)

[四十二、this 指针有什么作用，这个指针的值是从那里来的](https://xas-sunny.blog.csdn.net/article/details/140565534#t120)

[1.this 指针的作用](https://xas-sunny.blog.csdn.net/article/details/140565534#t121)

[3. this 指针的特性](https://xas-sunny.blog.csdn.net/article/details/140565534#t122)

[5. 总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t123)

[四十三、多线程同步方式。条件变量和信号量的区别 ？](https://xas-sunny.blog.csdn.net/article/details/140565534#t124)

[1. 条件变量 (Condition Variable)](https://xas-sunny.blog.csdn.net/article/details/140565534#t125)

[关键特性：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E5%85%B3%E9%94%AE%E7%89%B9%E6%80%A7%EF%BC%9A)

[使用场景：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF%EF%BC%9A)

[2. 信号量 (Semaphore)](https://xas-sunny.blog.csdn.net/article/details/140565534#t126)

[关键特性：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E5%85%B3%E9%94%AE%E7%89%B9%E6%80%A7%EF%BC%9A)

[使用场景：](https://xas-sunny.blog.csdn.net/article/details/140565534#%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF%EF%BC%9A)

[4. 总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t127)

[四十四、vector 扩容机制 。vector中删除元素是否会释放内存？](https://xas-sunny.blog.csdn.net/article/details/140565534#t128)

[1. 扩容机制原理](https://xas-sunny.blog.csdn.net/article/details/140565534#t129)

[四十五、vector在删除时，是否会产生迭代器失效](https://xas-sunny.blog.csdn.net/article/details/140565534#t130)

[1. 在循环中使用失效迭代器](https://xas-sunny.blog.csdn.net/article/details/140565534#t131)

[2. 扩容后迭代器失效](https://xas-sunny.blog.csdn.net/article/details/140565534#t132)

[四十六、使用 unique_ptr 和 裸指针 有什么却别，效率有什么区别？](https://xas-sunny.blog.csdn.net/article/details/140565534#t133)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t134)

[四十七、大小端有什么区别，分别有什么用](https://xas-sunny.blog.csdn.net/article/details/140565534#t135)

[1. 大端模式（Big-Endian）](https://xas-sunny.blog.csdn.net/article/details/140565534#t136)

[2. 小端模式（Little-Endian）](https://xas-sunny.blog.csdn.net/article/details/140565534#t137)

[5. 大小端在实践中的作用](https://xas-sunny.blog.csdn.net/article/details/140565534#t138)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t139)

[四十八、什么是B+树，它有什么特点？](https://xas-sunny.blog.csdn.net/article/details/140565534#t140)

[B+树的结构和特点](https://xas-sunny.blog.csdn.net/article/details/140565534#t141)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t142)

[四十九、虚函数实现机制](https://xas-sunny.blog.csdn.net/article/details/140565534#t143)

[五十、构造函数能否声明为虚函数？](https://xas-sunny.blog.csdn.net/article/details/140565534#t144)

[1. 构造函数](https://xas-sunny.blog.csdn.net/article/details/140565534#t145)

[2. 内联函数](https://xas-sunny.blog.csdn.net/article/details/140565534#t146)

[3. 静态函数](https://xas-sunny.blog.csdn.net/article/details/140565534#t147)

[4. 友元函数](https://xas-sunny.blog.csdn.net/article/details/140565534#t148)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t149)

[五十一、const 和 define 的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t150)

[五十二、 struct 和 class 的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t151)

[五十三、lambda 表达式的用法？](https://xas-sunny.blog.csdn.net/article/details/140565534#t152)

[1. 什么是 Lambda 表达式？](https://xas-sunny.blog.csdn.net/article/details/140565534#t153)

[2. Lambda 表达式的优点](https://xas-sunny.blog.csdn.net/article/details/140565534#t154)

[3. 应用场景](https://xas-sunny.blog.csdn.net/article/details/140565534#t155)

[4. 优化性能](https://xas-sunny.blog.csdn.net/article/details/140565534#t156)

[1. 对数组排序的例子](https://xas-sunny.blog.csdn.net/article/details/140565534#t157)

[2. 对字符串数组排序的例子](https://xas-sunny.blog.csdn.net/article/details/140565534#t158)

[五十四、什么是内联函数？](https://xas-sunny.blog.csdn.net/article/details/140565534#t159)

[2. 内联函数的优点](https://xas-sunny.blog.csdn.net/article/details/140565534#t160)

[3. 内联函数的限制](https://xas-sunny.blog.csdn.net/article/details/140565534#t161)

[五十五、虚函数表存在什么区域](https://xas-sunny.blog.csdn.net/article/details/140565534#t162)

[1. 虚函数表的存储区域](https://xas-sunny.blog.csdn.net/article/details/140565534#t163)

[2. 虚表指针（vptr）的位置](https://xas-sunny.blog.csdn.net/article/details/140565534#t164)

[五十六、动态连接和静态链接的区别](https://xas-sunny.blog.csdn.net/article/details/140565534#t165)

[1. 静态链接 (Static Linking)](https://xas-sunny.blog.csdn.net/article/details/140565534#t166)

[2. 动态链接 (Dynamic Linking)](https://xas-sunny.blog.csdn.net/article/details/140565534#t167)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t168)

[五十七、MySQL中什么是索引 ？](https://xas-sunny.blog.csdn.net/article/details/140565534#t169)

[建立索引的原则是什么？](https://xas-sunny.blog.csdn.net/article/details/140565534#t170)

[1. 选择性高的列优先创建索引](https://xas-sunny.blog.csdn.net/article/details/140565534#t171)

[2. 经常出现在 WHERE 子句中的列](https://xas-sunny.blog.csdn.net/article/details/140565534#t172)

[3. 在 JOIN 和 GROUP BY 中使用的列](https://xas-sunny.blog.csdn.net/article/details/140565534#t173)

[4. 避免为频繁更新的列创建索引](https://xas-sunny.blog.csdn.net/article/details/140565534#t174)

[5. 避免为小表创建过多索引](https://xas-sunny.blog.csdn.net/article/details/140565534#t175)

[五十八、什么是事务？](https://xas-sunny.blog.csdn.net/article/details/140565534#t176)

[1. 事务的四大特性（ACID）](https://xas-sunny.blog.csdn.net/article/details/140565534#t177)

[2. 事务的控制语句](https://xas-sunny.blog.csdn.net/article/details/140565534#t178)

[3. 事务的使用](https://xas-sunny.blog.csdn.net/article/details/140565534#t179)

[4. 事务隔离级别](https://xas-sunny.blog.csdn.net/article/details/140565534#t180)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t181)

[五十九、什么叫慢查询？](https://xas-sunny.blog.csdn.net/article/details/140565534#t182)

[六十、MySQL 是 主要通过什么数据结构实现？为什么用B+树？](https://xas-sunny.blog.csdn.net/article/details/140565534#t183)

[六十一、C++ 中 volatile 的作用](https://xas-sunny.blog.csdn.net/article/details/140565534#t184)

[输出结果的潜在问题](https://xas-sunny.blog.csdn.net/article/details/140565534#t185)

[volatile 的作用](https://xas-sunny.blog.csdn.net/article/details/140565534#t186)

[总结](https://xas-sunny.blog.csdn.net/article/details/140565534#t187)

---

## **<font style="color:#0d0016;">1.C++程序编译链接过程</font>**
> **<font style="color:#1c7331;">1. 预处理(Preprocessing): </font>**
>

+ **<font style="color:#be191c;background-color:#fef2f0;">预处理器处理源代码文件中的预处理指令，如 宏定义、条件编译指令 和 文件包含指令 等，生成预处理后的代码。</font>**

> **<font style="color:#1c7331;">2.编译(Compilation):</font>**
>

+ **<font style="color:#be191c;background-color:#fef2f0;">编译器将预处理后的代码 转换为汇编指令，并进行语法和语义分析，生成相应平台的汇编代码文件。</font>**

> **<font style="color:#1c7331;">3.汇编(Assembly):</font>**
>

+ **<font style="color:#be191c;background-color:#fef2f0;"> 汇编器将汇编代码转换为机器码，生成目标文件(通常是.obj或.o文件)。</font>**

> **<font style="color:#1c7331;background-color:#fef2f0;">4.链接(Linking)</font>**
>

+ **<font style="color:#be191c;">链接器将所有目标文件以及所需的库文件合并，解决代码中的外部引用，并生 成最终的可执行程序。</font>**

---

## **<font style="color:#0d0016;">2.浅拷贝和move有区别吗 </font>**
> **<font style="color:#be191c;background-color:#fef2f0;">浅拷贝: </font>**
>

+ 复制对象的指针或引用，新旧对象共享相同 的底层数据资源。

> **<font style="color:#be191c;background-color:#fef2f0;">移动操作(move):</font>**
>

+ 将资源从一个对象转移到另一个对象，原对 象资源被置为null或有效但未定义的状态，不再拥有资源。
+ 只有移动构造函数或移动赋值操作符才参与移动操作。

---

## **<font style="color:#0d0016;">3.深拷贝和浅拷贝的区别 </font>**
> **<font style="color:#be191c;background-color:#fef2f0;">浅拷贝:</font>**
>

+  仅复制对象的指针或引用，而不复制实际数据
+  新旧对象会共享同一份资源。
+  这意味着如果原对象和拷贝对象都包含指向相同数据的指针，修改其中一个对象的数据会影响到另一个对象。

> **<font style="color:#be191c;background-color:#fef2f0;">深拷贝:</font>**
>

+  复制对象的同时创建了一份数据的副本。
+  新旧对象各自拥有独立的资源，互不影响。
+  这意味着深拷贝会创建完全独立的对象，修改一个对象的数据不会影响到另一个对象。

> **<font style="color:#be191c;">默认的拷贝构造函数是深拷贝还是浅拷贝，为什么？</font>**
>

+ 默认的拷贝构造函数执行的是浅拷贝，因为他只复制对象的各个成员的值，对于指针成员，仅复制值得值而不复制指针所指向的数据。如果成员包含指向动态分配内存的指针，那么多个对象可能会指向相同的资源。

> **<font style="color:#0d0016;">怎么实现深拷贝 </font>**
>

+ 定义拷贝构造：在拷贝构造函数中，为对象中的每个需要拷贝的成员分配新的内存，并复制原对象相应的成员的内容

---

## **<font style="color:#0d0016;">4.空类的大小 </font>**
> **<font style="color:#be191c;background-color:#fef2f0;">空类的 大小为  1  字节 </font>**
>

---

## **<font style="color:#0d0016;">5.类的继承有几种方式，区别是什么？ </font>**
> **<font style="color:#956fe7;">C++中类的继承有三种方式 :</font>**
>

+ **<font style="color:#be191c;background-color:#fef2f0;">公有继承(public)：</font>**基类的公有成员和保护成员继承后在派生类中保持原有状态，基类的私有成员不能直接访问。
+ **<font style="color:#be191c;background-color:#fef2f0;">保护继承(protected)：</font>**基类的公有成员和保护成员继承后在派生类中变为保护成员。
+ **<font style="color:#be191c;">私有继承(private)：</font>**基类的公有成员和保护成员继承后在派生类中变为私有成员。

---

## **<font style="color:#0d0016;">六、extern 关键字的作用</font>**
> **<font style="color:#0d0016;">主要作用： </font>**
>

+ 允许在多个文件中访问同一个全局变量或函数 
+ 表明变量和函数的定义存在于其它文件中 

---

## **<font style="color:#0d0016;">七、static关键字的作用 </font>**
> **<font style="color:#0d0016;">在变量和函数中的作用：</font>**
>

1️⃣：在函数中声明变量时， **<font style="color:#be191c;background-color:#fef2f0;">static 关键字指定变量只初始化一次</font>**，并在之后调用该函数时保留其状态。  
2️⃣：在声明变量时，**<font style="color:#be191c;background-color:#fef2f0;">变量具有静态持续时间</font>**，并且除非您指定另一个值。  
3️⃣ ：**<font style="color:#0d0016;background-color:#f9eda6;">在全局和/或命名空间范围 (在单个文件范围内声明变量或函数时) static 关键字指定变量或函数为内部链接，即外部文件无法引用该变量或函数。</font>**  
4️⃣：static 关键字 **<font style="color:#0d0016;">没有赋值时，默认赋值为 0 </font>**

5️⃣：**<font style="color:#0d0016;background-color:#f9eda6;">static修饰局部变量时，会改变局部变量的存储位置</font>**，从而使得局部变量的生命周期变长。

> ** static**是 C/C++中的**关键字**之一，**是常见的函数与变量（C++中还包括类）的修饰符**，它常被用来**控制变量的存储方式和作用范围**。  
>

> 在C++类中的作用： 
>

+ 在类的成员函数前使用，静态成员变量属于类本身，而不是类的某个实例。换句话说，**<font style="color:#be191c;background-color:#fef2f0;">所有实例共享同一个静态成员变量，而不是每个实例各自拥有自己的副本。</font>**
+ 在类的成员函数前使用，**静态成员函数（static member function）** 的概念。**<font style="color:#be191c;background-color:#fef2f0;">静态成员函数属于类本身，而不是某个具体的对象实例。</font>**因此，你可以直接通过类名来调用静态成员函数，而不需要创建类的对象。静态成员函数可以在不创建对象实例的情况下直接调用，并且由于没有 `this` 指针，它们只能访问静态成员，而不能访问非静态成员。
+ 在局部变量前使用，表示该变量在函数调用前结束后不会被销毁，而是保持其值不变。
+ 在全局变量或者函数前使用，限制其作用范围，仅在定义的文件内，对其它文件不可见
+ 当类中声明了静态成员函数后，静态成员函数的定义必须在类的外部进行，无论该函数是否为inline函数
+ 静态成员函数可以被覆盖。即，派生类可以定义同名的静态成员函数。

---

## **<font style="color:#0d0016;">八、指针和引用的区别 </font>**
> 区别： 
>

+ 指针是一个存储变量地址的变量，而引用是一个变量的别名。
+ 指针可以被重新赋值以指向另一个不同的地址，但引用一旦绑定到一个对象，就不能改变指向另一个对象
+ 指针可以是nullptr或者指向任意地址的值，而引用必须进行初始化，且不能为nullptr
+ 指针使用* 操作符来访问目标变量值，引用直接像普通变量一样使用 

---

## <font style="color:#0d0016;">九、C++内存分配方式 </font>
> 分配方式：
>

+ 静态存储：编译时分配，如全局变量、静态变量
+ 自动存储：函数内部声明变量，如局部变量，随着函数调用创建和退出销毁
+ 动态存储：使用new 和 delete 进行手动分配和释放堆内存

---

## **<font style="color:#0d0016;">十、结构体对齐原则</font>**
> **<font style="color:#0d0016;">原则： </font>**
>

+ 第一个成员在与结构体变量偏移量为0的地址处
+ 其他成员变量要对齐到某个数字（对齐数）的整数倍的地址处
+ 对齐数 = 编译器默认的一个对齐数 与 该成员大小的较小值。
+ 【VS中默认的值为8、Linux环境默认不设对齐数（对齐数是结构体成员自身的大小）】
+ 结构体总大小为最大对齐数（每个成员变量都有一个对齐数）的整数倍
+ 如果嵌套了结构体的情况，嵌套的结构体对齐到自己的最大对齐数的整数倍处，结构体的整体大小就是所有最大对齐数（含嵌套结构体的对齐数）的整数倍

> 意义：
>

+ **性能优化**：内存对齐可以显著提高内存访问速度，因为数据对齐可以使处理器更加高效地访问内存。
+ **兼容性**：内存对齐可以确保程序在不同硬件架构上正确运行，避免未对齐的数据访问带来的问题。

> 结论： 
>

+ **<font style="color:#0d0016;">结构体内存对齐是一种用于</font>****<font style="color:#0d0016;background-color:#f9eda6;">优化数据访问的机制</font>****<font style="color:#0d0016;">，通过保证数据在内存中的存储方式符合处理器的要求，</font>****<font style="color:#0d0016;background-color:#f9eda6;">减少不必要的内存访问次数</font>****<font style="color:#0d0016;">，从而提高程序的性能和可靠性。这也是编译器在处理结构体时自动完成的一项重要优化。 </font>**

---

## **<font style="color:#0d0016;">十一、数据结构中，有了二叉树，为什么要有平衡二叉树？</font>**
> 在数据结构中，二叉树是一种常用的数据结构，**<font style="color:#be191c;background-color:#fef2f0;">具有树的基本特性和二叉结构的灵活性</font>**。然而，二叉树的结构并不一定总是平衡的，这可能导致某些操作的效率较低。**<font style="color:#be191c;background-color:#fef2f0;">为了提高二叉树在各种操作（如查找、插入、删除）中的性能</font>**，引入了平衡二叉树的概念。 
>

**<font style="color:#0d0016;background-color:#f9eda6;">二叉树的问题：</font>**

+ **最坏情况**：**<font style="color:#be191c;background-color:#fef2f0;">普通的二叉树在最坏情况下可能会退化成一条链表（例如，每次插入的元素都比当前节点大或小），</font>****<font style="color:#0d0016;">导致查找、插入、删除等操作的时间复杂度从理想情况下的 O(log⁡n）退化为 O(n)。</font>**
+ **效率低下**：当二叉树不平衡时，树的高度接近于节点的数量，操作效率会大幅降低。

**<font style="color:#0d0016;background-color:#f9eda6;">平衡二叉树的优势： </font>**

+ **保证操作效率**：平衡二叉树通过特定的规则保持树的平衡（例如**<font style="color:#be191c;background-color:#fef2f0;"> AVL 树和红黑树</font>**），使得树的高度保持在 O(logn) 的水平，**<font style="color:#956fe7;">确保了在最坏情况下查找、插入和删除操作的时间复杂度为 O(log⁡n)。</font>**
+ **均匀分布**：平衡二叉树的节点分布更加均匀，查找路径较短，可以显著提高访问数据的速度。
+ **空间利用**：平衡树的结构避免了普通二叉树中可能出现的节点过度集中在一侧的情况，提高了内存的利用率。

---

> **<font style="color:#be191c;background-color:#fef2f0;">什么是二叉树不平衡</font>**，能用简单的生活例子说明吗？ 
>

**<font style="color:#0d0016;background-color:#f9eda6;">生活中的例子：排队买票</font>**

> 假设你去电影院买票，那里有两个窗口（相当于二叉树的两个子节点）。如果所有人都只排在一个窗口前，而另一个窗口完全空着，这就像是二叉树不平衡的情况。
>

+ **不平衡的二叉树**：所有顾客都排在一个窗口前，导致这条队伍很长，排到最后一个顾客需要很久（查找时间变长）。
+ **平衡的二叉树**：顾客均匀分布在两个窗口前，两个队伍都很短，**<font style="color:#be191c;background-color:#fef2f0;">这样大家买票都能更快（查找时间缩短）。</font>**

> 在计算机中的二叉树，**<font style="color:#0d0016;">如果所有节点都倾向于一侧，就像所有顾客都挤在一个窗口前，这就导致了“树不平衡”，</font>**从而使得查找、插入、删除等操作变慢。为了避免这种情况，**<font style="color:#be191c;background-color:#fef2f0;">平衡二叉树会自动调整，让节点分布得更均匀，就像有人引导顾客均匀地排在两个窗口前一样。</font>**
>

---

> **<font style="color:#0d0016;background-color:#956fe7;">平衡二叉树会如何调节，让节点分布的更加均匀 </font>**
>

#### 平衡二叉树的调节机制
+ **旋转操作**：
    - **单旋转**：当插入或删除一个节点导致树不平衡时，平衡二叉树可以通过“单旋转”来恢复平衡。**<font style="color:#be191c;background-color:#fef2f0;">单旋转分为“左旋”和“右旋”，用于重新调整树的结构。</font>**
    - **双旋转**：当单旋转无法恢复平衡时，使用双旋转，它结合了左旋和右旋，进一步调整树的结构。

**自平衡算法**：

+ **AVL树**：每次插入或删除节点后，AVL树会检查每个节点的“平衡因子”（左右子树的高度差）。如果某个节点的平衡因子超过允许范围（通常是 -1、0、1），AVL树会通过旋转操作进行调整。
+ **红黑树**：红黑树通过对节点进行**<font style="color:#be191c;background-color:#fef2f0;">“染色”和“旋转”，确保在插入或删除节点时，树始终保持接近平衡的状态。</font>**

#### 生活中的类比：重新排队
> 回到电影院买票的例子，如果发现某个窗口前的队伍变得特别长，**<font style="color:#be191c;background-color:#fef2f0;">工作人员</font>**（**<font style="color:#be191c;background-color:#fef2f0;">相当于平衡二叉树的自调节机制</font>**）可能会引导一些顾客转到另一个较短的队伍中，进行**队伍重新调整**。**<font style="color:#be191c;background-color:#fef2f0;">这种重新排队就类似于平衡二叉树中的旋转操作，通过将队伍重新分配，使两个队伍保持在合理的长度，确保每个人都能更快买到票。</font>**
>

**<font style="color:#0d0016;background-color:#f9eda6;">具体例子：右旋 </font>**

假设有三个顾客 A、B、C 按顺序到来，他们都排在同一个窗口前，这样形成了一个不平衡的队伍（类似于一个倾向右侧的链表）。 

```plain
A



   \



    B



     \



      C
```

此时，工作人员（平衡二叉树的机制）发现了不平衡，于是他会把顾客 B 提到前面，把 A 放在 B 的左边，而 C 保持在 B 的右边。 

```plain
B



   / \



  A   C
```

> **<font style="color:#be191c;background-color:#fef2f0;">这个重新排队的过程就是右旋</font>**。这样，队伍重新平衡，所有顾客都可以更快地买票。
>
> 总结来说，平衡二叉树通过旋转操作和自平衡算法来调整节点的分布，就像重新排队以保证每个顾客能更快买到票一样。
>

---

> **<font style="color:#0d0016;">有了平衡二叉树</font>**，为什么要有红黑树 
>

虽然平衡二叉树（如 AVL 树）已经能很好地保持树的平衡，**<font style="color:#0d0016;">但红黑树作为另一种自平衡二叉查找树，具有独特的优势，使其在某些情况下更为实用。</font>**以下是为什么在有了平衡二叉树之后还需要红黑树的原因。 

#### **<font style="color:#0d0016;">AVL 树 vs 红黑树</font>**
1. <font style="color:#be191c;background-color:#fef2f0;">旋转操作的频率：</font>
    - **AVL 树**：每次插入或删除节点后，AVL 树都会进行平衡因子的检查，如果树不平衡就需要进行旋转操作。由于 AVL 树严格要求每个节点的左右子树高度差不超过 1，可能会频繁地进行旋转操作，尤其是在频繁插入和删除的情况下。
    - **红黑树**：**<font style="color:#be191c;background-color:#fef2f0;">红黑树通过对节点进行“染色”（红色或黑色）和较少的旋转操作来维持平衡</font>**。红黑树允许树在某种程度上的“不完全平衡”，因此在**<font style="color:#be191c;background-color:#fef2f0;">插入和删除操作时通常需要的旋转次数更少。</font>**它在保证 O(log⁡n) 操作复杂度的同时，减少了旋转操作的开销。

**操作性能**：

+ **AVL 树**：在查找操作上，AVL 树由于更加严格的平衡条件，往往具有更快的查找速度。因此，对于读操作较多的场景，AVL 树可能表现更好。
+ **红黑树**：红黑树由于旋转操作较少，因此在插入和删除操作频繁的场景中，红黑树的整体性能通常优于 AVL 树。许多标准库（如 C++ 的 `std::map` 和 `std::set`）都使用红黑树，因为它们需要频繁的插入、删除和查找操作。

**代码复杂度**：

+ **AVL 树**：实现相对复杂，尤其是在处理旋转操作和更新平衡因子时。
+ **红黑树**：相对来说，红黑树的实现虽然涉及染色和旋转，但其逻辑相对统一，维护平衡的规则较少，使得代码管理相对简单。

#### 现实生活中的类比：不同的队伍管理策略
假设你负责管理一个有多个窗口的售票厅，每天有大量顾客来买票。

+ **AVL 树（严格管理）**：你规定每个窗口前的顾客数必须严格保持平衡。如果有某个窗口的队伍变长或变短，你会马上进行调整。这种方式虽然能保证每个顾客排队的时间都差不多，但你可能会花很多精力去频繁地调整队伍。
+ **红黑树（宽松管理）**：**<font style="color:#be191c;background-color:#fef2f0;">你允许某些窗口前的队伍稍微长一点，只要不超过一定的限制。当某个窗口的队伍过长或过短时，你才进行调整</font>**。这样，你减少了调整队伍的频率，虽然偶尔某些顾客的等待时间会稍长，但整体管理更为轻松和高效。

---

## **<font style="color:#0d0016;">十二、哈希表插入元素一般有那几个重要的步骤</font>**
> 哈希表（Hash Table）是一种用于实现快速查找的数据结构，它通过将键（Key）映射到数组中的特定位置来实现快速的插入、删除和查找操作。插入元素到哈希表中时，一般包括以下几个重要的步骤： 
>

#### 1. **计算哈希值**（Hash Function）
+ **步骤**：首先，使用哈希函数（Hash Function）对给定的键进行计算，生成对应的哈希值。哈希函数将键转换为一个整数，通常是数组的索引。
+ **目的**：将不同的键映射到哈希表中的不同位置，从而实现快速访问。

**例子**：假设你有一组电话号码，你可以使用电话号码中的某些数字（如最后几位）来计算哈希值，将每个电话号码映射到哈希表的某个位置。

#### 2. **处理冲突**（Handling Collisions）
+ **步骤**：由于不同的键可能会生成相同的哈希值（即哈希冲突），因此需要处理冲突的策略。常见的冲突处理方法有两种： 
    - **开放地址法（Open Addressing）**：如果计算出的哈希值位置已经被占用，则查找下一个可用位置进行存储。这种方式包括线性探测（Linear Probing）、二次探测（Quadratic Probing）和双重散列（Double Hashing）等方法。
    - **链地址法（Chaining）**：每个哈希值位置存储一个链表或其他数据结构，当发生冲突时，将新的元素附加到链表的末尾。

**例子**：在电话本的例子中，如果两个电话号码的最后几位相同，就可能映射到同一个位置。你可以选择下一个空位置存储，也可以将这些电话号码链在一起。

#### 3. **插入元素**（Insert Element）
+ **步骤**：在找到适当的位置后，将元素（键和值对）插入到哈希表中。
+ **目的**：将数据实际存储在哈希表中，使其可以通过哈希值快速访问。

**例子**：找到空位置后，你将电话号码和联系人信息存储在该位置。

#### 4. **检查装载因子**（Check Load Factor）
+ **步骤**：插入元素后，检查哈希表的装载因子（Load Factor），即哈希表中已经使用的存储空间比例。如果装载因子超过了某个阈值（通常为0.75），则可能需要重新调整哈希表（如扩展哈希表或重新哈希）。
+ **目的**：确保哈希表的性能不会因为装载因子过高而下降。

**例子**：如果你的电话本几乎满了，就需要准备一个更大的电话本，将所有的电话号码重新安排。

#### 总结
在哈希表中插入元素的过程中，关键步骤包括计算哈希值、处理冲突、插入元素、检查装载因子，以及必要时进行动态扩展。通过这些步骤，哈希表能够高效地组织和管理数据，实现快速查找和操作。

> 哈希表什么时候扩容？ 
>

+ 哈希表通常在其负载因子达到某个阈值时，进行扩容。负载因子时当前存储的元素数量与哈希表容量的比值。当插入元素操作使得当前负载因子超过预期设的阈值时，哈希表会进行扩容，以保持操作的效率。

---

## **<font style="color:#0d0016;">十三、但是红黑树 和 哈希表有什么区别呢 </font>**
#### **数据结构类型**
+ **红黑树**：**<font style="color:#be191c;background-color:#fef2f0;">红黑树是一种自平衡的二叉查找树</font>**。它通过**<font style="color:#be191c;background-color:#fef2f0;">颜色（红色或黑色）和旋转</font>**操作保持树的平衡，使得插入、删除、查找操作的时间复杂度保持在 O(log⁡n)。
+ **哈希表****<font style="color:#be191c;background-color:#fef2f0;">：哈希表使用哈希函数将键映射到数组中的特定位置</font>**。其查找、插入、删除操作的平均时间复杂度为 O(1)，但在最坏情况下可能退化为 O(n)（例如所有键都映射到同一位置，产生大量冲突时）。

#### 2. **数据存储方式**
+ **红黑树**：红黑树**<font style="color:#be191c;background-color:#fef2f0;">以树形结构存储数据</font>**。**<font style="color:#be191c;background-color:#fef2f0;">节点按照键的顺序排列</font>**，使得中序遍历树时，可以得到有序的数据。
+ **哈希表**：**<font style="color:#be191c;background-color:#fef2f0;">哈希表将数据存储在数组中</font>**，位置由哈希函数决定。数据在哈希表中**<font style="color:#be191c;background-color:#fef2f0;">是无序的</font>**，无法通过遍历哈希表直接得到有序数据。

#### 3. **查找时间复杂度**
+ **红黑树**：由于其自平衡特性，红黑树的查找操作具有 O(log⁡n) 的时间复杂度。
+ **哈希表**：哈希表的查找操作平均时间复杂度为 O(1)，在冲突处理得当的情况下，性能非常高。

#### 4. **顺序性**
+ **红黑树**：红黑树是有序的，支持按键值顺序遍历。比如在 `std::map` 中，元素按照键的顺序存储，并且可以高效地查找某个范围内的元素。
+ **哈希表**：哈希表是无序的，在 `std::unordered_map` 中，键值对的存储顺序与插入顺序或键的值无关，因此不支持按顺序遍历。

#### 5. **内存使用**
+ **红黑树**：由于树结构的需要，红黑树的内存使用较为紧凑，没有额外的空间浪费，节点只存储与其直接相关的数据（如键、值、颜色指示器等）。
+ **哈希表**：**<font style="color:#be191c;background-color:#fef2f0;">哈希表通常需要更多的内存</font>**，因为它们**<font style="color:#be191c;background-color:#fef2f0;">必须预留空间来减少冲突的概率</font>**。如果装载因子过高，哈希表可能会进行扩展，导致内存使用量增加。

#### 6. **插入和删除操作**
+ **红黑树**：插入和删除操作需要维护树的平衡性，可能涉及旋转操作，时间复杂度为 O(log⁡n)。
+ **哈希表**：插入和删除操作通常非常快，时间复杂度为 O(1)。但在最坏情况下（如大量冲突或需要重新哈希时），插入和删除的时间复杂度可能上升。

#### 7. **适用场景**
+ **红黑树**：**<font style="color:#be191c;background-color:#fef2f0;">适用于需要保持元素有序的场景，或需要频繁进行区间查询的场景</font>**。例如，需要快速查找某个范围内的键的情况。
+ **哈希表**：**<font style="color:#be191c;background-color:#fef2f0;">适用于需要快速查找、插入和删除元素，但不关心元素顺序的场景</font>**。例如，存储和查找大量无序数据的情况。

#### 总结
+ **红黑树**（如 `std::map`）提供了有序的数据存储，适合需要按顺序访问或区间查询的场景，但其操作的时间复杂度为 O(log⁡n)。
+ **哈希表**（如 `std::unordered_map`）则提供了快速的查找和插入操作，适合需要高效查找的场景，但它存储的数据是无序的，且在最坏情况下性能可能下降。

---

## **<font style="color:#0d0016;">十四、不用第三个变量，如何交换ab</font>**
> 要在不使用第三个变量的情况下交换两个变量 `a` 和 `b` 的值，可以使用多种方法，下面介绍三种常见的方法：使用加减法、使用异或运算符以及使用乘除法。 
>

方法 1: 使用加减法 

```plain
a = a + b;



b = a - b;



a = a - b;
```

方法 2: 使用异或运算 

```plain
a = a ^ b;



b = a ^ b;



a = a ^ b;
```

---

## **<font style="color:#0d0016;">十五、什么是锁？为什么需要锁？</font>**
> 在多线程编程中，**<font style="color:#be191c;background-color:#fef2f0;">多个线程可以同时访问和操作共享资源</font>**（比如一个变量、文件、数据库等）。当多个线程同时读写这些共享资源时，**<font style="color:#be191c;background-color:#fef2f0;">可能会产生数据不一致或冲突的情况</font>**，这种情况称为**竞争条件（Race Condition）**。
>

+ **锁**是一种机制，用来确保**<font style="color:#be191c;background-color:#fef2f0;">在同一时刻只有一个线程可以访问共享资源</font>**。通过使用锁，可以防止多个线程同时修改共享资源，从而保证数据的一致性和正确性。

---

## **<font style="color:#0d0016;">十六、什么时互斥锁 </font>**
> 互斥锁（Mutex, Mutual Exclusion Lock）**是最常用的一种锁。**<font style="color:#be191c;background-color:#fef2f0;">它保证了在任何时刻，只有一个线程能够获得锁并访问共享资源</font>**，其他线程必须等待直到锁被释放。 
>

#### **互斥锁的作用**
> 互斥锁的主要作用是**<font style="color:#be191c;background-color:#fef2f0;">避免多个线程在同一时间访问和修改共享资源，从而防止竞争条件，确保程序运行的正确性。</font>**
>

+  为了更好地理解，我们可以用生活中的例子来说明锁和互斥锁的概念：

**<font style="color:#be191c;background-color:#fef2f0;">例子 1: 使用洗手间</font>**

想象一个**<font style="color:#0d0016;">有多个人的家庭</font>**，只有一个洗手间。这个洗手间就是共享资源，家庭成员就像不同的线程。

+ **锁的作用**：当一个家庭成员使用洗手间时，门会被锁上，这样其他人就无法进入，必须等待洗手间被释放（即前一个人用完并打开门）。**<font style="color:#be191c;background-color:#fef2f0;">这就是锁的作用，确保在任何时刻只有一个人（线程）可以使用洗手间（共享资源）。</font>**
+ **互斥锁的作用**：互斥锁确保家庭成员不会同时使用洗手间，避免出现尴尬的情况（数据不一致）。**<font style="color:#be191c;background-color:#fef2f0;">每次只能有一个人进出洗手间，确保所有人都可以正确、安全地使用它</font>**。

> + **锁**是用来控制对共享资源的访问的机制，避免竞争条件。
> + **互斥锁**是一种特殊的锁，确保在任何时刻只有一个线程可以访问共享资源。
>

---

## **<font style="color:#0d0016;">十七、互斥锁和自旋锁有什么区别 </font>**
#### 1. **互斥锁**
> 互斥锁是一种让线程等待直到锁被释放的机制。**<font style="color:#be191c;background-color:#fef2f0;">当一个线程获取到互斥锁后，其他线程如果想要获取同一个锁，就必须等待，期间这些线程会进入睡眠状态，直到锁被释放</font>**。
>

**生活例子：餐厅的独占包厢**

+ 想象你和朋友在餐厅里用餐，这里有一个独占包厢（相当于共享资源）。你进入包厢后，门就被锁上，其他想用这个包厢的人只能在外面等待。
+ **互斥锁的机制**：其他等待的人不会在包厢门外一直站着等（忙等待），而是坐在大厅的椅子上（睡眠状态），等你用餐完毕并离开包厢（释放锁）后，下一位客人才会被叫进去。

#### 2. **自旋锁**
> 自旋锁是一种忙等待的锁，**<font style="color:#be191c;background-color:#fef2f0;">当一个线程尝试获取锁时，如果锁已经被其他线程持有，线程不会进入睡眠状态，而是持续检查锁的状态，直到获取到锁为止。</font>**
>

**生活例子：洗手间的排队情况**

+ 想象你在一个有单独洗手间的办公室，洗手间外面没有椅子（没有睡眠状态）。当你需要上洗手间时，发现门被锁了，里面有人。你只能站在门外等待，不能去做其他事情，也不会离开（忙等待）。
+ **自旋锁的机制**：你在门外一直等着，直到里面的人出来（释放锁）。虽然站在外面等可能有点浪费时间，**<font style="color:#be191c;background-color:#fef2f0;">但如果洗手间使用时间很短，可能比坐下等待（互斥锁）更有效率。</font>**

#### **主要区别**
+ **线程等待方式**：
    - **互斥锁**：**<font style="color:#be191c;">线程等待时会进入睡眠状态，不占用CPU资源。</font>**
    - **自旋锁**：线程等待时不会进入睡眠，而是持续占用CPU资源进行忙等待。
+ **适用场景**：
    - **互斥锁**：适用于锁的持有时间较长的场景，因为线程进入睡眠状态后可以释放CPU资源，避免浪费。
    - **自旋锁**：适用于锁的持有时间非常短的场景，因为忙等待可以避免线程睡眠和唤醒带来的开销，提高效率。

---

## **<font style="color:#0d0016;">十八、什么是死锁，怎么构成死锁的 </font>**
> **什么是死锁？**
>
> 死锁发生在多个线程或进程在等待对方持有的资源时，导致所有线程或进程都无法继续执行。简而言之，**<font style="color:#be191c;background-color:#fef2f0;">死锁是指系统中的线程因相互等待而陷入一种僵局，导致程序无法继续执行下去。</font>**
>

#### **死锁的构成**
死锁通常有以下四个必要条件（称为“死锁的四个必要条件”）：

1. **互斥条件（Mutual Exclusion）：** 至少有一个资源被当前线程或进程独占，其他线程或进程不能同时访问该资源。
2. **占有且等待（Hold and Wait）：** 已经获得资源的线程或进程在等待获得其他资源时，不释放自己当前持有的资源。
3. **不剥夺条件（No Preemption）：** 资源不能被强行从线程或进程中剥夺，只能由持有资源的线程或进程自行释放。
4. **循环等待（Circular Wait）：** 存在一个等待资源的循环，其中每个线程或进程都在等待下一个线程或进程持有的资源。

**避免死锁的策略**

1. **<font style="color:#fe2c24;">资源分配顺序：</font>**确保所有进程以相同的顺序请求资源
2. **<font style="color:#fe2c24;">资源一次性分配：</font>**要求进程启动时一次性申请所要的所有资源
3. **<font style="color:#fe2c24;">资源使用限制</font>**：限制同一时刻请求资源的进程数量，或者通过预先分析确定资源的大量需求。
4. **<font style="color:#fe2c24;">使用锁超时：</font>**进程尝试锁定资源时，加入超时机制，超时未锁定则释放已占有的资源。

---

##### 例子 1: 共享停车位
**场景：**

+ 假设有两个停车位（资源），但每个停车位只能被一个车（线程）占用。
+ 车A和车B同时到达停车场，车A已经停在停车位1，车B停在停车位2。
+ 车A想进入停车位2，但停车位2已经被车B占用。车B也想进入停车位1，但停车位1被车A占用。

**死锁发生：**

+ 车A和车B都在等待对方的停车位，形成了一个等待的循环，两个车都无法继续停车。

---

#### **总结**
+ **死锁**是指多个线程或进程在等待对方持有的资源，导致所有线程或进程都无法继续执行。
+ **死锁的四个必要条件**包括互斥、占有且等待、不剥夺和循环等待。
+ **定位死锁**可以通过死锁检测、分析资源分配图、日志分析和使用调试工具来进行。
+ 通过生活中的例子（如共享停车位和购买商品的排队系统）可以帮助理解死锁的概念和构成。

---

## **<font style="color:#0d0016;">十九、什么是单例模式</font>**
> **单例模式是指****<font style="color:#be191c;background-color:#fef2f0;">在内存中只会创建且仅创建一次对象的设计模式</font>****。**在程序中**<font style="color:#be191c;">多次使用同一个对象且作用相同</font>**时，为了**<font style="color:#be191c;background-color:#fef2f0;">防止频繁地创建对象使得内存飙升</font>**，单例模式可以让程序仅在内存中**创建一个对象**，让所有需要调用的地方都共享这一单例对象。
>

![](/images/aad26c351f84379bd002d5449ecf2d97.png)

+ 私有化构造函数和析构函数，拷贝构造和赋值运算符重载，防止外部创建和销毁
+ 在类内部提供一个私有的静态指针变量指向唯一实例
+ 提供一个公共静态方法用于获取这个唯一的实

> 单例模式是一个设计模式，它确保一个类只有一个实例，并提供一个全局访问点来获取这个实例。 
>

#### 单例模式的最重要目的
+ **确保一个类只有一个实例**：在整个程序的生命周期内，某个类只有一个实例，并且提供全局访问点来获取这个实例。
+ **控制全局资源的访问**：它确保程序中的某些全局资源（如配置管理器、日志记录器、数据库连接等）只存在一个实例，避免资源冲突或重复创建导致的系统问题。

你可以将 **单例模式** 类比为整个城市的 **交通信号灯控制系统**。

##### 类比解释：
1. **唯一实例**：在整个城市中，只需要一个交通信号灯控制系统。它负责管理所有交通信号灯的开关，确保不同路口的交通流量平稳。而我们不希望在同一个城市中有多个独立的控制系统，否则会造成混乱。
2. **全局访问**：每个路口的交通信号灯都可以访问这个唯一的控制系统，并从它那里获取指令。也就是说，任何一个地方的信号灯系统都能够通过相同的方式与这个唯一的控制系统交互。

> 城市交通信号灯控制系统只需要一个实例，这样可以确保所有交通灯的一致性和统一管理，防止多个控制系统相互冲突。同样，单例模式确保在程序中一个类的实例只能有一个，并且全局共享。 
>

---

## **<font style="color:#0d0016;">二十、Vector的底层原理和扩容机制</font>**
> 底层原理： 
>

+ vector 是基于动态数组实现的，支持随机访问。
+ 在连续的空间中存储元素，允许快速访问。

> 扩容机制： 
>

+ 当向 vector 添加元素超过其当前容量时，它会创建一个更大的动态数组，并将所有现有元素复制到新数组中，释放就数组的内存。
+ 新容器通常是当前容量的两倍，不过这可能因实现而异。 

```plain
// 扩容



	void reserve(size_t n)



	{



		if (n > capacity())//判断是否需要扩容



		{



			//扩容



			size_t sz = size(); //提前算好增容前的数据个数



			T* tmp = new T[n];  //开辟n个空间



			if (_start)



			{



				//数据拷贝，也不能去使用memcpy函数



				for (size_t i = 0; i < sz; i++)



				{



					tmp[i] = _start[i];



				}



				delete[]_start; //释放旧空间



			}



			_start = tmp; //指向新空间



			_finish = _start + sz;



			_end_of_storage = _start + n;



		}



	}
```

---

## **<font style="color:#0d0016;">二十一、红黑树 和 avl 的区别，插入多个数据，选择 avl，还是红黑树</font>**
> 主要区别在于 **<font style="color:#be191c;background-color:#fef2f0;">平衡条件</font>** 和 **<font style="color:#be191c;background-color:#fef2f0;">平衡后树的高度</font>** 
>

1. 平衡条件

+ AVL 树要求任何节点的两个子树的高度差的绝对值对多为 1 ，这使得 AVL 树比红黑树更加平衡
+ 红黑树通过确保从根到叶子得所有路径上黑色节点得数量相同，并且不存在连续的红色节点来进行平衡

2. 高度和操作复杂度

+ 由于严格的平衡条件，AVL 树的高度比红黑树更低，因此在查找操作中 AVL 可能表现更好
+ 红黑树的插入删除操作引起的重新平衡操作比AVL树少，因此在频繁进行插入和删除的场景下红黑树可能有更好的性能

> **<font style="color:#0d0016;">选择：</font>**
>

+ 如果插入多个数据，并且**<font style="color:#be191c;background-color:#fef2f0;">插入操作比查找操作更加频繁</font>**，选择红黑树更加合适 ，因为它的插入和删除操作引起的平衡调整较少
+ 如果系统更加注重查找效率，并且对插入删除操作的效率要求不是很高，可与选择AVL树，因为它更加平衡，查找效率高

---

## **<font style="color:#0d0016;">二十二、C++多态的实现原理</font>**
> 多态的实现主要依赖于**<font style="color:#be191c;background-color:#fef2f0;">虚函数</font>**和**<font style="color:#be191c;background-color:#fef2f0;">虚函数表（vtable）</font>**。下面详细解释其原理。 
>

#### 1. 静态绑定 vs 动态绑定
+ **静态绑定**：函数调用在**<font style="color:#0d0016;background-color:#f9eda6;">编译时确定</font>**，通常用于**<font style="color:#be191c;background-color:#fef2f0;">非虚函数</font>**。这种方式**<font style="color:#be191c;">比较高效</font>**，**<font style="color:#0d0016;">但无法实现运行时多态。</font>**
+ **动态绑定**：函数调用在**<font style="color:#0d0016;background-color:#f9eda6;">运行时确定</font>**，通常用于**<font style="color:#be191c;background-color:#fef2f0;">虚函数</font>**。**<font style="color:#be191c;background-color:#fef2f0;">基类的指针或引用指向派生类对象时</font>**，**<font style="color:#be191c;background-color:#fef2f0;">通过动态绑定可以调用派生类的重写函数</font>**。

#### 2. 虚函数与虚函数表（vtable）
+ **虚函数**：在基类中使用 `**<font style="color:#be191c;background-color:#fef2f0;">virtual</font>**` 关键字声明的函数，允许派生类进行重写。**<font style="color:#be191c;background-color:#fef2f0;">虚函数实现动态绑定的基础。</font>**

```plain
class Base {



public:



    virtual void show() { // 虚函数



        std::cout << "Base class show" << std::endl;



    }



};



 



class Derived : public Base {



public:



    void show() override { // 重写基类的虚函数



        std::cout << "Derived class show" << std::endl;



    }



};
```

**虚函数表（vtable）**：**<font style="color:#be191c;background-color:#fef2f0;">编译器为包含虚函数的类创建一个虚函数表。</font>****<font style="color:#0d0016;">虚函数表</font>**是一个**<font style="color:#0d0016;background-color:#f9eda6;">函数指针的数组</font>**，**<font style="color:#be191c;background-color:#fef2f0;">存储类的每个虚函数的地址</font>**。当类对象被创建时，它的虚函数指针（`vptr`）指向该类的虚函数表。

+ 对于基类对象，`**<font style="color:#be191c;background-color:#fef2f0;">vptr</font>**`会指向基类的虚函数表；对于派生类对象，`vptr` 会指向派生类的虚函数表。
+ 当调用虚函数时，通过 `**<font style="color:#be191c;background-color:#fef2f0;">vptr</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 在运行时确定调用的函数是基类还是派生类的版本</font>**。

#### 3. 多态实现的流程
当基类的指针或引用调用虚函数时：

1. 对象的 `**<font style="color:#be191c;background-color:#fef2f0;">vptr</font>**` 会指向相应类的虚函数表。
2. 编译器通过 `**<font style="color:#be191c;background-color:#fef2f0;">vptr</font>**` 在虚函数表中查找相应函数的地址。
3. 在运行时调用派生类中重写的虚函数，而不是基类的版本。

```plain
#include <iostream>



using namespace std;



 



class Base {



public:



    virtual void show() { // 虚函数



        cout << "Base class show" << endl;



    }



};



 



class Derived : public Base {



public:



    void show() override { // 重写虚函数



        cout << "Derived class show" << endl;



    }



};



 



int main() {



    Base* ptr;



    Base baseObj;



    Derived derivedObj;



 



    ptr = &baseObj;



    ptr->show(); // 调用 Base::show，输出 "Base class show"



 



    ptr = &derivedObj;



    ptr->show(); // 调用 Derived::show，输出 "Derived class show"



 



    return 0;



}
```

#### 4. 运行时的表现
+ 当 `ptr` 指向 `baseObj` 时，调用的是 `Base::show`，因为 `ptr` 的 `vptr` 指向 `Base` 的虚函数表。
+ 当 `ptr` 指向 `derivedObj` 时，调用的是 `Derived::show`，因为 `ptr` 的 `vptr` 指向 `Derived` 的虚函数表。

#### 5. 多态的条件
+ 必须通过**基类的指针或引用**来调用虚函数。
+ 基类中的函数必须被声明为**虚函数**（`virtual`）。
+ 派生类中可以选择性地重写（`override`）该虚函数。

#### 总结
> C++ 多态的实现**<font style="color:#be191c;background-color:#fef2f0;">依赖于虚函数和虚函数表</font>**。编译器通过为**<font style="color:#0d0016;">每个包含虚函数的类创建虚函数表</font>**，并在运行时通过虚指针查找相应的函数，**<font style="color:#be191c;background-color:#fef2f0;">实现了动态绑定和多态</font>**。这使得同一个基类指针或引用可以在运行时根据实际对象的类型调用不同的派生类方法。
>

## **<font style="color:#0d0016;">二十三、多态分几种类型 </font>**
> C++ 中的多态主要分为两大类：**<font style="color:#be191c;background-color:#fef2f0;">编译时多态</font>**<font style="color:#be191c;background-color:#fef2f0;"> 和 </font>**<font style="color:#be191c;background-color:#fef2f0;">运行时多态</font>**。每种多态的实现方式和应用场景不同，具体分类如下： 
>

#### 1. 编译时多态（静态多态）
编译时多态，也称为**静态多态**，是在编译阶段确定函数调用的类型。其主要通过**<font style="color:#be191c;background-color:#fef2f0;">函数重载、运算符重载和模板来实现。</font>**

##### 1.1 函数重载
> **<font style="color:#be191c;background-color:#fef2f0;">函数重载</font>** 是指在**<font style="color:#be191c;">同一作用域中</font>**，**<font style="color:#0d0016;background-color:#f9eda6;">多个函数可以有相同的名字，但它们的参数列表不同</font>**。编译器根据函数调用时提供的参数数量和类型来决定调用哪个函数。
>

```plain
#include <iostream>



using namespace std;



 



void print(int i) {



    cout << "Integer: " << i << endl;



}



 



void print(double d) {



    cout << "Double: " << d << endl;



}



 



int main() {



    print(10);    // 调用 print(int)



    print(5.5);   // 调用 print(double)



    return 0;



}
```

##### 1.2 运算符重载
> **<font style="color:#be191c;background-color:#fef2f0;">运算符重载</font>** 是允许用户定义的类**<font style="color:#be191c;background-color:#fef2f0;">型重新定义内置运算符的行为</font>**。通过重载，用户可以让自定义对象像内置类型一样使用运算符。
>

```plain
#include <iostream>



using namespace std;



 



class Complex {



public:



    int real, imag;



    Complex(int r = 0, int i = 0) : real(r), imag(i) {}



 



    // 重载 + 运算符



    Complex operator + (const Complex& other) {



        return Complex(real + other.real, imag + other.imag);



    }



};



 



int main() {



    Complex c1(2, 3), c2(1, 4);



    Complex c3 = c1 + c2;  // 使用重载的 + 运算符



    cout << "Real: " << c3.real << ", Imaginary: " << c3.imag << endl;



    return 0;



}
```

#### 2. 运行时多态（动态多态）
> 运行时多态，也称为**动态多态**，是在程序运行过程中，**<font style="color:#0d0016;background-color:#f9eda6;">根据实际的对象类型决定调用哪个函数。它主要通过虚函数和继承来实现。</font>**
>

##### 2.1 虚函数
> 当一个**<font style="color:#be191c;background-color:#fef2f0;">基类指针或引用指向派生类对象时</font>**，使用虚函数可以在运行时决定调用派生类的函数。这是通过虚函数表（vtable）来实现的。
>

```plain
#include <iostream>



using namespace std;



 



class Base {



public:



    virtual void display() {



        cout << "Base display" << endl;



    }



};



 



class Derived : public Base {



public:



    void display() override {



        cout << "Derived display" << endl;



    }



};



 



int main() {



    Base* ptr;



    Derived obj;



    ptr = &obj;



    ptr->display();  // 调用 Derived::display



    return 0;



}
```

##### 2.2 抽象类与纯虚函数
> **<font style="color:#be191c;background-color:#fef2f0;">纯虚函数</font>**是没有实现的虚函数，用于在基类中定义接口。**<font style="color:#fe2c24;background-color:#fef2f0;">包含纯虚函数的类称为抽象类，无法实例化，必须通过派生类重写其纯虚函数并实现。</font>**
>

```plain
#include <iostream>



using namespace std;



 



class Shape {



public:



    // 纯虚函数，Shape 是抽象类



    virtual void draw() = 0;



};



 



class Circle : public Shape {



public:



    void draw() override {



        cout << "Drawing Circle" << endl;



    }



};



 



int main() {



    Shape* shape = new Circle();



    shape->draw();  // 调用 Circle::draw



    delete shape;



    return 0;



}
```

#### 3. 总结
+ **编译时多态（静态多态）**：
    - 函数重载、运算符重载、模板。
    - 在编译阶段确定函数调用，效率较高。
+ **运行时多态（动态多态）**：
    - 虚函数、继承、纯虚函数。
    - **<font style="color:#be191c;background-color:#fef2f0;">在运行时根据对象的实际类型决定调用哪个函数，更灵活，但需要一些额外的开销。</font>**

> 这两种多态各有优缺点，编译时多态效率较高，而运行时多态灵活性强，适用于复杂的对象模型和扩展性需求。
>

## **<font style="color:#0d0016;">二十四、多态有那些特点 </font>**
#### 1. **接口一致性**
> 多态允许使用**相同的接口**处理不同类型的对象。无论是基类对象还是派生类对象，只要它们继承自同一个基类，就可以通过基类的接口进行操作。这使得代码在处理不同类型的对象时保持简洁一致。
>

#### 2. **动态绑定**
> 多态的实现依赖于动态绑定（又称为**运行时绑定**）。当基类的指针或引用指向派生类对象时，虚函数的调用在运行时根据对象的实际类型确定。这与编译时的静态绑定不同，动态绑定提供了更大的灵活性。
>

+ **动态绑定**使得可以在运行时选择适合的函数调用，避免在编译期固定函数行为。这也是虚函数在 C++ 多态中发挥关键作用的原因。

#### 3. **继承和虚函数**
> 多态的实现依赖于**继承**机制以及**虚函数**。基类通过虚函数定义一个接口，派生类可以根据需要重写这些虚函数，提供自己的实现。这种机制允许基类指针或引用在运行时调用派生类的重写函数。
>

+ 基类的虚函数提供接口，派生类的重写函数提供具体实现。

#### 5. **提高代码的复用性**
> 多态通过继承和接口的复用，使得代码可以更容易地被重用。相同的基类接口可以应用于多个派生类，减少了重复代码。基于多态的设计模式，比如**工厂模式**和**策略模式**，也大大提高了代码的复用性。
>

#### 6. **抽象性**
> 通过多态性，基类通常只定义接口或抽象行为，而具体实现由派生类来完成。这种**抽象性**使得系统的设计更加模块化，有助于将功能分离为可重用的组件。
>

+ 抽象类通过纯虚函数定义接口，派生类提供具体实现。

#### 7. **灵活性**
> 多态提高了代码的**灵活性**，特别是在处理不同类型的对象时。程序可以在运行时动态地决定调用哪个类的实现，而不需要硬编码在编译时确定。这种特性在需要处理大量不同类型的对象时非常有用。
>

例如，游戏中的角色可以有多种类型（玩家、敌人、NPC 等），通过多态机制，可以为每种类型的角色提供不同的行为，但使用相同的接口处理它们。

#### 8. **支持运行时类型识别**
> 虽然多态的重点在于统一的接口，但有时我们需要在运行时判断实际的对象类型。在 C++ 中可以通过 `dynamic_cast` 进行**运行时类型识别（RTTI）**，以确认某个基类指针指向的具体对象类型。
>

```plain
Animal* animal = new Dog();



if (Dog* dog = dynamic_cast<Dog*>(animal)) {



    dog->sound();  // 确认是 Dog 类型



}
```

#### 总结
C++ 中多态的主要特点包括：

1. **接口一致性**：通过基类指针或引用调用派生类的函数，保持代码一致性。
2. **动态绑定**：在运行时根据对象的实际类型进行函数调用。
3. **继承和虚函数**：多态依赖继承和虚函数机制实现。
4. **代码的可扩展性**：无需修改已有代码即可添加新功能。
5. **提高代码的复用性**：减少代码重复，通过接口复用。
6. **抽象性**：通过抽象类和接口实现功能的分离和模块化。
7. **灵活性**：支持在运行时处理不同类型的对象，提高系统的灵活性。
8. **运行时类型识别**：支持通过 `dynamic_cast` 识别对象的实际类型。

---

## **<font style="color:#0d0016;">二十五、什么情况下会导致内存泄漏</font>**
> **<font style="color:#be191c;background-color:#fef2f0;">内存泄漏是指在程序中动态分配的内存没有被正确释放，导致这部分内存无法被再次使用。</font>**C++ 中的内存泄漏问题尤为常见，因为它允许手动内存管理。以下是一些常见导致内存泄漏的情况： 
>

#### 1. **动态分配的内存未释放**
> 最常见的内存泄漏场景是使用 `**<font style="color:#be191c;background-color:#fef2f0;">new</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 或 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">malloc()</font>**`**<font style="color:#0d0016;background-color:#f9eda6;">动态分配的内存没有被及时释放</font>**。C++ 需要手动释放动态分配的内存，**<font style="color:#0d0016;background-color:#f9eda6;">如果忘记使用 </font>**`**<font style="color:#0d0016;background-color:#f9eda6;">delete</font>**`**<font style="color:#0d0016;background-color:#f9eda6;"> 或 </font>**`**<font style="color:#0d0016;background-color:#f9eda6;">free()</font>**`**<font style="color:#0d0016;background-color:#f9eda6;"> 释放这块内存</font>**，程序退出之前这部分内存将无法被使用。
>

```plain
void memoryLeak() {



    int* arr = new int[100];  // 动态分配内存



    // 忘记释放内存



    // delete[] arr;  // 这行代码没有执行，导致内存泄漏



}
```

#### 2. **在异常处理时未释放内存**
> 当程序抛出异常时，**<font style="color:#0d0016;background-color:#f9eda6;">如果没有适当地处理内存释放，可能会导致内存泄漏</font>**。特别是在使用 `**<font style="color:#be191c;">new</font>**`动态分配内存的过程中，如果在释放内存之前发生了异常，分配的内存将无法被释放。
>

```plain
void exceptionLeak() {



    int* data = new int[10];  // 动态分配内存



    throw std::runtime_error("Some error");  // 抛出异常，但没有 delete[] data;



}
```

#### 3. **没有为类的析构函数释放内存**
> 在类中使用 `**<font style="color:#be191c;background-color:#fef2f0;">new</font>**`动态分配内存时，如果没有在析构函数中正确释放这部分内存，会导致每次创建对象时动态分配的内存得不到释放，进而造成内存泄漏。
>

```plain
class MyClass {



private:



    int* data;



public:



    MyClass() {



        data = new int[100];  // 动态分配内存



    }



    ~MyClass() {



        // 如果析构函数没有释放内存



        // delete[] data;  // 忘记释放内存，导致泄漏



    }



};
```

#### 4. **循环引用**
> 在使用智能指针（特别是 `**<font style="color:#be191c;background-color:#fef2f0;">std::shared_ptr</font>**`）时，循环引用是导致内存泄漏的一个常见原因。`**<font style="color:#be191c;background-color:#fef2f0;">std::shared_ptr</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 会使用引用计数来管理内存，当引用计数为 0 时，内存会被释放</font>**。然而，如果两个对象通过 `std::shared_ptr` 互相引用，它们的引用计数永远不会变为 0，导致内存无法释放。
>

```plain
#include <memory>



 



class B;



class A {



public:



    std::shared_ptr<B> b_ptr;



};



 



class B {



public:



    std::shared_ptr<A> a_ptr;



};



 



void circularReference() {



    std::shared_ptr<A> a = std::make_shared<A>();



    std::shared_ptr<B> b = std::make_shared<B>();



    a->b_ptr = b;  // A 引用 B



    b->a_ptr = a;  // B 引用 A，形成循环引用



    // 循环引用导致两者的引用计数无法变为 0，内存泄漏



}
```

解决办法是使用`**<font style="color:#be191c;background-color:#fef2f0;">std::weak_ptr</font>**` 解决循环引用问题。`**<font style="color:#be191c;background-color:#fef2f0;">std::weak_ptr</font>**` 不会增加引用计数，从而避免循环引用。 

```plain
class A {



public:



    std::weak_ptr<B> b_ptr;  // 使用 weak_ptr



};
```

---

## **<font style="color:#0d0016;">二十六、什么是智能指针，你在一般什么情况下使用智能指针 </font>**
> 智能指针（Smart Pointer）是 C++ 标准库中提供的一种自动化管理内存的工具。与传统的指针不同，**<font style="color:#0d0016;background-color:#f9eda6;">智能指针会自动管理所指向的对象，并在不再使用时自动释放对象的内存，避免了手动调用 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">delete</font>**`**<font style="color:#0d0016;background-color:#f9eda6;"> 来释放内存，减少了内存泄漏的风险。 </font>**
>

C++ 标准库中最常用的智能指针有三种：

1. `**<font style="color:#be191c;background-color:#fef2f0;">std::unique_ptr</font>**`<font style="color:#be191c;background-color:#fef2f0;">：</font>独占所有权的智能指针，不支持赋值和赋值操作。
2. `**<font style="color:#be191c;background-color:#fef2f0;">std::shared_ptr</font>**`<font style="color:#be191c;background-color:#fef2f0;">：</font>共享所有权的智能指针，引用技术机制，多个智能指针可共享同一个对象。
3. `**<font style="color:#be191c;background-color:#fef2f0;">std::weak_ptr</font>**`<font style="color:#be191c;background-color:#fef2f0;">：</font>避免循环引用的辅助智能指针，不对对象的所有权技术，用于解决shared_ptr的循环引用问题。

#### 智能指针的类型和使用场景
##### 1. `**<font style="color:#be191c;background-color:#fef2f0;">std::unique_ptr</font>**`**<font style="color:#be191c;background-color:#fef2f0;">（独占所有权）</font>**
+ **特点**：`**<font style="color:#be191c;background-color:#fef2f0;">std::unique_ptr</font>**` 拥有独占所有权，**<font style="color:#0d0016;background-color:#f9eda6;">意味着在同一时间只能有一个 </font>**`**<font style="color:#0d0016;background-color:#f9eda6;">unique_ptr</font>**`**<font style="color:#0d0016;background-color:#f9eda6;"> 指向一个对象。不能复制 </font>**`**<font style="color:#0d0016;background-color:#f9eda6;">unique_ptr</font>**`**<font style="color:#0d0016;background-color:#f9eda6;">，但可以通过 </font>**`**<font style="color:#0d0016;background-color:#f9eda6;">std::move</font>**`**<font style="color:#0d0016;background-color:#f9eda6;"> 转移所有权。</font>**
+ **应用场景**： 
    - 当你希望明确某个对象的生命周期并且不希望其被多个对象共享时，使用 `std::unique_ptr`。
    - 适用于对象的所有权明确归属单个作用域或单一逻辑控制者。

```plain
#include <memory>



#include <iostream>



 



void uniquePtrExample() {



    std::unique_ptr<int> ptr1(new int(10));  // 创建 unique_ptr



    std::cout << "Value: " << *ptr1 << std::endl;



 



    // std::unique_ptr<int> ptr2 = ptr1;  // 错误：无法复制 unique_ptr



    std::unique_ptr<int> ptr2 = std::move(ptr1);  // 转移所有权



 



    if (!ptr1) {



        std::cout << "ptr1 is null after move" << std::endl;



    }



    std::cout << "Value: " << *ptr2 << std::endl;



}
```

**何时使用**：

+ **<font style="color:#be191c;background-color:#fef2f0;">对象的生命周期在单个作用域内，且不需要多个指针共享该对象。</font>**
+ 你希望在对象不再使用时自动销毁它，避免内存泄漏。

---

##### `<font style="color:#be191c;background-color:#fef2f0;">2.std::shared_ptr</font>`<font style="color:#be191c;background-color:#fef2f0;">（共享所有权）</font>
+ **特点**：`std::shared_ptr` 允许多个指针共享同一个对象，**<font style="color:#be191c;background-color:#fef2f0;">并通过引用计数来管理对象的生命周期。只有当所有 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">shared_ptr</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 都释放对该对象的引用时，才会释放内存。</font>**
+ **应用场景**： 
    - 当你希望多个对象共享同一个资源时，使用 `std::shared_ptr`。
    - 适用于需要共享资源或对象生命周期无法明确归属单一对象的场景。

```plain
#include <memory>



#include <iostream>



 



void sharedPtrExample() {



    std::shared_ptr<int> ptr1 = std::make_shared<int>(20);  // 创建 shared_ptr



    std::shared_ptr<int> ptr2 = ptr1;  // 共享所有权



 



    std::cout << "Reference count: " << ptr1.use_count() << std::endl;



    std::cout << "Value: " << *ptr1 << std::endl;



 



    ptr2.reset();  // ptr2 不再共享该对象



    std::cout << "Reference count after reset: " << ptr1.use_count() << std::endl;



}
```

**何时使用**：

+ 当需要多个对象或作用域共享同一个对象，且该对象在某一特定时刻自动销毁时。
+ 对象的生命周期无法由一个所有者单独控制，而是依赖于多个共享者的引用计数来决定释放时间。

> `shared_ptr` 通过维护一个 **引用计数**（reference count）来管理对象的生命周期。每次创建一个新的 `shared_ptr` 或拷贝一个已有的 `shared_ptr` 时，引用计数会增加；每次销毁或重置一个 `shared_ptr` 时，引用计数会减少。当引用计数归零时，`shared_ptr` 自动释放所管理的对象。
>

1. 控制块：存储引用计数器和指向动态分配的对象的指针

2.智能指针对象：包含对控制块的引用

##### <font style="color:#be191c;background-color:#fef2f0;">3. </font>`<font style="color:#be191c;background-color:#fef2f0;">std::weak_ptr</font>`<font style="color:#be191c;background-color:#fef2f0;">（弱引用）</font>
+ **特点**：`std::weak_ptr` 不会增加对象的引用计数，通常用于打破 `std::shared_ptr` 之间的循环引用。`weak_ptr` 不能直接访问对象，而是通过 `lock()` 函数获取一个 `shared_ptr` 以访问对象。
+ **应用场景**： 
    - 用于解决 `std::shared_ptr` 之间的循环引用问题，确保对象可以被正确释放。
    - `std::weak_ptr` 的引用不会影响对象的生命周期。

```plain
#include <memory>



#include <iostream>



 



class Node {



public:



    std::shared_ptr<Node> next;



    std::weak_ptr<Node> prev;  // 使用 weak_ptr 避免循环引用



 



    ~Node() {



        std::cout << "Node destroyed" << std::endl;



    }



};



 



void weakPtrExample() {



    auto node1 = std::make_shared<Node>();



    auto node2 = std::make_shared<Node>();



    node1->next = node2;



    node2->prev = node1;  // 避免循环引用



 



    std::cout << "Both nodes created" << std::endl;



}
```

**何时使用**：

+ 当你需要避免 `std::shared_ptr` 之间的循环引用时，使用 `std::weak_ptr`。
+ 当你需要引用一个对象但不希望影响其生命周期时。

#### 总结
##### 一般情况下使用智能指针的场景：
1. **内存安全性**：智能指针的核心优势是自动释放动态分配的内存，避免内存泄漏。尤其是在复杂的对象生命周期管理中，智能指针可以避免由于手动管理内存而引入的错误。
2. **独占所有权**：如果某个对象的所有权明确属于单个对象或单个操作，使用 `std::unique_ptr` 来管理该对象的生命周期。
3. **共享所有权**：当对象需要在多个地方使用，且多个对象或作用域需要共享该对象时，使用 `std::shared_ptr`。
4. **避免循环引用**：在使用 `std::shared_ptr` 时，如果可能引发循环引用（比如在双向链表、树等数据结构中），使用 `std::weak_ptr` 来打破循环引用，避免内存泄漏。
5. **异常安全**：智能指针能够在异常发生时自动释放内存，避免资源泄露，确保程序健壮性。

通过使用智能指针，可以大大简化 C++ 中的内存管理工作，提高代码的安全性和可维护性。

---

## **<font style="color:#0d0016;">二十七、智能指针的本质是什么？ </font>**
> 智能指针的本质是通过**RAII** 机制结合指针操作符重载，自动管理动态分配的资源，避免内存泄漏和手动管理的复杂性。其核心功能包括：
>

1. **自动化内存管理**：通过构造函数和析构函数自动获取和释放内存。
2. **RAII 模式**：确保资源在对象超出作用域时被正确释放。
3. **指针运算符重载**：使智能指针的使用方式与普通指针类似，但更安全。
4. **引用计数（**`**shared_ptr**`**）**：通过引用计数共享资源，确保资源的唯一释放。
5. **异常安全**：智能指针确保即使在异常情况下也能正确释放资源。
6. **循环引用的管理（**`**weak_ptr**`**）**：防止 `shared_ptr` 引发的循环引用问题。

通过智能指针，程序员可以大大减少内存泄漏和悬空指针的风险，提高代码的安全性和可维护性。

---

## **<font style="color:#0d0016;">二十八、new 和 malloc 的区别 </font>**
![](/images/4f9ba683ee63c87b9f335685964f2a8f.png)

1. new 和 delete 是C++中用于动态内存分配和释放的操作符。new在分配内存的同时调用构造函数初始化对象，delete 释放内存前调用对象的析构函数

2. malloc 和 free 是C语言中用于动态内存分配和释放的函数。malloc 只分配内存，不进行初始化，free 只是放内存，不调用析构函数。 

## **<font style="color:#0d0016;">二十九、什么是线程同步，为什么需要线程同步？ </font>**
> **<font style="color:#be191c;background-color:#fef2f0;">线程同步</font>**是指在多线程编程中，通过某种机制来**<font style="color:#be191c;background-color:#fef2f0;">控制线程之间的执行顺序</font>**，**<font style="color:#be191c;background-color:#fef2f0;">确保多个线程可以正确、有序地访问共享资源，从而避免竞态条件（Race Condition）等问题</font>**。线程同步的**<font style="color:#0d0016;background-color:#f9eda6;">主要目的是防止数据竞争、死锁等多线程并发问题</font>**，使得程序在多个线程同时执行时仍然能够保持正确性和一致性
>

#### 为什么需要线程同步？
线程同步的目的是**协调多个线程对共享资源的访问**，防止出现以下问题：

+ **竞态条件（Race Condition）**：多个线程同时对共享数据进行修改，导致数据不一致的情况。例如，两个线程同时读取一个变量的值，然后进行修改，最后将修改结果写回，这样可能会丢失一个线程的更新。
+ **数据竞争（Data Race）**：如果两个线程同时读写同一个变量且至少有一个线程执行写操作，可能导致意外的结果。
+ **死锁（Deadlock）**：多个线程相互等待对方释放资源，导致线程永远无法继续执行。

---

##### 1. **互斥锁（Mutex）**
> 互斥锁是一种用于防止多个线程同时访问共享资源的同步机制。只有一个线程可以在任意时刻持有互斥锁，这确保了共享资源在同一时刻只能被一个线程访问。
>

##### 5. **信号量（Semaphore）**
> 信号量用于控制对共享资源的访问。它维护一个计数器，表示资源的可用数量。当计数器大于 0 时，线程可以获取信号量并访问资源；当计数器为 0 时，线程将阻塞直到资源可用。可以通过 `std::counting_semaphore`（C++20 引入）来实现。
>

##### 4. **自旋锁（Spinlock）**
> 自旋锁是一种轻量级的锁机制，它让线程在获取锁失败时不停地检查锁是否可用，而不是进入睡眠状态。这种方式适用于锁的等待时间较短的情况，减少了线程上下文切换的开销。
>

#### 总结
**线程同步**是为了避免多线程程序中对共享资源的并发访问导致数据不一致或其他错误。常用的同步机制包括：

1. **互斥锁（Mutex）**：确保同一时间只有一个线程访问共享资源。
2. **条件变量（Condition Variable）**：让线程等待特定条件的满足。
3. **读写锁（Read-Write Lock）**：支持多个线程同时读取数据，但只允许一个线程写入数据。
4. **自旋锁（Spinlock）**：用于等待时间短的场景，通过忙等待方式获取锁。
5. **信号量（Semaphore）**：控制线程对多个资源的访问。
6. **原子操作（Atomic Operation）**：轻量级的同步方式，适用于简单的共享数据访问。

---

## **<font style="color:#0d0016;">三十、进程的通信有哪些？</font>**
> 进程间通信（Inter-Process Communication, IPC）有很多种方式，常见的包括：
>

+ **管道（Pipe）**
+ **消息队列（Message Queue）**
+ **共享内存（Shared Memory）**
+ **信号量（Semaphore）**
+ **信号（Signal）**
+ **套接字（Socket）**
+ **文件（File）**

#### 我最熟悉的一种：共享内存（Shared Memory）
> **<font style="color:#be191c;background-color:#fef2f0;">共享内存</font>**是一种**<font style="color:#be191c;background-color:#fef2f0;">效率非常高的进程间通信方式</font>**，因为**<font style="color:#0d0016;background-color:#f9eda6;">它允许多个进程直接访问同一块内存区域，而不需要像其他方式那样频繁的内核态和用户态切换</font>**。共享内存的使用可以大大提高进程间通信的速度，适用于需要**<font style="color:#be191c;background-color:#fef2f0;">传递大量数据的场景</font>**。
>

##### 优点：
+ **速度快**：共享内存避免了在用户态和内核态之间频繁切换，**<font style="color:#be191c;background-color:#fef2f0;">因此非常适合传输大数据量的场景。</font>**
+ **节省资源**：**<font style="color:#0d0016;background-color:#f9eda6;">只需一块内存区域，多个进程可以共享使用，节省内存资源</font>**<font style="color:#0d0016;background-color:#f9eda6;">。</font>

##### 缺点：
+ **同步问题**：**<font style="color:#be191c;background-color:#fef2f0;">由于多个进程可能同时访问共享内存，所以需要使用同步机制（例如信号量）来避免数据竞争问题。</font>**

> 在进程间通信中，不同方式有各自的优缺点。共享内存适用于高效的大数据量传输，但需要处理好进程同步和竞争问题。
>

#### 1. **管道（Pipe）**
+ **特点**：单向通信，通常用于父子进程之间。
+ **优点**：简单易用，适合基本的父子进程通信。
+ **缺点**：数据只能单向传输，且只能在具有亲缘关系的进程之间使用。
+ **扩展**：命名管道（FIFO）可以在无亲缘关系的进程之间通信。

#### 2. **消息队列（Message Queue）**
+ **特点**：通过消息传递进行通信，可以在任意进程之间使用，消息以队列形式存储。
+ **优点**：消息可以按照优先级排序，允许随机读取队列中的消息。
+ **缺点**：内核管理消息队列，系统资源有限。

#### 3. **共享内存（Shared Memory）**
+ **特点**：多个进程共享同一块内存区域，数据直接读写，速度最快。
+ **优点**：适合大数据量传输，效率高。
+ **缺点**：需要额外的同步机制（如信号量）来避免数据竞争。

#### 4. **信号量（Semaphore）**
+ **特点**：主要用于同步多个进程或线程，确保多个进程能够有序地访问共享资源。
+ **优点**：可以控制多个进程对共享资源的访问。
+ **缺点**：不适用于传输数据，只用于进程同步。

#### 6. **套接字（Socket）**
+ **特点**：广泛用于网络通信，既可以在同一台机器的进程间通信，也可以跨网络进行通信。
+ **优点**：灵活性强，支持本地和远程进程通信。
+ **缺点**：比管道、共享内存等通信方式复杂，通信开销较大。

#### 7. **文件（File）**
+ **特点**：通过读写同一文件来共享信息。
+ **优点**：简单易实现，可以持久化数据。
+ **缺点**：效率较低，且需要额外的同步机制。

#### 8. **信号量（Semaphore）和互斥锁（Mutex）**
+ **特点**：这两种方式主要用于进程或线程的同步，确保对共享资源的有序访问。
+ **优点**：控制并发问题，防止资源竞争。

> 每种通信方式都有其适用的场景。比如，**<font style="color:#be191c;background-color:#fef2f0;">管道</font>**<font style="background-color:#f9eda6;">适合父子进程间的简单通信</font>，**<font style="color:#be191c;background-color:#fef2f0;">共享内存</font>**<font style="color:#0d0016;background-color:#f9eda6;">适合大数据量的高效传输</font>，**<font style="color:#be191c;background-color:#fef2f0;">消息队列</font>**和**<font style="color:#be191c;background-color:#fef2f0;">套接字</font>**<font style="color:#0d0016;background-color:#f9eda6;">则更加灵活，适合更复杂的进程间数据交互</font>。
>

![](/images/0cb29dfcfb336b7cb16b91771d3f857b.png)

#### 总结：
1. **管道** 和 **命名管道** 适合简单的、同一台机器上的进程通信，管道只能单向，而命名管道可以实现双向通信。
2. **消息队列** 支持有序、灵活的消息传递，但容量有限，适合需要多进程之间进行信息传递的场景。
3. **共享内存** 是最高效的数据传输方式，但需要额外的同步机制来避免数据竞争。
4. **信号量** 主要用于同步，避免进程对共享资源的竞争，不适合传输数据。
5. **信号** 主要用于简单通知或控制进程行为，信息量非常有限。
6. **套接字** 是最灵活的通信方式，支持本地和远程通信，但效率较低，特别是在远程通信场景中。

---

## **<font style="color:#0d0016;">三十一、多进程和多线程的优缺点 </font>**
![](/images/02d12957ac012f8e3025a534761e83d7.png)

#### 总结
+ **多进程**：适用于独立性强的任务，容错性好，但资源消耗大，通信复杂，适合高容错、分布式场景。
+ **多线程**：适合轻量级并发任务，资源消耗小，但同步复杂，容易出错，适合高效并发场景。

---

## **<font style="color:#0d0016;">三十二、进程和线程的区别 </font>**
> 进程和线程的区别主要体现在内存管理、资源使用、执行方式、通信方式等多个方面。以下是它们的详细对比： 
>
> + 进程它包含了运行程序的所需代码、数据以及其它系统资源。
> + 线程是进程中过的执行流，它是CPU调度和执行的最小单位
>

#### 1. **定义**
+ **进程**： 
    - **<font style="color:#be191c;background-color:#fef2f0;">进程是操作系统中资源分配的基本单位</font>**。每个进程有自己独立的内存空间、全局变量、文件句柄等资源，并且彼此之间相互独立。
+ **线程**： 
    - **<font style="color:#be191c;background-color:#fef2f0;">线程是CPU调度的基本单位</font>**。线程是进程内的一个执行流，多个线程共享同一个进程的资源，如内存、全局变量等。一个进程可以包含多个线程。

![](/images/69fa025cac18e256005dcc123da56ee9.png)

#### 总结：
+ **进程** 更适合需要高隔离度、独立运行的任务，开销较大，但容错性好，通信复杂。
+ **线程** 更适合轻量级的并发任务，资源消耗小，通信方便，但需要处理同步和潜在的竞争问题。
+ 进程拥有独立的地址空间，而同一进程下的线程共享地址空间
+ 进程间切换开销大，线程间切换开销小。
+ 进程间通信（IPC）方式多样但相对复杂，线程间可以直接通过读写共享内存来通信，更简单高效

---

## **<font style="color:#0d0016;">三十三、线程的通信方式有哪些，分别有什么不同 </font>**
> 线程之间的通信主要通过**<font style="color:#be191c;background-color:#fef2f0;">共享内存和同步机制来实现</font>**，因为同一进程内的线程共享内存空间，所以它们可以直接访问和修改共享的数据。然而，为了避免数据竞争和其他并发问题，线程通信通常需要一些同步机制来确保数据一致性和线程安全。常见的线程通信方式包括以下几种： 
>

![](/images/eb3ed59e460f863bdfb942b6a7ef7855.png)

#### 总结：
+ **共享内存** 是线程通信的基础，所有其他方式都是为了解决共享内存带来的同步问题。
+ **互斥锁** 和 **条件变量** 是最常用的同步机制，用于控制线程对共享资源的访问。
+ **信号量** 和 **自旋锁** 适用于特定场景，前者用于控制并发访问数量，后者用于短时间的忙等待。
+ **屏障** 和 **线程局部存储** 适用于需要同步执行和独立变量维护的场景。

---

## **<font style="color:#0d0016;">三十四、 Linux 如何查看进程状态</font>**
#### 1. `**ps**`** 命令**
> `**<font style="color:#be191c;background-color:#fef2f0;">ps</font>**` 命令用于显示当前系统中运行的进程信息。它可以通过不同的选项来获取特定的进程信息。
>

```plain
ps aux
```

+ `a`：显示所有终端的进程，不仅限于当前用户的。
+ `u`：以用户为主的输出格式，显示更多详细信息（如 CPU 和内存使用情况）。
+ `x`：显示没有控制终端的进程（即守护进程）。

#### 2. `**top**`** 命令**
> `**<font style="color:#be191c;background-color:#fef2f0;">top</font>**`命令提供了一个动态、实时的进程视图，显示系统中最耗资源的进程，并可以按资源使用情况进行排序。
>

```plain
top
```

+ 这将显示进程的 CPU、内存使用情况、运行时间、进程状态等。
+ 通过 `q` 退出 `top`。

---

## **<font style="color:#0d0016;">三十五、 linux 如何查看线程状态</font>**
#### 1. `**ps**`** 命令**
> `ps` 命令可以显示线程的详细信息。
>

+ 查看某个进程的线程：

```plain
ps -T -p PID
```

+ 其中，`PID` 是进程的 ID。这个命令会列出该进程下的所有线程及其状态。 

显示所有线程： 

```plain
ps -eLf
```

+ `e`：显示所有进程。
+ `L`：显示所有线程。
+ `f`：显示详细的格式。

---

## <font style="color:#0d0016;">三十六、 如何查看网络连接情况？</font>
#### 1. `**netstat**`** 命令**（已被 `ss` 替代，但仍然常用）
> `netstat` 是一个经典的网络工具，可以显示网络连接、路由表、接口状态等信息。
>

+ 查看所有当前的网络连接（包括监听和已建立的连接）：

```plain
netstat -an
```

+ `-a`：显示所有的连接（包括监听和非监听的）。
+ `-n`：以数字形式显示地址和端口。

查看 TCP 连接： 

```plain
netstat -tn
```

查看 UDP 连接：

```plain
netstat -un
```

查看某个进程使用的网络连接： 

```plain
netstat -tp
```

#### 2. `**ss**`** 命令**（`netstat` 的现代替代工具）
> `ss`（Socket Statistics）是查看网络连接的现代工具，比 `netstat` 更快、更高效。
>

+ `-t`：TCP。
+ `-u`：UDP。
+ `-n`：以数字形式显示地址。
+ `-l`：仅显示监听套接字。
+ `-p`：显示进程。

![](/images/9423802d7b95520a74b5f56c42d83653.png)

---

## **<font style="color:#0d0016;">三十七、TCP 和 UDP 的区别 </font>**
> TCP（Transmission Control Protocol）和 UDP（User Datagram Protocol）是两种常见的**<font style="color:#be191c;">传输层协议</font>**。它们的区别主要体现在**<font style="color:#be191c;background-color:#fef2f0;">数据传输方式、可靠性、连接性</font>**等方面。下面从几个维度详细对比这两种协议： 
>

#### 1. **连接性**
+ **TCP**：**<font style="color:#be191c;background-color:#fef2f0;">面向连接的协议</font>**。在传输数据之前，**<font style="color:#be191c;background-color:#fef2f0;">TCP 需要通过“三次握手”来建立连接</font>**，保证通信双方的连接可靠。数据传输完成后，**<font style="color:#0d0016;background-color:#f9eda6;">双方还需要通过“四次挥手”来断开连接</font>**。 
    - **三次握手**：发送方和接收方要进行三个数据包的交换来建立连接。
    - **四次挥手**：双方通过四个数据包来终止连接。
+ **UDP**：无连接的协议。**<font style="color:#be191c;">UDP 不需要建立连接，数据直接发送给目标</font>**，省略了连接建立和断开的过程。因此，UDP 通信开销较小，但没有连接管理的机制。

#### 2. **可靠性**
+ **TCP**：**<font style="color:#be191c;">提供可靠的传输</font>**。TCP 通过序列号、确认机制和重传策略确保数据包按序到达，并且数据不会丢失、重复或损坏。如果数据在传输中丢失，TCP 会自动重传丢失的数据包，直到对方确认接收。
+ **UDP**：不保证可靠性。U**<font style="color:#be191c;background-color:#fef2f0;">DP 不提供数据包的确认、重传、或顺序保证</font>**，因此有可能出现数据包丢失、重复或乱序的情况。UDP 适用于对可靠性要求不高的场景。

#### 5. **数据传输效率**
+ **TCP**：由于 TCP 的连接建立、确认、重传、拥塞控制等机制，**<font style="color:#be191c;background-color:#fef2f0;">数据传输的开销较大，效率较低。它更适合需要高可靠性和数据完整性的应用。</font>**
+ **UDP**：UDP 由于没有复杂的连接和控制机制，传输效率更高，**<font style="color:#be191c;background-color:#fef2f0;">适合需要低延迟、实时传输的场景。</font>**

#### 8. **传输单位**
+ **TCP**：**<font style="color:#be191c;background-color:#fef2f0;">基于流传输。TCP 会将数据分片</font>**，按需组装成数据流。因此，**<font style="color:#be191c;">TCP 更适合连续的数据传输，如文件传输。</font>**
+ **UDP**：基于**<font style="color:#0d0016;background-color:#f9eda6;">数据报文传输</font>**。UDP 以数据报文为单位进行传输，每个数据报文是独立的，且大小固定，适合传输短的消息。

![](/images/f3735ee9b2e5183093012bc7d47962b6.png)

#### 应用建议
+ 如果你的应用需要**高可靠性**（例如文件传输、银行交易、电子邮件等），使用 **TCP**。
+ 如果你的应用追求**低延迟**且**容忍一定的数据丢失**（例如视频直播、在线游戏等），使用 **UDP**。

---

## **<font style="color:#0d0016;">三十八、 左值和右值引用？</font>**
> 在 C++ 中，**左值**和**右值**引用是重要的概念，特别是在现代 C++（C++11 及之后）中，这些概念在性能优化和资源管理方面起到了关键作用。 
>

#### 1. **左值 (Lvalue) 与 右值 (Rvalue)**
首先，我们需要理解什么是**左值**和**右值**：

+ **左值 (Lvalue)**：指的是内存中有**<font style="color:#be191c;background-color:#fef2f0;">明确存储位置</font>**的对象，换句话说，左值可以出现在赋值操作符的**左边**。左值在程序的生命周期内是可以被修改的。常见的左值是变量和可以取地址的对象。

```plain
int x = 10;  // x 是左值，因为它有存储位置，可以被修改
```

+ **右值 (Rvalue)**：指的是**<font style="color:#be191c;background-color:#fef2f0;">不具备持久存储位置</font>**，通常是**<font style="color:#be191c;background-color:#fef2f0;">临时对象</font>**或字面量，无法取地址。右值通常出现在赋值操作符的**右边**，它们是短暂的、不可修改的。 

```plain
int y = x + 5;  // x + 5 是右值，它是临时生成的一个值，没有存储位置
```

![](/images/ee847e59955b784e68953c9812071e31.png)

#### 5. **移动语义和右值引用的应用场景（重点！！）**
+ **移动构造函数与移动赋值运算符**： 在移动构造函数和移动赋值运算符中，通过右值引用可以高效地“移动”对象的资源，而不是进行[深拷贝](https://so.csdn.net/so/search?q=%E6%B7%B1%E6%8B%B7%E8%B4%9D&spm=1001.2101.3001.7020)，显著提升性能。

```plain
class MyClass {



public:



    MyClass(std::string&& str) : data(std::move(str)) {}  // 使用右值引用和移动语义



private:



    std::string data;



};
```

**避免临时对象拷贝**： 右值引用可以让我们避免在传递临时对象时的深拷贝，从而减少不必要的性能开销。

#### 总结
+ **左值引用**：用于绑定左值，常用于传递可修改的对象。
+ **右值引用**：用于绑定右值，**<font style="color:#be191c;background-color:#fef2f0;">特别用于移动语义，减少不必要的对象拷贝。</font>**
+ **左值**：持久存在，具备存储位置。
+ **右值**：临时存在，不具备存储位置。

> 左值引用是对可寻址、可重复操作使用的对象（左值的）引用。它使用传统的单个 & 符号。右值引用是对临时对象（右值）的引用，使用双 && 符号。右值引用允许实现移动语义和完美转发，它可以将资源从一个（临时的）对象转移到另一个对象，提高效率、避免不必要的拷贝。 
>

## **<font style="color:#0d0016;">三十九、虚函数指针是什么时候初始化的 </font>**
> **<font style="color:#be191c;background-color:#fef2f0;">虚函数指针（vptr）</font>**是在 **<font style="color:#0d0016;background-color:#f9eda6;">对象构造时初始化的</font>**。具体地说，当一个包含虚函数的类的对象被构造时，编译器会在构造函数中为对象分配一个指向虚函数表（vtable）的指针，即虚函数指针 `vptr`。这个指针会指向该对象所属类的虚函数表，从而实现运行时的多态性。 
>

以下是虚函数指针初始化的几个关键点：

1. **在构造函数中初始化**：在对象的构造过程中，每个类的构造函数会负责初始化 `vptr`。如果类是派生类，派生类的构造函数会重新设置 `vptr`，使其指向派生类的虚函数表。
2. **初始化顺序**：如果有继承关系，基类的构造函数会首先运行，并设置基类的 `vptr`，然后派生类的构造函数会覆盖基类的 `vptr`，指向派生类的虚函数表。
3. **运行时多态**：通过 `vptr`，对象在调用虚函数时可以根据实际的对象类型选择相应的虚函数实现。

```plain
#include <iostream>



using namespace std;



 



class Base {



public:



    virtual void show() { cout << "Base class show" << endl; }



    Base() { cout << "Base constructor called" << endl; }



};



 



class Derived : public Base {



public:



    void show() override { cout << "Derived class show" << endl; }



    Derived() { cout << "Derived constructor called" << endl; }



};



 



int main() {



    Base* b = new Derived();



    b->show(); // 输出 "Derived class show"



    delete b;



    return 0;



}
```

在这个例子中，`Derived` 类对象构造时，基类 `Base` 的构造函数先被调用，初始化基类的 `vptr`，然后派生类 `Derived` 的构造函数调用时，重新设置 `vptr`，使其指向 `Derived` 的虚函数表。最终，通过 `b->show()` 调用的是 `Derived` 类的 `show` 方法，实现了多态。 

#### 总结
> 虚函数指针 `vptr` 在对象的构造过程中初始化，且在派生类构造函数中会重新设置为派生类的虚函数表，以保证虚函数调用的多态性。
>

## **<font style="color:#0d0016;">四十、析构函数不是虚函数会有什么问题 </font>**
> 如果基类的析构函数**不是虚函数**，而你通过基类指针删除指向派生类对象时，**不会调用派生类的析构函数**，这可能导致派生类特有资源无法正确释放，从而产生**资源泄漏**等问题。
>

问题的根源 

当**<font style="color:#be191c;background-color:#fef2f0;">基类的析构函数不是虚函数</font>**时，**<font style="color:#be191c;background-color:#fef2f0;">C++ 不会进行动态绑定</font>**。在通过基类指针指向派生类对象并调用 `delete` 时，只会调用基类的析构函数，而不会调用派生类的析构函数。这意味着派生类中分配的资源（如动态内存、文件句柄等）无法得到正确释放。 

举例说明： 

```plain
#include <iostream>



using namespace std;



 



class Base {



public:



    ~Base() { cout << "Base destructor called" << endl; }



};



 



class Derived : public Base {



public:



    ~Derived() { cout << "Derived destructor called" << endl; }



};



 



int main() {



    Base* b = new Derived();



    delete b;  // 只调用 Base 的析构函数



    return 0;



}
```

```plain
Base destructor called
```

在上面的代码中，`b` 是一个基类指针，但它实际上指向了 `Derived` 类的对象。当我们通过 `delete b` 释放对象时，由于 `Base` 类的析构函数不是虚函数，**只会调用 **`**Base**`** 的析构函数**，而不会调用 `Derived` 的析构函数。

这会导致 `Derived` 类中的资源没有被正确释放，从而可能导致资源泄漏。

#### 如何解决
**<font style="color:#be191c;background-color:#fef2f0;">解决方案是将基类的析构函数声明为虚函数</font>**。这样，当我们通过基类指针释放派生类对象时，编译器会正确调用派生类的析构函数，实现动态绑定，确保所有资源得到正确释放。

#### 总结
1. **析构函数不是虚函数时的问题**：通过基类指针删除派生类对象时，只会调用基类的析构函数，导致派生类特有的资源未释放，可能会引发内存泄漏或其他资源管理问题。
2. **虚析构函数的必要性**：为了解决这个问题，基类的析构函数应该声明为虚函数，以确保正确调用派生类的析构函数，从而正确释放资源。

> 当类含有虚函数时，**<font style="color:#be191c;background-color:#fef2f0;">务必将析构函数也声明为虚函数</font>**，以确保通过基类指针可以正确删除派生类对象，避免潜在的资源管理问题。 
>

## **<font style="color:#0d0016;">四十一、map使用find查找和[]查找的区别？为啥要有find </font>**
#### 1. **为什么需要 **`**find**`**？**
> `find` 的存在是为了提供一种**安全的、不改变容器内容的查找方式**。如果只依赖 `[]` 来查找元素，那么在有些情况下你可能会无意间修改了容器（插入了新键）。有些应用场景下，你可能只想做一个纯粹的查找操作，而不希望修改 `map` 的内容，这时候 `find` 非常必要。
>

#### 总结
+ `**find**`：仅查找，不会改变 `map`，返回一个迭代器，适合需要检查键是否存在时使用。
+ `**[]**`：查找并插入（如果键不存在），返回值的引用，适合在需要访问和修改 `map` 时使用。

> 通过这两种方式，可以灵活处理不同的查找需求：需要安全、不修改数据时用 `find`，需要自动插入和修改时用 `[]`。
>

## **<font style="color:#0d0016;">四十二、this 指针有什么作用，这个指针的值是从那里来的 </font>**
> `**<font style="color:#be191c;background-color:#fef2f0;">this</font>**`指针是 C++ 中**<font style="color:#be191c;background-color:#fef2f0;">一个隐含的、指向当前对象的指针</font>**，作用是指向当前对象自身，**<font style="color:#0d0016;background-color:#f9eda6;">使得类的成员函数能够访问该对象的成员变量和成员函数</font>**。它存在于每一个非静态成员函数中，**<font style="color:#0d0016;">并且在成员函数内部用于区分不同对象。 </font>**
>

#### `**1.this**`** 指针的作用**
+ **访问对象的成员**：在成员函数中，`**<font style="color:#be191c;background-color:#fef2f0;">this</font>**`指针可以访问**<font style="color:#be191c;background-color:#fef2f0;">该对象的成员变量和成员函数</font>**。
+ **避免命名冲突**：**<font style="color:#0d0016;background-color:#f9eda6;">当成员函数的形参与成员变量重名时</font>**，使用 `this` 来区分。例如，`**<font style="color:#be191c;background-color:#fef2f0;">this->member</font>**`表示访问成员变量 `member`，而不是函数参数 `member`。
+ **返回对象的地址**：可以使用 `<font style="color:#be191c;background-color:#fef2f0;">return *this</font>`<font style="color:#be191c;background-color:#fef2f0;"> 来返回对象的引用</font>，常见于链式调用或一些设计模式中。
+ **指向当前对象**：`this` 指针始终指向当前调用成员函数的对象。

2. `**this**`** 指针的来源** 

> `**<font style="color:#be191c;background-color:#fef2f0;">this</font>**`指针在编译时由编译器自动生成。对于每个非静态成员函数，编译器会隐含地将当前对象的地址作为第一个参数传递给成员函数，因此在成员函数内部，`**<font style="color:#be191c;background-color:#fef2f0;">this</font>**`指针就指向了调用该函数的对象。 
>

#### 3. `**this**`** 指针的特性**
+ **只存在于非静态成员函数中**：`this` 指针在静态成员函数中不可用，因为静态成员函数与具体对象无关。
+ **只读指针**：`**<font style="color:#be191c;background-color:#fef2f0;">this</font>**`指针本身是只读的，不能对它进行赋值操作，但可以通过它修改当前对象的成员。
+ **类型**：`**<font style="color:#be191c;background-color:#fef2f0;">this</font>**` 指针的类型是 `**<font style="color:#0d0016;background-color:#f9eda6;">T* const</font>**`，其中 `T` 是类的类型。例如在 `class MyClass` 中，`this` 的类型为 `MyClass* const`，指向当前对象。

1. **访问成员变量和函数** 

```plain
class MyClass {



public:



    int value;



 



    void setValue(int value) {



        // 使用 this-> 来区分成员变量和参数



        this->value = value;



    }



 



    void display() {



        // 直接通过 this 指针访问成员变量



        std::cout << "Value: " << this->value << std::endl;



    }



};
```

2. **返回当前对象的引用**（链式调用） 

```plain
class MyClass {



public:



    int value;



 



    MyClass& setValue(int v) {



        value = v;



        return *this;  // 返回当前对象的引用



    }



 



    void display() const {



        std::cout << "Value: " << value << std::endl;



    }



};



 



int main() {



    MyClass obj;



    obj.setValue(10).display();  // 链式调用



    return 0;



}
```

#### 5. **总结**
+ `**this**`** 指针的作用**是**<font style="color:#be191c;background-color:#fef2f0;">指向调用成员函数的当前对象</font>**，可以用于访问该对象的成员，避免命名冲突，或者返回对象本身。
+ `**this**`** 指针的值**是编**<font style="color:#be191c;background-color:#fef2f0;">译器在调用成员函数时自动生成的，指向当前对象的地址</font>**。
+ **编译时**，编译器会自动将对象的地址作为隐含参数传递给成员函数，因此在每次调用成员函数时，`this` 指针就指向了调用该函数的具体对象。

---

## **<font style="color:#0d0016;">四十三、多线程同步方式。条件变量和信号量的区别 ？</font>**
> 在多线程编程中，**<font style="color:#be191c;background-color:#fef2f0;">同步是确保多个线程安全地访问共享资源的关键机制</font>**。常见的多线程同步方式包括**<font style="color:#be191c;background-color:#fef2f0;">互斥锁</font>**<font style="color:#be191c;background-color:#fef2f0;">、</font>**<font style="color:#be191c;background-color:#fef2f0;">条件变量</font>**<font style="color:#be191c;background-color:#fef2f0;">、</font>**<font style="color:#be191c;background-color:#fef2f0;">信号量</font>**等。下面重点介绍**条件变量**和**信号量**的区别，以及它们的适用场景。 
>

#### 1. **条件变量 (Condition Variable)**
**<font style="color:#be191c;background-color:#fef2f0;">条件变量</font>**是一种用于线程间**<font style="color:#be191c;background-color:#fef2f0;">等待某个条件成立</font>**的同步机制。它通常与<font style="color:#be191c;background-color:#fef2f0;">互斥锁（</font>`<font style="color:#be191c;background-color:#fef2f0;">mutex</font>`<font style="color:#be191c;background-color:#fef2f0;">）一起使用</font>，使线程能够在等待某个条件时**释放锁并进入休眠**，直到某个线程通知条件已经满足。

##### 关键特性：
+ **条件变量是“触发机制”**：允许一个线程等待另一个线程发出某个条件成立的信号（`signal` 或 `broadcast`）。
+ **与互斥锁结合使用**：必须配合互斥锁，用于保护共享数据，同时防止多个线程并发修改该数据。
+ **等待条件**：线程调用 `wait()` 进入休眠，并释放持有的互斥锁，直到被另一个线程用 `notify_one()` 或 `notify_all()` 唤醒。

##### 使用场景：
+ 当你需要一个或多个线程等待某个特定条件满足，并且在等待期间需要释放互斥锁时。
+ 例如，生产者-消费者问题，消费者需要等待队列有数据可消费。

---

#### 2. **信号量 (Semaphore)**
**信号量**是一种计数器，用来控制**多个线程对共享资源的访问**。信号量可以用来限制可以访问资源的线程数量，允许多个线程同时访问资源或仅允许一个线程访问。

##### 关键特性：
+ **计数机制**：信号量有一个计数器，当线程进入临界区时，计数器减 1，当线程离开时，计数器加 1。如果计数器为 0，则新的线程必须等待，直到计数器大于 0。
+ **可以允许多个线程访问共享资源**：信号量允许多个线程同时访问某个资源，例如可以限制最多 3 个线程同时访问。
+ **二元信号量和计数信号量**：二元信号量类似互斥锁，只允许一个线程进入临界区。计数信号量允许多个线程同时进入临界区。

##### 使用场景：
+ 用于控制多个线程访问有限资源时，例如限制数据库连接池的最大连接数。
+ 二元信号量可以用来代替互斥锁，而计数信号量则用来控制对多个资源的访问。

![](/images/675089296fbde58eb3e6fd6d664cf587.png)

#### 4. **总结**
+ **条件变量**：用于线程间的条件同步，线程在等待某个条件时可以释放锁并休眠，适合生产者-消费者等场景。
+ **信号量**：通过计数器机制控制线程对共享资源的访问，允许同时管理多个线程对资源的访问。

> 选择哪种同步方式，取决于具体的使用场景。**<font style="color:#be191c;background-color:#fef2f0;">如果需要线程等待某种条件，可以使用条件变量；</font>**如果要**<font style="color:#be191c;background-color:#fef2f0;">控制线程对资源的并发访问，可以使用信号量。</font>**
>

## **<font style="color:#0d0016;">四十四、vector 扩容机制 。vector中删除元素是否会释放内存？ </font>**
> `**<font style="color:#be191c;background-color:#fef2f0;">std::vector</font>**`是 C++ 标准库中的动态数组，**<font style="color:#be191c;background-color:#fef2f0;">当 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">vector</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 的容量不足以容纳新元素时</font>**，会自动**扩容**。扩容是 `vector` 的关键特性之一，因为它隐藏了底层内存管理的复杂性。 
>

#### **1. 扩容机制原理**
`std::vector` 具有两种容量相关的属性：

+ **大小（size）**：当前 `vector` 中实际存储的元素数量。
+ **容量（capacity）**：当前 `vector` 为元素分配的内存空间，即最多可以容纳多少个元素。

当 `vector` 的大小超过当前容量时，它会触发扩容。扩容一般遵循如下步骤：

1. **分配更大的内存**：通常，`vector` 会按照一定的倍增策略分配更多内存，通常是**两倍**（这取决于具体的标准库实现）。新分配的容量通常比当前需要的大小大一些，以减少频繁的扩容操作。
2. **元素搬移**：**<font style="color:#be191c;background-color:#fef2f0;">将原来 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">vector</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 中的元素从旧的内存位置复制到新的内存空间</font>**。
3. **释放旧内存**：释放旧的内存块，指向新的内存区域。

```plain
#include <iostream>



#include <stdexcept>



 



template <typename T>



class MyVector {



private:



    T* data;         // 指向动态数组的指针



    size_t size;     // 当前元素数量



    size_t capacity; // 当前容量



 



public:



    // 构造函数



    MyVector() : data(nullptr), size(0), capacity(0) {}



 



    // 析构函数



    ~MyVector() {



        delete[] data; // 释放内存



    }



 



    // 获取当前元素数量



    size_t getSize() const {



        return size;



    }



 



    // 获取当前容量



    size_t getCapacity() const {



        return capacity;



    }



 



    // 添加元素



    void push_back(const T& value) {



        if (size == capacity) {



            expandCapacity();  // 当容量不足时扩容



        }



        data[size] = value;



        size++;



    }



 



    // 访问元素



    T& operator[](size_t index) {



        if (index >= size) {



            throw std::out_of_range("Index out of bounds");



        }



        return data[index];



    }



 



private:



    // 扩容函数



    void expandCapacity() {



        // 如果当前容量为 0，则初始化容量为 1；否则扩容为当前容量的 2 倍



        size_t newCapacity = (capacity == 0) ? 1 : capacity * 2;



        



        // 分配新空间



        T* newData = new T[newCapacity];



 



        // 将原有数据复制到新空间



        for (size_t i = 0; i < size; ++i) {



            newData[i] = data[i];



        }



 



        // 释放旧空间



        delete[] data;



 



        // 更新指针和容量



        data = newData;



        capacity = newCapacity;



    }



};



 



int main() {



    MyVector<int> vec;



 



    // 打印初始容量



    std::cout << "初始容量: " << vec.getCapacity() << std::endl;



 



    // 添加元素并观察扩容



    for (int i = 0; i < 10; ++i) {



        vec.push_back(i);



        std::cout << "添加元素 " << i << " 后的容量: " << vec.getCapacity() << std::endl;



    }



 



    // 打印最终的元素



    std::cout << "最终元素: ";



    for (int i = 0; i < vec.getSize(); ++i) {



        std::cout << vec[i] << " ";



    }



    std::cout << std::endl;



 



    return 0;



}
```

```plain
初始容量: 0



添加元素 1 后的容量: 1



添加元素 2 后的容量: 2



添加元素 3 后的容量: 4



添加元素 4 后的容量: 4



添加元素 5 后的容量: 8



添加元素 6 后的容量: 8



添加元素 7 后的容量: 8



添加元素 8 后的容量: 8



添加元素 9 后的容量: 16



添加元素 10 后的容量: 16



最终元素: 1 2 3 4 5 6 7 8 9 10
```

+ **扩容机制**：`std::vector` 的容量通常按倍数扩展，旧数据会被复制到新内存中，旧内存会被释放。
+ **删除元素后是否释放内存**：`**<font style="color:#be191c;background-color:#fef2f0;">erase()</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 和 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">pop_back()</font>**` 只改变元素数量，内存不会自动释放，`vector` 的 `capacity` 不会缩小。

## **<font style="color:#0d0016;">四十五、vector在删除时，是否会产生迭代器失效</font>**
> `**<font style="color:#be191c;background-color:#fef2f0;">std::vector</font>**` 在删除元素时可能会导致**<font style="color:#be191c;background-color:#fef2f0;">迭代器失效</font>**。不同的删除操作会导致不同类型的迭代器失效问题。下面详细解释迭代器失效的原因和情况： 
>

#### 1. **在循环中使用失效迭代器**
如果你在一个循环中删除 `vector` 的元素，**<font style="color:#be191c;background-color:#fef2f0;">但继续使用失效的迭代器来访问剩余元素，这通常会导致未定义行为、程序崩溃或错误输出</font>**。常见的场景是 `erase()` 操作未正确处理迭代器。

```plain
#include <iostream>



#include <vector>



 



int main() {



    std::vector<int> v = {1, 2, 3, 4, 5};



    



    // 在删除元素时，继续使用失效的迭代器



    for (auto it = v.begin(); it != v.end(); ++it) {



        if (*it == 3) {



            v.erase(it);  // 删除 3 后，`it` 失效



        }



        std::cout << *it << " ";  // 使用了失效迭代器



    }



    return 0;



}
```

当使用 `erase()` 删除元素时，迭代器会失效，因此应始终使用 `erase()` 返回的新迭代器来继续操作，而不是使用失效的旧迭代器。 

**<font style="color:#be191c;background-color:#fef2f0;">注意：erase 返回的是，删除元素的下一个位置 </font>**

```plain
#include <iostream>



#include <vector>



 



int main() {



    std::vector<int> v = {1, 2, 3, 4, 5};



    



    // 改进：使用 erase() 返回的有效迭代器



    for (auto it = v.begin(); it != v.end(); ) {



        if (*it == 3) {



            it = v.erase(it);  // 删除 3 后，使用 erase 返回的新迭代器



        } else {



            ++it;  // 仅当不删除时，才递增迭代器



        }



    }



 



    // 输出剩余元素



    for (auto val : v) {



        std::cout << val << " ";  // 输出: 1 2 4 5



    }



 



    return 0;



}
```

#### 2. **扩容后迭代器失效**
> 虽然你提到的是删除操作，但如果删除后添加元素，**<font style="color:#be191c;background-color:#fef2f0;">导致 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">vector</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 扩容</font>**，也会导致迭代器失效。**<font style="color:#be191c;background-color:#fef2f0;">在扩容时，所有的迭代器都会失效</font>**，因为 `**<font style="color:#be191c;background-color:#fef2f0;">vector</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 底层内存会重新分配</font>**。
>

```plain
#include <iostream>



#include <vector>



 



int main() {



    std::vector<int> v = {1, 2, 3, 4, 5};



    auto it = v.begin() + 2; // 指向 3



    v.erase(it);             // 删除 3，迭代器 it 仍然有效



 



    v.push_back(6);          // 如果扩容，之前的迭代器都会失效



 



    std::cout << *it << std::endl;  // 可能导致未定义行为



    return 0;



}
```

> `push_back(6)` 可能会触发 `vector` 扩容，导致所有指向原内存的迭代器失效。
>

vector迭代器失效有两种

+ **扩容、缩容导致野指针式失效**
+ **迭代器指向的位置意义变了**

系统越界机制检查，不一定能够检查到。编译实现检查机制，相对比较靠谱。

---

## **<font style="color:#0d0016;">四十六、使用 unique_ptr 和 裸指针 有什么却别，效率有什么区别？ </font>**
> `std::unique_ptr` 和裸指针（也就是普通的原生指针）在 C++ 中有不同的用途和特性，主要区别体现在**内存管理**、**安全性**以及**效率**等方面。
>

```plain
void useRawPointer() {



    int* ptr = new int(10);  // 动态分配内存



    std::cout << *ptr << std::endl;  // 使用



    delete ptr;  // 手动释放内存，避免内存泄漏



}
```

```plain
#include <memory>



void useUniquePointer() {



    std::unique_ptr<int> ptr = std::make_unique<int>(10);  // 自动管理内存



    std::cout << *ptr << std::endl;  // 使用



    // 离开作用域时，内存会自动释放



}
```

#### **总结**
+ **内存管理**：`**<font style="color:#be191c;">unique_ptr</font>**`自动管理内存，而裸指针需要手动管理。
+ **安全性**：`unique_ptr` 更安全，避免了裸指针常见的内存泄漏和悬空指针问题。
+ **效率**：裸指针在极少数情况下可能比 `unique_ptr` 更高效，但这种差异通常很小，`unique_ptr` 提供的自动化内存管理功能在大多数情况下是值得的。

---

## **<font style="color:#0d0016;">四十七、大小端有什么区别，分别有什么用</font>**
> **大小端（Endianness）** 是指**<font style="color:#be191c;background-color:#fef2f0;">多字节数据</font>**在**<font style="color:#0d0016;background-color:#f9eda6;">内存中的存储顺序</font>**，主要有两种方式：**大端模式（Big-Endian）** 和 **小端模式（Little-Endian）**。它们的区别在于**<font style="color:#0d0016;background-color:#f9eda6;">多字节数据</font>****的高低位字节如何排列**。 
>

#### **1. 大端模式（Big-Endian）**
+ **定义**：**<font style="color:#be191c;background-color:#fef2f0;">高位字节 --- 存储在 --- 低地址，低位字节 -- 存储在 -- 高地址。</font>**
+ **示例**：
    - 假设一个 32 位的整型数 `**0x12345678**`，在大端模式下，内存中存储顺序为：

```plain
地址:    0x00    0x01    0x02    0x03



数据:    0x12    0x34    0x56    0x78
```

**用处**：

+ **网络协议**：<font style="color:#be191c;background-color:#fef2f0;">大端模式广泛应用于网络字节序</font>（也称为**网络序**）。网络协议通常使用大端字节序传输数据，因为这样可以使得**<font style="color:#be191c;background-color:#fef2f0;">跨平台通信更加一致</font>**。
+ **人类可读性**：大端模式更符合人类的阅读习惯。对于一个数字 `0x12345678`，人们通常从左到右读作 "12 34 56 78"，这与大端模式的存储顺序一致。

#### **2. 小端模式（Little-Endian）**
+ **定义**：**<font style="color:#be191c;">低位字节存储在低地址，高位字节存储在高地址。</font>**
+ **示例**：
    - 假设一个 32 位的整型数 `**<font style="color:#0d0016;">0x12345678</font>**`，在小端模式下，内存中存储顺序为：

```plain
地址:    0x00    0x01    0x02    0x03



数据:    0x78    0x56    0x34    0x12
```

**用处**：

+ **计算机架构**：小端模式在大多数计算机架构中被广泛使用，特别是在**Intel x86** 和 **x86-64** 体系结构中。这是因为在处理器中，读取和操作数据时，低位字节通常是更频繁操作的部分，小端模式可以更有效地处理这种情况。

![](/images/948bbe332ce16848675255ccffc6523b.png)

#### **5. 大小端在实践中的作用**
+ **网络通信**： 
    - 网络协议（如 IP、TCP、UDP）规定数据必须按照**大端模式**（即网络字节序）进行传输。为了保证不同架构的设备能够正确地通信，发送方和接收方都需要在传输数据时按照大端顺序来发送字节。
+ **文件存储格式**： 
    - 一些文件格式（如图片、音频文件）对字节序有严格要求，比如 TIFF 图像格式可以使用大端或小端模式存储数据，**<font style="color:#be191c;background-color:#fef2f0;">PNG 格式规定使用大端模式</font>**。
+ **跨平台应用**： 
    - **<font style="color:#be191c;background-color:#fef2f0;">大端模式保证了不同平台之间的数据一致性</font>**，尤其是需要在网络或分布式系统中传输数据时。而小端模式则更适合于在单一平台上的高效处理。

#### **总结**
+ **大端模式**和**小端模式**的主要区别在**<font style="color:#be191c;background-color:#fef2f0;">于多字节数据的存储顺序</font>**。大端模式高位字节存储在低地址，小端模式低位字节存储在低地址。
+ **大端模式**更适合**<font style="color:#be191c;background-color:#fef2f0;">网络通信和跨平台的数据一致性</font>**，**小端模式**则更适合**<font style="color:#be191c;background-color:#fef2f0;">高效的位操作</font>**和处理低位字节的数据。
+ 了解并使用正确的字节序对于确保跨平台通信、文件存储和系统架构中的正确数据处理非常重要。

---

```plain
int main()



{



    int a = 1; // 定义一个整型变量 a，值为 1



    char* p = (char*)&a; // 将整型变量 a 的地址转换为 char* 指针，指向 a 的第一个字节



 



    if (*p == 1) // 检查最低地址的字节（也就是 a 的低位字节）



    {



        printf("小端\n"); // 如果最低字节是 1，说明是小端



    }



    else



    {



        printf("大端\n"); // 否则是大端



    }



    return 0;



}
```

---

## **<font style="color:#0d0016;">四十八、什么是B+树，它有什么特点？ </font>**
> **B+树** 是一种 **自平衡的树** 数据结构，广泛应用于 **数据库系统** 和 **文件系统** 中，以高效存储和检索大量数据。它在磁盘存储场景中尤为常见，因为它能减少磁盘I/O操作的次数，极大提高了查询效率。 
>

#### B+树的结构和特点
1. **<font style="color:#be191c;background-color:#fef2f0;">每个节点可以有多个子节点</font>**<font style="color:#be191c;background-color:#fef2f0;">：</font>
    - B+树的每个内部节点可以拥有多个子节点，而不像二叉树那样每个节点只有两个子节点。通常，B+树的阶（order）决定了每个节点的子节点数。
2. **叶子节点存储所有数据**：
    - 与 B 树不同，**<font style="color:#be191c;background-color:#fef2f0;">B+树的所有数据元素都存储在</font>****叶子节点** 中，**<font style="color:#0d0016;background-color:#f9eda6;">非叶子节</font>**点只存储索引信息，起到导航作用。
3. **叶子节点形成一个链表**：
    - **<font style="color:#be191c;background-color:#fef2f0;">所有的叶子节点通过指针相互连接，形成一个有序的链表</font>**。这样可以大幅提高区间查询（范围查询）的效率，只需遍历链表即可获取相邻的数据。
4. **节点中的元素是有序的**：
    - 每个节点中的数据（或索引）都是从小到大排序的，保证了高效的搜索、插入和删除操作。

![](/images/396c8513a4f40f4f273cd67301007f59.png)

#### 总结
+ **B+树** 适合 **<font style="color:#be191c;background-color:#fef2f0;">磁盘存储和范围查询</font>**，如数据库索引，树高度较低，查询和插入删除操作均为 O(log n)，叶子节点有序且通过链表连接，支持高效的顺序和范围查询。
+ **哈希表** 适合 **快速精确查找**，查找、插入和删除操作在理想情况下为 O(1)，**<font style="color:#be191c;background-color:#fef2f0;">但不支持范围查</font>**询，也可能因哈希冲突导致性能下降。
+ **红黑树** 是一种 **自平衡二叉搜索树**，广泛用于 **内存存储**，如 `map` 和 `set`，它支持 O(log n) 的查找、插入和删除操作，但范围查询性能不如 B+树，**<font style="color:#be191c;">通常用于内存中的小规模数据场景。</font>**

---

## **<font style="color:#0d0016;">四十九、虚函数实现机制</font>**
> C++中的虚函数通过虚函数表来实现。每一个使用虚函数的类都有一个对应的虚函数表，其中存储了指向类的虚函数指针。每一个对象实例包含一个指向其类的虚函数表的指针(vptr)，当调用一个对象的虚函数时，会通过这个指针在虚函数表中查找相应的函数实现进行调用，从而实现多态性。这种机制允许在运行时根据对象的实际类型来动态绑定方法，而不是在编译时。
>

## **<font style="color:#0d0016;">五十、构造函数能否声明为虚函数？</font>**
#### 1. **构造函数**
+ **原因**：构造函数的主要作用是初始化对象，而虚函数依赖于对象的完整初始化和虚表的正确配置。在对象构造的过程中，虚表尚未准备好，因此构造函数不能是虚函数。

#### 2. **内联函数**
+ 虽然内联函数和虚函数是可以同时存在的，但 **内联** 与 **虚函数** 存在一定的矛盾。虚函数是通过虚表（vtable）来动态绑定的，内联函数则是编译期就内联展开的。因此，即使将虚函数声明为内联函数，编译器也可能不会将其内联。

#### 3. **静态函数**
+ **原因**：静态函数属于类本身，而不属于某个具体的对象实例。虚函数需要依赖对象的虚表来进行动态调度，而静态函数没有对象的上下文，因此不能是虚函数。

#### 4. **友元函数**
+ **原因**：友元函数不是类的成员函数，它只是具有访问类私有成员的权限。由于虚函数依赖于对象实例的虚表，友元函数没有对象上下文，因此不能声明为虚函数。

#### 总结
+ **构造函数** 和 **静态函数** 是由于它们与对象的虚表无关，因此不能声明为虚函数。
+ **友元函数** 和 **模板函数** 由于其自身性质也不能声明为虚函数。

---

## **<font style="color:#0d0016;">五十一、const 和 define 的区别</font>**
> **<font style="color:#fe2c24;">const 定义的是常量</font>**，**<font style="color:#0d0016;background-color:#f9eda6;">有数据类型</font>**，**<font style="color:#fe2c24;">而 define 是 预处理器指令</font>**，**<font style="color:#0d0016;background-color:#f9eda6;">替换文本，没有数据类型</font>**
>

1. const 常量通常存储在程序的数据段中，define 只是简单的替换，不分配存储空间

2. const 是在编译阶段使用，define 发生在预处理阶段，编译器还未参与。

3. const 常量通常在调试中可见，因为它们是符号化的，而 define 替换后代码通常难以调试

---

## **<font style="color:#0d0016;">五十二、 struct 和 class 的区别</font>**
> struct 和 class 的主要区别在于 默认的 **<font style="color:#0d0016;background-color:#f9eda6;">访问权限 和 继承类型 </font>**
>

1. 默认访问权限：struct 中的成员默认是公开(public)的，而 class 中的成员默认是私有的（private）

2. 默认继承类型：使用 struct 继承 默认是公开的 (public) 继承，而使用 class 继承默认是私有继承 

---

## **<font style="color:#0d0016;">五十三、lambda 表达式的用法？</font>**
#### 1. 什么是 Lambda 表达式？
> **<font style="color:#be191c;background-color:#fef2f0;">Lambda 表达式是一种匿名函数</font>**，**<font style="color:#0d0016;background-color:#f9eda6;">允许你在需要函数的地方定义和使用内联函数</font>**，尤其适合用于短小的函数逻辑。语法格式如下：
>

```plain
[捕获列表](参数列表) -> 返回类型 {



    函数体



};
```

+ **捕获列表**：定义 lambda 函数**<font style="color:#be191c;background-color:#fef2f0;">如何访问外部作用域的变量</font>**。
+ **参数列表**：传递给 lambda **<font style="color:#be191c;background-color:#fef2f0;">函数的参数</font>**。
+ **返回类型**（可选）：可以省略，编译器会根据返回值推断类型。
+ **函数体**：**<font style="color:#be191c;background-color:#fef2f0;">实际执行的代码</font>**。

#### 2. Lambda 表达式的优点
+ **简洁性**：无需为短小的操作单独写一个函数，直接就地定义，代码更紧凑。
+ **可读性**：lambda 表达式常常用于回调函数或算法操作，使得代码更容易理解。
+ **灵活的捕获外部变量**：通过捕获列表，可以灵活访问函数外的变量，而不需要额外的传参。
+ **匿名特性**：不需要为临时使用的函数命名，非常适合一些一次性的操作。

#### 3. 应用场景
+ **STL 算法**：在使用 `std::sort`等 STL 算法时，lambda 表达式使得定义自定义操作非常简单。
+ **回调函数**：处理事件驱动或异步编程中的回调函数时，lambda 表达式可以简化代码。
+ **函数对象替代**：lambda 可以作为函数对象的替代，尤其适用于简单的逻辑。
+ **并发编程**：在多线程编程中，lambda 常被用于线程函数的定义，避免了额外的函数声明。

#### 4. 优化性能
+ **内联特性**：lambda **<font style="color:#be191c;">表达式由于可以直接内联</font>**，**<font style="color:#be191c;">减少了函数调用的开销</font>**，因此有时比使用普通函数性能更高。

#### 1. 对数组排序的例子
在这个例子中，我们将对一个整数数组进行降序排序：

```plain
#include <iostream>



#include <vector>



#include <algorithm>



 



int main() {



    std::vector<int> nums = {5, 2, 8, 1, 4};



 



    // 使用 lambda 表达式进行降序排序



    std::sort(nums.begin(), nums.end(), [](int a, int b) {



        return a > b;  // 降序



    });



 



    // 输出排序后的数组



    for (int n : nums) {



        std::cout << n << " ";



    }



    std::cout << std::endl;



 



    return 0;



}
```

#### 2. 对字符串数组排序的例子
在这个例子中，我们将根据字符串的长度对字符串数组进行升序排序：

---

## **<font style="color:#0d0016;">五十四、什么是内联函数？</font>**
> **<font style="color:#be191c;background-color:#fef2f0;">内联函数</font>**（`inline` function）是通过**<font style="color:#be191c;background-color:#fef2f0;">在函数定义前加上关键字 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">inline</font>**`来建议编译器**<font style="color:#be191c;background-color:#fef2f0;">将函数代码直接插入到每个调用它的地方</font>**，**<font style="color:#0d0016;background-color:#f9eda6;">而不是通过函数调用机制</font>**。这样可以**<font style="color:#956fe7;">减少函数调用时的开销</font>****<font style="color:#0d0016;">。内联函数通常用于那些频繁调用、代码短小的函数。 </font>**
>

```plain
inline int add(int a, int b) {



    return a + b;



}



 



int main() {



    int result = add(3, 4);  // 编译时，add(3, 4) 会直接展开为 3 + 4



    return 0;



}
```

#### 2. 内联函数的优点
+ **减少函数调用的开销**：内联函数避免了**<font style="color:#be191c;background-color:#fef2f0;">传统函数调用时压栈、出栈等开销</font>**，特别是在频繁调用的小函数中效果显著。
+ **代码优化**：编译器在内联展开时可以进行一些额外的优化，比如消除无用代码或常量折叠。
+ **方便调试**：内联函数保留了函数的语义，便于调试和维护，同时也能享受代码简化的好处。

#### 3. 内联函数的限制
+ **不能过于复杂**：内联函数应保持简短，如果函数体过于复杂，编译器通常会忽略 `inline` 建议，拒绝内联。
+ **递归函数通常不会内联**：递归函数无法内联，因为内联展开会导致无限递归的展开。
+ **编译器优化决定权**：即使使用了 `inline` 关键字，是否实际内联仍然由编译器决定，并非强制要求。

---

## **<font style="color:#0d0016;">五十五、虚函数表存在什么区域</font>**
#### 1. **虚函数表的存储区域**
虚函数表是存储在 **<font style="color:#be191c;background-color:#fef2f0;">静态内存区域（静态存储区）</font>**的。这是因为每个类的虚函数表是由编译器在编译期创建的，并且该表对于某个类的所有对象是共享的。因此，**<font style="color:#be191c;background-color:#fef2f0;">虚函数表被存储在静态内存区域（静态数据区），而不是在堆或栈中。</font>**

+ **静态内存区域**：包含**<font style="color:#be191c;background-color:#fef2f0;">静态变量、全局变量和常量</font>**，虚函数表因为其与类相关联，而不是与对象相关联，所以位于这个区域中。

#### 2. **虚表指针（vptr）的位置**
虽然虚函数表本身存储在静态区域中，但每个包含虚函数的对象会有一个隐藏的指针（称为 **虚表指针**，即 `vptr`），指向该类的虚函数表。这**<font style="color:#be191c;background-color:#fef2f0;">个指针存储在对象的内存中</font>**，通常在对象的头部。

---

## **<font style="color:#0d0016;">五十六、动态连接和静态链接的区别</font>**
> 动态链接和静态链接是程序编译和链接过程中使用的两种方式，它们的区别主要在于库的加载和程序的大小。以下是它们的详细对比： 
>

#### 1. **静态链接 (Static Linking)**
+ **定义**: 在编译时，将所有需要的库文件与程序的目标文件链接在一起，生成一个包含所有代码的可执行文件。
+ **库的加载**: 库在编译时就嵌入到可执行文件中，程序运行时不再需要外部库。
+ **优点**: 
    - 程序可以独立运行，不依赖外部库。
    - 运行时速度较快，因为不需要动态加载库。
+ **缺点**: 
    - 可执行文件较大，因为每个程序都要包含库的所有代码。
    - 当库发生变化时，需要重新编译所有使用该库的程序。

#### 2. **动态链接 (Dynamic Linking)**
+ **定义**: 在编译时不将库嵌入可执行文件，而是在程序运行时动态加载所需的库文件。
+ **库的加载**: 库文件在程序运行时由操作系统加载。
+ **优点**: 
    - 可执行文件较小，因为不需要包含库的代码。
    - 库可以被多个程序共享，减少内存使用。
    - 当库更新时，不需要重新编译程序，程序可以自动使用新版本的库（前提是库的接口保持不变）。
+ **缺点**: 
    - 程序依赖于外部库，缺少这些库时无法运行。
    - 运行时加载库会有一些性能开销。

#### 总结
+ **静态链接**更适合那些希望生成独立的、无需依赖外部库的可执行文件的场景，比如某些嵌入式系统或需要在环境中分发无需额外依赖的软件。
+ **动态链接**适合需要减少可执行文件大小、共享库资源、并希望在库更新时自动受益的场景，常见于现代桌面操作系统和服务器环境。

---

## **<font style="color:#0d0016;">五十七、MySQL中什么是索引 ？</font>**
> 在 MySQL 中，**索引**是一种用于**<font style="color:#be191c;background-color:#fef2f0;">快速查找数据库表中记录的数据结构</font>**。**<font style="color:#0d0016;background-color:#f9eda6;">索引类似于一本书的目录，可以加速查询操作，但也会增加维护成本</font>**。索引在数据库优化中扮演着关键角色，特别是在涉及大量数据的查询时。
>

### 建立索引的原则是什么？ 
> 建立索引时，**<font style="color:#be191c;background-color:#fef2f0;">需要遵循一些原则，以确保索引能够有效提高查询效率</font>**，同时避免过度使用索引带来的维护开销。以下是一些常见的建立索引的原则： 
>

#### 1. **选择性高的列优先创建索引**
+ **原则**: 对选择性高的列创建索引，所谓选择性高指的是该列中的值有较高的唯一性。
+ **原因**: 选择性高的列能有效过滤更多行，减少扫描的数据量，从而提高查询效率。
+ **例子**: 在一个有100万行的表中，如果`**<font style="color:#be191c;background-color:#fef2f0;">user_id</font>**`列几乎没有重复值（例如身份证号），那么为 `user_id` 创建索引可以显著提高查询性能。而如果为性别（`gender`，只有 "男" 和 "女"）创建索引，效果会不明显。

#### 2. **经常出现在 **`**WHERE**`** 子句中的列**
+ **原则**: 为 `**<font style="color:#be191c;background-color:#fef2f0;">WHERE</font>**` 子句中经常出现的列创建索引。
+ **原因**: 查询时，数据库需要通过`**<font style="color:#be191c;background-color:#fef2f0;">WHERE</font>**`条件筛选数据。为这些列建立索引可以加速筛选过程，避免全表扫描。
+ **例子**: 对于经常按日期筛选记录的查询，可以为日期字段（如 `created_at`）建立索引。

#### 3. **在 **`**JOIN**`** 和 **`**GROUP BY**`** 中使用的列**
+ **原则**: 为`**<font style="color:#be191c;background-color:#fef2f0;">JOIN</font>**`**<font style="color:#be191c;background-color:#fef2f0;">、</font>**`**<font style="color:#be191c;background-color:#fef2f0;">ORDER BY</font>**`**<font style="color:#be191c;background-color:#fef2f0;">、</font>**`**<font style="color:#be191c;background-color:#fef2f0;">GROUP BY</font>**`等操作中使用的列创建索引。
+ **原因**: 联表查询和分组操作都需要在多表之间匹配或者进行排序、分组操作。创建索引能加速这些操作。
+ **例子**: 当两个表通过 `user_id` 进行 `JOIN` 时，给 `user_id` 建立索引可以显著提高联表查询效率。

#### 4. **避免为频繁更新的列创建索引**
+ **原则**: 尽量避免为频繁变动的列创建索引。
+ **原因**: 每次对表进行 `**<font style="color:#be191c;background-color:#fef2f0;">INSERT</font>**`**<font style="color:#be191c;background-color:#fef2f0;">、</font>**`**<font style="color:#be191c;background-color:#fef2f0;">UPDATE</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 或 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">DELETE</font>**` 操作时，索引也需要更新，增加了额外的维护开销。因此，为经常变化的列创建索引会影响数据库的写入性能。
+ **例子**: 对于库存系统中的 `**<font style="background-color:#fef2f0;">stock_quantity</font>**`列（库存数量），由于它可能频繁更新，不建议为其创建索引。

#### 5. **避免为小表创建过多索引**
+ **原则**: 小表不应过度使用索引，甚至有时可以不创建索引。
+ **原因**: 对于行数较少的表，全表扫描的效率可能与索引扫描相差无几，索引的维护成本可能超过其带来的性能提升。
+ **例子**: 如果一个表只有几百行，查询时全表扫描的速度已经很快，不需要专门为其创建索引。

---

## <font style="color:#0d0016;">五十八、什么是事务？ </font>
> **事务**（Transaction）是**<font style="color:#be191c;background-color:#fef2f0;">数据库管理系统（DBMS）</font>**中用于保证<font style="color:#be191c;background-color:#fef2f0;">数据一致性和完整性的一种机制</font>。事务是一组被看作一个单一逻辑单元的操作，这些操作要么全都成功，要么全都失败。事务可以确保在并发环境中，多个用户或应用程序对数据库的操作不会导致数据不一致。 
>

#### 1. **事务的四大特性（ACID）**
事务有四个关键特性，简称为 **ACID**，它们是数据库事务的核心保障。

+ **原子性（Atomicity）**:
    - **定义**: 事务是一个不可分割的整体，要么完全执行成功，要么完全回滚失败，不会停留在中间状态。
    - **举例**: 假设有两个操作，一个从账户 A 转账到账户 B，另一个是从账户 B 接收金额。事务确保如果任何一个操作失败，整个转账操作将回滚，保证 A 和 B 的余额不会不一致。
+ **一致性（Consistency）**:
    - **定义**: 事务执行前后，数据库都必须处于一致的状态。即执行事务不会破坏数据库的完整性规则。
    - **举例**: 在银行转账中，无论事务执行成功与否，A 和 B 账户的余额总和在操作前后应该是一样的，不会凭空增减。
+ **隔离性（Isolation）**:
    - **定义**: 并发事务之间相互隔离，互不影响。一个事务的执行过程对其他事务是不可见的，直到该事务提交。
    - **举例**: 在一个用户执行转账的过程中，另一个用户查询账户余额时，不会看到未提交的转账操作，避免脏数据的读取。
+ **持久性（Durability）**:
    - **定义**: 一旦事务提交，它对数据库所做的变更将永久保存在数据库中，即使系统崩溃也不会丢失这些修改。
    - **举例**: 当用户在网上银行完成一笔转账并提交后，即便系统崩溃，用户稍后查询到的余额依然会包含这笔转账的变动。

#### 2. **事务的控制语句**
在 MySQL 等数据库中，常用的事务控制语句包括：

+ `**<font style="color:#be191c;background-color:#fef2f0;">START TRANSACTION</font>**`**<font style="color:#be191c;background-color:#fef2f0;">:</font>** 开始一个事务。
+ `**<font style="color:#be191c;background-color:#fef2f0;">COMMIT</font>**`: 提交事务，将事务中的所有操作持久化到数据库中。
+ `**<font style="color:#be191c;background-color:#fef2f0;">ROLLBACK</font>**`: 回滚事务，撤销事务中的所有操作。
+ `**<font style="color:#be191c;background-color:#fef2f0;">SAVEPOINT</font>**`: 设置事务中的一个保存点，可以部分回滚到某个保存点，而不是整个事务。
+ `**<font style="color:#be191c;background-color:#fef2f0;">SET TRANSACTION</font>**`**<font style="color:#be191c;background-color:#fef2f0;">:</font>** 设置事务的隔离级别。

#### 3. **事务的使用**
事务通常用于多个数据库操作需要作为一个整体执行时。例如银行系统中的转账操作，涉及多个步骤：

1. 从账户 A 扣款。
2. 向账户 B 存款。

这两个步骤必须保证要么全部完成，要么全部不完成（回滚）。因此，可以将它们作为一个事务来执行：

```plain
START TRANSACTION;



 



-- 扣款操作



UPDATE accounts SET balance = balance - 100 WHERE account_id = 'A';



 



-- 存款操作



UPDATE accounts SET balance = balance + 100 WHERE account_id = 'B';



 



COMMIT;
```

如果在执行过程中发生错误，可以使用 `ROLLBACK` 来回滚整个事务，保证数据一致性： 

```plain
ROLLBACK;
```

#### 4. **事务隔离级别**
**<font style="color:#be191c;background-color:#fef2f0;">事务的隔离性决定了并发事务之间的相互影响程度。</font>**数据库系统通常提供四种事务隔离级别，不同的隔离级别可以防止不同类型的并发问题：

+ **读未提交（Read Uncommitted）**: 事务可以读取其他未提交事务的修改，可能导致“脏读”问题。
+ **读已提交（Read Committed）**: 只能读取其他事务已经提交的数据，避免脏读。
+ **<font style="color:#be191c;">可重复读（Repeatable Read）</font>**<font style="color:#be191c;">: </font>在同一个事务中，多次读取同一数据时保证读取到的数据一致，避免“不可重复读”。
+ **序列化（Serializable）**: 最严格的隔离级别，事务完全隔离，相当于所有事务依次执行，避免所有并发问题，但性能最低。

> 最常用的级别是 **<font style="color:#be191c;">Repeatable Read ，因为它平衡了性能与一致性，同时也是MySQL的默认隔离级别</font>**
>

#### 总结
事务是数据库系统中确保数据一致性、完整性和安全性的重要机制。通过 ACID 特性，事务能够在并发环境中保护数据，并在发生错误时回滚变更，避免数据损坏。事务广泛应用于金融系统、订单处理系统等需要保证数据一致性的场景中。

---

## **<font style="color:#0d0016;">五十九、什么叫慢查询？</font>**
> 慢查询是指在数据库中执行时间过长，响应速度慢的查询操作。具体的时间阈值可以根据系统的具体需求进行定义。 
>

例如：

如果一条查询语句执行时间超过了设定的阈值（比如1秒），那么就可以将这条查询语句标记为慢查询。

> 慢查询的原因，可能包括 数据量过大、数据库设计不合理、索引问题等。
>

## **<font style="color:#0d0016;">六十、MySQL 是 主要通过什么数据结构实现？为什么用B+树？</font>**
> MySQL 索引主要通过 B+树数据结构实现的。 
>

原因：

1. 高效的读写：对于数据库读写操作频繁的场景，B+树平衡性能较好，读写性能稳定。

2. 范围查询优势：B+树支持范围擦汗寻，hash表只支持精确查询，而二叉树效率地下。

3. 磁盘读写优化：B+树的节点大小通常和磁盘扇区大小相同，这样可以最大化磁盘I/O的效率

4. 磁盘减少I/O次数：B+树的分支因子大，树的层级较低，可以减少在查询过程中磁盘的I/O的次数 

---

## **<font style="color:#0d0016;">六十一、C++ 中 volatile 的作用</font>**
> 在 C++ 中，`volatile` 关键字的主要作用是告诉编译器，**<font style="color:#be191c;background-color:#fef2f0;">被 </font>**`**<font style="color:#be191c;background-color:#fef2f0;">volatile</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 修饰的变量可能会在程序执行的过程中被外部环境（如硬件设备、中断、其他线程等）修改，因此编译器不应对该变量进行优化。 </font>**
>

```plain
#include <stdio.h>



#include <stdlib.h>



#include <pthread.h>



 



volatile int counter = 0;



 



void *increment(void *arg) {



    for (int i = 0; i < 100000; i++) {



        counter++;



    }



    return NULL;



}



 



int main() {



    pthread_t thread1, thread2;



 



    // 创建两个线程，分别执行increment函数



    pthread_create(&thread1, NULL, increment, NULL);



    pthread_create(&thread2, NULL, increment, NULL);



 



    // 等待两个线程执行完毕



    pthread_join(thread1, NULL);



    pthread_join(thread2, NULL);



 



    printf("Counter: %d\n", counter);



 



    return 0;



}
```

+ 这个代码中使用了 `volatile` 关键字来修饰全局变量 `counter`，并且创建了两个线程，分别执行 `increment()` 函数对 `counter` 进行递增操作。 

#### 输出结果的潜在问题
理论上，`counter` 最终应该输出 `200000`（每个线程递增 `100000` 次）。然而，由于多个线程同时对 `counter` 进行非原子操作，可能会出现**竞态条件**（race condition），导致结果比预期值要小。例如，两个线程同时读取了相同的 `counter` 值，结果都将其加一，然后写回内存。这种情况下，实际的递增只发生了一次，而不是两次。

#### `volatile` 的作用
`volatile` 关键字在这个代码中的作用是**告诉编译器**，`counter` 的值可能会被其他线程修改，因此每次都应该从内存中读取 `counter`，而不能缓存这个变量的值。这意味着即使一个线程对 `counter` 的值进行了修改，另一个线程也能正确读取到它的最新值。

**但是，**`**volatile**`** 只能防止编译器对变量进行优化，不能解决竞态条件问题**。线程操作 `counter++` 是一个非原子操作，包含了多个步骤（读取、增加、写回）。即使每次都读取到最新的值，由于线程之间并没有同步机制，多个线程仍然可能同时读取到相同的值，导致最终结果不正确。

#### 总结
+ `volatile` 关键字可以防止编译器对变量 `counter` 进行优化，确保每次都从内存读取最新的值，但这**不能**解决多线程并发访问时的竞态条件问题。
+ 要解决竞态条件问题，需要使用同步机制，比如使用 `mutex` 来保护对 `counter` 的访问，使得每次递增操作是一个原子操作。

> 在编程中，`**<font style="color:#be191c;background-color:#fef2f0;">volatile</font>**`**<font style="color:#be191c;background-color:#fef2f0;"> 就是用来提醒编译器，某些变量可能会被外部因素改变，每次都要从内存中读取最新的值，不能仅依赖上次的缓存。</font>** 
>

![](/images/a76778a72c7cc08229303c4c644878f0.png)  


> 来自: [【C/C++】 秋招常考面试题最全总结（让你有一种相见恨晚的感觉）_c++面试题 由浅及深-CSDN博客](https://xas-sunny.blog.csdn.net/article/details/140565534)
>

