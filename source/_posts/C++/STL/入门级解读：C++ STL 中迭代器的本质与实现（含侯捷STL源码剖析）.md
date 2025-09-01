---
title: 入门级解读：C++ STL 中迭代器的本质与实现（含侯捷STL源码剖析）
date: '2025-08-29 01:11:31'
updated: '2025-08-29 01:18:11'
---
## 前言
<font style="color:rgb(89, 89, 89);">很多 C++ 初学者一提 STL 头就大，尤其看到源码中复杂的 </font>`<font style="color:rgb(145, 109, 213);">__iterator_traits</font>`<font style="color:rgb(89, 89, 89);">、</font>`<font style="color:rgb(145, 109, 213);">advance</font>`<font style="color:rgb(89, 89, 89);">、</font>`<font style="color:rgb(145, 109, 213);">__distance</font>`<font style="color:rgb(89, 89, 89);"> 等等时，直接劝退。而在侯捷老师的《STL源码剖析》中，迭代器的讲解尤为精彩，深入浅出地剖析了“</font>**<font style="color:rgb(145, 109, 213);">迭代器本质就是一个聪明指针</font>**<font style="color:rgb(89, 89, 89);">”。</font>

<font style="color:rgb(89, 89, 89);">因为版权原因 b站已经下架，请按需下载。</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(254, 254, 242);">侯捷 C++ 视频 (百度网盘)</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(254, 254, 242);">网盘链接：</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(254, 254, 242);">链接: https://pan.baidu.com/s/1Lye100cXqCEBbwXhpMH-0A?pwd=6i1g</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(254, 254, 242);">提取码: 6i1g</font>

<font style="color:rgb(0, 0, 0);background-color:rgb(254, 254, 242);">包括：</font>

![](/images/625e44799d2a8d68b8610371448cd841.png)

---

## **<font style="color:rgb(89, 89, 89);">一、迭代器到底是什么？</font>**
<font style="color:rgb(89, 89, 89);">通俗讲：</font>**<font style="color:rgb(145, 109, 213);">迭代器是一个用来遍历容器的“泛型指针”</font>**<font style="color:rgb(89, 89, 89);">。</font>

<font style="color:rgb(89, 89, 89);">在 C 语言里我们操作数组靠指针，而 C++ STL 容器结构多样：</font>`<font style="color:rgb(145, 109, 213);">vector</font>`<font style="color:rgb(89, 89, 89);"> 是动态数组，</font>`<font style="color:rgb(145, 109, 213);">list</font>`<font style="color:rgb(89, 89, 89);"> 是双向链表，</font>`<font style="color:rgb(145, 109, 213);">map</font>`<font style="color:rgb(89, 89, 89);"> 是红黑树。那么算法如 </font>`<font style="color:rgb(145, 109, 213);">sort</font>`<font style="color:rgb(89, 89, 89);">、</font>`<font style="color:rgb(145, 109, 213);">copy</font>`<font style="color:rgb(89, 89, 89);"> 如何统一处理？——这就要靠迭代器。</font>

<font style="color:rgb(89, 89, 89);">迭代器本质上是一个封装了指针行为的对象，重载了：</font>

+ `<font style="color:rgb(89, 89, 89);">*p</font>`<font style="color:rgb(89, 89, 89);"> 访问元素</font>
+ `<font style="color:rgb(89, 89, 89);">++p</font>`<font style="color:rgb(89, 89, 89);"> / </font>`<font style="color:rgb(89, 89, 89);">--p</font>`<font style="color:rgb(89, 89, 89);"> 移动</font>
+ `<font style="color:rgb(89, 89, 89);">p == q</font>`<font style="color:rgb(89, 89, 89);"> 比较位置</font>

<font style="color:rgb(89, 89, 89);">侯捷老师总结：</font>**<font style="color:rgb(145, 109, 213);">指针是最原始的迭代器，而 STL 中的迭代器就是“行为像指针的类”</font>**<font style="color:rgb(89, 89, 89);">。</font>

---

## **<font style="color:rgb(89, 89, 89);">二、五种迭代器类型</font>**
<font style="color:rgb(89, 89, 89);">STL 根据操作能力，把迭代器分为五大类，每种都是前一种的超集。</font>

| **<font style="color:rgb(89, 89, 89);">类型</font>** | **<font style="color:rgb(89, 89, 89);">能力</font>** | **<font style="color:rgb(89, 89, 89);">举例容器</font>** |
| :--- | :--- | :--- |
| <font style="color:rgb(89, 89, 89);">InputIterator（输入）</font> | <font style="color:rgb(89, 89, 89);">只读，前移</font> | `<font style="color:rgb(89, 89, 89);">istream_iterator</font>` |
| <font style="color:rgb(89, 89, 89);">OutputIterator（输出）</font> | <font style="color:rgb(89, 89, 89);">只写，前移</font> | `<font style="color:rgb(89, 89, 89);">ostream_iterator</font>` |
| <font style="color:rgb(89, 89, 89);">ForwardIterator（前向）</font> | <font style="color:rgb(89, 89, 89);">可读写，前移</font> | `<font style="color:rgb(89, 89, 89);">forward_list</font>` |
| <font style="color:rgb(89, 89, 89);">BidirectionalIterator（双向）</font> | <font style="color:rgb(89, 89, 89);">可读写，前后移动</font> | `<font style="color:rgb(89, 89, 89);">list</font>`<br/><font style="color:rgb(89, 89, 89);">、</font>`<font style="color:rgb(89, 89, 89);">set</font>` |
| <font style="color:rgb(89, 89, 89);">RandomAccessIterator（随机访问）</font> | <font style="color:rgb(89, 89, 89);">可读写，可跳跃</font> | `<font style="color:rgb(89, 89, 89);">vector</font>`<br/><font style="color:rgb(89, 89, 89);">、</font>`<font style="color:rgb(89, 89, 89);">deque</font>` |


<font style="color:rgb(89, 89, 89);">你可以理解为五个“段位”：</font>

```cpp
Input < Forward < Bidirectional < RandomAccess
```

<font style="color:rgb(89, 89, 89);">侯捷提到：STL 的设计哲学是“最小需求设计”，算法根据最弱要求选择最小类型，然后在运行时通过模板 + traits 实现“能力分发”。</font>

---

## **<font style="color:rgb(89, 89, 89);">三、Traits 技术：类型萃取的魔法</font>**
<font style="color:rgb(89, 89, 89);">当我们写一个泛型算法时，并不知道传入的迭代器是啥类型。这时就靠 </font>`<font style="color:rgb(145, 109, 213);">iterator_traits</font>`<font style="color:rgb(89, 89, 89);"> 这个结构体提取类型信息：</font>

```cpp
template<typename Iterator>
struct iterator_traits {
    using iterator_category = typename Iterator::iterator_category;
    using value_type = typename Iterator::value_type;
    using difference_type = typename Iterator::difference_type;
    ...
};
```

<font style="color:rgb(89, 89, 89);">这样就能在算法中写：</font>

```cpp
typename iterator_traits<Iter>::iterator_category()
```

<font style="color:rgb(89, 89, 89);">来获得标签（如 </font>`<font style="color:rgb(145, 109, 213);">random_access_iterator_tag</font>`<font style="color:rgb(89, 89, 89);">），再通过函数重载进行调度。</font>

<font style="color:rgb(89, 89, 89);">👉</font><font style="color:rgb(89, 89, 89);"> 侯捷称其为 “类型的标签分发器”。</font>

---

## **<font style="color:rgb(89, 89, 89);">四、标签分发：用“空类”实现重载策略</font>**
<font style="color:rgb(89, 89, 89);">在 </font>`<font style="color:rgb(145, 109, 213);">advance</font>`<font style="color:rgb(89, 89, 89);"> 和 </font>`<font style="color:rgb(145, 109, 213);">distance</font>`<font style="color:rgb(89, 89, 89);"> 函数中，STL 利用标签分发实现了“为不同迭代器走不同逻辑”的技巧：</font>

### **<font style="color:rgb(89, 89, 89);">例：advance() 实现</font>**
```cpp
template <typename InputIterator, typename Distance>
void advance(InputIterator& it, Distance n) {
    _advance(it, n, iterator_traits<InputIterator>::iterator_category());
}
void _advance(InputIterator& it, Distance n, input_iterator_tag) {
    while (n--) ++it;
}
void _advance(BidirectionalIterator& it, Distance n, bidirectional_iterator_tag) {
    if (n >= 0) while (n--) ++it;
    else while (n++) --it;
}
void _advance(RandomAccessIterator& it, Distance n, random_access_iterator_tag) {
    it += n;
}
```

<font style="color:rgb(89, 89, 89);">这样就能根据 </font>`<font style="color:rgb(145, 109, 213);">it</font>`<font style="color:rgb(89, 89, 89);"> 的类型自动调度最高效版本，而不需要用户干预。你不写 </font>`<font style="color:rgb(145, 109, 213);">if</font>`<font style="color:rgb(89, 89, 89);">，编译器帮你选最优路径。</font>

<font style="color:rgb(89, 89, 89);">这就是</font>**<font style="color:rgb(145, 109, 213);">模板 + 类型标签</font>**<font style="color:rgb(89, 89, 89);">的魅力，侯捷称其为 STL 最经典技巧之一。</font>

---

## **<font style="color:rgb(89, 89, 89);">五、常用适配器介绍（配合容器更强大）</font>**
<font style="color:rgb(89, 89, 89);">STL 提供了很多“迭代器适配器”，用于将普通容器包装为“具有特殊行为”的对象。</font>

### **<font style="color:rgb(89, 89, 89);">1）reverse_iterator：反向迭代器</font>**
<font style="color:rgb(89, 89, 89);">让你从容器尾部向前遍历。</font>

```cpp
vector<int> v = {1,2,3,4};
for (auto rit = v.rbegin(); rit != v.rend(); ++rit)
    cout << *rit; // 输出 4 3 2 1
```

<font style="color:rgb(89, 89, 89);">它的 </font>`<font style="color:rgb(145, 109, 213);">base()</font>`<font style="color:rgb(89, 89, 89);"> 实际指向正向迭代器的“后一位”，所以 </font>`<font style="color:rgb(145, 109, 213);">*rit</font>`<font style="color:rgb(89, 89, 89);"> 实际是 </font>`<font style="color:rgb(145, 109, 213);">*(base() - 1)</font>`<font style="color:rgb(89, 89, 89);">。</font>

### **<font style="color:rgb(89, 89, 89);">2）back_insert_iterator：尾部插入器</font>**
<font style="color:rgb(89, 89, 89);">把赋值操作转成容器的 </font>`<font style="color:rgb(145, 109, 213);">push_back</font>`<font style="color:rgb(89, 89, 89);">：</font>

```cpp
vector<int> v;
auto it = back_inserter(v);
*it = 1;  // 等价于 v.push_back(1);
```

<font style="color:rgb(89, 89, 89);">适合配合 </font>`<font style="color:rgb(145, 109, 213);">copy</font>`<font style="color:rgb(89, 89, 89);"> 使用：</font>

```cpp
copy(src.begin(), src.end(), back_inserter(dest));
```

### **<font style="color:rgb(89, 89, 89);">3）insert_iterator：中间插入器</font>**
<font style="color:rgb(89, 89, 89);">让插入发生在指定位置上：</font>

```cpp
insert_iterator it(v, v.begin() + 3);
*it = 99;  // 插入在第4位前
```

---

## **<font style="color:rgb(89, 89, 89);">六、源码中 __normal_iterator 的作用</font>**
<font style="color:rgb(89, 89, 89);">侯捷特别分析了 </font>`<font style="color:rgb(145, 109, 213);">__normal_iterator</font>`<font style="color:rgb(89, 89, 89);">：STL 的 vector、string 的迭代器其实就是对原生指针加壳。</font>

```cpp
template<typename T>
class __normal_iterator {
    T* ptr;
public:
    __normal_iterator& operator++() { ++ptr; return *this; }
    reference operator*() const { return *ptr; }
    ...
};
```

<font style="color:rgb(89, 89, 89);">为什么不直接用裸指针？为了统一接口（traits支持、自定义功能等），让 </font>`<font style="color:rgb(145, 109, 213);">vector<int>::iterator</font>`<font style="color:rgb(89, 89, 89);"> 看起来也是一个类，而不仅仅是 </font>`<font style="color:rgb(145, 109, 213);">int*</font>`<font style="color:rgb(89, 89, 89);">。</font>

---

## **<font style="color:rgb(89, 89, 89);">七、侯捷源码剖析特色总结</font>**
<font style="color:rgb(89, 89, 89);">侯捷老师在书中对迭代器实现做了大量细节还原，比如：</font>

+ <font style="color:rgb(89, 89, 89);">自己写 </font>`<font style="color:rgb(89, 89, 89);">__type_traits</font>`<font style="color:rgb(89, 89, 89);"> 结构体</font>
+ <font style="color:rgb(89, 89, 89);">讲解 </font>`<font style="color:rgb(89, 89, 89);">distance_type()</font>`<font style="color:rgb(89, 89, 89);">、</font>`<font style="color:rgb(89, 89, 89);">value_type()</font>`<font style="color:rgb(89, 89, 89);"> 这些函数如何萃取类型</font>
+ <font style="color:rgb(89, 89, 89);">反复强调 </font>**<font style="color:rgb(145, 109, 213);">template + traits + tag dispatching</font>**<font style="color:rgb(89, 89, 89);"> 是 STL 三大法宝</font>

<font style="color:rgb(89, 89, 89);">这些思想你在看完这篇文章后再读《STL 源码剖析》会更有感觉。</font>

---

## **<font style="color:rgb(89, 89, 89);">八、结语：学习迭代器的意义</font>**
<font style="color:rgb(89, 89, 89);">理解 STL 迭代器，不只是为了写 </font>`<font style="color:rgb(145, 109, 213);">for (auto it = ...)</font>`<font style="color:rgb(89, 89, 89);"> 而已，更重要的是：</font>

1. <font style="color:rgb(89, 89, 89);">看懂源码 / 编写泛型库</font>
2. <font style="color:rgb(89, 89, 89);">掌握 STL 高性能设计技巧</font>
3. <font style="color:rgb(89, 89, 89);">为自己写容器打基础</font>
4. <font style="color:rgb(89, 89, 89);">为面试算法题打通容器适配技巧</font>

<font style="color:rgb(89, 89, 89);">就像侯捷说的：“STL 是 C++ 的皇冠，迭代器是皇冠上的明珠。” 把这颗明珠理解透，你就打开了 C++ 泛型编程的大门。</font>

---

<font style="color:rgb(89, 89, 89);">如果你对源码细节感兴趣，推荐搭配阅读：</font>

+ <font style="color:rgb(89, 89, 89);">《STL源码剖析》侯捷著</font>
+ <font style="color:rgb(89, 89, 89);">C++标准库实现源码（libstdc++、MSVC STL）</font>

> <font style="color:rgb(89, 89, 89);">来自: </font>[入门级解读：C++ STL 中迭代器的本质与实现（含侯捷STL源码剖析）](https://mp.weixin.qq.com/s?src=11&timestamp=1756393217&ver=6202&signature=KFFYMvZoBAmASEvIazuku7jqXtPTwDNh4cwvB0WNu1qr1wJ1Ag8R*Q7gI*-WC9O0l5gYTcofdCmdPhbilzK4rLP-3arwrMDuYZp32LTp0yX-NfYY-6p2jejtrlg25Q9O&new=1)
>





