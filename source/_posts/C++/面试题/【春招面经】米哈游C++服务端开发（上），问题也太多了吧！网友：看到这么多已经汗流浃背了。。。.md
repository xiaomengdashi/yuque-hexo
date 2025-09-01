---
title: 【春招面经】米哈游C++服务端开发（上），问题也太多了吧！网友：看到这么多已经汗流浃背了。。。
date: '2025-08-16 20:34:44'
updated: '2025-08-16 20:34:44'
---
<font style="color:rgb(89, 89, 89);">大家好，我是Q。</font>

<font style="color:rgb(89, 89, 89);">前几天有小伙伴在留言区说想看米哈游的面经：</font>

![](/images/f80594e23bfc5257e68bd0c68410e284.png)

<font style="color:rgb(89, 89, 89);">今天，它来了~</font>

<font style="color:rgb(89, 89, 89);">我不仅整理了米哈游的面经，还为这位小伙伴送去了一本书，以表示他对我的支持！</font>

<font style="color:rgb(89, 89, 89);">当然，不定期送书，大家都会有机会~</font>

<font style="color:rgb(89, 89, 89);">马上四月中旬了，已经有offer的小伙伴还在努力去拿更好的。</font>

<font style="color:rgb(89, 89, 89);">没offer的小伙伴，也不要气馁，机会都是留给有准备的人的！你只管努力，剩下的交给天意！</font>

<font style="color:rgb(89, 89, 89);">我陪大家一起all in春招！！！</font>

<font style="color:rgb(89, 89, 89);">今天要分享的是位大佬面试米哈游整理的面经，光是一面，内容就超乎想象的多！</font>

<font style="color:rgb(89, 89, 89);">内容非常多，这篇先分享一半，关注我，明天继续！</font>

<font style="color:rgb(178, 178, 178);">来源：</font>

<font style="color:rgb(178, 178, 178);">https://www.nowcoder.com/feed/main/detail/7f28c235a0134b4a9be9fea52efe9551</font>

## **<font style="color:#595959;">1、模板类 模板匹配失败是不是错误？</font>**
<font style="color:#595959;">模板匹配失败 在 C++ 中是一种编译期错误，但具体情况要看你是在哪里失败的，失败类型不同，表现形式也不同：</font>

#### <font style="color:black;">情况一：模板匹配失败但可以回退到其它重载</font>
<font style="color:#595959;">这是合法的，不算错误，只要编译器找到了其它可以匹配的函数模板或重载。</font>

```plain
template <typename T>
void func(T t) { std::cout << "Template version\n"; }

void func(int i) { std::cout << "Non-template version\n"; }

int main() {
    func(10); // 模板匹配 int，但也有非模板重载，选择非模板版本，合法！
}
```

#### <font style="color:black;">情况二：只有一个模板版本，但模板匹配失败</font>
<font style="color:#595959;">这是 编译错误，因为编译器无法推导或替换模板参数，导致函数/类无法实例化。</font>

```plain
template <typename T>
void func(T t1, T t2) { }

int main() {
    func(1, 1.0); // T 同时要匹配 int 和 double，推导失败，编译错误
}
```

<font style="color:#595959;">解决方法：显式指定模板类型或者使用两个模板参数：</font>

```plain
template <typename T1, typename T2>
void func(T1 t1, T2 t2); // OK
```

#### <font style="color:black;">情况三：模板类中的某个方法匹配失败</font>
<font style="color:#595959;">C++ 会在你实例化这个方法时才做替换</font><font style="color:#595959;">（SFINAE：Substitution Failure Is Not An Error）</font><font style="color:#595959;">，这也是标准行为的一部分。</font>

```plain
template <typename T>
class Wrapper {
public:
    void doSomething() {
        T::nonexistentMethod(); // 如果 T 没有这个方法，只有在用到这个函数时才报错
    }
};

int main() {
    Wrapper<int> w; // 编译通过
    // w.doSomething(); // 如果调用就报错
}
```

## **<font style="color:#595959;">2、三种智能指针？怎么用？区别？</font>**
#### `<font style="color:black;">std::unique_ptr</font>`<font style="color:black;">（唯一所有权指针）</font>
<font style="color:#595959;">独占资源，不能被复制，只能移动（move）。</font>

<font style="color:#595959;">生命周期随指针对象结束而自动释放资源。</font>

##### <font style="color:black;">用法</font>
```plain
#include <memory>

std::unique_ptr<int> ptr1(new int(10));      // 创建方式1
auto ptr2 = std::make_unique<int>(20);       // 推荐方式（C++14 起）

// 转移所有权
std::unique_ptr<int> ptr3 = std::move(ptr1); // ptr1 被清空，ptr3 拥有资源
```

##### <font style="color:black;">禁止复制</font>
```plain
std::unique_ptr<int> ptr4 = ptr2; // 错误，不能复制
```

#### `<font style="color:black;">std::shared_ptr</font>`<font style="color:black;">（共享所有权指针）</font>
<font style="color:#595959;">多个 </font>`<font style="color:#595959;">shared_ptr</font>`<font style="color:#595959;"> 可以共享同一块资源。</font>

<font style="color:#595959;">内部维护一个 引用计数，最后一个引用释放时自动释放资源。</font>

##### <font style="color:black;">用法</font>
```plain
#include <memory>

std::shared_ptr<int> sp1 = std::make_shared<int>(100); // 推荐方式
std::shared_ptr<int> sp2 = sp1; // 引用计数 +1

std::cout << sp1.use_count();  // 打印引用计数
```

**<font style="color:#595959;">「不能形成循环引用」</font>**<font style="color:#595959;">，否则内存泄漏！</font>

#### `<font style="color:black;">std::weak_ptr</font>`<font style="color:black;">（弱引用指针）</font>
<font style="color:#595959;">解决 </font>`<font style="color:#595959;">shared_ptr</font>`<font style="color:#595959;"> 循环引用问题。</font>

**<font style="color:#595959;">「不拥有资源」</font>**<font style="color:#595959;">，不会增加引用计数。</font>

<font style="color:#595959;">必须用 </font>`<font style="color:#595959;">.lock()</font>`<font style="color:#595959;"> 转换为 </font>`<font style="color:#595959;">shared_ptr</font>`<font style="color:#595959;"> 才能使用资源。</font>

##### <font style="color:black;">用法</font>
```plain
#include <memory>

std::shared_ptr<int> sp = std::make_shared<int>(42);
std::weak_ptr<int> wp = sp;

if (auto locked = wp.lock()) {
    std::cout << *locked << std::endl;  // 安全访问
} else {
    std::cout << "资源已释放" << std::endl;
}
```

#### <font style="color:black;">区别与对比</font>
| **<font style="color:#595959;">特性</font>** | **<font style="color:#595959;">unique_ptr</font>** | **<font style="color:#595959;">shared_ptr</font>** | **<font style="color:#595959;">weak_ptr</font>** |
| :--- | :--- | :--- | :--- |
| <font style="color:#595959;">所有权</font> | <font style="color:#595959;">独占</font> | <font style="color:#595959;">共享</font> | <font style="color:#595959;">无所有权</font> |
| <font style="color:#595959;">引用计数</font> | <font style="color:#595959;">无</font> | <font style="color:#595959;">有</font> | <font style="color:#595959;">无</font> |
| <font style="color:#595959;">复制</font> | <font style="color:#595959;">不可，只能移动</font> | <font style="color:#595959;">可复制</font> | <font style="color:#595959;">可复制但不控制</font> |
| <font style="color:#595959;">销毁时机</font> | <font style="color:#595959;">超出作用域</font> | <font style="color:#595959;">计数为 0</font> | <font style="color:#595959;">不直接销毁</font> |
| <font style="color:#595959;">访问方式</font> | <font style="color:#595959;">直接 * 或 -></font> | <font style="color:#595959;">直接 * 或 -></font> | <font style="color:#595959;">通过 lock()</font> |
| <font style="color:#595959;">性能开销</font> | <font style="color:#595959;">最低（无计数）</font> | <font style="color:#595959;">中等（计数管理）</font> | <font style="color:#595959;">中等（需检查）</font> |
| <font style="color:#595959;">主要用途</font> | <font style="color:#595959;">单一拥有者</font> | <font style="color:#595959;">多拥有者</font> | <font style="color:#595959;">打破循环引用</font> |


## **<font style="color:#595959;">3、</font>****<font style="color:#595959;">unique_ptr</font>****<font style="color:#595959;">能不能被复制？</font>**
<font style="color:#595959;">不能。</font>

<font style="color:#595959;">原因：</font>

+ <font style="color:#595959;">unique_ptr </font><font style="color:#595959;">的设计目标是确保资源（如动态分配的内存）只有一个明确的所有者，避免多个指针同时管理同一资源导致的双重释放或其他未定义行为。</font>
+ <font style="color:#595959;">复制会破坏独占性，因此编译器禁止复制操作。</font>

**<font style="color:#595959;">「支持移动而不是复制」</font>**

<font style="color:#595959;">unique_ptr </font><font style="color:#595959;">提供了移动构造函数和移动赋值运算符，允许将所有权从一个 </font><font style="color:#595959;">unique_ptr </font><font style="color:#595959;">转移到另一个。</font>

#### <font style="color:black;">为什么禁止复制？</font>
##### <font style="color:black;">独占所有权的语义</font>
<font style="color:#595959;">unique_ptr </font><font style="color:#595959;">的核心理念是“独占”，如果允许复制：</font>

+ <font style="color:#595959;">多个 </font><font style="color:#595959;">unique_ptr </font><font style="color:#595959;">会指向同一块内存。</font>
+ <font style="color:#595959;">当这些指针销毁时，会多次调用 delete，导致未定义行为（如双重释放）。</font>

##### <font style="color:black;">与 </font><font style="color:black;">shared_ptr </font><font style="color:black;">的对比</font>
<font style="color:#595959;">shared_ptr</font><font style="color:#595959;">支持复制，通过引用计数管理多个拥有者。</font>

<font style="color:#595959;">unique_ptr</font><font style="color:#595959;">不使用计数，追求轻量和明确性，因此禁用复制。</font>

## **<font style="color:#595959;">4、我一个类实例化不想被复制 怎么办？</font>**
1. <font style="color:#595959;">显式删除复制函数（C++11 及以上）：使用 = delete。</font>
2. <font style="color:#595959;">将复制函数声明为私有（C++98/03）：传统方法。</font>

#### <font style="color:black;">方法 1：使用 = delete（推荐）</font>
<font style="color:#595959;">将复制构造函数和复制赋值运算符标记为 deleted。</font>

```plain
#include <iostream>

class NoCopy {
public:
    NoCopy(int val) : value(val) {}
    void print() const { std::cout << "Value: " << value << "\n"; }

    // 禁用复制构造函数
    NoCopy(const NoCopy&) = delete;
    // 禁用复制赋值运算符
    NoCopy& operator=(const NoCopy&) = delete;

private:
    int value;
};

int main() {
    NoCopy obj1(10);
    obj1.print(); // Value: 10

    // NoCopy obj2 = obj1; // 编译错误：调用已删除的复制构造函数
    // NoCopy obj3(20);
    // obj3 = obj1;        // 编译错误：调用已删除的赋值运算符

    return0;
}
```

#### <font style="color:black;">方法 2：声明为私有（C++98/03）</font>
<font style="color:#595959;">将复制构造函数和赋值运算符声明为 private，不提供实现。</font>

```plain
#include <iostream>

class NoCopy {
public:
    NoCopy(int val) : value(val) {}
    void print() const { std::cout << "Value: " << value << "\n"; }

private:
    int value;
    NoCopy(const NoCopy&);            // 私有，未定义
    NoCopy& operator=(const NoCopy&); // 私有，未定义
};

int main() {
    NoCopy obj1(10);
    obj1.print();

    // NoCopy obj2 = obj1; // 编译错误：NoCopy(const NoCopy&) 不可访问
    // NoCopy obj3(20);
    // obj3 = obj1;        // 编译错误：operator= 不可访问

    return0;
}
```

## **<font style="color:#595959;">5、</font>****<font style="color:#595959;">shared_ptr</font>****<font style="color:#595959;">是否线程安全（引用计数为什么线程安全）？</font>**
<font style="color:#595959;">shared_ptr</font>**<font style="color:#595959;">「本身并非完全线程安全的」</font>**<font style="color:#595959;">，但它的引用计数管理在特定条件下是线程安全的。</font>

#### <font style="color:black;">引用计数线程安全</font>
<font style="color:#595959;">当多个线程对同一个底层资源（由 </font><font style="color:#595959;">shared_ptr </font><font style="color:#595959;">管理）的引用计数进行操作（如复制或销毁 </font><font style="color:#595959;">shared_ptr</font><font style="color:#595959;">），这些操作是原子化的，不会引发竞争。</font>

<font style="color:#595959;">这是标准库实现（如 </font><font style="color:#595959;">libstdc++、libc++</font><font style="color:#595959;">）通过原子操作（如 </font><font style="color:#595959;">std::atomic</font><font style="color:#595959;">）保证的。</font>

#### <font style="color:black;">对象本身线程不安全</font>
<font style="color:#595959;">如果多个线程同时修改同一个 </font><font style="color:#595959;">shared_ptr </font><font style="color:#595959;">对象（如赋值</font><font style="color:#595959;">sp=new_sp</font><font style="color:#595959;">），会导致数据竞争。</font>

<font style="color:#595959;">原因是 </font><font style="color:#595959;">shared_ptr </font><font style="color:#595959;">的赋值涉及指针更新，不是原子操作。</font>

#### <font style="color:black;">引用计数为什么线程安全？</font>
<font style="color:#595959;">shared_ptr </font><font style="color:#595959;">的引用计数：</font>

+ <font style="color:#595959;">内部维护两个计数器：</font>
    - <font style="color:#595959;">强引用计数（</font><font style="color:#595959;">use_count</font><font style="color:#595959;">() 返回）：表示有多少 </font><font style="color:#595959;">shared_ptr </font><font style="color:#595959;">拥有资源。</font>
    - <font style="color:#595959;">弱引用计数：表示有多少 </font><font style="color:#595959;">weak_ptr </font><font style="color:#595959;">引用资源。</font>
+ <font style="color:#595959;">这些计数器存储在控制块中，与管理的对象分开。</font>

<font style="color:#595959;">原子操作：</font>

<font style="color:#595959;">控制块中中的计数器使用原子操作（如</font><font style="color:#595959;">std::atomic</font><font style="color:#595959;">或底层CAS）实现。</font>

<font style="color:#595959;">常见操作：</font>

+ <font style="color:#595959;">复制 </font><font style="color:#595959;">shared_ptr</font><font style="color:#595959;">：原子递增强引用计数。</font>
+ <font style="color:#595959;">销毁 </font><font style="color:#595959;">shared_ptr</font><font style="color:#595959;">：原子递减强引用计数，若减至 0，则释放资源。</font>

## **<font style="color:#595959;">6、class一个类的内存大小由什么判断？</font>**
<font style="color:#595959;">C++ 中，一个类（class）的内存大小（通常通过 sizeof 操作符计算）是由其</font>**<font style="color:#595959;">「数据成员」</font>**<font style="color:#595959;">和</font>**<font style="color:#595959;">「内存对齐规则」</font>**<font style="color:#595959;">共同决定的，而不是由成员函数、虚函数表指针的数量、访问权限（如 public、private）等直接决定。</font>

#### <font style="color:black;">影响类内存大小的因素</font>
##### <font style="color:black;">数据成员</font>
<font style="color:#595959;">类中声明的非静态数据成员（如 int、double、指针等）直接占用内存。</font>

<font style="color:#595959;">规则：</font>

+ <font style="color:#595959;">每个数据成员的大小由其类型决定（如 int 通常 4 字节，double 通常 8 字节）。</font>
+ <font style="color:#595959;">非静态成员按声明顺序排列在对象内存中。</font>

<font style="color:#595959;">注意：静态成员（static）不占用对象内存，存储在全局数据段。</font>

##### <font style="color:black;">内存对齐</font>
<font style="color:#595959;">编译器为了优化内存访问效率，会对数据成员进行对齐填充（padding），使每个成员的起始地址满足特定对齐要求。</font>

<font style="color:#595959;">规则：</font>

+ <font style="color:#595959;">每个数据类型有对齐要求，通常是其大小的倍数（如 int 对齐到 4 字节，double 对齐到 8 字节）。</font>
+ <font style="color:#595959;">类的总大小是对齐到最大成员的对齐边界。</font>

<font style="color:#595959;">影响：可能在成员间插入填充字节，导致实际大小大于成员大小之和。</font>

##### <font style="color:black;">虚函数表指针</font>
<font style="color:#595959;">如果类有虚函数，编译器会为每个对象插入一个虚函数表指针（vptr），指向类的虚函数表（vtable）。</font>

<font style="color:#595959;">大小：</font>

+ <font style="color:#595959;">32 位系统：4 字节。</font>
+ <font style="color:#595959;">64 位系统：8 字节。</font>

<font style="color:#595959;">条件：只有声明了虚函数（包括虚析构函数）或继承自有虚函数的基类时，才会有 vptr。</font>

##### <font style="color:black;">继承</font>
<font style="color:#595959;">单继承：</font>

+ <font style="color:#595959;">派生类包含基类的非静态成员和自身的非静态成员。</font>
+ <font style="color:#595959;">如果基类有 vptr，派生类通常复用它（除非多继承）。</font>

<font style="color:#595959;">多继承：每个有虚函数的基类可能贡献一个 vptr，增加大小。</font>

<font style="color:#595959;">对齐：继承后仍需满足整体对齐。</font>

##### <font style="color:black;">空类</font>
<font style="color:#595959;">规则：空类（无数据成员、无虚函数）大小为 1 字节。</font>

<font style="color:#595959;">原因：C++ 标准要求每个对象有唯一地址，编译器插入 1 字节占位符。</font>

#### <font style="color:black;">内存大小计算规则</font>
<font style="color:#595959;">成员大小之和：计算所有非静态数据成员的原始大小。</font>

<font style="color:#595959;">对齐填充：</font>

+ <font style="color:#595959;">按声明顺序排列成员，每个成员起始地址对齐到其类型的要求。</font>
+ <font style="color:#595959;">在成员间插入填充字节。</font>

<font style="color:#595959;">整体对齐：类的总大小对齐到最大成员的对齐边界（或 vptr 大小）。</font>

<font style="color:#595959;">特殊情况：</font>

+ <font style="color:#595959;">添加 vptr（若有虚函数）。</font>
+ <font style="color:#595959;">继承时累加基类大小。</font>

## **<font style="color:#595959;">7、为什么要有内存对齐？</font>**
#### <font style="color:black;">什么是内存对齐？</font>
<font style="color:#595959;">内存对齐是指将数据存储在内存中的地址调整为特定边界（如 4 字节、8 字节）的倍数，而不是紧密排列。</font>

<font style="color:#595959;">表现：</font>

+ <font style="color:#595959;">每个数据类型有对齐要求（如 int 通常对齐到 4 字节，</font><font style="color:#595959;">double </font><font style="color:#595959;">对齐到 8 字节）。</font>
+ <font style="color:#595959;">结构体或类的总大小对齐到最大成员的对齐边界。</font>

<font style="color:#595959;">效果：</font>

+ <font style="color:#595959;">在成员间可能插入填充字节。</font>
+ <font style="color:#595959;">示例：</font>`<font style="color:#595959;">struct { char c; int i; }</font>`<font style="color:#595959;"> 大小不是 5（1+4），而是 8（对齐填充）。</font>

#### <font style="color:black;">为什么要有内存对齐？</font>
##### <font style="color:black;">硬件性能优化</font>
<font style="color:#595959;">CPU 访问效率：</font>

+ <font style="color:#595959;">现代处理器（如 x86、ARM）设计为按字长（word size，通常 4 或 8 字节）访问内存。</font>
+ <font style="color:#595959;">对齐的内存地址允许 CPU 一次读取整个数据块。</font>
+ <font style="color:#595959;">未对齐的访问可能需要多次读取和拼接，增加周期数。</font>

<font style="color:#595959;">假设 CPU 字长为 4 字节：</font>

+ <font style="color:#595959;">对齐的 int（4 字节）在地址 </font><font style="color:#595959;">0x1000</font><font style="color:#595959;">，一次读取。</font>
+ <font style="color:#595959;">未对齐的 int 在 </font><font style="color:#595959;">0x1001</font><font style="color:#595959;">，需读取</font><font style="color:#595959;"> 0x1000-0x1003 和 0x1004-0x1007 </font><font style="color:#595959;">两次，性能下降。</font>

##### <font style="color:black;">硬件限制</font>
<font style="color:#595959;">某些架构的要求：</font>

+ <font style="color:#595959;">在某些 CPU 架构（如</font><font style="color:#595959;"> RISC，ARM、SPARC</font><font style="color:#595959;">）上，未对齐的内存访问不仅慢，还会触发硬件异常（如 Bus Error）。</font>
+ <font style="color:#595959;">x86 架构虽支持未对齐访问，但性能较低。</font>

<font style="color:#595959;">后果：未对齐可能导致程序崩溃或不可预测行为。</font>

##### <font style="color:black;">数据一致性</font>
<font style="color:#595959;">多核处理器和缓存：</font>

+ <font style="color:#595959;">内存对齐确保数据在缓存行（</font><font style="color:#595959;">Cache Lin</font><font style="color:#595959;">e，通常 64 字节）中正确分布。</font>
+ <font style="color:#595959;">未对齐可能导致数据跨缓存行，增加缓存未命中和同步开销。</font>

<font style="color:#595959;">原子操作：对齐的内存支持高效的原子操作（如 s</font><font style="color:#595959;">td::atomic</font><font style="color:#595959;">），未对齐可能失败。</font>

#### <font style="color:black;">内存对齐的实现原理</font>
##### <font style="color:black;">对齐规则</font>
<font style="color:#595959;">类型对齐：</font>

<font style="color:#595959;">基本类型对齐到自身大小的倍数：</font>

+ <font style="color:#595959;">char：1 字节。</font>
+ <font style="color:#595959;">int：4 字节。</font>
+ <font style="color:#595959;">double：8 字节。</font>

<font style="color:#595959;">结构体/类对齐：</font>

+ <font style="color:#595959;">每个成员按其对齐要求排列。</font>
+ <font style="color:#595959;">总大小对齐到最大成员的对齐边界。</font>

<font style="color:#595959;">填充：在成员间插入字节，使下一成员地址满足对齐。</font>

## **<font style="color:#595959;">8、代码编译过程？</font>**
#### <font style="color:black;">预处理</font>
<font style="color:#595959;">处理源代码中的预处理器指令（如 </font><font style="color:#595959;">#include、#define）</font><font style="color:#595959;">，生成扩展后的源代码。</font>

<font style="color:#595959;">作用：</font>

+ <font style="color:#595959;">展开宏定义。</font>
+ <font style="color:#595959;">处理头文件（将 #</font><font style="color:#595959;">include </font><font style="color:#595959;">的内容嵌入）。</font>
+ <font style="color:#595959;">条件编译（如 #</font><font style="color:#595959;">ifdef</font><font style="color:#595959;">）。</font>
+ <font style="color:#595959;">移除注释。</font>

<font style="color:#595959;">输入：.</font><font style="color:#595959;">cpp </font><font style="color:#595959;">文件（源文件）。</font>

<font style="color:#595959;">输出：预处理后的临时文件（通常以 .i 为扩展名）。</font>

#### <font style="color:black;">编译</font>
<font style="color:#595959;">预处理后的源码会被编译器翻译成汇编代码，每个 </font>`<font style="color:#595959;">.cpp</font>`<font style="color:#595959;"> 文件单独编译，生成 </font>`<font style="color:#595959;">.o</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">.obj</font>`<font style="color:#595959;"> 文件（目标文件）。</font>

<font style="color:#595959;">作用：</font>

+ <font style="color:#595959;">语法检查：验证代码是否符合 C++ 语法。</font>
+ <font style="color:#595959;">语义分析：检查类型匹配、变量声明等。</font>
+ <font style="color:#595959;">代码优化：生成高效的中间表示。</font>
+ <font style="color:#595959;">生成汇编代码：针对特定硬件架构。</font>

<font style="color:#595959;">输入：.i 文件（预处理输出）。</font>

<font style="color:#595959;">输出：汇编文件（.s 文件）。</font>

#### <font style="color:black;">汇编</font>
<font style="color:#595959;">编译生成的汇编代码被 汇编器 转换为机器码（二进制），生成目标文件（</font>`<font style="color:#595959;">.o</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">.obj</font>`<font style="color:#595959;">）。</font>

<font style="color:#595959;">作用：</font>

+ <font style="color:#595959;">将人类可读的汇编指令（如 movl）翻译为二进制指令。</font>
+ <font style="color:#595959;">生成目标文件，包含未解析的符号引用。</font>

<font style="color:#595959;">输入：.s 文件（汇编代码）。</font>

<font style="color:#595959;">输出：目标文件（.o 或 .obj 文件）。</font>

#### <font style="color:black;">链接</font>
<font style="color:#595959;">把多个 </font>`<font style="color:#595959;">.o</font>`<font style="color:#595959;"> 文件和库文件链接在一起，解决函数、变量引用，生成最终的可执行文件（如 </font>`<font style="color:#595959;">a.out</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">.exe</font>`<font style="color:#595959;">）。</font>

<font style="color:#595959;">作用：</font>

+ <font style="color:#595959;">解析符号引用（如</font><font style="color:#595959;"> std::cout</font><font style="color:#595959;"> 的地址）。</font>
+ <font style="color:#595959;">合并代码段、数据段。</font>
+ <font style="color:#595959;">处理静态库（如</font><font style="color:#595959;">libstdc++.a）或动态库（如 libstdc++.so</font><font style="color:#595959;">）。</font>

<font style="color:#595959;">输入：</font>

+ <font style="color:#595959;">.o 文件（目标文件）。</font>
+ <font style="color:#595959;">库文件（如标准库）。</font>

<font style="color:#595959;">输出：可执行文件（如 a.</font><font style="color:#595959;">out </font><font style="color:#595959;">或指定名称）。</font>

## **<font style="color:#595959;">9、动态库静态库区别？怎么链接？</font>**
#### <font style="color:black;">静态库</font>
<font style="color:#595959;">静态库是一组目标文件（.o或.obj）的集合，通常以.a（</font><font style="color:#595959;">Linux</font><font style="color:#595959;">）或 .lib（</font><font style="color:#595959;">Windows</font><font style="color:#595959;">）为扩展名，在链接时将代码嵌入到最终的可执行文件中。</font>

<font style="color:#595959;">特点：</font>

+ <font style="color:#595959;">链接时直接合并到程序中。</font>
+ <font style="color:#595959;">可执行文件独立运行，无需外部依赖。</font>

#### <font style="color:black;">动态库</font>
<font style="color:#595959;">动态库是独立的可执行模块，通常以 </font><font style="color:#595959;">.so（Linux）、.dll（Windows）或 .dylib（macOS）</font><font style="color:#595959;">为扩展名，在运行时动态加载。</font>

<font style="color:#595959;">特点：</font>

+ <font style="color:#595959;">链接时仅记录引用，运行时加载。</font>
+ <font style="color:#595959;">可共享，节省内存，但依赖库文件存在。</font>

#### <font style="color:black;">区别</font>
| **<font style="color:#595959;">特性</font>** | **<font style="color:#595959;">静态库 (.a/.lib)</font>** | **<font style="color:#595959;">动态库 (.so/.dll)</font>** |
| :--- | :--- | :--- |
| <font style="color:#595959;">链接时机</font> | <font style="color:#595959;">编译时（链接阶段）</font> | <font style="color:#595959;">运行时（动态加载）</font> |
| <font style="color:#595959;">文件嵌入</font> | <font style="color:#595959;">嵌入到可执行文件中</font> | <font style="color:#595959;">不嵌入，独立文件</font> |
| <font style="color:#595959;">可执行文件大小</font> | <font style="color:#595959;">较大（包含库代码）</font> | <font style="color:#595959;">较小（仅含引用）</font> |
| <font style="color:#595959;">内存使用</font> | <font style="color:#595959;">每个程序独立占用</font> | <font style="color:#595959;">多个程序共享内存</font> |
| <font style="color:#595959;">更新性</font> | <font style="color:#595959;">需重新编译程序</font> | <font style="color:#595959;">替换库文件即可</font> |
| <font style="color:#595959;">依赖性</font> | <font style="color:#595959;">无运行时依赖</font> | <font style="color:#595959;">需确保库文件存在</font> |
| <font style="color:#595959;">加载速度</font> | <font style="color:#595959;">启动快（已嵌入）</font> | <font style="color:#595959;">启动稍慢（需加载）</font> |
| <font style="color:#595959;">平台扩展名</font> | <font style="color:#595959;">.a (Linux), .lib (Win)</font> | <font style="color:#595959;">.so (Linux), .dll (Win)</font> |


#### <font style="color:black;">链接方式</font>
##### <font style="color:black;">静态库</font>
<font style="color:#595959;">在链接阶段，链接器（如 ld）将静态库中的目标代码提取并嵌入到可执行文件中。</font>

<font style="color:#595959;">步骤：</font>

1. <font style="color:#595959;">编译源文件生成目标文件（.o）。</font>
2. <font style="color:#595959;">用 ar 创建静态库。</font>
3. <font style="color:#595959;">链接时指定静态库。</font>

##### <font style="color:black;">动态库</font>
<font style="color:#595959;">链接时仅记录动态库的符号引用，运行时由操作系统加载器（如 ld.so）解析。</font>

<font style="color:#595959;">步骤：</font>

1. <font style="color:#595959;">编译源文件生成动态库（需 -shared 和 -fPIC）。</font>
2. <font style="color:#595959;">链接时指定动态库路径或名称。</font>
3. <font style="color:#595959;">运行时确保库文件可找到。</font>

## **<font style="color:#595959;">10、多线程同步机制？</font>**
#### <font style="color:black;">互斥锁</font>
<font style="color:#595959;">互斥锁确保同一时刻只有一个线程访问临界区。</font>

<font style="color:#595959;">类型：</font>

+ `<font style="color:#595959;">std::mutex</font>`<font style="color:#595959;">：基本互斥锁。</font>
+ `<font style="color:#595959;">std::recursive_mutex</font>`<font style="color:#595959;">：允许同一线程多次锁定。</font>

<font style="color:#595959;">工作原理：</font>

+ <font style="color:#595959;">锁定（</font><font style="color:#595959;">lock</font><font style="color:#595959;">()）：线程获取锁，若已被占用则阻塞。</font>
+ <font style="color:#595959;">解锁（</font><font style="color:#595959;">unlock</font><font style="color:#595959;">()）：释放锁，允许其他线程获取。</font>

<font style="color:#595959;">工具：</font>

+ `<font style="color:#595959;">std::lock_guard</font>`<font style="color:#595959;">：RAII 封装，自动解锁。</font>
+ `<font style="color:#595959;">std::unique_lock</font>`<font style="color:#595959;">：更灵活，支持延迟锁定。</font>

<font style="color:#595959;">示例：</font>

```plain
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
int counter = 0;

void increment(int id) {
    std::lock_guard<std::mutex> lock(mtx); // 自动加锁和解锁
    counter++;
    std::cout << "Thread " << id << " counter: " << counter << "\n";
}

int main() {
    std::thread t1(increment, 1);
    std::thread t2(increment, 2);
    t1.join();
    t2.join();
    std::cout << "Final counter: " << counter << "\n";
    return0;
}
```

#### <font style="color:black;">读写锁</font>
<font style="color:#595959;">允许多个线程同时读，但写操作互斥。</font>

<font style="color:#595959;">C++ 支持：C++17 引入 </font>`<font style="color:#595959;">std::shared_mutex</font>`<font style="color:#595959;">。</font>

<font style="color:#595959;">工作原理：</font>

+ <font style="color:#595959;">共享锁（读锁）：多线程可同时获取。</font>
+ <font style="color:#595959;">独占锁（写锁）：仅一个线程获取。</font>

<font style="color:#595959;">示例：</font>

```plain
#include <iostream>
#include <thread>
#include <shared_mutex>

std::shared_mutex rw_mtx;
int data = 0;

void reader(int id) {
    std::shared_lock<std::shared_mutex> lock(rw_mtx); // 读锁
    std::cout << "Reader " << id << " reads: " << data << "\n";
}

void writer(int id) {
    std::unique_lock<std::shared_mutex> lock(rw_mtx); // 写锁
    data++;
    std::cout << "Writer " << id << " writes: " << data << "\n";
}

int main() {
    std::thread r1(reader, 1);
    std::thread r2(reader, 2);
    std::thread w1(writer, 1);
    r1.join(); r2.join(); w1.join();
    return0;
}
```

#### <font style="color:black;">条件变量</font>
<font style="color:#595959;">用于线程间同步，等待特定条件成立。</font>

<font style="color:#595959;">工具：</font>`<font style="color:#595959;">std::condition_variable</font>`<font style="color:#595959;">，需配合 </font>`<font style="color:#595959;">std::mutex</font>`<font style="color:#595959;">。</font>

<font style="color:#595959;">工作原理：</font>

+ `<font style="color:#595959;">wait()</font>`<font style="color:#595959;">：释放锁并等待通知。</font>
+ `<font style="color:#595959;">notify_one()/notify_all()</font>`<font style="color:#595959;">：唤醒等待线程。</font>

<font style="color:#595959;">示例：</font>

```plain
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void worker() {
    std::unique_lock<std::mutex> lock(mtx);
    cv.wait(lock, [] { return ready; }); // 等待 ready 为 true
    std::cout << "Worker proceeds\n";
}

void signaler() {
    std::this_thread::sleep_for(std::chrono::seconds(1));
    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one(); // 唤醒一个等待线程
}

int main() {
    std::thread t1(worker);
    std::thread t2(signaler);
    t1.join();
    t2.join();
    return0;
}
```

#### <font style="color:black;">信号量</font>
<font style="color:#595959;">计数器，控制多个线程对有限资源的访问。</font>

<font style="color:#595959;">C++ 支持：C++20引入</font>`<font style="color:#595959;">std::counting_semaphore</font>`<font style="color:#595959;">。</font>

<font style="color:#595959;">工作原理：</font>

+ `<font style="color:#595959;">acquire</font><font style="color:#595959;">()</font>`<font style="color:#595959;">：减少计数，若为 0 则阻塞。</font>
+ `<font style="color:#595959;">release</font><font style="color:#595959;">()</font>`<font style="color:#595959;">：增加计数，唤醒等待线程。</font>

<font style="color:#595959;">示例：</font>

```plain
#include <iostream>
#include <thread>
#include <semaphore>

std::counting_semaphore<2> sem(2); // 最多 2 个线程访问

void task(int id) {
    sem.acquire();
    std::cout << "Thread " << id << " enters\n";
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Thread " << id << " exits\n";
    sem.release();
}

int main() {
    std::thread t1(task, 1);
    std::thread t2(task, 2);
    std::thread t3(task, 3);
    t1.join(); t2.join(); t3.join();
    return0;
}
```

#### <font style="color:black;">原子操作</font>
<font style="color:#595959;">无锁操作，直接在硬件层面保证线程安全。</font>

<font style="color:#595959;">工具：</font>`<font style="color:#595959;">std::</font><font style="color:#595959;">atomic</font>`<font style="color:#595959;">。</font>

<font style="color:#595959;">工作原理：使用 CPU 提供的原子指令（如 CAS）。</font>

<font style="color:#595959;">示例：</font>

```plain
#include <iostream>
#include <thread>
#include <atomic>

std::atomic<int> counter(0);

void increment(int id) {
    counter.fetch_add(1); // 原子递增
    std::cout << "Thread " << id << " counter: " << counter << "\n";
}

int main() {
    std::thread t1(increment, 1);
    std::thread t2(increment, 2);
    t1.join(); t2.join();
    std::cout << "Final counter: " << counter << "\n";
    return0;
}
```

#### <font style="color:black;">对比与选择</font>
| **<font style="color:#595959;">机制</font>** | **<font style="color:#595959;">互斥性</font>** | **<font style="color:#595959;">同步性</font>** | **<font style="color:#595959;">性能开销</font>** | **<font style="color:#595959;">适用场景</font>** |
| :--- | :--- | :--- | :--- | :--- |
| <font style="color:#595959;">互斥锁</font> | <font style="color:#595959;">是</font> | <font style="color:#595959;">否</font> | <font style="color:#595959;">中等</font> | <font style="color:#595959;">保护共享资源</font> |
| <font style="color:#595959;">读写锁</font> | <font style="color:#595959;">是</font> | <font style="color:#595959;">否</font> | <font style="color:#595959;">中等</font> | <font style="color:#595959;">读多写少</font> |
| <font style="color:#595959;">条件变量</font> | <font style="color:#595959;">否</font> | <font style="color:#595959;">是</font> | <font style="color:#595959;">中等</font> | <font style="color:#595959;">线程等待条件</font> |
| <font style="color:#595959;">信号量</font> | <font style="color:#595959;">是</font> | <font style="color:#595959;">是</font> | <font style="color:#595959;">中等</font> | <font style="color:#595959;">资源计数</font> |
| <font style="color:#595959;">原子操作</font> | <font style="color:#595959;">是</font> | <font style="color:#595959;">否</font> | <font style="color:#595959;">低</font> | <font style="color:#595959;">简单变量操作</font> |


## **<font style="color:#595959;">11、生产者消费者线程同步机制用的什么？</font>**
<font style="color:#595959;">在 C++ 中，生产者-消费者（Producer-Consumer）问题是一个经典的多线程同步场景，要求生产者线程生成数据并放入共享缓冲区，消费者线程从中取出数据处理。为了避免数据竞争和确保线程间的正确协作，通常使用特定的同步机制。</font>

#### <font style="color:black;">互斥锁 + 条件变量</font>
<font style="color:#595959;">工具：</font>

+ `<font style="color:#595959;">std::mutex</font>`<font style="color:#595959;">：保护缓冲区。</font>
+ `<font style="color:#595959;">std::condition_variable</font>`<font style="color:#595959;">：通知缓冲区状态（空/满）。</font>

<font style="color:#595959;">工作原理：</font>

+ <font style="color:#595959;">互斥锁确保缓冲区操作的线程安全性。</font>
+ <font style="color:#595959;">条件变量让线程等待特定条件（如缓冲区非空或不满）。</font>

<font style="color:#595959;">优点：</font>

+ <font style="color:#595959;">C++11 原生支持，灵活通用。</font>
+ <font style="color:#595959;">可处理复杂条件。</font>

<font style="color:#595959;">缺点：锁和条件检查有一定开销。</font>

<font style="color:#595959;">示例：</font>

```plain
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

std::mutex mtx;
std::condition_variable cv;
std::queue<int> buffer;
constint BUFFER_SIZE = 5;
bool finished = false;

void producer(int id) {
    for (int i = 0; i < 10; ++i) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [] { return buffer.size() < BUFFER_SIZE; }); // 等待不满
        buffer.push(i);
        std::cout << "Producer " << id << " produced: " << i << "\n";
        lock.unlock();
        cv.notify_one(); // 通知消费者
        std::this_thread::sleep_for(std::chrono::milliseconds(100)); // 模拟生产
    }
    std::lock_guard<std::mutex> lock(mtx);
    if (id == 1) finished = true; // 假设两个生产者，id=1 结束
}

void consumer(int id) {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx);
        cv.wait(lock, [] { return !buffer.empty() || finished; }); // 等待非空
        if (buffer.empty() && finished) break;
        if (!buffer.empty()) {
            int data = buffer.front();
            buffer.pop();
            std::cout << "Consumer " << id << " consumed: " << data << "\n";
        }
        lock.unlock();
        cv.notify_one(); // 通知生产者
        std::this_thread::sleep_for(std::chrono::milliseconds(150)); // 模拟消费
    }
}

int main() {
    std::thread p1(producer, 1);
    std::thread p2(producer, 2);
    std::thread c1(consumer, 1);
    std::thread c2(consumer, 2);

    p1.join(); p2.join();
    c1.join(); c2.join();
    return0;
}
```

#### <font style="color:black;">信号量</font>
<font style="color:#595959;">工具：C++20 的 std::counting_semaphore。</font>

<font style="color:#595959;">工作原理：</font>

+ <font style="color:#595959;">一个信号量表示空闲槽位（生产者用）。</font>
+ <font style="color:#595959;">另一个信号量表示已有数据（消费者用）。</font>

<font style="color:#595959;">优点：</font>

+ <font style="color:#595959;">直观，计数器天然适合缓冲区管理。</font>
+ <font style="color:#595959;">无需显式条件检查。</font>

<font style="color:#595959;">缺点：C++20 前需自己实现或用第三方库。</font>

<font style="color:#595959;">示例：</font>

```plain
#include <iostream>
#include <thread>
#include <semaphore>
#include <queue>

std::queue<int> buffer;
std::counting_semaphore<5> sem_empty(5); // 空闲槽位
std::counting_semaphore<5> sem_full(0);  // 数据项
std::mutex mtx;

void producer(int id) {
    for (int i = 0; i < 10; ++i) {
        sem_empty.acquire(); // 等待空位
        {
            std::lock_guard<std::mutex> lock(mtx);
            buffer.push(i);
            std::cout << "Producer " << id << " produced: " << i << "\n";
        }
        sem_full.release(); // 通知有数据
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}

void consumer(int id) {
    for (int i = 0; i < 10; ++i) {
        sem_full.acquire(); // 等待数据
        int data;
        {
            std::lock_guard<std::mutex> lock(mtx);
            data = buffer.front();
            buffer.pop();
            std::cout << "Consumer " << id << " consumed: " << data << "\n";
        }
        sem_empty.release(); // 释放空位
        std::this_thread::sleep_for(std::chrono::milliseconds(150));
    }
}

int main() {
    std::thread p(producer, 1);
    std::thread c(consumer, 1);
    p.join();
    c.join();
    return0;
}
```

#### <font style="color:black;">原子操作 + 无锁队列</font>
<font style="color:#595959;">工具：std::atomic + 自定义无锁队列。</font>

<font style="color:#595959;">工作原理：</font>

+ <font style="color:#595959;">使用原子变量（如头尾指针）实现无锁读写。</font>
+ <font style="color:#595959;">避免锁的开销。</font>

<font style="color:#595959;">优点：高性能，适合高并发。</font>

<font style="color:#595959;">缺点：实现复杂，易出错。</font>

<font style="color:#595959;">示例：无锁队列实现较复杂，这里仅示意</font>

```plain
#include <iostream>
#include <thread>
#include <atomic>
#include <vector>

std::atomic<int> counter(0);

void producer() {
    for (int i = 0; i < 10; ++i) {
        counter.fetch_add(1); // 原子递增
        std::cout << "Produced: " << i << "\n";
    }
}

void consumer() {
    while (counter.load() < 10) {
        int val = counter.load();
        std::cout << "Consumed: " << val << "\n";
    }
}

int main() {
    std::thread p(producer);
    std::thread c(consumer);
    p.join();
    c.join();
    return0;
}
```

## **<font style="color:#595959;">12、系统调用是什么？new是系统调用还是用户调用？</font>**
#### <font style="color:black;">什么是系统调用？</font>
<font style="color:#595959;">系统调用 是用户程序（运行在用户态）请求操作系统内核（运行在内核态）提供服务的接口。</font>

<font style="color:#595959;">作用：</font>

+ <font style="color:#595959;">用户程序无法直接访问硬件或内核资源（如文件、内存、网络），需通过系统调用请求服务。</font>
+ <font style="color:#595959;">系统调用将控制权从用户态切换到内核态，执行后返回用户态。</font>

#### <font style="color:black;">工作原理</font>
<font style="color:#595959;">用户态 vs 内核态：</font>

+ <font style="color:#595959;">用户态：应用程序运行的环境，权限受限。</font>
+ <font style="color:#595959;">内核态：操作系统内核运行的环境，可访问硬件。</font>

<font style="color:#595959;">调用过程：</font>

1. <font style="color:#595959;">用户程序调用封装函数（如 C 库的 read）。</font>
2. <font style="color:#595959;">触发系统调用指令（如</font><font style="color:#595959;"> syscall 或 int 0x80</font><font style="color:#595959;">）。</font>
3. <font style="color:#595959;">CPU 切换到内核态，执行内核服务。</font>
4. <font style="color:#595959;">返回结果，切换回用户态。</font>

#### <font style="color:black;">new 是系统调用还是用户调用？</font>
<font style="color:#595959;">new </font>**<font style="color:#595959;">「不是系统调用」</font>**<font style="color:#595959;">，而是</font>**<font style="color:#595959;">「用户态调用」</font>**<font style="color:#595959;">。</font>

<font style="color:#595959;">在 C++ 中，new 是一个操作符，用于动态分配内存并构造对象。</font>

<font style="color:#595959;">new 的工作：</font>

1. <font style="color:#595959;">内存分配：调用内存分配函数（如 malloc 或底层分配器）。</font>
2. <font style="color:#595959;">对象构造：调用构造函数初始化对象。</font>

<font style="color:#595959;">用户态部分：</font>

+ <font style="color:#595959;">new 本身是 C++ 标准库的一部分，运行在用户态。</font>
+ <font style="color:#595959;">它通常通过调用 C 标准库的 malloc 来分配内存。</font>

<font style="color:#595959;">内核态部分：malloc 最终可能触发系统调用（如 Linux 的 brk 或 mmap）向操作系统请求内存。</font>

## **<font style="color:#595959;">13、系统调用流程？</font>**
<font style="color:#595959;">系统调用是用户程序请求操作系统内核提供服务的关键机制。系统调用的流程涉及用户态到内核态的切换、内核服务的执行以及结果的返回。</font>

#### <font style="color:black;">用户态准备</font>
<font style="color:#595959;">用户程序调用高级接口（如 C 库函数），设置系统调用所需的参数。</font>

<font style="color:#595959;">作用：</font>

+ <font style="color:#595959;">将请求转化为标准格式（如系统调用号和参数）。</font>
+ <font style="color:#595959;">通过库函数封装，避免直接操作底层。</font>

<font style="color:#595959;">实现：</font>

+ <font style="color:#595959;">C 标准库（如 libc）提供封装函数。</font>
+ <font style="color:#595959;">示例</font><font style="color:#595959;">：read、write、open。</font>

<font style="color:#595959;">细节：</font>

+ <font style="color:#595959;">参数通常存放在寄存器或栈中。</font>
+ <font style="color:#595959;">系统调用号（唯一标识服务的整数）由库函数设置。</font>

#### <font style="color:black;">触发系统调用</font>
<font style="color:#595959;">用户程序通过特定指令触发系统调用，切换到内核态。</font>

<font style="color:#595959;">作用：将控制权从用户态交给内核。</font>

<font style="color:#595959;">实现：</font>

+ <font style="color:#595959;">Linux：</font>
    - <font style="color:#595959;">老方法：软件中断 int 0x80。</font>
    - <font style="color:#595959;">新方法：syscall 指令（x86_64，更高效）。</font>
+ <font style="color:#595959;">Windows：使用 int 0x2e 或 sysenter。</font>

<font style="color:#595959;">细节：</font>

+ <font style="color:#595959;">系统调用号和参数通过寄存器传递：x86_64：rax（调用号）、rdi、rsi、rdx 等（参数）。</font>
+ <font style="color:#595959;">CPU 执行指令，切换到内核态。</font>

#### <font style="color:black;">内核态执行</font>
<font style="color:#595959;">操作系统内核接收请求，执行对应的服务。</font>

<font style="color:#595959;">作用：</font>

+ <font style="color:#595959;">处理用户请求（如读文件、分配内存）。</font>
+ <font style="color:#595959;">访问硬件或其他内核资源。</font>

<font style="color:#595959;">实现：</font>

+ <font style="color:#595959;">内核维护系统调用表（System Call Table），根据调用号分派服务。</font>
+ <font style="color:#595959;">示例（Linux）：read → sys_read（内核函数）。</font>

<font style="color:#595959;">步骤：</font>

1. <font style="color:#595959;">参数校验：检查参数合法性（如文件描述符有效性）。</font>
2. <font style="color:#595959;">服务执行：调用底层驱动或内核逻辑。</font>
3. <font style="color:#595959;">结果准备：将返回值写入寄存器或缓冲区。</font>

#### <font style="color:black;">返回用户态</font>
<font style="color:#595959;">内核完成服务后，将结果返回用户程序，切换回用户态。</font>

<font style="color:#595959;">作用：将执行结果（如返回值、错误码）传递给用户。</font>

<font style="color:#595959;">实现：</font>

+ <font style="color:#595959;">使用返回指令（如 sysret 或 iret）。</font>
+ <font style="color:#595959;">返回值通常存放在寄存器（如 rax）。</font>

<font style="color:#595959;">细节：</font>

+ <font style="color:#595959;">Linux：负值表示错误（如 -errno）。</font>
+ <font style="color:#595959;">用户态库将结果转换为标准形式（如 errno）。</font>

#### <font style="color:black;">流程示例</font>
```plain
#include <unistd.h>
#include <iostream>

int main() {
    char buf[10];
    ssize_t n = read(0, buf, 10); // 从标准输入读取
    if (n >= 0) {
        write(1, buf, n); // 输出到标准输出
    }
    return 0;
}
```

<font style="color:#595959;">（1）用户态准备：</font>

+ <font style="color:#595959;">调用 read(0, buf, 10)。</font>
+ <font style="color:#595959;">libc设置：</font>
    - <font style="color:#595959;">rax = 0（read 的系统调用号）。</font>
    - <font style="color:#595959;">rdi = 0（stdin）。</font>
    - <font style="color:#595959;">rsi = buf 地址。</font>
    - <font style="color:#595959;">rdx = 10。</font>

<font style="color:#595959;">（2）触发系统调用：</font>

<font style="color:#595959;">执行 syscall，切换到内核态。</font>

<font style="color:#595959;">（3）内核态执行：</font>

+ <font style="color:#595959;">内核查找 sys_read，从标准输入读取数据到 buf。</font>
+ <font style="color:#595959;">返回字节数（如 5）到 rax。</font>

<font style="color:#595959;">（4）返回用户态：</font>

+ <font style="color:#595959;">sysret 切换回用户态，n = 5。</font>
+ <font style="color:#595959;">write 类似流程输出数据。</font>

<font style="color:#595959;">前面也总结了超100篇面经，可以直接访问专栏：</font>

<font style="color:rgb(89, 89, 89);">《</font>[<font style="color:rgb(89, 89, 89);">面经专栏</font>](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU5MTgxNzI5Nw==&action=getalbum&album_id=3104351725295288321&scene=173&subscene=&sessionid=svr_f62a4cf6b3a&enterid=1743181880&from_msgid=2247492118&from_itemidx=1&count=3&nolastread=1&scene=21#wechat_redirect)<font style="color:rgb(89, 89, 89);">》</font>

<font style="color:#595959;">或者</font>

<font style="color:#595959;">Github：https://github.com/aqjsp</font>

<font style="color:#595959;">也放在了码云上，国内网可以顺利访问学习：</font>

<font style="color:#595959;">Gitee：https://gitee.com/aqjsp</font>

<font style="color:#595959;">发现有问题的知识点大家直接留言就行了，欢迎大家指正哦~</font>

<font style="color:#595959;">最后，我再跟大家说明一件事儿：</font>

**<font style="color:#595959;">「以后不再维护免费技术群！」</font>**

<font style="color:#595959;">其实大家能刷到我（菜鸟），也会刷到过其他大佬，他们都会有自己的学习交流群。</font>

<font style="color:#595959;">我今天为什么会说我不再免费维护呢？</font>

1. <font style="color:#595959;">之前有几个免费群，也有近千人，大家都在里面唠嗑干啥，无可厚非。可以说除了技术，什么都交流，我觉得这违背了我建群的初衷。</font>
2. <font style="color:#595959;">个人精力有限，新群会有其他大厂大佬一起维护，技术范围更广！</font>
3. <font style="color:#595959;">在轻松的氛围学习技术，学起来更高效。</font>

<font style="color:#595959;">最后一个免费群，大家可以先进：</font><font style="color:rgb(255, 76, 65);">广告党请绕道</font>

![]()

<font style="color:#595959;">也可以去主页查看联系方式~</font>

<font style="color:#595959;">往期精选：</font>

[<font style="color:#595959;">【春招面经】快手音视频C++一面，面试官虽然有点严肃，不过还是给过了！！</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492166&idx=1&sn=a0c287912e6c082f7d35ce39626e6fef&scene=21#wechat_redirect)

[<font style="color:#595959;">【春招面经】腾讯QQ--C++一面面经，虽然有些问题没回答好，但是算法题还是挽回了些分数，顺利二面！！！</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492118&idx=1&sn=3a649c7d2a2fbd8902b4e4b2a55cd785&scene=21#wechat_redirect)

[<font style="color:#595959;">【春招面经】腾讯地图C++一面凉经，只背了八股没准备好项目，面试结束秒挂。。。</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492089&idx=1&sn=e97cdf4bfe9eb56fa4cbc729b0036960&scene=21#wechat_redirect)

[<font style="color:#595959;">【春招面经】腾讯二面，三个半小时。。。腾讯都这么顶嘛？？</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492068&idx=1&sn=c795f1bbace48205b49b9d5ec476363b&scene=21#wechat_redirect)

[<font style="color:#595959;">【春招面经】字节C++系统工程师二面，面试官真大佬，不问到我答不上就继续深问。。。</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492042&idx=1&sn=a643f14e9ffcfaf7f1a76efed4b7b181&scene=21#wechat_redirect)

<font style="color:#595959;"></font>

> <font style="color:#595959;">来自: </font>[【春招面经】米哈游C++服务端开发（上），问题也太多了吧！网友：看到这么多已经汗流浃背了。。。](https://mp.weixin.qq.com/s/z6tifLfmzlZ0pOhM9ABMyQ)
>





