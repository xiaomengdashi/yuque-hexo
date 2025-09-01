---
title: 【秋招面经】字节--抖音C++开发面经，超详细解读，建议收藏~
date: '2025-08-31 02:05:24'
updated: '2025-08-31 02:05:24'
---
<font style="color:rgb(89, 89, 89);">大家好，我是Q。</font>

<font style="color:rgb(89, 89, 89);">前几天为大家总结了C++中的一些模块的知识点，包括：</font>[<font style="color:rgb(89, 89, 89);">智能指针</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492279&idx=1&sn=34a78a965e3b90d468edd66a404f3dc8&scene=21#wechat_redirect)<font style="color:rgb(89, 89, 89);">、</font>[<font style="color:rgb(89, 89, 89);">设计模式</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492311&idx=1&sn=daff1e60799cfeaef3add27087f17702&scene=21#wechat_redirect)<font style="color:rgb(89, 89, 89);">、</font>[<font style="color:rgb(89, 89, 89);">多线程编程</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492334&idx=1&sn=de1ca3caacdb650401277d05cc2b2343&scene=21#wechat_redirect)<font style="color:rgb(89, 89, 89);">、</font>[<font style="color:rgb(89, 89, 89);">STL剖析</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492370&idx=1&sn=c5963be03c21cdb3ccd94ebdb860c011&scene=21#wechat_redirect)<font style="color:rgb(89, 89, 89);">等，反馈相当不错！！</font>

<font style="color:rgb(89, 89, 89);">就在昨天，由我亲自打造的一个可以写在简历上的项目--《</font>[<font style="color:rgb(89, 89, 89);">高性能C++内存池</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492389&idx=1&sn=451aaf6cd7ac5c89250c654f17bdc046&scene=21#wechat_redirect)<font style="color:rgb(89, 89, 89);">》正式上线！！！</font>

<font style="color:rgb(89, 89, 89);">已有不少的反馈和报名，项目</font>**<font style="color:rgb(89, 89, 89);">核心代码5600+</font>**<font style="color:rgb(89, 89, 89);">行，测试代码2500+行，非常适合在校和即将毕业的小伙伴们！</font>

<font style="color:rgb(0, 128, 255);">如果你的简历缺少一个像样的项目经历，那就抓紧时间联系我，仅用七天，让你从0到1完全实现，全程陪伴！！</font>

<font style="color:rgb(255, 41, 65);">文末有提取方式~</font>

<font style="color:rgb(89, 89, 89);">今天给大家分享的是一位小伙伴总结的抖音的C++开发面经，一起来看看吧！</font>

> <font style="color:rgb(89, 89, 89);background-color:rgb(246, 238, 255);">来源：https://www.nowcoder.com/feed/main/detail/0f1cf5724c584895ab38beeec379a838</font>
>

## **<font style="color:rgb(89, 89, 89);">1、#include 头文件重复包含问题？</font>**
<font style="color:rgb(89, 89, 89);">在C++编程中，头文件（.h 或 .hpp 文件）是用来声明类、函数、变量等的地方，它们通常会被多个源文件（.cpp 文件）包含。然而，如果同一个头文件在同一个源文件中被多次包含，或者通过间接包含导致多次包含，就会引发“头文件重复包含”的问题。这可能导致以下几种情况：</font>

1. <font style="color:#595959;">编译错误：如果头文件中定义了变量或函数（而不是声明），多次包含会导致重复定义错误。</font>
2. <font style="color:#595959;">编译警告：即使没有直接的编译错误，重复包含也会增加编译时间，因为编译器需要处理相同的内容多次。</font>
3. <font style="color:#595959;">逻辑错误：在某些复杂情况下，重复包含可能导致宏定义冲突或类型重定义，从而引发难以调试的逻辑错误。</font>

<font style="color:#595959;">为了解决头文件重复包含问题，C++提供了两种主要的机制：头文件保护符和</font>`<font style="color:#595959;">#pragma once</font>`<font style="color:#595959;">。</font>

##### <font style="color:black;">头文件保护符</font>
<font style="color:#595959;">头文件保护符是C/C++语言中一种传统的、跨平台的解决方案，它利用预处理器宏来确保头文件内容只被编译一次。其基本形式如下：</font>

```plain
#ifndef MY_HEADER_H
#define MY_HEADER_H

// 头文件内容
// 类声明、函数声明、宏定义等

#endif // MY_HEADER_H
```

<font style="color:#595959;">工作原理：</font>

+ <font style="color:#595959;">当预处理器第一次遇到 </font>`<font style="color:#595959;">#ifndef MY_HEADER_H</font>`<font style="color:#595959;"> 时，如果宏 </font>`<font style="color:#595959;">MY_HEADER_H</font>`<font style="color:#595959;"> 尚未定义，则条件为真。预处理器会继续处理 </font>`<font style="color:#595959;">#define MY_HEADER_H</font>`<font style="color:#595959;">，定义该宏，然后处理头文件中的所有内容，直到 </font>`<font style="color:#595959;">#endif</font>`<font style="color:#595959;">。</font>
+ <font style="color:#595959;">如果同一个头文件再次被包含，预处理器再次遇到 </font>`<font style="color:#595959;">#ifndef MY_HEADER_H</font>`<font style="color:#595959;"> 时，由于 </font>`<font style="color:#595959;">MY_HEADER_H</font>`<font style="color:#595959;"> 已经被定义，条件为假。预处理器会跳过 </font>`<font style="color:#595959;">#ifndef</font>`<font style="color:#595959;"> 和 </font>`<font style="color:#595959;">#endif</font>`<font style="color:#595959;"> 之间的所有内容，从而避免了重复编译。</font>

<font style="color:#595959;">命名约定：</font>

<font style="color:#595959;">通常，宏的名称会根据头文件的路径和名称来命名，例如，对于 </font>`<font style="color:#595959;">my_project/include/utils/my_header.h</font>`<font style="color:#595959;">，宏名可以是 </font>`<font style="color:#595959;">MY_PROJECT_INCLUDE_UTILS_MY_HEADER_H</font>`<font style="color:#595959;">。这样做可以有效避免宏名冲突。</font>

##### `<font style="color:black;">#pragma once</font>`
`<font style="color:#595959;">#pragma once</font>`<font style="color:#595959;"> 是一个非标准的、编译器特定的预处理指令，但它被绝大多数现代C++编译器（如GCC、Clang、MSVC）所支持。它的作用与头文件保护符相同，都是为了防止头文件被重复包含。</font>

```plain
#pragma once

// 头文件内容
// 类声明、函数声明、宏定义等
```

<font style="color:#595959;">工作原理：</font>

+ <font style="color:#595959;">当编译器第一次遇到 </font>`<font style="color:#595959;">#pragma once</font>`<font style="color:#595959;"> 指令时，它会记录下当前头文件的路径。如果后续再次尝试包含同一个路径的头文件，编译器会直接跳过其内容，不再进行处理。</font>

<font style="color:#595959;">与头文件保护符的比较：</font>

| **<font style="color:#595959;">特性</font>** | **<font style="color:#595959;">头文件保护符（</font>**`**<font style="color:#595959;">#ifndef/#define/#endif</font>**`**<font style="color:#595959;">）</font>** | `**<font style="color:#595959;">#pragma once</font>**` |
| :--- | :--- | :--- |
| <font style="color:#595959;">标准性</font> | <font style="color:#595959;">C/C++标准的一部分</font> | <font style="color:#595959;">非标准，但被广泛支持</font> |
| <font style="color:#595959;">兼容性</font> | <font style="color:#595959;">跨平台，所有编译器都支持</font> | <font style="color:#595959;">某些老旧或不常见的编译器可能不支持</font> |
| <font style="color:#595959;">性能</font> | <font style="color:#595959;">每次包含都需要进行宏检查</font> | <font style="color:#595959;">编译器层面优化，通常更快，尤其是在大型项目中</font> |
| <font style="color:#595959;">安全性</font> | <font style="color:#595959;">依赖宏名唯一性，可能存在宏名冲突风险</font> | <font style="color:#595959;">依赖文件路径唯一性，更安全，不易出错</font> |
| <font style="color:#595959;">易用性</font> | <font style="color:#595959;">需要手动编写三行代码</font> | <font style="color:#595959;">只需要一行代码，更简洁</font> |


## **<font style="color:#595959;">2、详细解释栈帧？</font>**
<font style="color:#595959;">栈帧是调用栈上的一个逻辑单元，它为每个正在执行的函数调用（或过程调用）提供独立的存储空间。每当一个函数被调用时，一个新的栈帧就会被压入调用栈；当函数执行完毕返回时，对应的栈帧就会被弹出。</font>

##### <font style="color:black;">栈帧的组成部分</font>
<font style="color:#595959;">一个典型的栈帧通常包含以下几个主要部分（具体组成和顺序可能因编译器、操作系统和CPU架构而异，但核心思想是相同的）：</font>

1. <font style="color:#595959;">函数参数：被调用函数接收的参数。这些参数通常在函数调用前被压入栈中，或者通过寄存器传递。</font>
2. <font style="color:#595959;">返回地址：调用函数执行完毕后，程序应该返回到调用函数中的哪条指令继续执行。这个地址在函数调用时被压入栈中。</font>
3. <font style="color:#595959;">旧的栈基址/帧指针：在进入新函数时，当前函数的栈帧会保存调用者的栈基址。这个指针用于在函数返回时恢复调用者的栈帧，并提供一个固定的参考点来访问栈帧中的数据。</font>
4. <font style="color:#595959;">局部变量：被调用函数内部定义的非静态局部变量。这些变量在函数执行期间分配在栈帧中，函数返回时自动销毁。</font>
5. <font style="color:#595959;">临时变量：编译器为了表达式计算或优化而生成的临时变量。</font>
6. <font style="color:#595959;">寄存器保存区：为了避免被调用函数修改调用者使用的寄存器，被调用函数会将一些重要的寄存器（如通用寄存器、浮点寄存器等）的值保存到栈帧中，并在返回前恢复它们。</font>

##### <font style="color:black;">栈指针和帧指针</font>
+ <font style="color:#595959;">栈指针：指向当前栈的顶部（栈顶地址）。栈是向下增长的（从高地址向低地址增长），所以栈指针总是指向栈中最后一个被压入的元素的地址。当数据被压入栈时，栈指针减小；当数据从栈中弹出时，栈指针增大。</font>
+ <font style="color:#595959;">帧指针：指向当前栈帧的底部（或某个固定位置，如旧的栈基址）。帧指针在函数执行期间保持不变，它提供了一个稳定的基准，使得函数可以通过相对于帧指针的偏移量来访问其参数和局部变量，而无需关心栈指针的动态变化。</font>

##### <font style="color:black;">函数调用过程中栈帧的变化（以x86/x64为例）</font>
<font style="color:#595959;">一个简单的函数调用，说明栈帧的创建和销毁过程：</font>

```plain
void func_b(int y) {
    int z = y + 1;
    // ...
}

void func_a(int x) {
    int a = x * 2;
    func_b(a);
    // ...
}

int main() {
    func_a(10);
    return0;
}
```

1. `<font style="color:#595959;">main</font>`<font style="color:#595959;"> 函数的栈帧：</font>
    - `<font style="color:#595959;">main</font>`<font style="color:#595959;"> 函数开始执行，其栈帧被创建。</font>
    - `<font style="color:#595959;">main</font>`<font style="color:#595959;"> 函数调用 </font>`<font style="color:#595959;">func_a(10)</font>`<font style="color:#595959;">。</font>
2. <font style="color:#595959;">调用 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> ：</font>
    - <font style="color:#595959;">参数 </font>`<font style="color:#595959;">10</font>`<font style="color:#595959;"> 被压入栈中（或通过寄存器传递）。</font>
    - `<font style="color:#595959;">main</font>`<font style="color:#595959;"> 函数的返回地址（即 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 调用后的下一条指令地址）被压入栈中。</font>
    - <font style="color:#595959;">控制权转移到 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 的入口点。</font>
3. `<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 函数的序言：</font>
    - `<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 将 </font>`<font style="color:#595959;">main</font>`<font style="color:#595959;"> 函数的旧帧指针（</font>`<font style="color:#595959;">EBP_main</font>`<font style="color:#595959;">）压入栈中。</font>
    - `<font style="color:#595959;">EBP</font>`<font style="color:#595959;"> 寄存器更新为当前的 </font>`<font style="color:#595959;">ESP</font>`<font style="color:#595959;"> 值，作为 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 的新帧指针（</font>`<font style="color:#595959;">EBP_func_a</font>`<font style="color:#595959;">）。</font>
    - `<font style="color:#595959;">ESP</font>`<font style="color:#595959;"> 寄存器减小，为 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 的局部变量（如 </font>`<font style="color:#595959;">a</font>`<font style="color:#595959;">）分配空间。</font>
    - <font style="color:#595959;">此时，</font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 的栈帧已经建立。</font>
4. `<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 调用 </font>`<font style="color:#595959;">func_b</font>`<font style="color:#595959;">：</font>
    - <font style="color:#595959;">参数 </font>`<font style="color:#595959;">a</font>`<font style="color:#595959;"> 的值被压入栈中（或通过寄存器传递）。</font>
    - `<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 的返回地址（即 </font>`<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 调用后的下一条指令地址）被压入栈中。</font>
    - <font style="color:#595959;">控制权转移到 </font>`<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 的入口点。</font>
5. `<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 函数的序言：</font>
    - `<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 将 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 函数的旧帧指针（</font>`<font style="color:#595959;">EBP_func_a</font>`<font style="color:#595959;">）压入栈中。</font>
    - `<font style="color:#595959;">EBP</font>`<font style="color:#595959;"> 寄存器更新为当前的 </font>`<font style="color:#595959;">ESP</font>`<font style="color:#595959;"> 值，作为 </font>`<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 的新帧指针（</font>`<font style="color:#595959;">EBP_func_b</font>`<font style="color:#595959;">）。</font>
    - `<font style="color:#595959;">ESP</font>`<font style="color:#595959;"> 寄存器减小，为 </font>`<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 的局部变量（如 </font>`<font style="color:#595959;">z</font>`<font style="color:#595959;">）分配空间。</font>
    - <font style="color:#595959;">此时，</font>`<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 的栈帧已经建立。</font>
6. `<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 函数的执行和返回：</font>
    - `<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 执行完毕。</font>
    - `<font style="color:#595959;">ESP</font>`<font style="color:#595959;"> 寄存器恢复到 </font>`<font style="color:#595959;">EBP_func_b</font>`<font style="color:#595959;"> 的值，释放局部变量空间。</font>
    - <font style="color:#595959;">从栈中弹出 </font>`<font style="color:#595959;">EBP_func_a</font>`<font style="color:#595959;">，恢复 </font>`<font style="color:#595959;">EBP</font>`<font style="color:#595959;"> 寄存器。</font>
    - <font style="color:#595959;">从栈中弹出 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 的返回地址，并跳转到该地址，控制权返回给 </font>`<font style="color:#595959;">func_a</font>`<font style="color:#595959;">。</font>
    - `<font style="color:#595959;">func_b</font>`<font style="color:#595959;"> 的栈帧被销毁。</font>
7. `<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 函数的执行和返回：</font>
    - `<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 执行完毕。</font>
    - `<font style="color:#595959;">ESP</font>`<font style="color:#595959;"> 寄存器恢复到 </font>`<font style="color:#595959;">EBP_func_a</font>`<font style="color:#595959;"> 的值，释放局部变量空间。</font>
    - <font style="color:#595959;">从栈中弹出 </font>`<font style="color:#595959;">EBP_main</font>`<font style="color:#595959;">，恢复 </font>`<font style="color:#595959;">EBP</font>`<font style="color:#595959;"> 寄存器。</font>
    - <font style="color:#595959;">从栈中弹出 </font>`<font style="color:#595959;">main</font>`<font style="color:#595959;"> 的返回地址，并跳转到该地址，控制权返回给 </font>`<font style="color:#595959;">main</font>`<font style="color:#595959;">。</font>
    - `<font style="color:#595959;">func_a</font>`<font style="color:#595959;"> 的栈帧被销毁。</font>

## **<font style="color:#595959;">3、用户态到内核态穿越步骤，越详细越好？</font>**
<font style="color:#595959;">操作系统为了保护系统资源和提高系统稳定性，将CPU的执行权限划分为不同的级别，通常分为用户态和内核态。用户程序运行在用户态，权限受限，不能直接访问硬件资源或执行特权指令；操作系统内核运行在内核态，拥有最高权限，可以访问所有硬件资源和执行任何指令。</font>

<font style="color:#595959;">当用户程序需要执行一些特权操作（如文件I/O、网络通信、内存分配等）时，它必须通过系统调用的方式从用户态切换到内核态，由操作系统内核代为完成。这个从用户态到内核态的切换过程，通常被称为“穿越”或“陷入”。</font>

<font style="color:#595959;">用户态到内核态穿越的详细步骤：</font>

1. <font style="color:#595959;">用户程序发起系统调用：</font>
    - <font style="color:#595959;">用户程序通过调用库函数（如C标准库中的 </font>`<font style="color:#595959;">printf</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">read</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">write</font>`<font style="color:#595959;"> 等）来间接发起系统调用。这些库函数通常是系统调用的封装，它们会准备好系统调用所需的参数。</font>
    - <font style="color:#595959;">库函数会将系统调用号（标识具体是哪个系统调用）以及其他参数放入特定的寄存器中，或者压入栈中，以便内核能够识别和获取这些信息。</font>
    - <font style="color:#595959;">最后，库函数会执行一条特殊的指令，如 </font>`<font style="color:#595959;">int 0x80</font>`<font style="color:#595959;"> (x86架构下的软中断指令) 或 </font>`<font style="color:#595959;">syscall</font>`<font style="color:#595959;"> (x64架构下的系统调用指令)。这条指令会触发一个软中断或陷阱。</font>
2. <font style="color:#595959;">CPU响应中断/陷阱：</font>
    - <font style="color:#595959;">特权级切换：CPU的当前特权级从用户态（通常是Ring 3）切换到内核态（通常是Ring 0）。</font>
    - <font style="color:#595959;">栈切换：CPU会切换到内核栈。用户程序的栈指针（ESP/RSP）和栈段寄存器（SS）会被保存，并加载内核栈的指针和段寄存器。这是为了防止用户程序恶意修改内核栈，也为了在内核态执行时有独立的栈空间。</font>
    - <font style="color:#595959;">上下文保存（部分）：CPU会将一些关键的寄存器（如指令指针 EIP/RIP、标志寄存器 EFLAGS/RFLAGS、用户态栈指针 ESP/RSP、用户态栈段寄存器 SS 等）自动压入到新的内核栈中。这些信息构成了用户态程序的执行上下文，以便在系统调用完成后能够恢复。</font>
    - <font style="color:#595959;">当CPU检测到 </font>`<font style="color:#595959;">int 0x80</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">syscall</font>`<font style="color:#595959;"> 指令时，它会立即停止当前用户程序的执行。</font>
    - <font style="color:#595959;">CPU会根据中断向量表或系统调用表查找与该中断号或系统调用号对应的中断服务例程或系统调用处理函数的入口地址。</font>
    - <font style="color:#595959;">在跳转到中断服务例程之前，CPU会自动完成一系列硬件级别的操作，以实现特权级切换和上下文保存：</font>
3. <font style="color:#595959;">进入内核态，执行系统调用处理函数：</font>
    - <font style="color:#595959;">CPU跳转到中断向量表或系统调用表指向的内核态地址，开始执行对应的系统调用处理函数。</font>
    - <font style="color:#595959;">此时，程序已经在内核态运行，拥有了最高权限。</font>
    - <font style="color:#595959;">系统调用处理函数会从之前保存的寄存器或内核栈中获取用户程序传递过来的系统调用号和参数。</font>
    - <font style="color:#595959;">内核会根据系统调用号执行相应的操作，例如，如果是 </font>`<font style="color:#595959;">read</font>`<font style="color:#595959;"> 系统调用，内核会访问文件系统，从磁盘读取数据到用户指定的缓冲区。</font>
4. <font style="color:#595959;">内核处理系统调用：</font>
    - <font style="color:#595959;">内核执行具体的系统调用逻辑。这可能涉及到访问硬件、管理内存、调度进程、执行特权指令等。</font>
    - <font style="color:#595959;">在处理过程中，内核可能会因为等待I/O完成或其他原因而将当前进程置于等待状态，并进行进程调度，切换到其他进程执行。</font>
    - <font style="color:#595959;">系统调用处理完成后，内核会将结果（如成功/失败代码、读取的数据量等）存储在特定的寄存器中，以便用户程序获取。</font>
5. <font style="color:#595959;">内核态返回用户态：</font>
    - <font style="color:#595959;">上下文恢复：CPU从内核栈中弹出之前保存的用户态寄存器值（EIP/RIP、EFLAGS/RFLAGS、ESP/RSP、SS 等），恢复用户程序的执行上下文。</font>
    - <font style="color:#595959;">栈切换：CPU切换回用户栈。</font>
    - <font style="color:#595959;">特权级切换：CPU的当前特权级从内核态（Ring 0）切换回用户态（Ring 3）。</font>
    - <font style="color:#595959;">系统调用处理函数执行完毕后，会执行一条特殊的返回指令，如 </font>`<font style="color:#595959;">iret</font>`<font style="color:#595959;"> (x86中断返回指令) 或 </font>`<font style="color:#595959;">sysret</font>`<font style="color:#595959;"> (x64系统调用返回指令)。</font>
    - <font style="color:#595959;">CPU检测到这条返回指令后，会自动完成一系列硬件级别的操作，以实现特权级恢复和上下文恢复：</font>
6. <font style="color:#595959;">用户程序继续执行：</font>
    - <font style="color:#595959;">CPU跳转到之前保存的用户态返回地址，用户程序从系统调用点之后的下一条指令继续执行。</font>
    - <font style="color:#595959;">用户程序可以从寄存器中获取系统调用的返回结果。</font>

## **<font style="color:#595959;">4、进程地址空间？</font>**
<font style="color:#595959;">进程地址空间是操作系统为每个进程提供的一个抽象概念，它是一个独立的、连续的、私有的虚拟内存区域。</font>

<font style="color:#595959;">对于32位系统，这个虚拟地址空间通常是4GB（0x00000000到0xFFFFFFFF），对于64位系统，这个空间则大得多（例如，Windows上通常是128TB，Linux上是128TB或256TB）。</font>

<font style="color:#595959;">核心思想：</font>

+ <font style="color:#595959;">虚拟化：进程看到的地址是虚拟地址，而不是物理地址。操作系统通过内存管理单元（MMU）将虚拟地址映射到物理内存地址。这使得每个进程都认为自己拥有独立的、完整的内存空间，即使物理内存远小于虚拟地址空间的总和。</font>
+ <font style="color:#595959;">隔离性：每个进程都有自己的独立地址空间，一个进程无法直接访问另一个进程的内存，从而实现了进程间的隔离和保护。这大大增强了系统的稳定性和安全性，防止一个进程的错误影响到其他进程或操作系统本身。</font>
+ <font style="color:#595959;">连续性：尽管物理内存可能是不连续的，但通过虚拟内存机制，进程看到的虚拟地址空间是连续的，这简化了程序的编写和内存管理。</font>

##### <font style="color:black;">进程地址空间的布局</font>
<font style="color:#595959;">一个典型的进程地址空间通常被划分为多个逻辑区域，每个区域有不同的用途和访问权限。以下是常见的布局（从低地址到高地址）：</font>

1. <font style="color:#595959;">代码段：</font>
    - <font style="color:#595959;">内容：存放可执行程序的机器指令（代码）。</font>
    - <font style="color:#595959;">特性：通常是只读的（Read-Only），以防止程序意外修改自身代码，并允许多个进程共享同一份代码（例如，多个进程运行同一个程序时，它们可以共享同一份代码段的物理内存）。</font>
2. <font style="color:#595959;">数据段：</font>
    - <font style="color:#595959;">内容：存放已初始化的全局变量和静态变量。</font>
    - <font style="color:#595959;">特性：可读写（Read/Write）。这部分内存在程序启动时由加载器从可执行文件中加载。</font>
3. <font style="color:#595959;">BSS段：</font>
    - <font style="color:#595959;">内容：存放未初始化的全局变量和静态变量。</font>
    - <font style="color:#595959;">特性：可读写。与数据段不同，BSS段在程序启动时不会从可执行文件中加载，而是由操作系统在程序加载时将其内容清零（通常是初始化为0）。这样做可以减小可执行文件的大小。</font>
4. <font style="color:#595959;">堆（Heap）：</font>
    - <font style="color:#595959;">内容：用于动态内存分配，例如C++中的 </font>`<font style="color:#595959;">new</font>`<font style="color:#595959;">/</font>`<font style="color:#595959;">delete</font>`<font style="color:#595959;"> 或C语言中的 </font>`<font style="color:#595959;">malloc</font>`<font style="color:#595959;">/</font>`<font style="color:#595959;">free</font>`<font style="color:#595959;">。程序运行时，可以根据需要从堆中申请和释放内存。</font>
    - <font style="color:#595959;">特性：可读写。堆从低地址向高地址增长。堆的管理通常由运行时库负责，而不是直接由操作系统管理。当堆空间不足时，运行时库会向操作系统请求更多的内存（例如通过 </font>`<font style="color:#595959;">brk</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">mmap</font>`<font style="color:#595959;"> 系统调用）。</font>
5. <font style="color:#595959;">文件映射区域：</font>
    - <font style="color:#595959;">内容：用于内存映射文件，或者通过 </font>`<font style="color:#595959;">mmap</font>`<font style="color:#595959;"> 系统调用分配的匿名内存。动态链接库（DLL/SO）通常也会被映射到这个区域。</font>
    - <font style="color:#595959;">特性：可读写，可执行（对于动态链接库）。这个区域通常位于堆和栈之间，从高地址向低地址增长（或在某些系统上从低地址向高地址增长，具体取决于实现）。</font>
6. <font style="color:#595959;">栈（Stack）：</font>
    - <font style="color:#595959;">内容：用于存放局部变量、函数参数、返回地址以及函数调用上下文。每当函数被调用时，一个新的栈帧就会被压入栈中；函数返回时，栈帧被弹出。</font>
    - <font style="color:#595959;">特性：可读写。栈从高地址向低地址增长。栈的大小通常是固定的（或有最大限制），但可以动态调整。</font>
7. <font style="color:#595959;">内核空间（Kernel Space）：</font>
    - <font style="color:#595959;">内容：这部分虚拟地址空间是为操作系统内核保留的。用户程序无法直接访问这部分空间。</font>
    - <font style="color:#595959;">特性：在32位系统中，通常是虚拟地址空间的最高1GB（0xC0000000到0xFFFFFFFF）；在64位系统中，通常是虚拟地址空间的高半部分。这部分空间在所有进程中都是相同的，但映射到不同的物理内存页，或者映射到相同的物理内存页（例如，内核代码和数据）。</font>

##### <font style="color:black;">内存保护机制</font>
<font style="color:#595959;">操作系统通过硬件（MMU）和软件（页表）结合的方式实现内存保护：</font>

+ <font style="color:#595959;">页表（Page Table）：每个进程都有自己的页表，记录了虚拟地址到物理地址的映射关系，以及每个页的访问权限（读、写、执行）。</font>
+ <font style="color:#595959;">MMU（Memory Management Unit）：CPU中的硬件单元，负责在每次内存访问时进行虚拟地址到物理地址的转换，并检查访问权限。如果访问违反了权限（例如，尝试写入只读的代码段），MMU会触发一个页错误（Page Fault），操作系统会介入处理，通常会终止违规进程。</font>

##### <font style="color:black;">地址转换过程</font>
<font style="color:#595959;">当CPU生成一个虚拟地址时：</font>

1. <font style="color:#595959;">MMU会根据进程的页表基址寄存器（如CR3寄存器）找到当前进程的页表。</font>
2. <font style="color:#595959;">MMU将虚拟地址分解为页目录索引、页表索引和页内偏移量。</font>
3. <font style="color:#595959;">MMU通过这些索引在页目录和页表中查找对应的页表项（PTE）。</font>
4. <font style="color:#595959;">页表项中包含了物理页框号（Physical Page Frame Number）和访问权限位。</font>
5. <font style="color:#595959;">MMU将物理页框号与页内偏移量组合，得到最终的物理地址。</font>
6. <font style="color:#595959;">同时，MMU会检查访问权限位，如果当前操作（读/写/执行）与页表项中定义的权限不符，则触发页错误。</font>

## **<font style="color:#595959;">5、HTTPS协议？</font>**
<font style="color:#595959;">HTTPS是一种通过计算机网络进行安全通信的传输协议。它在HTTP协议的基础上，通过TLS或其前身SSL协议来提供加密通信、身份认证和数据完整性保护。</font>

<font style="color:#595959;">简单来说，HTTPS = HTTP + TLS/SSL。</font>

##### <font style="color:black;">为什么需要HTTPS？</font>
<font style="color:#595959;">传统的HTTP协议是明文传输的，存在以下安全风险：</font>

+ <font style="color:#595959;">数据窃听：攻击者可以截获传输中的数据，获取敏感信息（如用户名、密码、银行卡号等）。</font>
+ <font style="color:#595959;">数据篡改：攻击者可以修改传输中的数据，例如在网页中插入恶意代码或修改交易金额。</font>
+ <font style="color:#595959;">身份冒充：攻击者可以冒充服务器或客户端，进行欺诈活动。</font>

<font style="color:#595959;">HTTPS通过引入加密、认证和完整性保护机制，有效解决了这些问题。</font>

##### <font style="color:black;">HTTPS的核心组成部分</font>
1. <font style="color:#595959;">HTTP协议：负责定义客户端和服务器之间如何交换Web内容（请求方法、状态码、头部信息等）。</font>
2. <font style="color:#595959;">TLS/SSL协议：位于HTTP层之下，TCP层之上。它负责在客户端和服务器之间建立一个安全的通信通道。</font>

##### <font style="color:black;">TLS/SSL握手过程（以TLS 1.2为例）</font>
<font style="color:#595959;">TLS握手是HTTPS通信中最关键的步骤，它在实际数据传输之前完成，用于协商加密算法、交换密钥以及进行身份认证。以下是简化的握手过程：</font>

1. <font style="color:#595959;">客户端发起连接：</font>
    - <font style="color:#595959;">支持的TLS协议版本（如TLS 1.2, TLS 1.3）。</font>
    - <font style="color:#595959;">客户端生成的随机数 </font>`<font style="color:#595959;">Client Random</font>`<font style="color:#595959;">，用于后续生成会话密钥。</font>
    - <font style="color:#595959;">客户端支持的加密套件列表，包括对称加密算法、非对称加密算法、哈希算法等。</font>
    - <font style="color:#595959;">支持的压缩方法。</font>
    - <font style="color:#595959;">扩展信息（如SNI，Server Name Indication，用于指示客户端要访问的域名）。</font>
    - <font style="color:#595959;">客户端向服务器发送 </font>`<font style="color:#595959;">Client Hello</font>`<font style="color:#595959;"> 消息，包含以下信息：</font>
2. <font style="color:#595959;">服务器回应：</font>
    - <font style="color:#595959;">选定的TLS协议版本。</font>
    - <font style="color:#595959;">服务器生成的随机数 </font>`<font style="color:#595959;">Server Random</font>`<font style="color:#595959;">，用于后续生成会话密钥。</font>
    - <font style="color:#595959;">选定的加密套件。</font>
    - <font style="color:#595959;">选定的压缩方法。</font>
    - <font style="color:#595959;">服务器收到 </font>`<font style="color:#595959;">Client Hello</font>`<font style="color:#595959;"> 后，从客户端提供的列表中选择一个它也支持的TLS协议版本和加密套件。</font>
    - <font style="color:#595959;">服务器发送 </font>`<font style="color:#595959;">Server Hello</font>`<font style="color:#595959;"> 消息，包含：</font>
3. <font style="color:#595959;">服务器发送证书：</font>

<font style="color:#595959;">服务器发送其数字证书给客户端。证书中包含了服务器的公钥、服务器的身份信息（域名、组织等）以及证书颁发机构（CA）的数字签名。</font>

4. <font style="color:#595959;">服务器发送密钥交换参数：</font>

<font style="color:#595959;">如果选定的加密套件需要额外的密钥交换参数（例如，使用Diffie-Hellman密钥交换算法），服务器会发送这些参数。</font>

5. <font style="color:#595959;">服务器发送握手完成信号：</font>

<font style="color:#595959;">服务器发送 </font>`<font style="color:#595959;">Server Hello Done</font>`<font style="color:#595959;"> 消息，表示服务器端的握手消息已发送完毕。</font>

6. <font style="color:#595959;">客户端验证证书：</font>
    - <font style="color:#595959;">信任链验证：检查证书的颁发机构是否是客户端信任的根证书颁发机构（或其下级CA）。客户端操作系统或浏览器内置了受信任的根CA列表。</font>
    - <font style="color:#595959;">有效期验证：检查证书是否在有效期内。</font>
    - <font style="color:#595959;">域名匹配：检查证书中包含的域名是否与客户端请求的域名一致。</font>
    - <font style="color:#595959;">撤销状态：检查证书是否已被吊销（通过CRL或OCSP）。</font>
    - <font style="color:#595959;">客户端收到服务器证书后，会进行以下验证：</font>
    - <font style="color:#595959;">如果证书验证失败，客户端会终止连接并向用户发出警告。</font>
7. <font style="color:#595959;">客户端生成预主密钥并加密发送：</font>
    - <font style="color:#595959;">客户端生成一个预主密钥。这个密钥是本次会话的关键，它将与 </font>`<font style="color:#595959;">Client Random</font>`<font style="color:#595959;"> 和 </font>`<font style="color:#595959;">Server Random</font>`<font style="color:#595959;"> 一起用于生成最终的会话密钥。</font>
    - <font style="color:#595959;">客户端使用服务器证书中的公钥加密这个预主密钥，然后发送给服务器。</font>
8. <font style="color:#595959;">客户端发送变更密码规范通知：</font>

<font style="color:#595959;">客户端发送 </font>`<font style="color:#595959;">Change Cipher Spec</font>`<font style="color:#595959;"> 消息，通知服务器后续的通信将使用协商好的加密套件和密钥进行加密。</font>

9. <font style="color:#595959;">客户端发送握手完成消息：</font>

<font style="color:#595959;">客户端使用新协商的加密算法和密钥，加密一条包含之前所有握手消息的哈希值，然后发送 </font>`<font style="color:#595959;">Finished</font>`<font style="color:#595959;"> 消息。这是对整个握手过程的完整性校验。</font>

10. <font style="color:#595959;">服务器解密预主密钥并生成会话密钥：</font>
    - <font style="color:#595959;">服务器使用自己的私钥解密客户端发送的预主密钥。</font>
    - <font style="color:#595959;">服务器也使用 </font>`<font style="color:#595959;">Client Random</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">Server Random</font>`<font style="color:#595959;"> 和解密后的预主密钥，通过相同的算法生成主密钥，进而生成用于对称加密的会话密钥。</font>
11. <font style="color:#595959;">服务器发送变更密码规范通知：</font>

<font style="color:#595959;">服务器发送 </font>`<font style="color:#595959;">Change Cipher Spec</font>`<font style="color:#595959;"> 消息，通知客户端后续的通信将使用协商好的加密套件和密钥进行加密。</font>

12. <font style="color:#595959;">服务器发送握手完成消息：</font>

<font style="color:#595959;">服务器也使用新协商的加密算法和密钥，加密一条包含之前所有握手消息的哈希值，然后发送 </font>`<font style="color:#595959;">Finished</font>`<font style="color:#595959;"> 消息。</font>

<font style="color:#595959;">至此，TLS握手完成。客户端和服务器都拥有了相同的会话密钥，并且验证了对方的身份（至少是服务器的身份）。</font>

##### <font style="color:black;">数据传输阶段</font>
<font style="color:#595959;">握手完成后，客户端和服务器之间的所有HTTP应用层数据都将使用协商好的对称加密算法（如AES、ChaCha20）和会话密钥进行加密传输。同时，为了保证数据完整性，还会使用哈希算法（如SHA256）对数据进行消息认证码（MAC）计算，防止数据被篡改。</font>

##### <font style="color:black;">加密算法</font>
+ <font style="color:#595959;">非对称加密：在握手阶段用于密钥交换和身份认证（如RSA、ECC）。公钥加密，私钥解密。</font>
+ <font style="color:#595959;">对称加密：在数据传输阶段用于实际数据的加密和解密（如AES、DES、ChaCha20）。加密和解密使用相同的密钥，效率高。</font>
+ <font style="color:#595959;">哈希算法：用于生成消息摘要，验证数据完整性（如MD5、SHA1、SHA256）。</font>

## **<font style="color:#595959;">6、动静态多态实现、虚表？</font>**
<font style="color:#595959;">在C++中，多态是面向对象编程的三大特性之一（封装、继承、多态），它允许我们使用一个基类指针或引用来操作派生类对象，并且能够根据实际指向的对象的类型来调用相应的成员函数。</font>

<font style="color:#595959;">C++中的多态可以分为两种：静态多态和动态多态。</font>

##### <font style="color:black;">静态多态（编译时多态）</font>
<font style="color:#595959;">静态多态在编译时确定函数的调用。它的实现主要依赖于函数重载和运算符重载，以及模板。</font>

+ <font style="color:#595959;">函数重载：在同一个作用域内，允许定义多个同名函数，但它们的参数列表（参数类型、参数个数或参数顺序）必须不同。编译器在编译时根据函数调用时提供的参数类型和数量来决定调用哪个函数。</font>
+ <font style="color:#595959;">运算符重载：允许为自定义类型重新定义运算符的行为。编译器在编译时根据操作数的类型来决定调用哪个重载的运算符函数。</font>
+ <font style="color:#595959;">模板：允许编写泛型代码，使得函数或类可以处理多种数据类型。编译器在编译时根据模板参数的类型生成具体的代码。</font>

<font style="color:#595959;">静态多态的特点是效率高，因为函数调用在编译时就已经确定，没有运行时的额外开销。</font>

##### <font style="color:black;">动态多态（运行时多态）</font>
<font style="color:#595959;">动态多态在运行时确定函数的调用。它主要通过虚函数和虚函数表来实现。</font>

<font style="color:#595959;">动态多态要求：</font>

1. <font style="color:#595959;">继承关系：必须存在基类和派生类之间的继承关系。</font>
2. <font style="color:#595959;">虚函数：基类中必须声明虚函数，派生类可以重写（override）这些虚函数。</font>
3. <font style="color:#595959;">基类指针或引用：必须通过基类的指针或引用来调用虚函数。</font>

```plain
class Animal {
public:
    virtual void speak() {
        std::cout << "Animal makes a sound" << std::endl;
    }
    virtual ~Animal() {} // 虚析构函数，防止内存泄漏
};

class Dog :public Animal {
public:
    void speak() override {
        std::cout << "Woof! Woof!" << std::endl;
    }
};

class Cat :public Animal {
public:
    void speak() override {
        std::cout << "Meow! Meow!" << std::endl;
    }
};

int main() {
    Animal* myAnimal1 = new Dog();
    Animal* myAnimal2 = new Cat();
    Animal* myAnimal3 = new Animal();

    myAnimal1->speak(); // 运行时调用 Dog::speak()
    myAnimal2->speak(); // 运行时调用 Cat::speak()
    myAnimal3->speak(); // 运行时调用 Animal::speak()

    delete myAnimal1;
    delete myAnimal2;
    delete myAnimal3;
    return0;
}
```

<font style="color:#595959;">这个例子中，尽管 </font>`<font style="color:#595959;">myAnimal1</font>`<font style="color:#595959;"> 和 </font>`<font style="color:#595959;">myAnimal2</font>`<font style="color:#595959;"> 都是 </font>`<font style="color:#595959;">Animal</font>`<font style="color:#595959;"> 类型的指针，但它们在运行时调用了各自派生类中的 </font>`<font style="color:#595959;">speak()</font>`<font style="color:#595959;"> 方法，这就是动态多态。</font>

##### <font style="color:black;">虚函数表</font>
<font style="color:#595959;">动态多态的实现机制是虚函数表（VTable）。每个包含虚函数的类（或继承了虚函数的类）都会有一个虚函数表。虚函数表是一个由函数指针组成的数组，每个指针指向该类中虚函数的实际实现。</font>

<font style="color:#595959;">虚函数表的结构和工作原理：</font>

1. <font style="color:#595959;">虚函数表指针（vptr）：当一个类中声明了虚函数（或继承了虚函数）时，编译器会在该类的对象实例中添加一个隐藏的成员——</font>**<font style="color:#595959;">虚函数表指针（vptr）</font>**<font style="color:#595959;">。</font>`<font style="color:#595959;">vptr</font>`<font style="color:#595959;"> 通常是对象实例的第一个成员（在大多数编译器中），它指向该类对应的虚函数表。</font>
2. <font style="color:#595959;">虚函数表（VTable）：每个包含虚函数的类都会有一个唯一的虚函数表。这个表是在编译时由编译器生成的，它是一个静态的、只读的数组，存储了该类所有虚函数的地址。</font>
    - <font style="color:#595959;">如果派生类重写了基类的虚函数，那么派生类的虚函数表中对应位置的函数指针将指向派生类中重写后的函数实现。</font>
    - <font style="color:#595959;">如果派生类没有重写基类的虚函数，那么派生类的虚函数表中对应位置的函数指针将指向基类中的虚函数实现。</font>
3. <font style="color:#595959;">虚函数调用过程：</font>

<font style="color:#595959;">当通过基类指针或引用调用一个虚函数时（例如 </font>`<font style="color:#595959;">myAnimal1->speak()</font>`<font style="color:#595959;">）：</font>

    - <font style="color:#595959;">编译器首先通过基类指针（</font>`<font style="color:#595959;">myAnimal1</font>`<font style="color:#595959;">）找到它所指向的对象的内存地址。</font>
    - <font style="color:#595959;">从对象的内存地址的起始位置（通常是第一个成员）找到 </font>`<font style="color:#595959;">vptr</font>`<font style="color:#595959;">。</font>
    - <font style="color:#595959;">通过 </font>`<font style="color:#595959;">vptr</font>`<font style="color:#595959;"> 找到对应的虚函数表。</font>
    - <font style="color:#595959;">在虚函数表中，根据虚函数在类声明中的偏移量（编译时确定），找到对应虚函数的函数指针。</font>
    - <font style="color:#595959;">通过这个函数指针，调用实际的函数实现。</font>

**<font style="color:#595959;">「示例图解」</font>**<font style="color:#595959;">：</font>

```plain
对象实例内存布局：
+-------------------+
| vptr              |  <-- 指向虚函数表
+-------------------+
| 其他成员变量        |
+-------------------+

虚函数表（VTable）内存布局：
+-------------------+
| &Animal::speak    |  <-- Animal类的虚函数表
+-------------------+
| &Animal::~Animal  |
+-------------------+

+-------------------+
| &Dog::speak       |  <-- Dog类的虚函数表
+-------------------+
| &Animal::~Animal  |
+-------------------+

+-------------------+
| &Cat::speak       |  <-- Cat类的虚函数表
+-------------------+
| &Animal::~Animal  |
+-------------------+
```

<font style="color:#595959;">虚析构函数：</font>

<font style="color:#595959;">基类的析构函数通常也应该声明为虚函数（</font>`<font style="color:#595959;">virtual ~BaseClass()</font>`<font style="color:#595959;">）。</font>

<font style="color:#595959;">这是为了确保当通过基类指针删除派生类对象时，能够正确调用派生类的析构函数，从而避免内存泄漏。</font>

<font style="color:#595959;">如果基类析构函数不是虚函数，那么 </font>`<font style="color:#595959;">delete basePtr;</font>`<font style="color:#595959;"> 只会调用基类的析构函数，而不会调用派生类的析构函数。</font>

## **<font style="color:#595959;">7、模板元编程？</font>**
<font style="color:#595959;">核心思想：</font>

<font style="color:#595959;">模板元编程将类型作为参数，将编译器的模板实例化过程视为一种计算过程。它利用模板特化、递归模板、类型推导等机制，在编译时进行条件判断、循环、甚至执行复杂的算法。最终的结果是生成高度优化的、针对特定类型或值定制的代码，或者在编译时进行类型检查和错误报告。</font>

<font style="color:#595959;">基本构成要素：</font>

1. <font style="color:#595959;">类模板和函数模板：模板元编程的基础，允许编写泛型代码。</font>
2. <font style="color:#595959;">模板特化：允许为特定类型或值提供模板的特定实现。这是实现条件判断和递归终止的关键。</font>
3. <font style="color:#595959;">递归模板：通过模板的递归实例化来模拟循环结构。</font>
4. `<font style="color:#595959;">typedef</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">using</font>`<font style="color:#595959;"> 别名：用于在编译时传递计算结果（通常是类型或常量）。</font>
5. `<font style="color:#595959;">enum</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">static const</font>`<font style="color:#595959;"> 成员：用于在编译时传递计算结果（通常是整数常量）。</font>
6. <font style="color:#595959;">SFINAE (Substitution Failure Is Not An Error)：一种编译时特性，用于在模板实例化失败时，不产生编译错误，而是将该模板从候选集中移除。这使得可以根据类型特性进行编译时选择。</font>

<font style="color:#595959;">示例：编译时计算阶乘</font>

```plain
// 递归模板定义：计算N的阶乘
template <int N>
struct Factorial {
    staticconstint value = N * Factorial<N - 1>::value;
};

// 模板特化：递归终止条件，0的阶乘是1
template <>
struct Factorial<0> {
    staticconstint value = 1;
};

int main() {
    // 在编译时计算 5! = 120
    std::cout << "Factorial of 5 is: " << Factorial<5>::value << std::endl; // 输出 120

    // 在编译时计算 0! = 1
    std::cout << "Factorial of 0 is: " << Factorial<0>::value << std::endl; // 输出 1

    // 编译时错误：负数阶乘
    // std::cout << Factorial<-1>::value << std::endl; // 会导致编译错误，因为没有 Factorial<-1> 的特化或递归终止条件
    return0;
}
```

<font style="color:#595959;">例子中，</font>`<font style="color:#595959;">Factorial<N>::value</font>`<font style="color:#595959;"> 的计算是在编译时完成的。编译器会根据模板参数 </font>`<font style="color:#595959;">N</font>`<font style="color:#595959;"> 的值，递归地实例化 </font>`<font style="color:#595959;">Factorial</font>`<font style="color:#595959;"> 模板，直到遇到 </font>`<font style="color:#595959;">Factorial<0></font>`<font style="color:#595959;"> 的特化版本，从而得到最终的编译时常量。</font>

<font style="color:#595959;">模板元编程的优势：</font>

1. <font style="color:#595959;">性能优化：将计算从运行时提前到编译时，消除了运行时的计算开销，生成更高效的代码。</font>
2. <font style="color:#595959;">类型安全：在编译时进行类型检查和约束，可以捕获许多潜在的类型错误，提高程序的健壮性。</font>
3. <font style="color:#595959;">代码生成：可以根据不同的模板参数生成不同的代码路径，实现高度定制化的代码，避免运行时条件判断。</font>
4. <font style="color:#595959;">泛型编程：支持更高级别的泛型编程，实现更灵活、更通用的算法和数据结构。</font>
5. <font style="color:#595959;">消除运行时开销：例如，策略模式可以通过模板元编程在编译时选择策略，避免虚函数调用的运行时开销。</font>

## **<font style="color:#595959;">8、虚拟内存-内存映射文件、页交换文件原理与区别？</font>**
<font style="color:#595959;">虚拟内存是现代操作系统的一项核心技术，它为每个进程提供了一个独立的、连续的、私有的地址空间，使得程序认为自己拥有比实际物理内存大得多的内存。</font>

<font style="color:#595959;">虚拟内存通过将程序的逻辑地址（虚拟地址）映射到物理内存地址来实现，并利用磁盘空间作为物理内存的扩展。</font>

<font style="color:#595959;">虚拟内存的实现依赖于内存管理单元（MMU）和页表（Page Table）。当程序访问一个虚拟地址时，MMU会查找页表，将虚拟地址转换为物理地址。如果虚拟地址对应的页面不在物理内存中，就会触发缺页中断，操作系统会介入处理，将所需的页面从磁盘加载到物理内存中。</font>

<font style="color:#595959;">在虚拟内存体系中，内存映射文件和页交换文件是两种重要的机制，它们都利用磁盘空间来扩展内存，但目的和工作原理有所不同。</font>

##### <font style="color:black;">内存映射文件</font>
<font style="color:#595959;">原理：</font>

<font style="color:#595959;">内存映射文件是一种将文件内容直接映射到进程虚拟地址空间的技术。一旦文件被映射，程序就可以像访问内存数组一样直接读写文件内容，而无需使用传统的 </font>`<font style="color:#595959;">read()</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">write()</font>`<font style="color:#595959;"> 系统调用。操作系统负责在需要时将文件的部分内容（页面）从磁盘加载到物理内存，并在修改后写回磁盘。</font>

<font style="color:#595959;">特点：</font>

+ <font style="color:#595959;">高效I/O：避免了传统文件I/O中数据在用户缓冲区和内核缓冲区之间的多次复制，直接通过页表映射实现数据传输，提高了I/O效率。</font>
+ <font style="color:#595959;">简化编程：程序可以直接通过指针操作文件内容，简化了文件I/O的编程模型。</font>
+ <font style="color:#595959;">进程间通信（IPC）：多个进程可以将同一个文件映射到各自的地址空间，从而实现共享内存式的进程间通信。对映射区域的修改会反映到所有映射该文件的进程中。</font>
+ <font style="color:#595959;">延迟加载：只有当程序实际访问映射区域的某个页面时，操作系统才会将该页面从磁盘加载到物理内存中（按需加载）。</font>
+ <font style="color:#595959;">持久性：对映射区域的修改可以直接反映到磁盘上的文件中，实现数据的持久化。</font>

<font style="color:#595959;">应用场景：</font>

+ <font style="color:#595959;">大文件处理：处理远大于物理内存的大文件，无需一次性加载整个文件。</font>
+ <font style="color:#595959;">高性能文件I/O：数据库系统、日志系统等需要频繁读写文件的应用。</font>
+ <font style="color:#595959;">进程间通信：在同一台机器上实现高效的数据共享。</font>
+ <font style="color:#595959;">加载动态链接库：操作系统通常将可执行文件和动态链接库映射到进程的地址空间。</font>

##### <font style="color:black;">页交换文件</font>
<font style="color:#595959;">原理：</font>

<font style="color:#595959;">页交换文件（在Windows中称为“页面文件”，在Linux中称为“交换空间”或“交换文件/分区”）是操作系统在磁盘上预留的一块特殊区域，用于作为物理内存的后备存储。当物理内存不足时，操作系统会将物理内存中不经常使用或优先级较低的页面暂时写入到页交换文件中，从而释放出物理内存供其他更活跃的进程或页面使用。这个过程称为页面置换或换出。</font>

<font style="color:#595959;">当程序再次访问被换出的页面时，会触发缺页中断，操作系统会从页交换文件中将该页面重新加载到物理内存中，这个过程称为换入。</font>

<font style="color:#595959;">特点：</font>

+ <font style="color:#595959;">物理内存扩展：作为物理内存的逻辑扩展，使得系统可以运行的程序总大小超过实际物理内存。</font>
+ <font style="color:#595959;">透明性：对于应用程序来说，页交换过程是完全透明的，应用程序无需关心页面是否在物理内存中。</font>
+ <font style="color:#595959;">性能瓶颈：磁盘I/O速度远低于内存访问速度，频繁的页面换入换出（称为“颠簸”或“抖动”，Thrashing）会导致系统性能急剧下降。</font>
+ <font style="color:#595959;">非持久性：页交换文件中的数据通常是临时性的，用于保存内存页面的副本，系统关闭后通常会被清空或重置。</font>

<font style="color:#595959;">应用场景：</font>

+ <font style="color:#595959;">内存不足时的应急措施：当物理内存紧张时，提供额外的虚拟内存容量，防止程序因内存不足而崩溃。</font>
+ <font style="color:#595959;">不活跃页面的存储：将长时间不被访问的页面换出到磁盘，释放物理内存给更活跃的进程。</font>

##### <font style="color:black;">内存映射文件与页交换文件的区别</font>
| **<font style="color:#595959;">特性</font>** | **<font style="color:#595959;">内存映射文件</font>** | **<font style="color:#595959;">页交换文件</font>** |
| :--- | :--- | :--- |
| <font style="color:#595959;">目的</font> | <font style="color:#595959;">将文件内容映射到虚拟地址空间，实现高效文件I/O和IPC</font> | <font style="color:#595959;">作为物理内存的扩展，用于页面置换，缓解内存压力</font> |
| <font style="color:#595959;">数据来源</font> | <font style="color:#595959;">磁盘上的实际文件</font> | <font style="color:#595959;">物理内存中被换出的页面副本</font> |
| <font style="color:#595959;">持久性</font> | <font style="color:#595959;">对映射区域的修改会反映到原始文件，数据持久化</font> | <font style="color:#595959;">数据通常是临时的，系统关闭后清空或重置</font> |
| <font style="color:#595959;">主动性</font> | <font style="color:#595959;">应用程序主动请求映射文件</font> | <font style="color:#595959;">操作系统在物理内存不足时自动进行页面置换</font> |
| <font style="color:#595959;">共享性</font> | <font style="color:#595959;">多个进程可以映射同一个文件，实现共享内存</font> | <font style="color:#595959;">通常是进程私有的内存页面的副本，不直接用于进程间共享</font> |
| <font style="color:#595959;">I/O模式</font> | <font style="color:#595959;">像访问内存一样访问文件内容</font> | <font style="color:#595959;">操作系统在后台进行页面换入换出</font> |
| <font style="color:#595959;">典型用途</font> | <font style="color:#595959;">大文件处理、IPC、程序加载</font> | <font style="color:#595959;">缓解内存不足、支持更多并发进程</font> |


## **<font style="color:#595959;">9、用户态与内核态间进程间通讯方法 ？</font>**
<font style="color:#595959;">进程间通信（IPC）是操作系统中一个重要的概念，它允许不同进程之间交换信息。</font>

<font style="color:#595959;">当涉及到用户态进程和内核态之间的数据交换时，情况会变得更加复杂，因为这涉及到特权级的切换和内存访问权限的控制。</font>

<font style="color:#595959;">以下是用户态进程与内核态之间进行通信的常见方法：</font>

1. <font style="color:#595959;">系统调用：</font>
    - <font style="color:#595959;">单向请求-响应：通常是用户态发起请求，内核执行并返回结果。</font>
    - <font style="color:#595959;">安全：内核对所有传入参数进行严格校验，防止恶意或非法操作。</font>
    - <font style="color:#595959;">开销：涉及上下文切换和特权级切换，有一定的性能开销。</font>
    - <font style="color:#595959;">原理：这是用户态进程与内核态通信最基本、最常用的方式。用户态进程通过调用操作系统提供的API（即系统调用）来请求内核执行特权操作或访问受保护的资源。在系统调用过程中，会发生从用户态到内核态的切换。</font>
    - <font style="color:#595959;">通信方式：参数通过寄存器或用户栈传递给内核，返回值通过寄存器或用户栈返回给用户态。数据通常通过用户态和内核态共享的缓冲区进行复制。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">示例：</font>`<font style="color:#595959;">read()</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">write()</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">open()</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">fork()</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">socket()</font>`<font style="color:#595959;"> 等。</font>
2. <font style="color:#595959;">内存映射文件（Memory-Mapped Files）：</font>
    - <font style="color:#595959;">高效：避免了数据在用户态和内核态之间的复制，尤其适用于大量数据传输。</font>
    - <font style="color:#595959;">复杂性：需要额外的同步机制（如互斥锁、信号量）来协调用户态和内核态对共享内存的访问，防止竞态条件。</font>
    - <font style="color:#595959;">安全性：需要谨慎处理内存访问权限，防止用户态程序非法访问内核数据。</font>
    - <font style="color:#595959;">原理：虽然主要用于进程间通信和文件I/O，但内存映射文件也可以用于用户态和内核态之间的数据交换。内核可以将某个文件（或匿名内存区域）映射到用户态进程的地址空间，同时内核自身也可以访问这块映射区域。</font>
    - <font style="color:#595959;">通信方式：用户态进程和内核可以直接读写共享的内存区域，避免了数据复制。</font>
    - <font style="color:#595959;">特点：</font>
3. <font style="color:#595959;">IOCTL：</font>
    - <font style="color:#595959;">设备特定：通常用于与特定硬件设备或虚拟设备进行交互。</font>
    - <font style="color:#595959;">灵活性：可以定义任意数量的控制命令和数据结构。</font>
    - <font style="color:#595959;">安全性：驱动程序需要对传入的参数和缓冲区进行严格的验证。</font>
    - <font style="color:#595959;">原理：IOCTL是一种特殊的系统调用，它允许用户态应用程序直接向设备驱动程序发送控制命令和数据，或者从驱动程序获取数据。它提供了一种灵活的、设备特定的通信机制，用于执行那些不属于标准 </font>`<font style="color:#595959;">read</font>`<font style="color:#595959;">/</font>`<font style="color:#595959;">write</font>`<font style="color:#595959;"> 操作的控制功能。</font>
    - <font style="color:#595959;">通信方式：通过 </font>`<font style="color:#595959;">ioctl()</font>`<font style="color:#595959;"> 系统调用，用户态程序传递一个命令码和可选的输入/输出缓冲区指针。内核将请求转发给相应的设备驱动程序，驱动程序在内核态执行操作，并通过缓冲区与用户态交换数据。</font>
    - <font style="color:#595959;">特点：</font>
4. <font style="color:#595959;">Netlink Socket (Linux特有)：</font>
    - <font style="color:#595959;">双向异步：用户态和内核态都可以主动发起通信。</font>
    - <font style="color:#595959;">多播/广播：支持向多个监听进程发送消息。</font>
    - <font style="color:#595959;">灵活：可以定义自定义的Netlink协议。</font>
    - <font style="color:#595959;">主要用于系统配置和事件通知：如路由信息、防火墙规则、进程事件等。</font>
    - <font style="color:#595959;">原理：Netlink是一种Linux内核提供的、基于socket的IPC机制，专门用于内核模块与用户态进程之间的通信。它提供了一种灵活的、异步的、双向的通信方式。</font>
    - <font style="color:#595959;">通信方式：用户态进程创建一个Netlink socket，并绑定到特定的Netlink协议族。内核模块也可以通过Netlink接口发送和接收消息。通信可以是单播、多播或广播。</font>
    - <font style="color:#595959;">特点：</font>
5. <font style="color:#595959;">procfs/sysfs (Linux特有)：</font>
    - <font style="color:#595959;">简单易用：使用标准文件I/O接口，无需特殊的API。</font>
    - <font style="color:#595959;">主要用于查询和配置：不适合大量数据传输或频繁的事件通知。</font>
    - <font style="color:#595959;">安全性：文件权限控制访问。</font>
    - <font style="color:#595959;">原理：</font>`<font style="color:#595959;">procfs</font>`<font style="color:#595959;"> 和 </font>`<font style="color:#595959;">sysfs</font>`<font style="color:#595959;"> 是Linux内核提供的虚拟文件系统，它们将内核数据结构和配置参数以文件和目录的形式暴露给用户态。用户态程序可以通过标准的 </font>`<font style="color:#595959;">open()</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">read()</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">write()</font>`<font style="color:#595959;"> 系统调用来访问这些“文件”，从而实现与内核的通信。</font>
    - <font style="color:#595959;">通信方式：用户态程序读写 </font>`<font style="color:#595959;">/proc</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">/sys</font>`<font style="color:#595959;"> 目录下的文件，内核在处理这些文件操作时，实际上是在读写内部数据结构。</font>
    - <font style="color:#595959;">特点：</font>
6. <font style="color:#595959;">信号：</font>
    - <font style="color:#595959;">异步通知：不阻塞进程执行。</font>
    - <font style="color:#595959;">信息量小：只能传递事件类型，不能传递复杂数据。</font>
    - <font style="color:#595959;">主要用于异常处理和进程控制：如 </font>`<font style="color:#595959;">SIGKILL</font>`<font style="color:#595959;"> (终止进程)、</font>`<font style="color:#595959;">SIGTERM</font>`<font style="color:#595959;"> (请求终止)、</font>`<font style="color:#595959;">SIGINT</font>`<font style="color:#595959;"> (中断)。</font>
    - <font style="color:#595959;">原理：信号是一种软件中断，用于通知进程发生了某个事件。内核可以向用户态进程发送信号，用户态进程也可以向其他用户态进程发送信号（通过内核）。</font>
    - <font style="color:#595959;">通信方式：信号本身不携带数据，只是一种通知机制。用户态进程可以注册信号处理函数来响应特定信号。</font>
    - <font style="color:#595959;">特点：</font>
7. <font style="color:#595959;">共享内存：</font>
    - <font style="color:#595959;">最高效：数据传输速度最快，因为没有数据复制。</font>
    - <font style="color:#595959;">需要同步：必须配合其他同步机制（如信号量、互斥锁）来保证数据一致性。</font>
    - <font style="color:#595959;">复杂性：管理共享内存的生命周期和同步机制较为复杂。</font>
    - <font style="color:#595959;">原理：虽然共享内存主要用于用户态进程间的通信，但内核也可以参与到共享内存的创建和管理中。例如，内核可以分配一块物理内存，并将其映射到多个用户态进程的地址空间，或者映射到内核自身的地址空间，从而实现高效的数据共享。</font>
    - <font style="color:#595959;">通信方式：用户态和内核态直接读写共享的物理内存区域，避免数据复制。</font>
    - <font style="color:#595959;">特点：</font>

## **<font style="color:#595959;">10、线程、协程区别？</font>**
##### <font style="color:black;">线程（Thread）</font>
<font style="color:#595959;">定义：</font>

<font style="color:#595959;">线程是操作系统调度的最小单位。一个进程可以包含一个或多个线程。所有线程共享进程的地址空间（包括代码段、数据段、堆等），但每个线程有独立的栈、程序计数器（PC）、寄存器上下文等。</font>

<font style="color:#595959;">特点：</font>

1. <font style="color:#595959;">操作系统调度：线程的调度由操作系统内核负责。当一个线程阻塞（例如等待I/O）时，操作系统会将CPU切换到另一个可运行的线程。</font>
2. <font style="color:#595959;">抢占式调度：操作系统可以根据时间片、优先级等策略，随时中断一个线程的执行，切换到另一个线程，实现真正的并行（在多核CPU上）或并发。</font>
3. <font style="color:#595959;">资源消耗：</font>
    - <font style="color:#595959;">创建/销毁开销大：线程的创建和销毁需要向操作系统申请和释放资源（如内核对象、栈空间），开销相对较大。</font>
    - <font style="color:#595959;">上下文切换开销大：线程上下文切换需要保存和恢复大量的CPU寄存器、程序计数器、栈指针等信息，并涉及到用户态到内核态的切换，开销较大。</font>
    - <font style="color:#595959;">内存消耗：每个线程都需要独立的栈空间（通常几MB），以及内核维护的线程控制块（TCB）等，内存消耗相对较大。</font>
4. <font style="color:#595959;">同步机制复杂：由于线程共享进程地址空间，多个线程访问共享数据时需要复杂的同步机制（如互斥锁、读写锁、条件变量、信号量等）来避免竞态条件和数据不一致。</font>
5. <font style="color:#595959;">并行执行：在多核CPU上，不同线程可以同时在不同的CPU核心上执行，实现真正的并行。</font>

<font style="color:#595959;">适用场景：</font>

+ <font style="color:#595959;">CPU密集型任务：需要充分利用多核CPU的计算能力。</font>
+ <font style="color:#595959;">需要阻塞I/O操作的场景：当一个线程执行阻塞I/O时，操作系统可以调度其他线程执行，提高CPU利用率。</font>
+ <font style="color:#595959;">需要利用操作系统提供的并发原语和调度能力的场景。</font>

##### <font style="color:black;">协程（Coroutine）</font>
<font style="color:#595959;">定义：</font>

<font style="color:#595959;">协程是一种用户态的轻量级线程，或者说是一种“用户态的协作式多任务”。它不是由操作系统内核调度的，而是由程序自身（或协程库）进行调度。协程在执行过程中可以主动暂停（yield）并将控制权交给其他协程，并在需要时从暂停点恢复执行。</font>

<font style="color:#595959;">特点：</font>

1. <font style="color:#595959;">用户态调度：协程的调度完全由用户程序控制，通常由协程库或语言运行时实现。内核对协程一无所知。</font>
2. <font style="color:#595959;">协作式调度：协程的切换是协作式的，一个协程只有在主动放弃CPU（例如通过 </font>`<font style="color:#595959;">yield</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">await</font>`<font style="color:#595959;">）时，才会将控制权交给其他协程。如果一个协程不主动放弃，它会一直占用CPU，直到完成或遇到阻塞操作。</font>
3. <font style="color:#595959;">资源消耗：</font>
    - <font style="color:#595959;">创建/销毁开销小：协程的创建和销毁只是简单的函数调用和栈空间的分配，不涉及系统调用，开销非常小。</font>
    - <font style="color:#595959;">上下文切换开销小：协程上下文切换只涉及少量寄存器和栈指针的保存和恢复，不涉及用户态到内核态的切换，开销极小。</font>
    - <font style="color:#595959;">内存消耗：协程的栈空间通常很小（几KB到几十KB），可以创建成千上万个协程而不会耗尽内存。</font>
4. <font style="color:#595959;">同步机制相对简单：由于协程是协作式的，通常不需要复杂的锁机制来保护共享数据，因为在同一时间只有一个协程在运行。但如果协程内部有阻塞操作，或者需要与线程进行交互，仍然需要同步。</font>
5. <font style="color:#595959;">单核并发：协程在单核CPU上实现的是并发，而不是并行。它们通过快速切换来模拟并行。</font>

**<font style="color:#595959;">「适用场景」</font>**<font style="color:#595959;">：</font>

+ <font style="color:#595959;">I/O密集型任务：当一个协程遇到I/O阻塞时，它可以主动让出CPU，让其他协程继续执行，从而提高I/O并发能力和吞吐量。</font>
+ <font style="color:#595959;">高并发连接服务：例如Web服务器、游戏服务器等，可以为每个连接创建一个协程，以极低的资源消耗支持大量并发连接。</font>
+ <font style="color:#595959;">状态机和事件驱动编程：协程的暂停和恢复机制非常适合实现复杂的异步逻辑和状态机。</font>
+ <font style="color:#595959;">简化异步编程：通过同步的方式编写异步代码，避免回调地狱（Callback Hell）。</font>

##### <font style="color:black;">线程与协程的区别总结</font>
| **<font style="color:#595959;">特性</font>** | **<font style="color:#595959;">线程（Thread）</font>** | **<font style="color:#595959;">协程（Coroutine）</font>** |
| :--- | :--- | :--- |
| <font style="color:#595959;">调度者</font> | <font style="color:#595959;">操作系统内核</font> | <font style="color:#595959;">用户程序/协程库/语言运行时</font> |
| <font style="color:#595959;">调度方式</font> | <font style="color:#595959;">抢占式调度</font> | <font style="color:#595959;">协作式调度（主动让出CPU）</font> |
| <font style="color:#595959;">资源消耗</font> | <font style="color:#595959;">创建/销毁、上下文切换开销大；内存消耗大</font> | <font style="color:#595959;">创建/销毁、上下文切换开销小；内存消耗小</font> |
| <font style="color:#595959;">并行性</font> | <font style="color:#595959;">多核CPU上可实现并行</font> | <font style="color:#595959;">单核CPU上实现并发，无法并行</font> |
| <font style="color:#595959;">共享资源</font> | <font style="color:#595959;">共享进程地址空间，需复杂同步机制</font> | <font style="color:#595959;">共享进程地址空间，同步相对简单（但仍需注意）</font> |
| <font style="color:#595959;">可见性</font> | <font style="color:#595959;">内核可见</font> | <font style="color:#595959;">内核不可见，只在用户态存在</font> |
| <font style="color:#595959;">栈空间</font> | <font style="color:#595959;">通常较大（MB级别）</font> | <font style="color:#595959;">通常较小（KB级别）</font> |
| <font style="color:#595959;">编程模型</font> | <font style="color:#595959;">传统同步编程，阻塞I/O导致线程阻塞</font> | <font style="color:#595959;">异步编程同步化，通过 </font>`<font style="color:#595959;">yield</font>`<font style="color:#595959;">/</font>`<font style="color:#595959;">await</font>`<font style="color:#595959;"> 避免阻塞</font> |


## **<font style="color:#595959;">11、异步I/O模型？</font>**
<font style="color:#595959;">异步I/O是一种I/O操作模式，它允许程序在发起I/O请求后立即返回，而无需等待I/O操作完成。当I/O操作完成后，操作系统会通过某种机制通知程序。</font>

<font style="color:#595959;">这与传统的同步I/O形成对比，同步I/O在发起请求后会阻塞程序，直到I/O操作完成并返回结果。</font>

<font style="color:#595959;">异步I/O的主要目的是提高程序的并发性和吞吐量，尤其是在I/O密集型应用中，可以避免CPU在等待I/O完成时处于空闲状态。</font>

<font style="color:#595959;">常见的异步I/O模型包括：</font>

1. <font style="color:#595959;">阻塞I/O（Blocking I/O）：</font>
    - <font style="color:#595959;">原理：这是最简单的I/O模型。当应用程序调用 </font>`<font style="color:#595959;">read()</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">write()</font>`<font style="color:#595959;"> 等I/O操作时，如果数据尚未准备好或缓冲区已满，系统调用会一直阻塞，直到数据传输完成或发生错误。应用程序在等待I/O期间无法执行其他任务。</font>
    - <font style="color:#595959;">特点：编程简单，但效率低下，不适合高并发场景。</font>
    - <font style="color:#595959;">示例：传统的 </font>`<font style="color:#595959;">read()</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">write()</font>`<font style="color:#595959;">。</font>
2. <font style="color:#595959;">非阻塞I/O（Non-Blocking I/O）：</font>
    - <font style="color:#595959;">原理：应用程序将套接字或文件描述符设置为非阻塞模式。当应用程序调用 </font>`<font style="color:#595959;">read()</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">write()</font>`<font style="color:#595959;"> 时，如果I/O操作不能立即完成，系统调用会立即返回一个错误码（如 </font>`<font style="color:#595959;">EAGAIN</font>`<font style="color:#595959;"> 或 </font>`<font style="color:#595959;">EWOULDBLOCK</font>`<font style="color:#595959;">），而不是阻塞。应用程序需要通过轮询（Polling）的方式反复调用I/O操作，直到数据准备好。</font>
    - <font style="color:#595959;">特点：避免了阻塞，但需要应用程序不断轮询，CPU利用率可能不高，且编程复杂。</font>
    - <font style="color:#595959;">示例：</font>`<font style="color:#595959;">fcntl(fd, F_SETFL, O_NONBLOCK)</font>`<font style="color:#595959;"> 设置非阻塞模式。</font>
3. <font style="color:#595959;">I/O多路复用（I/O Multiplexing）：</font>
    - `<font style="color:#595959;">select</font>`<font style="color:#595959;">：通过位图来表示文件描述符集合，每次调用都需要将整个集合从用户态复制到内核态，并遍历所有描述符，效率较低。</font>
    - `<font style="color:#595959;">poll</font>`<font style="color:#595959;">：与 </font>`<font style="color:#595959;">select</font>`<font style="color:#595959;"> 类似，但使用 </font>`<font style="color:#595959;">pollfd</font>`<font style="color:#595959;"> 结构体数组，没有文件描述符数量限制，但效率仍不高。</font>
    - `<font style="color:#595959;">epoll</font>`<font style="color:#595959;"> (Linux)：基于事件驱动，通过 </font>`<font style="color:#595959;">epoll_create</font>`<font style="color:#595959;"> 创建一个epoll实例，</font>`<font style="color:#595959;">epoll_ctl</font>`<font style="color:#595959;"> 添加/修改/删除事件，</font>`<font style="color:#595959;">epoll_wait</font>`<font style="color:#595959;"> 等待事件。只返回就绪的描述符，效率高，适合高并发。</font>
    - `<font style="color:#595959;">kqueue</font>`<font style="color:#595959;"> (FreeBSD/macOS)：功能类似于 </font>`<font style="color:#595959;">epoll</font>`<font style="color:#595959;">。</font>
    - <font style="color:#595959;">单线程处理多连接：一个线程可以同时管理多个I/O连接，避免了为每个连接创建线程的开销。</font>
    - <font style="color:#595959;">避免轮询：应用程序不再需要主动轮询每个文件描述符，而是等待内核通知。</font>
    - <font style="color:#595959;">效率：</font>`<font style="color:#595959;">epoll</font>`<font style="color:#595959;"> 和 </font>`<font style="color:#595959;">kqueue</font>`<font style="color:#595959;"> 等高级多路复用机制在处理大量并发连接时效率更高，因为它们只返回就绪的描述符，并且内核维护事件列表。</font>
    - <font style="color:#595959;">原理：应用程序通过一个系统调用（如 </font>`<font style="color:#595959;">select</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">poll</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">epoll</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">kqueue</font>`<font style="color:#595959;">）同时监听多个文件描述符（套接字或文件）。当这些文件描述符中的任何一个准备好进行I/O操作时（例如，有数据可读或可写），系统调用就会返回，应用程序然后可以对这些“就绪”的文件描述符进行非阻塞I/O操作。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">示例：</font>
4. <font style="color:#595959;">信号驱动I/O（Signal-Driven I/O）：</font>
    - <font style="color:#595959;">异步通知：无需阻塞或轮询。</font>
    - <font style="color:#595959;">复杂性：信号处理机制相对复杂，且信号本身不携带具体数据，需要额外机制获取就绪的文件描述符。</font>
    - <font style="color:#595959;">适用性有限：通常只用于UDP套接字或少数特定设备。</font>
    - <font style="color:#595959;">原理：应用程序通过 </font>`<font style="color:#595959;">sigaction</font>`<font style="color:#595959;"> 注册一个信号处理函数，并设置文件描述符为信号驱动模式（</font>`<font style="color:#595959;">O_ASYNC</font>`<font style="color:#595959;">）。当I/O操作准备就绪时，内核会向应用程序发送一个 </font>`<font style="color:#595959;">SIGIO</font>`<font style="color:#595959;"> 信号。应用程序在信号处理函数中进行I/O操作。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">示例：</font>`<font style="color:#595959;">fcntl(fd, F_SETOWN, getpid())</font>`<font style="color:#595959;"> 和 </font>`<font style="color:#595959;">fcntl(fd, F_SETFL, O_ASYNC)</font>`<font style="color:#595959;">。</font>
5. <font style="color:#595959;">异步I/O（Asynchronous I/O / AIO）：</font>
    - <font style="color:#595959;">完全非阻塞：应用程序无需等待I/O完成，可以继续执行其他任务。</font>
    - <font style="color:#595959;">高效率：内核直接处理I/O，应用程序无需参与轮询或事件循环。</font>
    - <font style="color:#595959;">复杂性：编程模型相对复杂，需要处理回调或事件通知。</font>
    - <font style="color:#595959;">原理：这是真正意义上的异步I/O。应用程序发起I/O请求后，系统调用会立即返回，而I/O操作在后台由内核完成。当I/O操作完成后，内核会通过回调函数、事件通知、完成端口（Completion Port）等机制通知应用程序，并传递操作结果。</font>
    - <font style="color:#595959;">特点：</font>

## **<font style="color:#595959;">12、const关键字？</font>**
`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 是C++中一个非常重要的关键字，它用于声明一个常量，表示某个值是不可修改的。</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 的使用旨在提高代码的健壮性、可读性和安全性，并允许编译器进行更多的优化。</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 可以用于变量、指针、引用、函数参数、函数返回值以及成员函数。</font>

##### `<font style="color:black;">const</font>`<font style="color:black;"> 的多种用法</font>
1. <font style="color:#595959;">修饰变量：</font>
    - <font style="color:#595959;">常量变量：声明一个变量为常量，其值在初始化后不能被修改。</font>

```plain
const int MAX_VALUE = 100; // 常量，必须初始化
// MAX_VALUE = 200; // 错误：不能修改常量
```

    - `<font style="color:#595959;">const</font>`<font style="color:#595959;"> 变量必须在声明时或构造函数初始化列表中初始化。</font>
2. <font style="color:#595959;">修饰指针：</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 修饰指针时，位置不同，含义也不同，这常常是面试的考点。</font>
    - <font style="color:#595959;">指向常量的指针：指针指向的内容是常量，不能通过该指针修改其指向的值，但指针本身可以修改，指向其他地方。</font>

```plain
const int* ptr_to_const; // int 是常量
int value = 10;
const int const_value = 20;

ptr_to_const = &value;      // 可以指向非const变量
// *ptr_to_const = 15;    // 错误：不能通过ptr_to_const修改value
value = 15;                 // 可以通过非const方式修改value

ptr_to_const = &const_value; // 可以指向const变量
```

<font style="color:#595959;">可以读作“</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 在 </font>`<font style="color:#595959;">*</font>`<font style="color:#595959;"> 左边，修饰的是 </font>`<font style="color:#595959;">*ptr_to_const</font>`<font style="color:#595959;">，即指针指向的值是常量”。</font>

    - <font style="color:#595959;">常量指针：指针本身是常量，一旦初始化后，不能再指向其他地方，但可以通过该指针修改其指向的值（如果指向的值不是常量）。</font>

```plain
int* const const_ptr = &value; // 指针本身是常量
*const_ptr = 25;               // 可以通过const_ptr修改value
// const_ptr = &const_value; // 错误：不能修改const_ptr的指向
```

<font style="color:#595959;">可以读作“</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 在 </font>`<font style="color:#595959;">*</font>`<font style="color:#595959;"> 右边，修饰的是 </font>`<font style="color:#595959;">const_ptr</font>`<font style="color:#595959;">，即指针本身是常量”。</font>

    - <font style="color:#595959;">指向常量的常量指针：指针本身和指针指向的内容都是常量，都不能修改。</font>

```plain
const int* const const_ptr_to_const = &value; // 两者都是常量
// *const_ptr_to_const = 30; // 错误
// const_ptr_to_const = &const_value; // 错误
```

3. <font style="color:#595959;">修饰引用：</font>
    - <font style="color:#595959;">常量引用：引用本身是常量，不能通过该引用修改其引用的值。常量引用可以绑定到非 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 对象、</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 对象、临时对象或字面量。</font>

```plain
int a = 10;
const int& ref_a = a; // 常量引用绑定到非const变量
// ref_a = 20; // 错误：不能通过ref_a修改a
a = 20; // 可以通过a修改a

const int b = 30;
const int& ref_b = b; // 常量引用绑定到const变量

const int& ref_temp = 100; // 常量引用可以绑定到临时对象或字面量
```

    - <font style="color:#595959;">常量引用在函数参数传递中非常有用，可以避免不必要的对象复制，同时保证函数不会修改传入的参数。</font>
4. <font style="color:#595959;">修饰函数参数：</font>
    - <font style="color:#595959;">使用 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 修饰函数参数，表示函数内部不会修改该参数的值。这对于指针和引用参数尤其重要。</font>

```plain
void print_value(const int& val) {
    // val = 10; // 错误：不能修改const引用
    std::cout << val << std::endl;
}

void print_array(const int* arr, int size) {
    // arr[0] = 1; // 错误：不能通过const指针修改数组内容
    for (int i = 0; i < size; ++i) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}
```

    - <font style="color:#595959;">这提高了函数的安全性，并向调用者表明该函数是“只读”的，不会产生副作用。</font>
5. <font style="color:#595959;">修饰函数返回值：</font>
    - <font style="color:#595959;">返回 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 值：对于基本类型，返回 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 值没有太大意义，因为返回值会被复制，复制后的副本不再是 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;">。但对于类类型，返回 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 引用或 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 对象可以防止修改返回值。</font>

```plain
const int get_const_int() {
    return10;
}

class MyClass {
public:
    int value = 0;
    const MyClass& get_const_ref() const {
        return *this;
    }
};

int main() {
    // get_const_int() = 20; // 错误：临时对象不能被修改
    MyClass obj;
    // obj.get_const_ref().value = 1; // 错误：通过const引用不能修改成员
    return0;
}
```

    - <font style="color:#595959;">通常用于返回引用或指针，以防止通过返回值修改对象状态。</font>
6. <font style="color:#595959;">修饰成员函数（</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 成员函数）：</font>
    - <font style="color:#595959;">在成员函数声明的末尾加上 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 关键字，表示该成员函数不会修改对象的状态（即不会修改类的非静态成员变量）。</font>

```plain
class MyClass {
public:
    int data;

    void set_data(int d) { // 非const成员函数，可以修改成员变量
        data = d;
    }

    int get_data() const { // const成员函数，不能修改成员变量
        // data = 10; // 错误：在const成员函数中不能修改非mutable成员变量
        return data;
    }

    void print_data() const;
};

void MyClass::print_data() const {
    std::cout << "Data: " << data << std::endl;
}
```

    - `<font style="color:#595959;">const</font>`<font style="color:#595959;"> 对象的限制：</font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 对象只能调用 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 成员函数。</font>

```plain
const MyClass obj_c;
// obj_c.set_data(10); // 错误：const对象不能调用非const成员函数
obj_c.get_data();     // 正确：const对象可以调用const成员函数
```

    - `<font style="color:#595959;">mutable</font>`<font style="color:#595959;"> 关键字：如果需要在 </font>`<font style="color:#595959;">const</font>`<font style="color:#595959;"> 成员函数中修改某个非静态成员变量，可以使用 </font>`<font style="color:#595959;">mutable</font>`<font style="color:#595959;"> 关键字修饰该成员变量。这通常用于那些不影响对象逻辑状态的内部缓存或计数器。</font>

```plain
class MyClassWithMutable {
public:
    int value;
    mutable int access_count; // 即使在const函数中也可以修改

    MyClassWithMutable(int v) : value(v), access_count(0) {}

    int get_value() const {
        access_count++; // 在const函数中修改mutable成员
        return value;
    }
};
```

## **<font style="color:#595959;">13、多线程同步机制，锁的底层原理等？</font>**
<font style="color:#595959;">在多线程编程中，多个线程共享进程的地址空间，这意味着它们可以同时访问和修改相同的共享数据。</font>

<font style="color:#595959;">如果不加以控制，这种并发访问可能导致竞态条件和数据不一致的问题。为了确保共享数据的正确性和一致性，我们需要使用多线程同步机制。</font>

<font style="color:#595959;">同步机制的核心思想是协调线程的执行顺序，确保在任何给定时刻，只有一个线程能够访问关键代码区域（临界区）或共享资源。</font>

##### <font style="color:black;">常见的多线程同步机制</font>
1. <font style="color:#595959;">互斥锁（Mutex）：</font>
    - <font style="color:#595959;">原子操作：互斥锁的实现依赖于CPU提供的原子操作（如 </font>`<font style="color:#595959;">Test-and-Set</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">Compare-and-Swap / CAS</font>`<font style="color:#595959;">）。这些指令能够在一个不可中断的步骤中完成读取、修改和写入内存的操作，从而保证了在多核环境下对锁状态的修改是安全的。</font>
    - <font style="color:#595959;">自旋锁：在某些情况下，如果预期锁的持有时间很短，线程可能会采用自旋的方式（即在一个循环中不断尝试获取锁，而不放弃CPU）来等待锁。自旋锁避免了上下文切换的开销，但如果锁的持有时间长，会浪费CPU周期。</font>
    - <font style="color:#595959;">内核态等待：当自旋等待一段时间后仍无法获取锁，或者锁的持有时间可能较长时，线程会放弃CPU，进入睡眠状态，并被放入等待队列。当锁被释放时，操作系统会唤醒等待队列中的一个线程。这涉及到用户态到内核态的切换。</font>
    - <font style="color:#595959;">排他性：同一时间只有一个线程可以持有锁。</font>
    - <font style="color:#595959;">简单易用：适用于保护共享数据。</font>
    - <font style="color:#595959;">死锁风险：如果锁的获取和释放顺序不当，可能导致死锁。</font>
    - <font style="color:#595959;">原理：互斥锁是最基本的同步原语。它提供了一种排他性的访问机制。当一个线程需要访问临界区时，它会尝试“锁定”互斥锁。如果锁已经被其他线程持有，当前线程就会被阻塞，直到锁被释放。当线程完成对临界区的访问后，它会“解锁”互斥锁。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">底层原理：</font>
    - <font style="color:#595959;">C++11及以后：</font>`<font style="color:#595959;">std::mutex</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">std::lock_guard</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">std::unique_lock</font>`<font style="color:#595959;">。</font>
2. <font style="color:#595959;">读写锁（Read-Write Lock）：</font>
    - <font style="color:#595959;">提高并发性：在读多写少的场景下，可以显著提高并发性能。</font>
    - <font style="color:#595959;">写优先/读优先：不同的实现可能偏向于写线程或读线程，以避免饥饿。</font>
    - <font style="color:#595959;">原理：互斥锁在任何时候都只允许一个线程访问临界区，即使是读操作。读写锁则允许多个读线程同时访问共享资源，但只允许一个写线程访问。当有写线程访问时，所有读线程和写线程都会被阻塞。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">C++17及以后：</font>`<font style="color:#595959;">std::shared_mutex</font>`<font style="color:#595959;"> (读写锁) 和 </font>`<font style="color:#595959;">std::shared_lock</font>`<font style="color:#595959;"> (读锁)。</font>
3. <font style="color:#595959;">条件变量（Condition Variable）：</font>
    - <font style="color:#595959;">等待-通知机制：解决线程间基于条件的同步问题（如生产者-消费者问题）。</font>
    - <font style="color:#595959;">避免忙等待：线程在等待条件时不会占用CPU。</font>
    - <font style="color:#595959;">原理：条件变量通常与互斥锁一起使用，用于线程间的等待和通知。一个线程在满足特定条件之前，可以释放互斥锁并进入等待状态。当另一个线程改变了条件并通知条件变量时，等待的线程会被唤醒，并重新尝试获取互斥锁。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">C++11及以后：</font>`<font style="color:#595959;">std::condition_variable</font>`<font style="color:#595959;">。</font>
4. <font style="color:#595959;">信号量（Semaphore）：</font>
    - <font style="color:#595959;">资源计数：可以控制同时访问某个资源的线程数量。</font>
    - <font style="color:#595959;">互斥锁的泛化：当信号量计数器为1时，可以作为互斥锁使用。</font>
    - `<font style="color:#595959;">wait()</font>`<font style="color:#595959;">：信号量计数器减1。如果计数器变为负数，线程阻塞。</font>
    - `<font style="color:#595959;">signal()</font>`<font style="color:#595959;">：信号量计数器加1。如果计数器小于或等于0，唤醒一个等待的线程。</font>
    - <font style="color:#595959;">原理：信号量是一个计数器，用于控制对共享资源的访问数量。它有两个原子操作：</font>`<font style="color:#595959;">wait()</font>`<font style="color:#595959;"> (或 </font>`<font style="color:#595959;">P</font>`<font style="color:#595959;"> 操作，</font>`<font style="color:#595959;">acquire</font>`<font style="color:#595959;">) 和 </font>`<font style="color:#595959;">signal()</font>`<font style="color:#595959;"> (或 </font>`<font style="color:#595959;">V</font>`<font style="color:#595959;"> 操作，</font>`<font style="color:#595959;">release</font>`<font style="color:#595959;">)。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">C++20及以后：</font>`<font style="color:#595959;">std::counting_semaphore</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">std::binary_semaphore</font>`<font style="color:#595959;">。</font>
5. <font style="color:#595959;">原子操作（Atomic Operations）：</font>
    - <font style="color:#595959;">无锁（Lock-Free）：避免了锁带来的开销和死锁风险。</font>
    - <font style="color:#595959;">性能高：通常比锁更高效，尤其是在竞争不激烈的情况下。</font>
    - <font style="color:#595959;">适用范围有限：只适用于对单个变量的简单操作。</font>
    - <font style="color:#595959;">原理：原子操作是不可中断的操作，它们要么完全执行，要么不执行，不会被其他线程的操作打断。对于简单的共享变量（如计数器），使用原子操作可以避免使用锁，从而提高性能。</font>
    - <font style="color:#595959;">特点：</font>
    - <font style="color:#595959;">底层原理：依赖于CPU的原子指令（如 </font>`<font style="color:#595959;">LOCK CMPXCHG</font>`<font style="color:#595959;"> 在x86上）。这些指令保证了在多处理器环境下，对内存的读-改-写操作是原子的。</font>
    - <font style="color:#595959;">C++11及以后：</font>`<font style="color:#595959;">std::atomic</font>`<font style="color:#595959;"> 模板类，提供了各种原子操作（</font>`<font style="color:#595959;">load</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">store</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">fetch_add</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">compare_exchange_weak</font>`<font style="color:#595959;"> 等）。</font>

##### <font style="color:black;">锁的底层原理（以互斥锁为例）</font>
<font style="color:#595959;">互斥锁的实现通常结合了硬件原子指令和操作系统调度机制。</font>

1. <font style="color:#595959;">用户态尝试获取锁（自旋）：</font>
    - <font style="color:#595959;">当一个线程尝试获取互斥锁时，它首先会在用户态使用CPU提供的原子指令（如 </font>`<font style="color:#595959;">XCHG</font>`<font style="color:#595959;">、</font>`<font style="color:#595959;">CMPXCHG</font>`<font style="color:#595959;">）来尝试修改锁的状态（例如，将一个标志位从0设置为1）。</font>
    - <font style="color:#595959;">如果成功，表示获取到锁，线程进入临界区。</font>
    - <font style="color:#595959;">如果失败（锁已被其他线程持有），线程会进入一个短时间的自旋循环，不断地尝试获取锁。自旋的目的是为了避免频繁的用户态/内核态切换，因为上下文切换的开销较大。如果锁很快被释放，自旋可以更快地获取到锁。</font>
2. <font style="color:#595959;">内核态等待（阻塞）：</font>
    - <font style="color:#595959;">如果自旋一段时间后仍然无法获取锁，线程会判断锁的竞争可能比较激烈，或者锁的持有时间可能较长。此时，线程会放弃CPU，通过系统调用进入内核态。</font>
    - <font style="color:#595959;">在内核态，操作系统会将该线程的状态设置为“等待”（Waiting/Blocked），并将其从CPU的运行队列中移除，放入互斥锁的等待队列中。</font>
    - <font style="color:#595959;">操作系统会进行上下文切换，调度其他可运行的线程执行。</font>
3. <font style="color:#595959;">锁的释放与唤醒：</font>
    - <font style="color:#595959;">当持有锁的线程完成临界区操作后，它会通过原子操作释放锁（例如，将标志位从1设置为0）。</font>
    - <font style="color:#595959;">如果互斥锁的等待队列中有线程在等待，释放锁的线程会通过系统调用通知操作系统。</font>
    - <font style="color:#595959;">操作系统会从等待队列中选择一个或多个线程（通常是第一个等待的线程），将其状态设置为“就绪”（Ready），并将其放入CPU的运行队列中，等待被调度执行。</font>
    - <font style="color:#595959;">被唤醒的线程会从之前阻塞的地方继续执行，重新尝试获取锁。</font>

<font style="color:#595959;">前面也总结了超150篇面经，可以直接访问专栏：</font>

<font style="color:rgb(89, 89, 89);">《</font>[<font style="color:rgb(89, 89, 89);">C++大厂面经专栏</font>](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzU5MTgxNzI5Nw==&action=getalbum&album_id=3104351725295288321&scene=173&subscene=&sessionid=svr_f62a4cf6b3a&enterid=1743181880&from_msgid=2247492118&from_itemidx=1&count=3&nolastread=1&scene=21#wechat_redirect)<font style="color:rgb(89, 89, 89);">》</font>

<font style="color:#595959;">或者</font>

<font style="color:#595959;">Github：https://github.com/aqjsp</font>

<font style="color:#595959;">也放在了码云上，国内网可以顺利访问学习：</font>

<font style="color:#595959;">Gitee：https://gitee.com/aqjsp</font>

<font style="color:#595959;">发现有问题的知识点大家直接留言就行了，欢迎大家指正哦~</font>

<font style="color:#595959;">如果你也对高性能内存池项目感兴趣，</font>**<font style="color:#595959;">先进群，然后私我。</font>**

![](/images/2eecb35f68213d29672b907925fd0bca.jpeg)

<font style="color:#595959;">给你全方位的解答。</font>

<font style="color:#595959;">往期精选：</font>

[<font style="color:#595959;">高性能C++内存池，一个能写在简历上的项目。核心代码5600+！这次玩真的！！！</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492389&idx=1&sn=451aaf6cd7ac5c89250c654f17bdc046&scene=21#wechat_redirect)

[<font style="color:#595959;">C++程序员必读：STL深度剖析与高效实战，告别“劝退”走向“真香”！！！</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492370&idx=1&sn=c5963be03c21cdb3ccd94ebdb860c011&scene=21#wechat_redirect)

[<font style="color:#595959;">C++多线程编程：从入门到精通，构建高性能并发应用，非常详细，强烈建议收藏！！！</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492334&idx=1&sn=de1ca3caacdb650401277d05cc2b2343&scene=21#wechat_redirect)

[<font style="color:#595959;">C++设计模式全解析，近十万字！代码解读！！超全分析，强烈建议收藏！！！</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492311&idx=1&sn=daff1e60799cfeaef3add27087f17702&scene=21#wechat_redirect)

[<font style="color:#595959;">C++智能指针全解析：从auto_ptr到weak_ptr，彻底掌握内存管理利器！强烈建议收藏！！！</font>](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492279&idx=1&sn=34a78a965e3b90d468edd66a404f3dc8&scene=21#wechat_redirect)

<font style="color:#595959;"></font>

> <font style="color:#595959;">来自: </font>[【秋招面经】字节--抖音C++开发面经，超详细解读，建议收藏~](https://mp.weixin.qq.com/s?__biz=MzU5MTgxNzI5Nw==&mid=2247492411&idx=1&sn=d7fb41a1ac9801f4fd01db18d189c96a&chksm=fe2b9c05c95c151380defcd92dc269de7b75e7516c36bf9402cc73dc3c65c2c8f91bee395f61&cur_album_id=3104351725295288321&scene=190#rd)
>





