---
title: 深度剖析（1）：Boost C++ 库，《智能指针》全维度解析
date: '2025-03-09 02:13:28'
updated: '2025-03-09 02:15:30'
---
![](/images/b2714bcae5a06a2b442ad8d4124ae8f7.gif)

<font style="color:rgba(0, 0, 0, 0.9);">Boost C++库是一个由全球顶尖C++开发者共同打造的开源、跨平台的C++库集合，它就像是一个装满了各种珍贵编程工具的百宝箱。这些工具涵盖了从基础的算法、数据结构到高级的并发编程、网络通信，再到文件系统操作等众多领域。无论是想要提升代码的性能，还是追求更高的开发效率，亦或是解决一些标准库难以应对的复杂问题，Boost C++库都能为我们提供有效的解决方案。</font>

<font style="color:rgba(0, 0, 0, 0.9);">它不仅填补了C++标准库的一些空白，还为我们展示了现代C++编程的先进理念和最佳实践。许多特性和功能甚至成为了后续C++标准的灵感来源，逐步被纳入到标准库中。可以说，Boost C++库是C++编程世界中不可或缺的一部分，是每一位C++开发者都值得深入探索和掌握的宝藏。</font>

本篇文章学习目录如下：

![](/images/c476d305f1115599412b7560d1cc7313.png)

<font style="color:rgba(0, 0, 0, 0.9);">1998年修订的第一版C++标准只提供了一种智能指针： </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 。 它基本上就像是个普通的指针： 通过地址来访问一个动态分配的对象。 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 之所以被看作是智能指针，是因为它会在析构的时候调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符来自动释放所包含的对象。 当然这要求在初始化的时候，传给它一个由 </font>`<font style="color:rgba(0, 0, 0, 0.9);">new</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符返回的对象的地址。 既然 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的析构函数会调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符，它所包含的对象的内存会确保释放掉。 这是智能指针的一个优点。</font>

<font style="color:rgba(0, 0, 0, 0.9);">当和异常联系起来时这就更加重要了：没有 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 这样的智能指针，每一个动态分配内存的函数都需要捕捉所有可能的异常，以确保在异常传递给函数的调用者之前将内存释放掉。 Boost C++ 库 </font><font style="color:rgba(0, 0, 0, 0.9);">Smart Pointers</font><font style="color:rgba(0, 0, 0, 0.9);"> 提供了许多可以用在各种场合的智能指针。</font>

## **<font style="color:rgb(255, 255, 255);">二：RAII</font>**
<font style="color:rgba(0, 0, 0, 0.9);">智能指针的原理基于一个常见的习语叫做 RAII ：资源申请即初始化。 智能指针只是这个习语的其中一例——当然是相当重要的一例。 智能指针确保在任何情况下，动态分配的内存都能得到正确释放，从而将开发人员从这项任务中解放了出来。 这包括程序因为异常而中断，原本用于释放内存的代码被跳过的场景。 用一个动态分配的对象的地址来初始化智能指针，在析构的时候释放内存，就确保了这一点。 因为析构函数总是会被执行的，这样所包含的内存也将总是会被释放。</font>

<font style="color:rgba(0, 0, 0, 0.9);">无论何时，一定得有第二条指令来释放之前另一条指令所分配的资源时，RAII 都是适用的。 许多的 C++ 应用程序都需要动态管理内存，因而智能指针是一种很重要的 RAII 类型。 不过 RAII 本身是适用于许多其它场景的。</font>

```cpp
#include <windows.h> 


class windows_handle
{
public:
windows_handle(HANDLE h)
: handle_(h)
{
}


~windows_handle()
{
    CloseHandle(handle_);
}


HANDLE handle() const
{
    return handle_;
}


private:
HANDLE handle_;
};


int main()
{
    windows_handle h(OpenProcess(PROCESS_SET_INFORMATION, FALSE, GetCurrentProcessId()));
    SetPriorityClass(h.handle(), HIGH_PRIORITY_CLASS);
}
```

<font style="color:rgba(0, 0, 0, 0.9);">上面的例子中定义了一个名为 </font>`<font style="color:rgba(0, 0, 0, 0.9);">windows_handle</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的类，它的析构函数调用了 </font>`<font style="color:rgba(0, 0, 0, 0.9);">CloseHandle()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数。 这是一个 Windows API 函数，因而这个程序只能在 Windows 上运行。 在 Windows 上，许多资源在使用之前都要求打开。 这暗示着一旦资源不再使用之后就应该关闭。 </font>`<font style="color:rgba(0, 0, 0, 0.9);">windows_handle</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类的机制能确保这一点。</font>

`<font style="color:rgba(0, 0, 0, 0.9);">windows_handle</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类的实例以一个句柄来初始化。 Windows 使用句柄来唯一的标识资源。 比如说，</font>`<font style="color:rgba(0, 0, 0, 0.9);">OpenProcess()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数返回一个 </font>`<font style="color:rgba(0, 0, 0, 0.9);">HANDLE</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类型的句柄，通过该句柄可以访问当前系统中的进程。 在示例代码中，访问的是进程自己——换句话说就是应用程序本身。</font>

<font style="color:rgba(0, 0, 0, 0.9);">我们通过这个返回的句柄提升了进程的优先级，这样它就能从调度器那里获得更多的 CPU 时间。 这里只是用于演示目的，并没什么实际的效应。 重要的一点是：通过 </font>`<font style="color:rgba(0, 0, 0, 0.9);">OpenProcess()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 打开的资源不需要显示的调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">CloseHandle()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 来关闭。 当然，应用程序终止时资源也会随之关闭。 然而，在更加复杂的应用程序里， </font>`<font style="color:rgba(0, 0, 0, 0.9);">windows_handle</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类确保当一个资源不再使用时就能正确的关闭。 某个资源一旦离开了它的作用域——上例中 </font><font style="color:rgba(0, 0, 0, 0.9);">h</font><font style="color:rgba(0, 0, 0, 0.9);"> 的作用域在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">main()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数的末尾——它的析构函数会被自动的调用，相应的资源也就释放掉了。</font>

## **<font style="color:rgb(255, 255, 255);">三：作用域指针</font>**
<font style="color:rgba(0, 0, 0, 0.9);">一个作用域指针独占一个动态分配的对象。 对应的类名为 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">，它的定义在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost/scoped_ptr.hpp</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 中。 不像 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">，一个作用域指针不能传递它所包含的对象的所有权到另一个作用域指针。 一旦用一个地址来初始化，这个动态分配的对象将在析构阶段释放。</font>

<font style="color:rgba(0, 0, 0, 0.9);">因为一个作用域指针只是简单保存和独占一个内存地址，所以 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的实现就要比 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 简单。 在不需要所有权传递的时候应该优先使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 。 在这些情况下，比起 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 它是一个更好的选择，因为可以避免不经意间的所有权传递。</font>

```cpp
#include <boost/scoped_ptr.hpp> 
int main() 
{ 
    boost::scoped_ptr<int> i(new int); 
    *i = 1; 
    *i.get() = 2; 
    i.reset(new int); 
}
```

<font style="color:rgba(0, 0, 0, 0.9);">一经初始化，智能指针 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 所包含的对象，可以通过类似于普通指针的接口来访问。 这是因为重载了相关的操作符 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator*()</font>`<font style="color:rgba(0, 0, 0, 0.9);">，</font>`<font style="color:rgba(0, 0, 0, 0.9);">operator->()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator bool()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 。 此外，还有 </font>`<font style="color:rgba(0, 0, 0, 0.9);">get()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 方法。 前者返回所含对象的地址，后者用一个新的对象来重新初始化智能指针。 在这种情况下，新创建的对象赋值之前会先自动释放所包含的对象。</font>

`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的析构函数中使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符来释放所包含的对象。 这对 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 所包含的类型加上了一条重要的限制。 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 不能用动态分配的数组来做初始化，因为这需要调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete[]</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 来释放。 在这种情况下，可以使用下面将要介绍的 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost:scoped_array</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类。</font>

## **<font style="color:rgb(255, 255, 255);">四：作用域数组</font>**
<font style="color:rgba(0, 0, 0, 0.9);">作用域数组的使用方式与作用域指针相似。 关键不同在于，作用域数组的析构函数使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete[]</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符来释放所包含的对象。 因为该操作符只能用于数组对象，所以作用域数组必须通过动态分配的数组来初始化。</font>

<font style="color:rgba(0, 0, 0, 0.9);">对应的作用域数组类名为 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_array</font>`<font style="color:rgba(0, 0, 0, 0.9);">，它的定义在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost/scoped_array.hpp</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 里。</font>

```cpp
#include <boost/scoped_array.hpp> 
int main() 
{ 
    boost::scoped_array<int> i(new int[2]); 
    *i.get() = 1; 
    i[1] = 2; 
    i.reset(new int[3]); 
}
```

`<font style="color:rgba(0, 0, 0, 0.9);">boost:scoped_array</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类重载了操作符 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator[]()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator bool()</font>`<font style="color:rgba(0, 0, 0, 0.9);">。 可以通过 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator[]()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符访问数组中特定的元素，于是 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_array</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类型对象的行为就酷似它所含的数组。</font>

<font style="color:rgba(0, 0, 0, 0.9);">正如 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 那样, </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost:scoped_array</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 也提供了 </font>`<font style="color:rgba(0, 0, 0, 0.9);">get()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 方法，用来返回和重新初始化所含对象的地址。</font>

## **<font style="color:rgb(255, 255, 255);">五：共享指针</font>**
<font style="color:rgba(0, 0, 0, 0.9);">这是使用率最高的智能指针，但是 C++ 标准的第一版中缺少这种指针。 它已经作为技术报告1（TR 1）的一部分被添加到标准里了。 如果开发环境支持的话，可以使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">memory</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 中定义的 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">。 在 Boost C++ 库里，这个智能指针命名为 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">，定义在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost/shared_ptr.hpp</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 里。</font>

<font style="color:rgba(0, 0, 0, 0.9);">智能指针 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 基本上类似于 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">。 关键不同之处在于 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 不一定要独占一个对象。 它可以和其他 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类型的智能指针共享所有权。 在这种情况下，当引用对象的最后一个智能指针销毁后，对象才会被释放。</font>

<font style="color:rgba(0, 0, 0, 0.9);">因为所有权可以在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 之间共享，任何一个共享指针都可以被复制，这跟 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 是不同的。 这样就可以在标准容器里存储智能指针了——你不能在标准容器中存储 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">，因为它们在拷贝的时候传递了所有权。</font>

```cpp
#include <boost/shared_ptr.hpp> 
#include <vector> 
int main() 
{ 
    std::vector<boost::shared_ptr<int> > v; 
    v.push_back(boost::shared_ptr<int>(new int(1))); 
    v.push_back(boost::shared_ptr<int>(new int(2))); 
}
```

<font style="color:rgba(0, 0, 0, 0.9);">多亏了有 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">，我们才能像上例中展示的那样，在标准容器中安全的使用动态分配的对象。 因为 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 能够共享它所含对象的所有权，所以保存在容器中的拷贝（包括容器在需要时额外创建的拷贝）都是和原件相同的。如前所述，</font>`<font style="color:rgba(0, 0, 0, 0.9);">std::auto_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">做不到这一点，所以绝对不应该在容器中保存它们。</font>

<font style="color:rgba(0, 0, 0, 0.9);">类似于 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::scoped_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">， </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类重载了以下这些操作符：</font>`<font style="color:rgba(0, 0, 0, 0.9);">operator*()</font>`<font style="color:rgba(0, 0, 0, 0.9);">，</font>`<font style="color:rgba(0, 0, 0, 0.9);">operator->()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator bool()</font>`<font style="color:rgba(0, 0, 0, 0.9);">。另外还有 </font>`<font style="color:rgba(0, 0, 0, 0.9);">get()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数来获取和重新初始化所包含的对象的地址。</font>

```cpp
#include <boost/shared_ptr.hpp> 
int main() 
{ 
    boost::shared_ptr<int> i1(new int(1)); 
    boost::shared_ptr<int> i2(i1); 
    i1.reset(new int(2)); 
}
```

<font style="color:rgba(0, 0, 0, 0.9);">本例中定义了2个共享指针 </font><font style="color:rgba(0, 0, 0, 0.9);">i1</font><font style="color:rgba(0, 0, 0, 0.9);">和</font><font style="color:rgba(0, 0, 0, 0.9);">i2</font><font style="color:rgba(0, 0, 0, 0.9);">，它们都引用到同一个</font>`<font style="color:rgba(0, 0, 0, 0.9);">int</font>`<font style="color:rgba(0, 0, 0, 0.9);">类型的对象。</font><font style="color:rgba(0, 0, 0, 0.9);">i1</font><font style="color:rgba(0, 0, 0, 0.9);">通过</font>`<font style="color:rgba(0, 0, 0, 0.9);">new</font>`<font style="color:rgba(0, 0, 0, 0.9);">操作符返回的地址显示的初始化，</font><font style="color:rgba(0, 0, 0, 0.9);">i2</font><font style="color:rgba(0, 0, 0, 0.9);">通过</font><font style="color:rgba(0, 0, 0, 0.9);">i1</font><font style="color:rgba(0, 0, 0, 0.9);">拷贝构造而来。</font><font style="color:rgba(0, 0, 0, 0.9);">i1</font><font style="color:rgba(0, 0, 0, 0.9);">接着调用</font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);">，它所包含的整数的地址被重新初始化。不过它之前所包含的对象并没有被释放，因为</font><font style="color:rgba(0, 0, 0, 0.9);">i2</font><font style="color:rgba(0, 0, 0, 0.9);">仍然引用着它。 智能指针</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 记录了有多少个共享指针在引用同一个对象，只有在最后一个共享指针销毁时才会释放这个对象。</font>

<font style="color:rgba(0, 0, 0, 0.9);">默认情况下，</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符来销毁所含的对象。 然而，具体通过什么方法来销毁，是可以指定的，就像下面的例子里所展示的：</font>

```cpp
#include <boost/shared_ptr.hpp> 
#include <windows.h> 
int main() 
{ 
    boost::shared_ptr<void> h(OpenProcess(PROCESS_SET_INFORMATION, FALSE, GetCurrentProcessId()), CloseHandle); 
    SetPriorityClass(h.get(), HIGH_PRIORITY_CLASS); 
}
```

`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的构造函数的第二个参数是一个普通函数或者函数对象，该参数用来销毁所含的对象。 在本例中，这个参数是 Windows API 函数 </font>`<font style="color:rgba(0, 0, 0, 0.9);">CloseHandle()</font>`<font style="color:rgba(0, 0, 0, 0.9);">。 当变量 </font><font style="color:rgba(0, 0, 0, 0.9);">h</font><font style="color:rgba(0, 0, 0, 0.9);"> 超出它的作用域时，调用的是这个函数而不是 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符来销毁所含的对象。 为了避免编译错误，该函数只能带一个 </font>`<font style="color:rgba(0, 0, 0, 0.9);">HANDLE</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类型的参数， </font>`<font style="color:rgba(0, 0, 0, 0.9);">CloseHandle()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 正好符合要求。</font>

<font style="color:rgba(0, 0, 0, 0.9);">该例和本章稍早讲述 RAII 习语时所用的例子的运行是一样的。 然而，本例没有单独定义一个 </font>`<font style="color:rgba(0, 0, 0, 0.9);">windows_handle</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类，而是利用了 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的特性，给它的构造函数传递一个方法，这个方法会在共享指针超出它的作用域时自动调用。</font>

## **<font style="color:rgb(255, 255, 255);">六：共享数组</font>**
<font style="color:rgba(0, 0, 0, 0.9);">共享数组的行为类似于共享指针。 关键不同在于共享数组在析构时，默认使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">delete[]</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符来释放所含的对象。 因为这个操作符只能用于数组对象，共享数组必须通过动态分配的数组的地址来初始化。</font>

<font style="color:rgba(0, 0, 0, 0.9);">共享数组对应的类型是 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_array</font>`<font style="color:rgba(0, 0, 0, 0.9);">，它的定义在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost/shared_array.hpp</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 里。</font>

```cpp
#include <boost/shared_array.hpp> 
#include <iostream> 
int main() 
{ 
    boost::shared_array<int> i1(new int[2]); 
    boost::shared_array<int> i2(i1); 
    i1[0] = 1; 
    std::cout << i2[0] << std::endl; 
}
```

<font style="color:rgba(0, 0, 0, 0.9);">就像共享指针那样，所含对象的所有权可以跟其他共享数组来共享。 这个例子中定义了2个变量 </font><font style="color:rgba(0, 0, 0, 0.9);">i1</font><font style="color:rgba(0, 0, 0, 0.9);"> 和 </font><font style="color:rgba(0, 0, 0, 0.9);">i2</font><font style="color:rgba(0, 0, 0, 0.9);">，它们引用到同一个动态分配的数组。</font><font style="color:rgba(0, 0, 0, 0.9);">i1</font><font style="color:rgba(0, 0, 0, 0.9);"> 通过 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator[]()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 操作符保存了一个整数1——这个整数可以被 </font><font style="color:rgba(0, 0, 0, 0.9);">i2</font><font style="color:rgba(0, 0, 0, 0.9);"> 引用，比如打印到标准输出。</font>

<font style="color:rgba(0, 0, 0, 0.9);">和本章中所有的智能指针一样，</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_array</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 也同样提供了 </font>`<font style="color:rgba(0, 0, 0, 0.9);">get()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 方法。 另外还重载了 </font>`<font style="color:rgba(0, 0, 0, 0.9);">operator bool()</font>`<font style="color:rgba(0, 0, 0, 0.9);">。</font>

## **<font style="color:rgb(255, 255, 255);">七：弱指针</font>**
<font style="color:rgba(0, 0, 0, 0.9);">到目前为止介绍的各种智能指针都能在不同的场合下独立使用。 相反，弱指针只有在配合共享指针一起使用时才有意义。 弱指针 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::weak_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的定义在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost/weak_ptr.hpp</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 里。</font>

```cpp
#include <windows.h> 
#include <boost/shared_ptr.hpp> 
#include <boost/weak_ptr.hpp> 
#include <iostream> 
DWORD WINAPI reset(LPVOID p) 
{ 
    boost::shared_ptr<int> *sh = static_cast<boost::shared_ptr<int>*>(p); 
    sh->reset(); 
    return 0; 
} 
DWORD WINAPI print(LPVOID p) 
{ 
    boost::weak_ptr<int> *w = static_cast<boost::weak_ptr<int>*>(p); 
    boost::shared_ptr<int> sh = w->lock(); 
    if (sh) 
        std::cout << *sh << std::endl; 
    return 0; 
} 
int main() 
{ 
    boost::shared_ptr<int> sh(new int(99)); 
    boost::weak_ptr<int> w(sh); 
    HANDLE threads[2]; 
    threads[0] = CreateThread(0, 0, reset, &sh, 0, 0); 
    threads[1] = CreateThread(0, 0, print, &w, 0, 0); 
    WaitForMultipleObjects(2, threads, TRUE, INFINITE); 
}
```

`<font style="color:rgba(0, 0, 0, 0.9);">boost::weak_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 必定总是通过 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 来初始化的。一旦初始化之后，它基本上只提供一个有用的方法: </font>`<font style="color:rgba(0, 0, 0, 0.9);">lock()</font>`<font style="color:rgba(0, 0, 0, 0.9);">。此方法返回的</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 与用来初始化弱指针的共享指针共享所有权。 如果这个共享指针不含有任何对象，返回的共享指针也将是空的。</font>

<font style="color:rgba(0, 0, 0, 0.9);">当函数需要一个由共享指针所管理的对象，而这个对象的生存期又不依赖于这个函数时，就可以使用弱指针。 只要程序中还有一个共享指针掌管着这个对象，函数就可以使用该对象。 如果共享指针复位了，就算函数里能得到一个共享指针，对象也不存在了。</font>

<font style="color:rgba(0, 0, 0, 0.9);">上例的 </font>`<font style="color:rgba(0, 0, 0, 0.9);">main()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数中，通过 Windows API 创建了2个线程。 于是乎，该例只能在 Windows 平台上编译运行。</font>

<font style="color:rgba(0, 0, 0, 0.9);">第一个线程函数 </font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的参数是一个共享指针的地址。 第二个线程函数 </font>`<font style="color:rgba(0, 0, 0, 0.9);">print()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的参数是一个弱指针的地址。 这个弱指针是之前通过共享指针初始化的。</font>

<font style="color:rgba(0, 0, 0, 0.9);">一旦程序启动之后，</font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">print()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 就都开始执行了。 不过执行顺序是不确定的。 这就导致了一个潜在的问题：</font>`<font style="color:rgba(0, 0, 0, 0.9);">reset()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 线程在销毁对象的时候</font>`<font style="color:rgba(0, 0, 0, 0.9);">print()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 线程可能正在访问它。</font>

<font style="color:rgba(0, 0, 0, 0.9);">通过调用弱指针的 </font>`<font style="color:rgba(0, 0, 0, 0.9);">lock()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数可以解决这个问题：如果对象存在，那么 </font>`<font style="color:rgba(0, 0, 0, 0.9);">lock()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数返回的共享指针指向这个合法的对象。否则，返回的共享指针被设置为0，这等价于标准的null指针。</font>

<font style="color:rgba(0, 0, 0, 0.9);">弱指针本身对于对象的生存期没有任何影响。 </font>`<font style="color:rgba(0, 0, 0, 0.9);">lock()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 返回一个共享指针，</font>`<font style="color:rgba(0, 0, 0, 0.9);">print()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数就可以安全的访问对象了。 这就保证了——即使另一个线程要释放对象——由于我们有返回的共享指针，对象依然存在。</font>

## **<font style="color:rgb(255, 255, 255);">八：介入式指针</font>**
<font style="color:rgba(0, 0, 0, 0.9);">大体上，介入式指针的工作方式和共享指针完全一样。 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 在内部记录着引用到某个对象的共享指针的数量，可是对介入式指针来说，程序员就得自己来做记录。 对于框架对象来说这就特别有用，因为它们记录着自身被引用的次数。</font>

<font style="color:rgba(0, 0, 0, 0.9);">介入式指针 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::intrusive_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 定义在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost/intrusive_ptr.hpp</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 里。</font>

```cpp
#include <boost/intrusive_ptr.hpp> 
#include <atlbase.h> 
#include <iostream> 
void intrusive_ptr_add_ref(IDispatch *p) 
{ 
  p->AddRef(); 
} 
void intrusive_ptr_release(IDispatch *p) 
{ 
  p->Release(); 
} 
void check_windows_folder() 
{ 
  CLSID clsid; 
  CLSIDFromProgID(CComBSTR("Scripting.FileSystemObject"), &clsid); 
  void *p; 
  CoCreateInstance(clsid, 0, CLSCTX_INPROC_SERVER, __uuidof(IDispatch), &p); 
  boost::intrusive_ptr<IDispatch> disp(static_cast<IDispatch*>(p)); 
  CComDispatchDriver dd(disp.get()); 
  CComVariant arg("C:\\Windows"); 
  CComVariant ret(false); 
  dd.Invoke1(CComBSTR("FolderExists"), &arg, &ret); 
  std::cout << (ret.boolVal != 0) << std::endl; 
} 
void main() 
{ 
  CoInitialize(0); 
  check_windows_folder(); 
  CoUninitialize(); 
}
```

<font style="color:rgba(0, 0, 0, 0.9);">上面的例子中使用了 COM（组件对象模型）提供的函数，于是乎只能在 Windows 平台上编译运行。 COM 对象是使用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::intrusive_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 的绝佳范例，因为 COM 对象需要记录当前有多少指针引用着它。 通过调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">AddRef()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">Release()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 函数，内部的引用计数分别增 1 或者减 1。当引用计数为 0 时，COM 对象自动销毁。</font>

<font style="color:rgba(0, 0, 0, 0.9);">在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">intrusive_ptr_add_ref()</font>`<font style="color:rgba(0, 0, 0, 0.9);">和</font>`<font style="color:rgba(0, 0, 0, 0.9);">intrusive_ptr_release()</font>`<font style="color:rgba(0, 0, 0, 0.9);">内部调用</font>`<font style="color:rgba(0, 0, 0, 0.9);">AddRef()</font>`<font style="color:rgba(0, 0, 0, 0.9);">和</font>`<font style="color:rgba(0, 0, 0, 0.9);">Release()</font>`<font style="color:rgba(0, 0, 0, 0.9);">这两个函数，来增加或减少相应 COM 对象的引用计数。 这个例子中用到的 COM 对象名为 'FileSystemObject'，在 Windows 上它是默认可用的。通过这个对象可以访问底层的文件系统，比如检查一个给定的目录是否存在。 在上例中，我们检查</font>`<font style="color:rgba(0, 0, 0, 0.9);">C:\Windows</font>`<font style="color:rgba(0, 0, 0, 0.9);">目录是否存在。 具体它在内部是怎么实现的，跟</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::intrusive_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">的功能无关，完全取决于 COM。 关键点在于一旦介入式指针</font><font style="color:rgba(0, 0, 0, 0.9);">disp</font><font style="color:rgba(0, 0, 0, 0.9);">离开了它的作用域——</font>`<font style="color:rgba(0, 0, 0, 0.9);">check_windows_folder()</font>`<font style="color:rgba(0, 0, 0, 0.9);">函数的末尾，函数</font>`<font style="color:rgba(0, 0, 0, 0.9);">intrusive_ptr_release()</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 将会被自动调用。 这将减少 COM 对象 'FileSystemObject' 的内部引用计数到0，于是该对象就销毁了。</font>

## **<font style="color:rgb(255, 255, 255);">九：指针容器</font>**
<font style="color:rgba(0, 0, 0, 0.9);">在你见过Boost C++库的各种智能指针之后，应该能够编写安全的代码，来使用动态分配的对象和数组。多数时候，这些对象要存储在容器里，如上所述——使用</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);">和</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_array</font>`<font style="color:rgba(0, 0, 0, 0.9);">这就相当简单了。</font>

```cpp
#include <boost/shared_ptr.hpp> 
#include <vector> 
int main() 
{ 
  std::vector<boost::shared_ptr<int> > v; 
  v.push_back(boost::shared_ptr<int>(new int(1))); 
  v.push_back(boost::shared_ptr<int>(new int(2))); 
}
```

<font style="color:rgba(0, 0, 0, 0.9);">上面例子中的代码当然是正确的，智能指针确实可以这样用，然而因为某些原因，实际情况中并不这么用。 第一，反复声明 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 需要更多的输入。 其次，将 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 拷进，拷出，或者在容器内部做拷贝，需要频繁的增加或者减少内部引用计数，这肯定效率不高。 由于这些原因，Boost C++ 库提供了 </font><font style="color:rgba(0, 0, 0, 0.9);">指针容器</font><font style="color:rgba(0, 0, 0, 0.9);"> 专门用来管理动态分配的对象。</font>

```cpp
#include <boost/ptr_container/ptr_vector.hpp> 
int main() 
{ 
  boost::ptr_vector<int> v; 
  v.push_back(new int(1)); 
  v.push_back(new int(2)); 
}
```

`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_vector</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 类的定义在 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost/ptr_container/ptr_vector.hpp</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 里，它跟前一个例子中用 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::shared_ptr</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 模板参数来初始化的容器具有相同的工作方式。 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_vector</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 专门用于动态分配的对象，它使用起来更容易也更高效。 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_vector</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 独占它所包含的对象，因而容器之外的共享指针不能共享所有权，这跟 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::vector<boost::shared_ptr<int> ></font>`<font style="color:rgba(0, 0, 0, 0.9);"> 相反。</font>

<font style="color:rgba(0, 0, 0, 0.9);">除了 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_vector</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 之外，专门用于管理动态分配对象的容器还包括：</font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_deque</font>`<font style="color:rgba(0, 0, 0, 0.9);">， </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_list</font>`<font style="color:rgba(0, 0, 0, 0.9);">， </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_set</font>`<font style="color:rgba(0, 0, 0, 0.9);">， </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_map</font>`<font style="color:rgba(0, 0, 0, 0.9);">， </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_unordered_set</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::ptr_unordered_map</font>`<font style="color:rgba(0, 0, 0, 0.9);">。这些容器等价于C++标准里提供的那些。最后两个容器对应于</font>`<font style="color:rgba(0, 0, 0, 0.9);">std::unordered_set</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">std::unordered_map</font>`<font style="color:rgba(0, 0, 0, 0.9);">，它们作为技术报告1的一部分加入 C++ 标准。 如果所使用的 C++ 标准实现不支持技术报告1的话，还可以使用 Boost C++ 库里实现的 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::unordered_set</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 和 </font>`<font style="color:rgba(0, 0, 0, 0.9);">boost::unordered_map</font>`<font style="color:rgba(0, 0, 0, 0.9);">。</font>

## **<font style="color:rgb(255, 255, 255);">十：习题练习操作</font>**
### <font style="color:rgb(26, 152, 255);">1：使用适当的智能指针优化下面的程序：</font><font style="color:rgb(26, 152, 255);">  
</font>
```cpp
#include <iostream> 
#include <cstring> 


char* get(const char* s)
{
    int size = std::strlen(s);
    char* text = new char[size + 1];
    std::strncpy(text, s, size + 1);
    return text;
}


void print(char* text)
{
    std::cout << text << std::endl;
}


int main(int argc, char* argv[])
{
    if (argc < 2)
    {
        std::cerr << argv[0] << " <data>" << std::endl;
        return 1;
    }


    char* text = get(argv[1]);
    print(text);
    delete[] text;
}
```

<font style="color:rgba(0, 0, 0, 0.9);">输出结果：</font>

![](/images/5f4b48ae7bd42cfea11568d1a2616611.png)

### <font style="color:rgb(26, 152, 255);">2：优化下面的程序：</font><font style="color:rgb(26, 152, 255);">  
</font>
```cpp
#include <vector> 
template <typename T> 
T *create() 
{ 
  return new T; 
} 
int main() 
{ 
  std::vector<int*> v; 
  v.push_back(create<int>()); 
}
```

  


> 来自: [深度剖析（1）：Boost C++ 库，《智能指针》全维度解析](https://mp.weixin.qq.com/s?__biz=Mzg5NzA0NjYyNA==&mid=2247485899&idx=1&sn=9e12a805eaa24c9020414f7cec5ebe91&chksm=c07688c4f70101d2ab1686a7a90430a9e0c56474db6bbf75a076c064b94c029d791aa5ae8d46&cur_album_id=3814454053487198212&scene=190#rd)
>

