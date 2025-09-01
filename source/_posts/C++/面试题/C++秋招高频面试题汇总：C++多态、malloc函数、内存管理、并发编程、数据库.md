---
title: C++秋招高频面试题汇总：C++多态、malloc函数、内存管理、并发编程、数据库
date: '2025-09-01 01:47:33'
updated: '2025-09-01 01:47:33'
---
![](/images/c934fe96e457b00f925849529da2ce1e.gif)

<font style="color:rgb(25, 27, 31);">在竞争激烈的 C++ 秋招战场上，扎实掌握核心知识、精准应对面试提问是脱颖而出的关键。今天，为大家精心汇总 C++ 秋招中的高频面试题，聚焦多态、malloc、内存管理、并发编程、数据库这些重点领域。多态作为 C++ 面向对象编程的核心特性，让代码更具灵活性与扩展性，面试官常围绕其原理、实现方式深入考查。比如，动态多态如何借助虚函数与运行时绑定来达成，不同类型的多态（像函数重载体现的静态多态）在实际场景中怎样运用。malloc 作为 C 语言就有的动态内存分配函数，在 C++ 开发里也常用到。理解它与 C++ 专属的 new 操作符的差异，掌握其内存分配机制、失败处理方式，是应对面试的必备技能。</font>

<font style="color:rgb(25, 27, 31);">内存管理向来是 C++ 开发的重难点，内存泄漏、悬空指针等问题时刻考验开发者功底。智能指针如何借助 RAII 机制实现自动内存管理，堆和栈内存的区别及使用场景，都可能在面试中被问到。并发编程随着多核处理器普及愈发重要。线程与进程的区别、线程同步方式（如互斥锁、条件变量的运用）、死锁的成因与预防，都是面试官爱考察的方向。数据库方面，无论是 MySQL 等关系型数据库的索引原理、事务特性，还是 Redis 这类非关系型数据库的应用场景、数据结构，都可能出现在面试题中。接下来，就让我们深入剖析这些高频面试题，为你的秋招之路添砖加瓦，助力你斩获心仪 offer ！</font>

## **<font style="color:white;background-color:rgb(37, 132, 181);">Part1</font>****<font style="color:rgb(37, 132, 181);">C++多态：让代码 “七十二变”</font>**
### <font style="color:rgb(25, 27, 31);">1.1 C++多态</font>
<font style="color:rgb(25, 27, 31);">多态，是面向对象编程中的关键概念，简单来说，就是 “同一接口，不同实现” ，一个事物可以有多种形态。就好比生活中的交通工具，都有 “行驶” 这个接口，但汽车、自行车、飞机的行驶方式截然不同。在编程世界里，多态允许不同类的对象对同一消息做出不同响应。</font>

<font style="color:rgb(25, 27, 31);">假设我们有一个图形绘制程序，定义一个基类Shape，其中包含一个draw方法用于绘制图形。然后派生出Circle（圆形）和Rectangle（矩形）类，它们都重写了draw方法，以各自的方式实现图形绘制。当我们使用Shape类型的变量来引用Circle或Rectangle对象时，调用draw方法就会根据实际对象的类型，绘制出圆形或矩形。这就是多态的魅力，它让代码更加灵活和可扩展，无需大量的条件判断语句。</font>

<font style="color:rgb(25, 27, 31);">在 C++ 中，多态通过虚函数和动态绑定来实现；Java 则依靠方法重写和运行时动态绑定机制达成多态效果。多态在框架设计、插件系统、策略模式等场景中广泛应用，是构建灵活可扩展系统的重要基石。</font>

**<font style="color:rgb(25, 27, 31);">具体来说，多态的实现涉及以下几个关键点：</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">‌</font>**<font style="color:rgba(0, 0, 0, 0.9);">虚函数</font>**<font style="color:rgba(0, 0, 0, 0.9);">‌：在基类中使用virtual关键字声明的函数称为虚函数。这些函数可以在派生类中被重写。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">‌</font>**<font style="color:rgba(0, 0, 0, 0.9);">虚函数表（vtable）</font>**<font style="color:rgba(0, 0, 0, 0.9);">‌：编译器为包含虚函数的类生成一个虚函数表，表中存储了该类所有虚函数的地址。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">‌</font>**<font style="color:rgba(0, 0, 0, 0.9);">动态绑定</font>**<font style="color:rgba(0, 0, 0, 0.9);">‌：在运行时，根据对象的实际类型从虚函数表中查找并调用相应的函数。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">‌</font>**<font style="color:rgba(0, 0, 0, 0.9);">动态联编</font>**<font style="color:rgba(0, 0, 0, 0.9);">‌：通过基类指针或引用调用虚函数时，编译时不确定具体调用哪个实现，而是在运行时根据对象的实际类型确定。</font>

<font style="color:rgb(25, 27, 31);">通过这种方式，多态允许我们编写与特定实现无关的代码，提高了代码的灵活性和可维护性。同时，这也带来了额外的空间和时间开销，因为每个有虚函数的对象都需要额外的空间来存储vptr，并且在运行时需要进行虚函数表的查找。尽管如此，多态带来的好处通常超过了这些额外的开销，使得它在许多情况下成为首选的设计模式‌。</font>

### <font style="color:rgb(25, 27, 31);">1.2</font>**<font style="color:rgb(25, 27, 31);">多态如何解决代码复用难题</font>**
<font style="color:rgb(25, 27, 31);">在软件开发中，代码复用是提高开发效率、降低维护成本的关键。然而，在没有多态的情况下，实现代码复用往往面临诸多挑战。比如，我们要开发一个图形绘制系统，其中包含圆形、矩形和三角形等多种图形。如果不使用多态，那么为了绘制这些不同的图形，我们可能需要编写大量重复的代码。</font>

```plain
class Circle {
public:
    void drawCircle() {
        // 绘制圆形的具体代码
        std::cout << "Drawing a circle" << std::endl;
    }
};

class Rectangle {
public:
    void drawRectangle() {
        // 绘制矩形的具体代码
        std::cout << "Drawing a rectangle" << std::endl;
    }
};

class Triangle {
public:
    void drawTriangle() {
        // 绘制三角形的具体代码
        std::cout << "Drawing a triangle" << std::endl;
    }
};

int main() {
    Circle circle;
    Rectangle rectangle;
    Triangle triangle;

    circle.drawCircle();
    rectangle.drawRectangle();
    triangle.drawTriangle();

    return 0;
}
```

<font style="color:rgb(25, 27, 31);">在这段代码中，每个图形类都有自己独立的绘制函数，当我们需要绘制不同的图形时，需要分别调用不同的函数。如果后续要添加新的图形，比如梯形，就需要再次编写新的绘制函数，并且在使用时也需要额外添加调用逻辑，代码的扩展性和复用性都很差 。</font>

<font style="color:rgb(25, 27, 31);">而当我们引入多态后，情况就大不相同了。我们可以定义一个基类，比如Shape，在其中声明一个虚函数draw，然后让各个图形类继承自Shape类，并重写draw函数。</font>

```plain
class Shape {
public:
    virtual void draw() = 0; // 纯虚函数，使Shape成为抽象类
};

class Circle : public Shape {
public:
    void draw() override {
        // 绘制圆形的具体代码
        std::cout << "Drawing a circle" << std::endl;
    }
};

class Rectangle : public Shape {
public:
    void draw() override {
        // 绘制矩形的具体代码
        std::cout << "Drawing a rectangle" << std::endl;
    }
};

class Triangle : public Shape {
public:
    void draw() override {
        // 绘制三角形的具体代码
        std::cout << "Drawing a triangle" << std::endl;
    }
};

void drawShapes(Shape* shapes[], int count) {
    for (int i = 0; i < count; ++i) {
        shapes[i]->draw();
    }
}

int main() {
    Circle circle;
    Rectangle rectangle;
    Triangle triangle;

    Shape* shapes[] = {&circle, &rectangle, &triangle};
    int count = sizeof(shapes) / sizeof(shapes[0]);

    drawShapes(shapes, count);

    return 0;
}
```

<font style="color:rgb(25, 27, 31);">在这个改进后的代码中，drawShapes函数可以接受一个Shape类型的指针数组，无论数组中的元素是指向Circle、Rectangle还是Triangle对象，都可以通过调用draw函数来实现正确的绘制。这样，当我们需要添加新的图形时，只需要创建一个新的派生类并重写draw函数，而drawShapes函数的代码无需修改，大大提高了代码的复用性和可扩展性。</font>

### <font style="color:rgb(25, 27, 31);">1.3多态的实现原理</font>
<font style="color:rgb(25, 27, 31);">C++实现多态的主要方式有：</font>

**<font style="color:rgb(25, 27, 31);">(1)重载（Overloading）：</font>**<font style="color:rgb(25, 27, 31);">通过函数名相同但参数不同的多个函数实现不同行为。在编译时通过参数类型决定调用哪个函数。</font>

```plain
void add(int a, int b) { ... } 
void add(double a, double b) { ... }
```

**<font style="color:rgb(25, 27, 31);">(2)重写（Overriding）：</font>**<font style="color:rgb(25, 27, 31);">通过继承让派生类重新实现基类的虚函数。在运行时通过指针/引用的实际类型调用对应的函数。</font>

```plain
class Base {
public:
    virtual void func() { ... }
};

class Derived extends Base {
public:
    virtual void func() { ... } 
}; 

Base* b = new Derived();
b->func(); // Calls Derived::func()
```

**<font style="color:rgb(25, 27, 31);">(3)编译时多态：</font>**<font style="color:rgb(25, 27, 31);">通过模板和泛型实现针对不同类型具有不同实现的函数。在编译时通过传入类型决定具体实现。</font>

```plain
template <typename T>
void func(T t) { ... }

func(1);   // Calls func<int> 
func(3.2); // Calls func<double>
```

**<font style="color:rgb(25, 27, 31);">(4)条件编译：</font>**<font style="color:rgb(25, 27, 31);">通过#ifdef/#elif等预处理命令针对不同条件编译不同的代码实现产生不同行为的程序。编译时通过定义的宏决定具体实现</font>

```plain
#ifdef _WIN32 
    void func() { ... }   // Windows version
#elif __linux__
    void func() { ... }   // Linux version   
#endif
```

<font style="color:rgb(25, 27, 31);">综上,C++通过重载、重写、模板、条件编译等手段实现多态。其中,重写基于继承和虚函数实现真正的运行时多态,增强了程序的灵活性和可扩展性。</font>

**<font style="color:rgb(25, 27, 31);">一个接口，多种方法：</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">用virtual关键字申明的函数叫做虚函数，虚函数肯定是类的成员函数。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">存在虚函数的类都有一个一维的虚函数表叫做虚表。当类中声明虚函数时，编译器会在类中生成一个虚函数表。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">类的对象有一个指向虚表开始的虚指针。虚表是和类对应的，虚表指针是和对象对应的。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">虚函数表是一个存储类成员函数指针的数据结构。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">虚函数表是由编译器自动生成与维护的。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">virtual成员函数会被编译器放入虚函数表中。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">当存在虚函数时，每个对象中都有一个指向虚函数的指针（C++编译器给父类对象，子类对象提前布局vptr指针），当进行test(parent *base)函数的时候，C++编译器不需要区分子类或者父类对象，只需要在base指针中，找到vptr指针即可）。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">vptr一般作为类对象的第一个成员。</font>

## **<font style="color:white;background-color:rgb(37, 132, 181);">Part2</font>****<font style="color:rgb(37, 132, 181);">malloc：动态内存分配的魔法棒</font>**
### <font style="color:rgb(25, 27, 31);">2.1 malloc技术</font>
<font style="color:rgb(25, 27, 31);">在 C 和 C++ 编程中，malloc（memory allocation，动态内存分配）是一个非常重要的函数，它能在程序运行时根据需要动态分配内存。想象一下，你正在搭建一座积木城堡，一开始不知道需要多少积木，malloc就像是一个神奇的袋子，能随时根据你的需求提供积木（内存）。</font>

<font style="color:rgb(25, 27, 31);">malloc函数的原型是void* malloc(size_t size)，它接受一个参数size，表示需要分配的内存字节数。返回值是一个void*类型的指针，指向分配的内存块起始地址，如果分配失败则返回NULL 。例如，我们要动态分配一个能存储 10 个整数的数组，可以这样写：</font>

```plain
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr;
    // 分配10个整数大小的内存空间
    arr = (int*)malloc(10 * sizeof(int)); 
    if (arr == NULL) {
        printf("内存分配失败\n");
        return 1;
    }
    // 使用数组
    for (int i = 0; i < 10; i++) {
        arr[i] = i;
    }
    // 输出数组元素
    for (int i = 0; i < 10; i++) {
        printf("%d ", arr[i]);
    }
    // 释放内存
    free(arr); 
    return 0;
}
```

<font style="color:rgb(25, 27, 31);">在使用malloc时，有几个关键的注意事项。首先，一定要检查返回值是否为NULL，以确保内存分配成功，否则对空指针进行操作会导致程序崩溃。其次，当不再需要分配的内存时，必须使用free函数释放内存，否则会造成内存泄漏，就像你借了图书馆的书却不归还，导致后续其他人无法借阅（内存无法被重新利用）。并且，free只能释放由malloc、calloc、realloc分配的内存，不能释放其他类型的内存。同时，释放内存后，应将指针置为NULL，防止成为野指针，避免不小心再次访问已释放的内存区域。</font>

<font style="color:rgb(25, 27, 31);">与其他内存分配方式相比，malloc属于动态内存分配，与静态内存分配（如在函数内部定义的局部变量）不同，静态内存的生命周期在函数结束时就结束了，而动态分配的内存只要不调用free，就会一直存在，这使得它在需要灵活管理内存的场景中非常有用。和 C++ 中的new操作符相比，malloc只是分配内存，不会调用对象的构造函数进行初始化，而new会调用构造函数；malloc分配失败返回NULL，new则是抛出异常。</font>

### <font style="color:rgb(25, 27, 31);">2.2</font>**<font style="color:rgb(25, 27, 31);">Malloc函数</font>**
**<font style="color:rgb(25, 27, 31);">（1）数据结构</font>**

<font style="color:rgb(25, 27, 31);">首先我们要确定所采用的数据结构。一个简单可行方案是将堆内存空间以块的形式组织起来，每个块由meta区和数据区组成，meta区记录数据块的元信息（数据区大小、空闲标志位、指针等等），数据区是真实分配的内存区域，并且数据区的第一个字节地址即为malloc返回的地址，可以使用如下结构体定义一个block</font>

```plain
typedef struct s_block *t_block;
struck s_block{
    size_t size;//数据区大小
    t_block next;//指向下个块的指针
    int free;//是否是空闲块
    int padding;//填充4字节，保证meta块长度为8的倍数
    char data[1];//这是一个虚拟字段，表示数据块的第一个字节，长度不应计入meta
};
```

**<font style="color:rgb(25, 27, 31);">（2）寻找合适的block</font>**

<font style="color:rgb(25, 27, 31);">现在考虑如何在block链中查找合适的block。一般来说有两种查找算法：First fit:从头开始，使用第一个数据区大小大于要求size的块所谓此次分配的块 Best fit:从头开始，遍历所有块，使用数据区大小大于size且差值最小的块作为此次分配的块 两种方式各有千秋，best fit有较高的内存使用率（payload较高），而first fit具有较高的运行效率。这里我们采用first fit算法</font>

```plain
t_block find_block(t_block *last,size_t size){
    t_block b = first_block;
    while(b&&b->size>=size)
    {
        *last = b;
        b = b->next;
    }
    return b;
}
```

<font style="color:rgb(25, 27, 31);">find_block从first_block开始，查找第一个符合要求的block并返回block起始地址，如果找不到这返回NULL，这里在遍历时会更新一个叫last的指针，这个指针始终指向当前遍历的block.这是为了如果找不到合适的block而开辟新block使用的。</font>

**<font style="color:rgb(25, 27, 31);">（3）开辟新的block</font>**

<font style="color:rgb(25, 27, 31);">如果现有block都不能满足size的要求，则需要在链表最后开辟一个新的block。这里关键是如何只使用sbrk创建一个struct：</font>

```plain
#define BLOCK_SIZE 24

t_block extend_heap{
    t_block b;
    b = sbrk(0);
        if(sbrk(BLOCK_SIZE+s)==(void*)-1)
        return NULL;
        b->size = s;
        b->next - NULL;
        if(last)
        last->next = b;
        b->free = 0;
        return b;
};
```

**<font style="color:rgb(25, 27, 31);">（4）分裂block</font>**

<font style="color:rgb(25, 27, 31);">First fit有一个比较致命的缺点，就是可能会让更小的size占据很大的一块block，此时，为了提高payload,应该在剩余数据区足够大的情况下，将其分裂为一个新的block：</font>

```plain
void split_block(t_block b,size_t s)
{
    t_block new;
    new = b->data;
    new->size = b->size-s-BLOCK_SIZE;
    new->next = b->next;
    new ->free = 1;
    b->size = s;
    b->next = new;
}
```

### <font style="color:rgb(25, 27, 31);">2.3</font>**<font style="color:rgb(25, 27, 31);">Malloc函数的实现原理</font>**
**<font style="color:rgb(25, 27, 31);">（1）空闲链表机制</font>**

<font style="color:rgb(25, 27, 31);">在malloc函数背后，有着依靠空闲链表来管理内存的一套机制。当我们调用malloc函数时，它会沿着空闲链表去查找满足用户请求大小的内存块。比如说，链表上有多个不同大小的空闲内存块，它就会依次遍历这些块来找到合适的那一个。</font>

<font style="color:rgb(25, 27, 31);">找到合适的内存块后，如果这个内存块比用户请求的大小要大，那么就会按需将其分割成两部分，一部分的大小刚好与用户请求的大小相等，这部分就会分配给用户使用，而剩下的那部分则会被放回空闲链表中，等待后续其他的内存分配请求再进行分配。例如，空闲链表中有一个 50 字节的空闲块，而用户请求分配 20 字节的内存，这时malloc就会把这个 50 字节的块分成 20 字节（分配给用户）和 30 字节（放回空闲链表）两块。</font>

<font style="color:rgb(25, 27, 31);">而当我们使用free函数释放内存时，相应被释放的内存块又会被重新连接到空闲链上。这样，整个空闲链表就处于一个动态变化的过程，不断地有内存块被分配出去，也不断地有释放的内存块回归链表，以实现内存的循环利用，避免浪费。不过，随着程序不断地分配和释放内存，空闲链有可能会被切成很多的小内存片段，要是后续用户申请一个较大的内存片段时，空闲链上可能暂时没有可以满足要求的片段了，这时malloc函数可能就需要进行一些整理操作，比如对这些小的空闲块尝试合并等，以便能满足较大内存请求的情况。</font>

**<font style="color:rgb(25, 27, 31);">（2）虚拟内存地址和物理内存地址</font>**

<font style="color:rgb(25, 27, 31);">为了简单，现代操作系统在处理物理内存地址时，普遍采用虚拟内存地址技术。即在汇编程序层面，当涉及内存地址时，都是使用的虚拟内存地址。采用这种技术时，每个进程仿佛自己独享一片2N字节的内存，其中N是机器位数。例如在64位CPU和64位操作系统下每个进程的虚拟地址空间为264Byte。</font>

<font style="color:rgb(25, 27, 31);">这种虚拟地址空间的作用主要是简化程序的编写及方便操作系统对进程间内存的隔离管理，真实中的进程不太可能如此大的空间，实际能用到的空间大小取决于物理内存的大小。由于在机器语言层面都是采用虚拟地址，当实际的机器码程序涉及到内存操作时，需要根据当前进程运行的实际上下文将虚拟地址转化为物理内存地址，才能实现对内存数据的操作。这个转换一般由一个叫MMU的硬件完成。</font>

**<font style="color:rgb(25, 27, 31);">（3）页与地址构成</font>**

<font style="color:rgb(25, 27, 31);">在现代操作系统中，不论是虚拟内存还是物理内存，都不是以字节为单位进行管理的，而是以页为单位。一个内存页是一段固定大小的连续的连续内存地址的总称，具体到Linux中，典型的内存页大小为4096 Byte。所以内存地址可以分为页号和页内偏移量。下面以64位机器，4G物理内存，4K页大小为例，虚拟内存地址和物理内存地址的组成如下：</font>

![](/images/15e6319662227b92b3d759df1105d6df.jpeg)

<font style="color:rgb(25, 27, 31);">上面是虚拟内存地址，下面是物理内存地址。由于页大小都是4k，所以页内偏移都是用低12位表示，而剩下的高地址表示页号 MMU映射单位并不是字节，而是页，这个映射通过差一个常驻内存的数据结构页表来实现。现在计算机具体的内存地址映射比较复杂，为了加快速度会引入一系列缓存和优化，例如TLB等机制，</font>**<font style="color:rgb(25, 27, 31);">下面给出一个经过简化的内存地址翻译示意图：</font>**

![](/images/830430397bf3b53c31d9270252c252bf.jpeg)

**<font style="color:rgb(25, 27, 31);">（4）内存页与磁盘页</font>**

<font style="color:rgb(25, 27, 31);">我们知道一般将内存看做磁盘的缓存，有时MMU在工作时，会发现页表表名某个内存页不在物理内存页不在物理内存中，此时会触发一个缺页异常，此时系统会到磁盘中相应的地方将磁盘页载入到内存中，然后重新执行由于缺页而失败的机器指令。关于这部分，因为可以看做对malloc实现是透明的，所以不再详述。</font>

<font style="color:rgb(25, 27, 31);">真实地址翻译流程：</font>

![](/images/643ae5307e891ee22cd619720ba1d9cb.jpeg)

**<font style="color:rgb(25, 27, 31);">（5）Linux进程级内存管理</font>**

<font style="color:rgb(25, 27, 31);">内存排布：明白了虚拟内存和物理内存的关系及相关的映射机制，下面看一下具体在一个进程内是如何排布内存的。以Linux 64位系统为例。理论上，64bit内存地址空间为0x0000000000000000-0xFFFFFFFFFFFFFFF，这是个相当庞大的空间，Linux实际上只用了其中一小部分。具体分布如图所示：</font>

![](/images/b4918a55339aebcccaab6727b8958091.jpeg)

<font style="color:rgb(25, 27, 31);">对用户来说主要关心的是User Space。将User Space放大后，可以看到里面主要分成如下几段：</font>

+ <font style="color:rgba(0, 0, 0, 0.9);">Code：这是整个用户空间的最低地址部分，存放的是指令（也就是程序所编译成的可执行机器码） Data：这里存放的是初始化过的全局变量</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">BSS：这里存放的是未初始化的全局变量</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">Heap：堆，这是我们本文主要关注的地方，堆自底向上由低地址向高地址增长</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">Mapping Area：这里是与mmap系统调用相关区域。大多数实际的malloc实现会考虑通过mmap分配较大块的内存空间，本文不考虑这种情况，这个区域由高地址像低地址增长Stack:栈区域，自高地址像低地址增长 。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">Heap内存模型：一般来说，malloc所申请的内存主要从Heap区域分配，来看看Heap的结构是怎样的。</font>

![](/images/c20d4a66f93bf57b2cdc969b62a16ec7.jpeg)

<font style="color:rgb(25, 27, 31);">Linux维护一个break指针，这个指针执行堆空间的某个地址，从堆开始到break之间的地址空间为映射好的，可以供进程访问，而从break往上，是未映射的地址空间，如果访问这段空间则程序会报错。</font>

### <font style="color:rgb(25, 27, 31);">2.4malloc函数的使用注意事项</font>
**<font style="color:rgb(25, 27, 31);">⑴内存分配成功判断</font>**

<font style="color:rgb(25, 27, 31);">在使用 malloc 函数时，一定要进行内存分配成功与否的判断哦。因为 malloc 函数在执行后，有可能会由于系统内存不足等原因，导致无法按照要求分配出相应的内存空间。此时，它会返回一个 NULL 指针来表示分配失败。</font>

<font style="color:rgb(25, 27, 31);">例如，我们写这样一段代码：</font>

```plain
int *p = (int *)malloc(1000000000 * sizeof(int));  // 尝试分配非常大的内存空间，可能超出系统可分配范围
if (p == NULL) {
    printf("内存分配失败，无法继续执行后续操作！\n");
    // 在这里可以添加一些应对分配失败的处理逻辑，比如返回错误码、进行相应提示等
    return -1;
}
// 如果分配成功，就可以继续使用这块内存，例如进行赋值等操作
*p = 10;
```

<font style="color:rgb(25, 27, 31);">像这样通过检查返回的指针是否为 NULL，就能知道内存分配是不是成功啦。要是忽略了这个判断，后续还继续去使用这个可能为 NULL 的指针，就很容易引发程序崩溃，比如出现段错误等情况呢，所以这一步的判断千万不能省略哦。</font>

**<font style="color:rgb(25, 27, 31);">⑵内存释放操作</font>**

<font style="color:rgb(25, 27, 31);">使用完 malloc 函数分配的内存后，及时用 free 函数进行释放是非常重要的操作呀。因为如果一直不释放这些申请来的内存，它们就会始终被占用，久而久之，就容易造成内存泄漏问题。内存泄漏积累起来，会不断消耗系统的可用内存资源，可能使得程序运行越来越卡顿，严重的话甚至会导致整个系统瘫痪呢。</font>

<font style="color:rgb(25, 27, 31);">free 函数的使用方式很简单，它的原型是 void free(void *FirstByte)，参数就是之前 malloc 分配内存时返回的那个指针。举个简单的例子来说明一下：</font>

```plain
char *str = (char *)malloc(50 * sizeof(char));  // 分配可以存放50个字符的内存空间
if (str!= NULL) {
    strcpy(str, "Hello World");  // 使用分配的内存空间存放字符串
    // 一些其他对这块内存的操作......
    free(str);  // 使用完后，用free函数释放内存
}
```

<font style="color:rgb(25, 27, 31);">这样就把通过 malloc 申请的内存归还给系统了，让系统可以把这些内存再次分配给其他需要的部分使用哦。</font>

**<font style="color:rgb(25, 27, 31);">⑶避免野指针问题</font>**

<font style="color:rgb(25, 27, 31);">当我们使用 free 函数释放了 malloc 分配的内存后，还有一个需要特别注意的点，那就是要避免野指针的出现哦。野指针就是指向了一块已经被释放的内存区域或者是指向不确定内存位置的指针啦。</font>

<font style="color:rgb(25, 27, 31);">比如说，有这样一段代码：</font>

```plain
int *ptr = (int *)malloc(10 * sizeof(int));
if (ptr!= NULL) {
    *ptr = 10;
    free(ptr);
    // 这里如果没有将ptr置为NULL，ptr就变成了野指针
    if (ptr!= NULL) {  // 这个判断其实是无效的哦，可能会意外进入
        *ptr = 20;  // 此时再去对这块已经释放的内存区域进行写操作，就是错误的，可能导致程序出现未定义行为，比如崩溃或者数据错乱等情况
    }
}
```

<font style="color:rgb(25, 27, 31);">所以呀，为了避免这种情况发生，在释放内存后，正确的做法是把对应的指针置为 NULL，像这样修改一下上面的代码：</font>

```plain
int *ptr = (int *)malloc(10 * sizeof(int));
if (ptr!= NULL) {
    *ptr = 10;
    free(ptr);
    ptr = NULL;  // 释放后将指针置为NULL，避免成为野指针
}
```

<font style="color:rgb(25, 27, 31);">这样就能有效地防止意外访问已经释放的内存区域，减少程序出现错误的风险啦。</font>

## **<font style="color:white;background-color:rgb(37, 132, 181);">Part3</font>****<font style="color:rgb(37, 132, 181);">内存管理：守护程序的 “内存管家”</font>**
### <font style="color:rgb(25, 27, 31);">3.1 Linux内存管理</font>
<font style="color:rgb(25, 27, 31);">内存管理，是计算机系统中极为关键的一环，就像一个精密工厂的物料管理员，负责着内存资源的分配、回收和优化，确保程序能够高效稳定地运行。在多任务环境下，多个程序同时运行，内存管理要合理地为每个程序分配内存空间，避免冲突和资源浪费。</font>

<font style="color:rgb(25, 27, 31);">内存分配方式主要有静态分配和动态分配。静态分配在程序编译时就确定了内存大小和位置，比如全局变量和静态变量，它们在程序运行期间一直占据固定内存，优点是简单高效，缺点是缺乏灵活性 。动态分配则是在程序运行时根据需要申请内存，如 C 语言中的malloc、C++ 的new以及 Java 中的new操作，它赋予了程序根据实际情况灵活使用内存的能力，但也带来了管理的复杂性，如内存泄漏和内存碎片等问题。</font>

<font style="color:rgb(25, 27, 31);">内存回收同样至关重要。在 C 和 C++ 中，使用free（对应malloc）和delete（对应new）手动释放不再使用的内存；Java、Python 等语言则引入了垃圾回收机制（Garbage Collection，GC），自动检测和回收不再被引用的对象所占用的内存，大大减轻了开发者的负担，不过垃圾回收也会带来一定的性能开销，并且可能导致程序出现短暂停顿。</font>

<font style="color:rgb(25, 27, 31);">内存优化是提升系统性能的关键。优化方法包括减少内存碎片，内存碎片是由于频繁的内存分配和释放导致的内存不连续现象，会降低内存利用率，可通过采用合适的内存分配算法（如伙伴系统算法、 slab 分配器等）来缓解；合理使用缓存，缓存利用局部性原理，将常用数据存储在高速缓存中，减少对主内存的访问次数，从而提高数据访问速度；还有优化数据结构，选择合适的数据结构可以减少内存占用，例如在需要频繁插入和删除元素的场景中，使用链表比数组更节省内存。</font>

<font style="color:rgb(25, 27, 31);">内存管理不当会引发一系列问题，如内存泄漏，内存泄漏指程序中已分配的内存由于某种原因未被释放，随着程序运行，泄漏的内存越来越多，最终导致系统内存耗尽，程序崩溃。再比如空指针引用，当访问一个空指针指向的内存时，会导致程序异常终止，这通常是由于内存分配失败或内存释放后未将指针置为NULL造成的。</font>

### <font style="color:rgb(25, 27, 31);">3.2</font>**<font style="color:rgb(25, 27, 31);">物理内存与虚拟内存</font>**
**<font style="color:rgb(25, 27, 31);">（1）物理内存的管理</font>**

<font style="color:rgb(25, 27, 31);">物理内存是计算机系统中实际存在的内存，它由计算机硬件直接管理，通常由 DRAM 芯片组成，是计算机系统中最快的存储器 ，其大小通常是固定的，取决于计算机硬件的配置。在 Linux 内核中，物理内存被划分为一个个固定大小的页框（Page Frame），每个页框通常为 4KB（也有其他大小，如 2MB、1GB 的大页）。内核通过页框号（Page Frame Number，PFN）来管理这些页框，就像给图书馆的每一本书都编上了唯一的编号，方便查找和管理。</font>

<font style="color:rgb(25, 27, 31);">物理内存的作用至关重要，它是程序运行和数据存储的直接场所。当程序运行时，其代码和数据会被加载到物理内存中，CPU 直接从物理内存中读取指令和数据进行处理。比如，当我们启动一个文本编辑器时，编辑器的程序代码会被加载到物理内存的某个区域，我们在编辑器中输入的文本数据也会存储在物理内存中，这样 CPU 才能快速地对这些数据进行处理，实现文本的编辑、保存等操作。</font>

**<font style="color:rgb(25, 27, 31);">（2）虚拟内存的奥秘</font>**

<font style="color:rgb(25, 27, 31);">虚拟内存，简单来说，是一种内存管理技术，它为每个进程提供了一个独立的、连续的地址空间，让进程误以为自己拥有一块完整且足够大的内存空间 ，而无需关心实际物理内存的具体布局和大小限制。这就好比你拥有一个超大的虚拟仓库，你可以随意规划货物的摆放位置，而不用担心仓库空间不够。</font>

<font style="color:rgb(25, 27, 31);">虚拟内存的主要作用之一是实现内存地址转换。在 Linux 系统中，每个进程都有自己的虚拟地址空间，这个空间通过页表（Page Table）与物理内存进行映射。页表就像是一本地址翻译字典，负责将进程使用的虚拟地址翻译成实际的物理地址。</font>

<font style="color:rgb(25, 27, 31);">当程序运行时，它所访问的内存地址都是虚拟地址。例如，当程序需要读取某个变量的值时，它会给出一个虚拟地址。CPU 首先会根据这个虚拟地址中的页号（Page Number）在页表中查找对应的物理页框号（Page Frame Number）。如果页表中存在这个映射关系（即页表项有效），CPU 就可以通过物理页框号和虚拟地址中的页内偏移（Offset）计算出实际的物理地址，从而访问到物理内存中的数据。</font>

<font style="color:rgb(25, 27, 31);">但如果页表中没有找到对应的映射关系（即发生缺页异常，Page Fault），系统会认为这个虚拟页还没有被加载到物理内存中。此时，操作系统会介入，从磁盘的交换区（Swap Area）或者文件系统中找到对应的物理页，并将其加载到物理内存中，同时更新页表，建立虚拟地址与物理地址的映射关系。之后，程序就可以通过新建立的映射关系访问到数据了。</font>

<font style="color:rgb(25, 27, 31);">为了更直观地理解，我们可以把虚拟内存想象成一个图书馆的目录系统。每个进程就像是一个读者，拥有自己的目录（虚拟地址空间）。当读者想要查找某本书（访问数据）时，会先在自己的目录中找到对应的条目（虚拟地址），然后通过这个条目去书架（物理内存）上找到实际的书。如果书架上没有这本书（缺页异常），图书馆管理员（操作系统）就会从仓库（磁盘）中把书取出来放到书架上，并更新目录（页表），以便下次读者能更快地找到这本书。</font>

**<font style="color:rgb(25, 27, 31);">例如：对于程序计数器位数为32位的处理器来说，他的地址发生器所能发出的地址数目为2^32=4G个，于是这个处理器所能访问的最大内存空间就是4G。在计算机技术中，这个值就叫做处理器的寻址空间或寻址能力。</font>**

<font style="color:rgb(25, 27, 31);">照理说，为了充分利用处理器的寻址空间，就应按照处理器的最大寻址来为其分配系统的内存。如果处理器具有32位程序计数器，那么就应该按照下图的方式，为其配备4G的内存：</font>

![](/images/f3941515c06799a340a40c7e4c62a115.jpeg)

<font style="color:rgb(25, 27, 31);">这样，处理器所发出的每一个地址都会有一个真实的物理存储单元与之对应；同时，每一个物理存储单元都有唯一的地址与之对应。这显然是一种最理想的情况。</font>

### <font style="color:rgb(25, 27, 31);">3.3分页：虚拟内存的基石</font>
<font style="color:rgb(25, 27, 31);">在 Linux 的世界里，内存分页机制就像是一位有条不紊的大管家，精心管理着系统的内存资源。简单来说，内存分页机制就是把物理内存和虚拟内存分割成固定大小的小块，这些小块被称作 “页” ，每个页的大小一般为 4KB 或者 8KB。就好比你有一个巨大的仓库（内存），为了更好地管理里面的货物（数据），你把仓库划分成了一个个大小相同的小隔间（页）。</font>

![](/images/7a787875a49124bdeae857a50a7766ec.jpeg)

**<font style="color:rgb(25, 27, 31);">(1)什么是分页机制</font>**

<font style="color:rgb(25, 27, 31);">分页机制是 80x86 内存管理机制的第二部分。它在分段机制的基础上完成虚拟地址到物理地址的转换过程。分段机制把逻辑地址转换成线性地址，而分页机制则把线性地址转换成物理地址。分页机制可用于任何一种分段模型。处理器分页机制会把线性地址空间划分成页面，然后这些线性地址空间页面被映射到物理地址空间的页面上。分页机制的几种页面级保护措施，可和分段机制保护措施或用或替代分段机制的保护措施。</font>

**<font style="color:rgb(25, 27, 31);">(2)分页机制如何启用</font>**

<font style="color:rgb(25, 27, 31);">在我们进行程序开发的时候，一般情况下，是不需要管理内存的，也不需要操心内存够不够用，其实，这就是分页机制给我们带来的好处。它是实现虚拟存储的关键，位于线性地址与物理地址之间，在使用这种内存分页管理方法时，每个执行中的进程（任务）可以使用比实际内存容量大得多的连续地址空间。而且当系统内存实际上被分成很多凌乱的块时，它可以建立一个大而连续的内存空间的映象，好让程序不用操心和管理这些分散的内存块。</font>

<font style="color:rgb(25, 27, 31);">分页机制增强了分段机制的性能。页地址变换是建立在段变换基础之上的。因为，段管理机制对于Intel处理器来说是最基本的，任何时候都无法关闭。所以即使启用了页管理功能，分段机制依然是起作用的，段部件也依然工作。</font>

<font style="color:rgb(25, 27, 31);">分页只能在保护模式（CR0.PE = 1）下使用。在保护模式下，是否开启分页，由 CR0. PG 位（位 31）决定：</font>

+ <font style="color:rgba(0, 0, 0, 0.9);">当 CR0.PG = 0 时，未开启分页，线性地址等同于物理地址；</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">当 CR0.PG = 1 时，开启分页。</font>

**<font style="color:rgb(25, 27, 31);">(3)分页机制线性地址到物理地址转换过程</font>**

<font style="color:rgb(25, 27, 31);">80x86使用 4K 字节固定大小的页面，每个页面均是 4KB，并且对其于 4K 地址边界处。这表示分页机制把 2^32字节(4GB)的线性地址空间划分成 2^20(1M = 1048576)个页面。分页机制通过把线性地址空间中的页面重新定位到物理地址空间中进行操作。由于 4K 大小的页面作为一个单元进行映射，并且对其于 4K 边界，因此线性地址的低 12 位可做为页内偏移地量直接作为物理地址的低 12 位。分页机制执行的重定向功能可以看作是把线性地址的高 20 位转换到对应物理地址的高 20 位。</font>

<font style="color:rgb(25, 27, 31);">线性到物理地址转换功能，被扩展成允许一个线性地址被标注为无效的，而非要让其产生一个物理地址。以下两种情况一个页面可以被标注为无效的：</font>

+ <font style="color:rgb(25, 27, 31);">1. 操作系统不支持的线性地址。</font>
+ <font style="color:rgb(25, 27, 31);">2. 对应的虚拟内存系统中的页面在磁盘上而非在物理内存中。</font>

<font style="color:rgb(25, 27, 31);">在第一中情况下，产生无效地址的程序必须被终止，在第二种情况下，该无效地址实际上是请求 操作系统虚拟内存管理器 把对应的页面从磁盘加载到物理内存中，以供程序访问。因为无效页面通常与虚拟存储系统相关，因此它们被称为不存在页面，由页表中称为存在的属性来确定。</font>

<font style="color:rgb(25, 27, 31);">当使用分页时，处理器会把线性地址空间划分成固定大小的页面（4KB），这些页面可以映射到物理内存中或磁盘存储空间中，当一个程序引用内存中的逻辑地址时，处理器会把该逻辑地址转换成一个线性地址，然后使用分页机制把该线性地址转换成对应的物理地址。</font>

<font style="color:rgb(25, 27, 31);">如果包含线性地址的页面不在当前物理内存中，处理器就会产生一个页错误异常。页错误异常处理程序就会让操作系统从磁盘中把相应页面加载到物理内存中（操作过程中可能会把物理内存中不同的页面写到磁盘上）。当页面加载到物理内存之后，从异常处理过程的返回操作会使异常的指令被重新执行。处理器把用于线性地址转换成物理地址和用于产生页错误的信息包含在存储与内存中的页目录与页表中。</font>

**<font style="color:rgb(25, 27, 31);">(4)分页机制与分段机制的不同</font>**

<font style="color:rgb(25, 27, 31);">分页与分段的最大的不同之处在于分页使用了固定长度的页面。段的长度通常与存放在其中的代码或数据结构有相同的长度。与段不同，页面有固定的长度。如果仅使用分段地址转换，那么存储在物理内存中的一个数据结构将包含其所有的部分。如果使用了分页，那么一个数据结构就可以一部分存储与物理内存中，而另一部分保存在磁盘中。</font>

<font style="color:rgb(25, 27, 31);">为了减少地址转换所要求的总线周期数量，最近访问的页目录和页表会被存放在处理器的一个叫做转换查找缓冲区（TLB）的缓冲器件中。TLB 可以满足大多数读页目录和页表的请求而无需使用总线周期。只有当 TLB 中不包含所要求的页表项是才会出现使用额外的总线周期从内存读取页表项。通常在一个页表项很长时间没有访问过时才会出现这种情况。</font>

### <font style="color:rgb(25, 27, 31);">3.4交换空间：内存不足时的后盾</font>
<font style="color:rgb(25, 27, 31);">首先呢，提一个概念，交换空间（swap space），这个大家应该不陌生，在重装系统的时候，会让你选择磁盘分区，就比如说一个硬盘分几个部分去管理。其中就会分一部分磁盘空间用作交换，叫做swap space。其实就是一段临时存储空间，内存不够用的时候就用它了，虽然它也在磁盘中，但省去了很多的查找时间啊。当发生进程切换的时候，内存与交换空间就要发生数据交换一满足需求。所以啊，进程的切换消耗是很大的，这也说明了为什么自旋锁比信号量效率高的原因。</font>

<font style="color:rgb(25, 27, 31);">那么我们的程序里申请的内存的时候，linux内核其实只分配一个虚拟内存（ 线性地址），并没有分配实际的物理内存。只有当程序真正使用这块内存时，才会分配物理内存。这就叫做延迟分配和请页机制。释放内存时，先释放线性区对应的物理内存，然后释放线性区；"请页机制"将物理内存的分配延后了，这样是充分利用了程序的局部性原来，节约内存空间，提高系统吞吐；就是说一个函数可能只在物理内存中呆了一会，用完了就被清除出去了，虽然在虚拟地址空间还在。（不过虚拟地址空间不是事实上的存储，所以只能说这个函数占据了一段虚拟地址空间，当你访问这段地址时，就会产生缺页处理，从交换区把对应的代码搬到物理内存上来）</font>

<font style="color:rgb(25, 27, 31);">Swap 交换机制是 Linux 虚拟内存管理的另一个重要组成部分。简单来说，Swap 是磁盘上的一块区域，当物理内存不足时，系统会将一部分暂时不用的内存页面（Page）交换到 Swap 空间中，腾出物理内存给更需要的进程使用 。当被交换出去的页面再次被访问时，系统会将其从 Swap 空间换回到物理内存中。</font>

<font style="color:rgb(25, 27, 31);">Swap 交换机制的工作原理涉及到内存回收和页面置换算法。当系统内存紧张时，内核会启动内存回收机制，扫描内存中的页面，选择一些不常用或最近最少使用的页面进行回收。如果这些页面是匿名页面（没有关联到文件的内存页面，如进程的堆和栈空间），就会被交换到 Swap 空间中；如果是文件映射页面（关联到文件的内存页面，如共享库、文件缓存等），则会根据情况进行处理，脏页面（被修改过的页面）会被写回文件，干净页面（未被修改过的页面）可以直接释放。</font>

<font style="color:rgb(25, 27, 31);">Swap 交换机制对系统性能有着重要的影响。当 Swap 使用频繁时，说明物理内存不足，系统需要频繁地在物理内存和 Swap 空间之间交换页面，这会导致磁盘 I/O 增加，系统性能下降 。因为磁盘的读写速度远远低于内存，过多的 Swap 操作会使系统变得迟缓。因此，在实际应用中，需要合理配置 Swap 空间的大小，并密切关注系统的内存使用情况，避免 Swap 过度使用。</font>

<font style="color:rgb(25, 27, 31);">例如，可以通过调整/proc/sys/vm/swappiness参数来控制系统对 Swap 的使用倾向，swappiness的值范围是 0 - 100，表示系统将内存页面交换到 Swap 空间的倾向程度，值越大表示越倾向于使用 Swap 。一般来说，对于内存充足的系统，可以将swappiness设置为较低的值，如 10 或 20，以减少不必要的 Swap 操作；对于内存紧张的系统，可以适当提高swappiness的值，但也要注意不要过高，以免严重影响性能。</font>

### <font style="color:rgb(25, 27, 31);">3.5内存映射：高效 I/O 的秘诀</font>
<font style="color:rgb(25, 27, 31);">内存映射，英文名为 Memory - mapped I/O，从字面意思理解，就是将磁盘文件的数据映射到内存中。在 Linux 系统中，这一机制允许进程把一个文件或者设备的数据关联到内存地址空间，使得进程能够像访问内存一样对文件进行操作 。</font>

<font style="color:rgb(25, 27, 31);">举个简单的例子，假设有一个文本文件，通常我们读取它时，会使用read函数，数据从磁盘先读取到内核缓冲区，再拷贝到用户空间。而内存映射则直接在进程的虚拟地址空间中为这个文件创建一个映射区域，进程可以直接通过指针访问这个映射区域，就好像文件数据已经在内存中一样，大大简化了文件操作的流程 。</font>

<font style="color:rgb(25, 27, 31);">内存映射的工作原理涉及到虚拟内存、页表以及文件系统等多个方面的知识。当进程调用mmap函数进行内存映射时，</font>**<font style="color:rgb(25, 27, 31);">大致会经历以下几个关键步骤 ：</font>**

1. <font style="color:rgba(0, 0, 0, 0.9);">虚拟内存区域创建：系统首先在进程的虚拟地址空间中寻找一段满足要求的连续空闲虚拟地址，然后为这段虚拟地址分配一个vm_area_struct结构，这个结构用于描述虚拟内存区域的各种属性，如起始地址、结束地址、权限等，并将其插入到进程的虚拟地址区域链表或树中 。就好比在一片空地上，规划出一块特定大小和用途的区域，并做好标记。</font>
2. <font style="color:rgba(0, 0, 0, 0.9);">地址映射建立：通过待映射的文件指针，找到对应的文件描述符，进而链接到内核 “已打开文件集” 中该文件的文件结构体。再通过这个文件结构体，调用内核函数mmap，定位到文件磁盘物理地址，然后通过remap_pfn_range函数建立页表，实现文件物理地址和进程虚拟地址的一一映射关系 。这一步就像是在规划好的区域和实际的文件存储位置之间建立起一条通道，让数据能够顺利流通。不过，此时只是建立了地址映射，真正的数据还没有拷贝到内存中 。</font>
3. <font style="color:rgba(0, 0, 0, 0.9);">数据加载（缺页异常处理）：当进程首次访问映射区域中的数据时，由于数据还未在物理内存中，会触发缺页异常。内核会捕获这个异常，然后在交换缓存空间（swap cache）中寻找需要访问的内存页，如果没有找到，则调用nopage函数把所缺的页从磁盘装入到主存中 。这个过程就像是当你需要使用某个物品，但它不在身边，你就需要去存放它的地方把它取回来。之后，进程就可以对这片主存进行正常的读或写操作，如果写操作改变了数据内容，系统会在一定时间后自动将脏页面回写脏页面到对应磁盘地址，完成写入到文件的过程 。当然，也可以调用msync函数来强制同步，让数据立即保存到文件里 。</font>

**<font style="color:rgb(25, 27, 31);">mmap内存映射的实现过程，总的来说可以分为三个阶段：</font>**

**<font style="color:rgb(25, 27, 31);">①进程启动映射过程，并在虚拟地址空间中为映射创建虚拟映射区域</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">1、进程在用户空间调用库函数mmap，原型：void *mmap(void *start, size_t length, int prot, int flags, int fd, off_t offset);</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">2、在当前进程的虚拟地址空间中，寻找一段空闲的满足要求的连续的虚拟地址</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">3、为此虚拟区分配一个vm_area_struct结构，接着对这个结构的各个域进行了初始化</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">4、将新建的虚拟区结构（vm_area_struct）插入进程的虚拟地址区域链表或树中</font>

**<font style="color:rgb(25, 27, 31);">②调用内核空间的系统调用函数mmap（不同于用户空间函数），实现文件物理地址和进程虚拟地址的一一映射关系</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">5、为映射分配了新的虚拟地址区域后，通过待映射的文件指针，在文件描述符表中找到对应的文件描述符，通过文件描述符，链接到内核“已打开文件集”中该文件的文件结构体（struct file），每个文件结构体维护着和这个已打开文件相关各项信息。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">6、通过该文件的文件结构体，链接到file_operations模块，调用内核函数mmap，其原型为：int mmap(struct file *filp, struct vm_area_struct *vma)，不同于用户空间库函数。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">7、内核mmap函数通过虚拟文件系统inode模块定位到文件磁盘物理地址。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">8、通过remap_pfn_range函数建立页表，即实现了文件地址和虚拟地址区域的映射关系。此时，这片虚拟地址并没有任何数据关联到主存中。</font>

**<font style="color:rgb(25, 27, 31);">③进程发起对这片映射空间的访问，引发缺页异常，实现文件内容到物理内存（主存）的拷贝</font>**

<font style="color:rgb(25, 27, 31);">注：前两个阶段仅在于创建虚拟区间并完成地址映射，但是并没有将任何文件数据的拷贝至主存。真正的文件读取是当进程发起读或写操作时。</font>

+ <font style="color:rgba(0, 0, 0, 0.9);">9、进程的读或写操作访问虚拟地址空间这一段映射地址，通过查询页表，发现这一段地址并不在物理页面上。因为目前只建立了地址映射，真正的硬盘数据还没有拷贝到内存中，因此引发缺页异常。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">10、缺页异常进行一系列判断，确定无非法操作后，内核发起请求调页过程。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">11、调页过程先在交换缓存空间（swap cache）中寻找需要访问的内存页，如果没有则调用nopage函数把所缺的页从磁盘装入到主存中。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">12、之后进程即可对这片主存进行读或者写的操作，如果写操作改变了其内容，一定时间后系统会自动回写脏页面到对应磁盘地址，也即完成了写入到文件的过程。</font>
+ **<font style="color:rgb(25, 27, 31);">注：修改过的脏页面并不会立即更新回文件中，而是有一段时间的延迟，可以调用msync()来强制同步, 这样所写的内容就能立即保存到文件里了。</font>**

## **<font style="color:white;background-color:rgb(37, 132, 181);">Part4</font>****<font style="color:rgb(37, 132, 181);">并发编程：开启程序高效运行的大门</font>**
### <font style="color:rgb(25, 27, 31);">4.1并发编程技术</font>
<font style="color:rgb(25, 27, 31);">随着计算机硬件技术的飞速发展，多核处理器已经成为主流，并发编程也因此变得愈发重要。并发编程，简单来说，就是让程序中的多个任务在同一时间段内执行，从而充分利用多核 CPU 的资源，显著提升程序的执行效率和响应速度。</font>

<font style="color:rgb(25, 27, 31);">在日常生活中，并发编程的应用场景随处可见。比如在 Web 服务器中，当多个用户同时访问网站时，服务器需要并发处理这些请求，快速响应每个用户，而不是让用户排队等待；在视频编辑软件里，一边加载视频素材，一边进行视频渲染，多个任务并发进行，节省用户的时间。</font>

<font style="color:rgb(25, 27, 31);">并发编程带来强大功能的同时，也伴随着诸多挑战。其中，数据竞争是一个常见问题，当多个线程同时访问和修改共享数据时，就可能出现数据不一致的情况。例如，两个线程同时读取一个变量的值，然后各自进行加 1 操作，最后再写回变量，由于线程执行顺序的不确定性，最终变量的值可能并不是预期的增加了 2 。死锁也是一个棘手的问题，当两个或多个线程相互等待对方释放资源时，就会陷入死锁状态，程序无法继续执行。比如线程 A 持有资源 1，等待资源 2，而线程 B 持有资源 2，等待资源 1，此时就会发生死锁。</font>

<font style="color:rgb(25, 27, 31);">为了解决这些问题，人们想出了许多有效的方法。对于数据竞争，可以使用锁机制，如互斥锁（Mutex），它就像一把钥匙，同一时间只有一个线程能拿到这把钥匙，进入临界区访问共享数据，从而保证数据的一致性。读写锁（Read - Write Lock）也是一种常用的锁，它允许多个线程同时进行读操作，但写操作时会独占资源，适用于读多写少的场景。还可以采用原子操作，像 Java 中的Atomic类，它提供了一系列原子操作方法，如AtomicInteger的incrementAndGet方法，能保证对整数的自增操作是原子的，不会受到其他线程的干扰。对于死锁问题，避免死锁的策略包括按照固定顺序获取锁，避免嵌套锁；设置锁的超时时间，当等待锁超时后，线程放弃等待，避免无限期等待；使用资源分配图算法检测死锁，一旦发现死锁，及时采取措施解除死锁。</font>

<font style="color:rgb(25, 27, 31);">在并发编程领域，有许多常用的编程模型和工具。线程池是一种广泛使用的工具，它预先创建好一定数量的线程，当有任务到来时，直接从线程池中获取线程执行任务，任务完成后，线程又回到线程池中等待下一个任务，这样可以避免频繁创建和销毁线程带来的开销，提高线程的复用性和执行效率。例如，在 Java 中，可以使用ThreadPoolExecutor类来创建和管理线程池，通过设置核心线程数、最大线程数、任务队列等参数，灵活控制线程池的行为。还有并发集合类，如 Java 中的ConcurrentHashMap，它是线程安全的哈希表，在多线程环境下能高效地进行数据存储和读取操作，比传统的HashMap更适合并发场景 。消息队列也是一种重要的并发编程模型，它通过异步消息传递的方式，解耦生产者和消费者，实现任务的异步处理和并发执行，常见的消息队列有 Kafka、RabbitMQ 等。</font>

### <font style="color:rgb(25, 27, 31);">4.2什么是无锁编程</font>
<font style="color:rgb(25, 27, 31);">无锁编程，即不使用锁的情况下实现多线程之间的变量同步，也就是在没有线程被阻塞的情况下实现变量的同步，所以也叫非阻塞同步（Non-blocking Synchronization），实现非阻塞同步的方案称为“无锁编程算法”。</font>

**<font style="color:rgb(25, 27, 31);">为什么要非阻塞同步，使用lock实现线程同步有非常多缺点：</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">产生竞争时，线程被阻塞等待，无法做到线程实时响应</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">dead lock</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">live lock</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">优先级反转</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">使用不当，造成性能下降</font>

<font style="color:rgb(25, 27, 31);">假设在不使用 lock 的情况下，实现变量同步，那就会避免非常多问题。尽管眼下来看，无锁编程并不能替代 lock。</font>

**<font style="color:rgb(25, 27, 31);">实现级别</font>**

<font style="color:rgb(25, 27, 31);">非同步阻塞的实现分为三个级别：wait-free/lock-free/obstruction-free</font>

**<font style="color:rgb(25, 27, 31);">(1)wait-free</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">最理想的模式，整个操作保证每一个线程在有限步骤下完毕</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">保证系统级吞吐（system-wide throughput）以及无线程饥饿</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">截止2011年，没有多少详细的实现。即使实现了，也须要依赖于详细CPU</font>

**<font style="color:rgb(25, 27, 31);">(2)lock-free</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">同意个别线程饥饿，但保证系统级吞吐。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">确保至少有一个线程可以继续运行。</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">wait-free的算法必然也是lock-free的。</font>

**<font style="color:rgb(25, 27, 31);">(3)obstruction-free</font>**

<font style="color:rgb(25, 27, 31);">在不论什么时间点，一个线程被隔离为一个事务进行运行（其它线程suspended），而且在有限步骤内完毕。在运行过程中，一旦发现数据被改动（採用时间戳、版本），则回滚，也叫做乐观锁，即乐观并发控制(OOC)。</font>

**<font style="color:rgb(25, 27, 31);">事务的过程是：</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">读取，并写时间戳</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">准备写入，版本号校验</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">检验通过则写入，检验不通过，则回滚</font>

<font style="color:rgb(25, 27, 31);">lock-free必然是obstruction-free的。</font>

### <font style="color:rgb(25, 27, 31);">4.3为什么要无锁?</font>
<font style="color:rgb(25, 27, 31);">首先是性能考虑。通信项目一般对性能有极致的追求，这是我们使用无锁的重要原因。当然，无锁算法如果实现的不好，性能可能还不如使用锁，所以我们选择比较擅长的数据结构和算法进行lock-free实现，比如Queue，对于比较复杂的数据结构和算法我们通过lock来控制，比如Map（虽然我们实现了无锁Hash，但是大小是限定的，而Map是大小不限定的），对于性能数据，后续文章会给出无锁和有锁的对比。</font>

**<font style="color:rgb(25, 27, 31);">次要是避免锁的使用引起的错误和问题：</font>**

+ <font style="color:rgba(0, 0, 0, 0.9);">死锁(dead lock)：两个以上线程互相等待</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">锁护送(lock convoy)：多个同优先级的线程反复竞争同一个锁，抢占锁失败后强制上下文切换，引起性能下降</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">优先级反转(priority inversion)：低优先级线程拥有锁时被中优先级的线程抢占，而高优先级的线程因为申请不到锁被阻塞。</font>

### <font style="color:rgb(25, 27, 31);">4.4如何无锁?</font>
<font style="color:rgb(25, 27, 31);">在现代的 CPU 处理器上，很多操作已经被设计为原子的，比如对齐读（Aligned Read）和对齐写（Aligned Write）等。Read-Modify-Write（RMW）操作的设计让执行更复杂的事务操作变成了原子操作，当有多个写入者想对相同的内存进行修改时，保证一次只执行一个操作。</font>

**<font style="color:rgb(25, 27, 31);">RMW 操作在不同的 CPU 家族中是通过不同的方式来支持的：</font>**

+ <font style="color:rgb(25, 27, 31);">x86/64 和 Itanium 架构通过 Compare-And-Swap (CAS) 方式来实现</font>
+ <font style="color:rgb(25, 27, 31);">PowerPC、MIPS 和 ARM 架构通过 Load-Link/Store-Conditional (LL/SC) 方式来实现</font>

<font style="color:rgb(25, 27, 31);">在x64下进行实践的，用的是CAS操作，CAS操作是lock-free技术的基础，我们可以用下面的代码来描述：</font>

```plain
template <class T>
bool CAS(T* addr, T expected, T value)
{
  if (*addr == expected)
  {
     *addr = value;
     return true;
  }
  return false;
}
```

<font style="color:rgb(25, 27, 31);">在GCC中，CAS操作如下所示：</font>

```plain
bool __sync_bool_compare_and_swap (type *ptr, type oldval type newval, ...)
type __sync_val_compare_and_swap (type *ptr, type oldval type newval, ...)
```

<font style="color:rgb(25, 27, 31);">这两个函数提供原子的比较和交换，如果*ptr == oldval，就将newval写入*ptr，第一个函数在相等并写入的情况下返回true，第二个函数的内置行为和第一个函数相同，只是它返回操作之前的值。</font>

<font style="color:rgb(25, 27, 31);">后面的可扩展参数(...)用来指出哪些变量需要memory barrier，因为目前gcc实现的是full barrier，所以可以略掉这个参数,除过CAS操作，GCC还提供了其他一些原子操作，可以在无锁算法中灵活使用：</font>

```plain
type __sync_fetch_and_add (type *ptr, type value, ...)
type __sync_fetch_and_sub (type *ptr, type value, ...)
type __sync_fetch_and_or (type *ptr, type value, ...)
type __sync_fetch_and_and (type *ptr, type value, ...)
type __sync_fetch_and_xor (type *ptr, type value, ...)
type __sync_fetch_and_nand (type *ptr, type value, ...)

type __sync_add_and_fetch (type *ptr, type value, ...)
type __sync_sub_and_fetch (type *ptr, type value, ...)
type __sync_or_and_fetch (type *ptr, type value, ...)
type __sync_and_and_fetch (type *ptr, type value, ...)
type __sync_xor_and_fetch (type *ptr, type value, ...)
type __sync_nand_and_fetch (type *ptr, type value, ...)
```

<font style="color:rgb(25, 27, 31);">_sync_*系列的built-in函数，用于提供加减和逻辑运算的原子操作。这两组函数的区别在于第一组返回更新前的值，第二组返回更新后的值。</font>

<font style="color:rgb(25, 27, 31);">无锁算法感触最深的是复杂度的分解，比如多线程对于一个双向链表的插入或删除操作，如何能一步一步分解成一个一个串联的原子操作，并能保证事务内存的一致性。</font>

## **<font style="color:white;background-color:rgb(37, 132, 181);">Part5</font>****<font style="color:rgb(37, 132, 181);">数据库：数据世界的坚固堡垒</font>**
<font style="color:rgb(25, 27, 31);">简单来说，数据库就是按照一定结构组织并长期存储在计算机内、可共享的数据集合 ，它就像是一个巨大的仓库，有条不紊地存放着各种数据。</font>

<font style="color:rgb(25, 27, 31);">数据库管理系统（DBMS）则是操作和管理数据库的软件，是数据库的核心枢纽，像常见的 MySQL、Oracle、SQL Server 等都是广泛使用的数据库管理系统，它们为用户和程序员提供了创建、检索、更新和管理数据的便捷方式，还具备数据的安全性保障、一致性维护、并发控制以及恢复机制等重要功能。</font>

<font style="color:rgb(25, 27, 31);">数据库的类型丰富多样，其中关系型数据库基于关系模型设计，以表格形式存储数据，通过行和列来组织数据，每个表格代表一种实体类型，每一行是一个实例，每一列对应实例的一个属性，并且各表之间通过键建立关联关系，它支持 SQL 语言进行强大的数据查询和操作，能保证数据的一致性和完整性，适用于对数据一致性要求高、事务处理复杂的场景，如银行系统、电商订单管理系统等 。常见的关系型数据库有 MySQL、PostgreSQL、Oracle、Microsoft SQL Server 等。</font>

<font style="color:rgb(25, 27, 31);">随着大数据时代的来临，数据量呈爆炸式增长，数据类型也愈发复杂多样，NoSQL 数据库应运而生。它打破了传统关系型数据库的固定表结构束缚，更适合处理大规模分布式环境下的非结构化数据，具有灵活的数据模型、高可扩展性和高性能等特点 。像 MongoDB 属于文档型 NoSQL 数据库，它以文档（类似 JSON 格式）的形式存储数据，每个文档可以有不同的结构，非常适合存储和处理内容管理系统、日志记录系统等场景下的数据；Redis 是键值型 NoSQL 数据库，它将数据存储为键值对，读写速度极快，常被用作缓存、消息队列等，在高并发读写的场景中表现出色。</font>

<font style="color:rgb(25, 27, 31);">在数据库设计阶段，需求分析至关重要，需要深入了解业务需求，明确数据的存储、查询和处理要求。概念设计时，通常使用实体 - 关系（ER）模型来描述数据之间的关系，将现实世界中的实体、属性以及它们之间的联系抽象为 ER 图，这就像是搭建房子前的设计蓝图，为后续的设计奠定基础 。逻辑设计则是将 ER 模型转换为具体的数据库模型，如关系模型，确定数据库的表结构、字段类型、主键、外键等。物理设计阶段要考虑数据库在物理存储介质上的存储结构和访问方法，选择合适的存储设备、索引策略等，以提高数据库的性能。</font>

<font style="color:rgb(25, 27, 31);">优化数据库性能的方法有很多，比如合理设计索引，索引能加速数据检索，就像书的目录，帮助快速定位到所需数据，但过多的索引会增加存储开销和数据更新的时间，因此要根据实际查询需求创建合适的索引；优化 SQL 语句也十分关键，编写高效的 SQL 查询，避免全表扫描、减少子查询的使用、合理使用 JOIN 操作等，可以显著提升查询效率；数据库配置优化同样不可忽视，根据服务器硬件资源和业务负载，调整数据库的参数配置，如缓冲池大小、连接数、日志设置等，能让数据库更好地适应实际运行环境 。</font>

<font style="color:rgb(25, 27, 31);">数据库在我们的生活中无处不在，电商平台利用数据库存储商品信息、用户订单，方便用户购物和商家管理；社交网络依靠数据库保存用户资料、动态和好友关系，让人们能够便捷地交流互动；企业的财务管理系统通过数据库存储财务数据，支持财务分析和决策制定。可以说，数据库已经成为现代信息系统的基石，支撑着各个领域的信息化运作，其重要性不言而喻。</font>

<font style="color:rgb(25, 27, 31);"></font>

> <font style="color:rgb(25, 27, 31);">来自: </font>[C++秋招高频面试题汇总：C++多态、malloc函数、内存管理、并发编程、数据库](https://mp.weixin.qq.com/s/jp0T4bViM5dYBhYOu3qu4Q)
>





