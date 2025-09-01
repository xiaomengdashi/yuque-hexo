---
title: '[C++]掌握C++异步编程-CSDN博客'
date: '2025-04-29 01:49:37'
updated: '2025-06-09 23:49:20'
---
![]()

#### 💻文章目录
+ [📄前言](https://blog.csdn.net/CaTianRi/article/details/137866576#_10)
+ [异步任务](https://blog.csdn.net/CaTianRi/article/details/137866576#_13)
    - [概念](https://blog.csdn.net/CaTianRi/article/details/137866576#_14)
    - [期待与承诺](https://blog.csdn.net/CaTianRi/article/details/137866576#_27)
        * [future](https://blog.csdn.net/CaTianRi/article/details/137866576#future_29)
        * [promise](https://blog.csdn.net/CaTianRi/article/details/137866576#promise_53)
        * [异常处理](https://blog.csdn.net/CaTianRi/article/details/137866576#_78)
    - [执行异步任务](https://blog.csdn.net/CaTianRi/article/details/137866576#_119)
        * [async](https://blog.csdn.net/CaTianRi/article/details/137866576#async_120)
        * [packaged_task](https://blog.csdn.net/CaTianRi/article/details/137866576#packaged_task_180)
+ [📓总结](https://blog.csdn.net/CaTianRi/article/details/137866576#_207)

---

## 📄前言
> [异步任务](https://so.csdn.net/so/search?q=%E5%BC%82%E6%AD%A5%E4%BB%BB%E5%8A%A1&spm=1001.2101.3001.7020)是多线程编程的核心，若想学习多线程设计，深入了解这些基本概念是必不可少的。如果你从未了解过这些概念，亦或者对c++异步任务的库函数有所遗忘了，不妨点进本文来学习一下。
>

## 异步任务
### 概念
异步任务就是在程序后台执行的任务，而异步指的是与主线程所不同步奏。用生活的例子来讲就是，我在~~一边看视频，一边吃饭~~。创建一个线程，让它执行一个函数，这就是一个简单的异步任务。你可能会觉得这也太简单了，但先不要走，其实我们要讨论的还不止如此。

C++为异步任务提供的可不止`thread`，还有为了异步任务返回值而诞生的 `期待(future) `与 `承诺(promise)`，以及打包任务所需的 `packaged_task` 和 用更高级的异步任务创建工具—`async`。

**惯例地讨论下优缺点：**

+ **异步任务的优点：**
    - **提高程序的性能**：多个线程并发运行能够显著提高程序的性能（前提是有合理的设计）。
    - **提高程序响应性**：一个程序可能需要等待I/O输入的同时，能够同时处理后台的各种任务（如网络数据传输）。
+ **异步任务的缺点**： 
    - **让程序调试难度提高**：多线程的 bug 可能难以重现、甚至只会在特定的机器出现问题。
    - **可能会导致死锁、竞态条件等问题**：不合理的设计可能会导致程序的性能提高不显著，甚至导致程序无响应。

### 期待与承诺
如果你有使用过 thread 函数，那么你肯定会发现**它是无法通过函数返回值来查看运算结果**，虽然可以通过引用传参来获取返回值，但这样获取的数据在[多线程](https://so.csdn.net/so/search?q=%E5%A4%9A%E7%BA%BF%E7%A8%8B&spm=1001.2101.3001.7020)情况下还得自己解决数据二义性问题，而 **future 和 promise 提供了一种线程安全且方便的返回方式**。

#### future
future 如同其名—期待，期待一个任务结果的获取，我们不需要立刻知道结果是否就绪，只需要在我们需要用到结果时才去访问。如果此时结果还未就绪，线程就会阻塞等待这个结果的获取。

用一个生活的小例子来比喻就是：我和朋友约定去公园一起玩，我在去公园的路上不知道朋友是否已经到达，只有我到了公园才知道，我当然会期待到公园的时候他就已经到达，但如果他还没到公园，我就在此地等待。相信这个例子已经足够说明期待的本质了，**在C++中 future 一般与 async 、promise、package_task 等工具一起使用**。

**简单的函数使用演示：**

```plain
#include <future>
#include <iostream>
#include <thread>
int sum(int x, int y) {
    std::cout << "线程运行ing" << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(3));	
    std::cout << "线程结束" << std::endl;
    return x + y;
}

int main() {
    std::future<int> res = std::async(sum, 1, 9);	
    std::cout << res.get() << std::endl;	
    
    return 0;
}
```

#### promise
promise 与 future 也是一样的“人与其名”，承诺会给 future 一个结果，而这个结果可能会及时得到，也可能会非常晚才得到。**promise 需要与 future 一同使用**，一般通过传参使用。

**函数使用：**

```plain
void func(std::promise<std::string>& ip, std::promise<std::string>& msg) {
    
    std::this_thread::sleep_for(std::chrono::seconds(1));
    ip.set_value("8.8.8.8");    
    msg.set_value("Hello!");         
}

int main() {
    std::promise<std::string> get_ip, get_msg;
    std::future<std::string> ip = get_ip.get_future();	
    std::future<std::string> msg = get_msg.get_future();
    
    std::thread t1(func, std::ref(get_ip), std::ref(get_msg));
    
    t1.detach();	
    printf("[%s] %s", ip.get().c_str(), msg.get().c_str());

    return 0;
}
```

#### 异常处理
**future 和 promise可以用于接受异常**，这也是它们的一大特点之一，如果使用 async 中发生异常，则异常会存储到它的返回值中，当调用 get() 时再次被抛出。当然使用promise也能够设置异常，然后让future 接收。

```plain
int func(int x, int y)
{
    if(y == 0)
        throw std::runtime_error("x / 0");
    else
        return x / y;
}

void errorfunc(std::promise<int>& ret)
{
    try
    {
        int res = func(29, 0);
        ret.set_value(res);
    }catch(const std::runtime_error& e)
    {   
        ret.set_exception(std::current_exception() );
    }
}


int main() {
    try {
        std::promise<int> ret;
        std::future<int> f = ret.get_future();  
        std::thread(errorfunc, std::ref(ret)).detach();
        int result = f.get();   
        std::cout << result << std::endl;
    }catch(const std::runtime_error& e)
    {
        std::cerr << "error: " << e.what() << std::endl;
    }

    return 0;
}
```

### 执行异步任务
#### async
async 是 C++ 中更智能的一种创建线程的方式，它能够**自动管理线程的生命周期，并且自动控制线程的 数量（程序线程过多将不会创建）**，它的**返回值是一个带函数返回值的 future** ，可以用它来得知函数的运行结果 或 函数发生的异常。

+ async 的优点： 
    - 自动管理线程生命周期：线程不需要自己来管理，函数将自己管理。
    - 异常安全：future能接受函数中产生的异常

**函数使用：**

```plain
template <typename Iterator, typename T>
T parallel_accumulate(Iterator first, Iterator last, T init)
{
    
    const unsigned long length = std::distance(first, last);

    const unsigned long min_per_thread = 25; 
    
    if(length <= 25)    
        return std::accumulate(first, last, init); 
    else
    {
        Iterator mid_point = first + length / 2; 
        
        std::future<T> first_half_result = 
                std::async(parallel_accumulate<Iterator, T>, mid_point, last, init);   


        
        return first_half_result.get() + std::accumulate(first, mid_point, T());

    }
}

int main() {
    std::vector<int> nums(100);
    
    std::iota(nums.begin(), nums.end(), 1);
	
    auto sum = parallel_accumulate(nums.begin(), nums.end(), 123);


    return 0;
}



                                          [0-100)
                                             |
                      +-----------------------------------------+
                      |                                         |
                  [0-50)                                 [50-100)
                      |                                         |
          +-------------------+                      +-------------------+
          |                   |                      |                   |
      [0-25)             [25-50)                [50-75)             [75-100)
        |                   |                      |                   |
   std::accumulate   std::accumulate        std::accumulate     std::accumulate
```

#### packaged_task
**packaged_task 是用于打包异步任务的工具，****它可以对****普通函数、类内函数、lambda函数**进行打包，然后在另一个线程中进行运行，经常用于像线程池等需要打包任务的场景。

**简单的函数使用：**

```plain
int func(int x, int y)
{
    return x + y;
}

int main() {
    std::packaged_task<int(int, int)> task1(func);	
    std::packaged_task<int()> task2([&] { return func(2, 3); });	

    std::future<int> res1 = task1.get_future();	
    std::future<int> res2 = task2.get_future();

    std::thread t1(std::move(task1), 9, 9).detach;	
    std::thread t2(std::move(task2) ).detach;

    std::cout << res1.get() << ":" << res2.get() << std::endl;

    return 0;
}
```

## 📓总结
在C++中异步任务常用的工具有future、promise、async、packaged_task等，掌握它们对编写一个高效的多线程至关总要，希望本文能够对你有所帮助。

| 工具 | 用途 |
| --- | --- |
| `std::async` | 用于以简化的方式启动异步任务，自动管理线程生命周期，并自动控制线程数量，返回`std::future`<br/>对象以获取任务的执行结果或异常。 |
| `std::future` | 提供一种机制来访问异步操作的结果。当异步操作完成时，可以通过`std::future`<br/>对象获取结果或捕获在异步操作中抛出的异常。 |
| `std::promise` | 允许在某个线程中设置值或异常，这些值或异常将在未来某个时刻通过与之关联的`std::future`<br/>对象被其他线程访问。 |
| `std::packaged_task` | 封装一个可调用对象，并允许其异步执行，同时提供一个`std::future`<br/>对象，以便获取该可调用对象的返回值或在执行过程中捕获的异常。 |


> 📜博客主页：[主页](https://blog.csdn.net/CaTianRi)  
📫我的专栏：[C++](http://t.csdnimg.cn/stIat)  
📱我的github：[github](https://github.com/CaTianRi)
>

![]()



> 来自: [「C++」掌握C++异步编程-CSDN博客](https://blog.csdn.net/CaTianRi/article/details/137866576)
>





