---
title: Rust开发笔记 | LinkedList 双向链表
date: '2024-12-04 22:44:44'
updated: '2024-12-23 23:15:14'
---
<font style="color:rgb(53, 53, 53);">在Rust中，链表（LinkedList）是一种由节点组成的序列结构，每个节点包含一个值和一个指向下一个节点的指针或引用。链表主要用于实现更高级的数据结构，如栈、队列和关联数组。Rust的</font>`<font style="color:rgb(248, 57, 41);">std::collections</font>`<font style="color:rgb(53, 53, 53);">提供了一个</font>`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">结构，它是一个双向链表，在队首和队尾的插入和删除操作都是O(1)复杂度，但索引操作是O(n)复杂度。</font>

<font style="color:rgb(53, 53, 53);">在Rust中，</font>`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">是一种非常灵活的数据结构。它可以在不需要提前知道数据大小的情况下高效地进行插入和删除操作。我们先来看一个简单的例子，如何在Rust中创建一个</font>`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">：</font>

```rust
use std::collections::LinkedList;

fn main() {
    let mut list: LinkedList<i32> = LinkedList::new();
    list.push_back(1);
    list.push_back(2);
    list.push_back(3);

    for element in &list {
        println!("{}", element);
    }
}
```

<font style="color:rgb(53, 53, 53);">上面的例子中，我们创建了一个存储</font>`<font style="color:rgb(248, 57, 41);">i32</font>`<font style="color:rgb(53, 53, 53);">类型数据的链表，并向其中添加了三个元素。最后，我们迭代链表并打印每个元素的值。</font>

## **<font style="color:rgb(34, 34, 34);">LinkedList的基本操作</font>**
### **<font style="color:rgb(0, 0, 0);">插入操作</font>**
`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">提供了多种插入元素的方法，如</font>`<font style="color:rgb(248, 57, 41);">push_front</font>`<font style="color:rgb(53, 53, 53);">和</font>`<font style="color:rgb(248, 57, 41);">push_back</font>`<font style="color:rgb(53, 53, 53);">。我们可以分别在链表的头部和尾部插入元素。以下是相关的代码示例：</font>

```rust
use std::collections::LinkedList;

fn main() {
    let mut list: LinkedList<i32> = LinkedList::new();
    list.push_front(1); // 在头部插入
    list.push_back(2);  // 在尾部插入
    list.push_front(3); // 在头部插入

    for element in &list {
        println!("{}", element);
    }
}
```

<font style="color:rgb(53, 53, 53);">运行结果为：</font>

```rust
3
1
2
```

### **<font style="color:rgb(0, 0, 0);">删除操作</font>**
<font style="color:rgb(53, 53, 53);">类似地，</font>`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">也提供了从头部和尾部删除元素的方法：</font>`<font style="color:rgb(248, 57, 41);">pop_front</font>`<font style="color:rgb(53, 53, 53);">和</font>`<font style="color:rgb(248, 57, 41);">pop_back</font>`<font style="color:rgb(53, 53, 53);">。</font>

```rust
use std::collections::LinkedList;

fn main() {
    let mut list: LinkedList<i32> = LinkedList::new();
    list.push_back(1);
    list.push_back(2);
    list.push_back(3);

    list.pop_front(); // 删除头部元素
    list.pop_back();  // 删除尾部元素

    for element in &list {
        println!("{}", element);
    }
}
```

<font style="color:rgb(53, 53, 53);">运行结果为：</font>

```rust
2
```

## **<font style="color:rgb(34, 34, 34);">LinkedList的高级操作</font>**
### **<font style="color:rgb(0, 0, 0);">迭代器</font>**
<font style="color:rgb(53, 53, 53);">Rust中的</font>`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">提供了多种迭代器，允许我们以不同的方式遍历链表。除了常规的不可变迭代器外，还有可变迭代器和消费性迭代器。</font>

```rust
use std::collections::LinkedList;

fn main() {
    let mut list: LinkedList<i32> = LinkedList::new();
    list.push_back(1);
    list.push_back(2);
    list.push_back(3);

    // 不可变迭代器
    for element in &list {
        println!("{}", element);
    }

    // 可变迭代器
    for element in &mut list {
        *element += 1;
    }

    // 消费性迭代器
    for element in list.into_iter() {
        println!("{}", element);
    }
}
```

### **<font style="color:rgb(0, 0, 0);">索引操作</font>**
<font style="color:rgb(53, 53, 53);">尽管链表的索引操作是O(n)复杂度，但我们仍然可以通过遍历获取指定索引处的元素：</font>

```rust
use std::collections::LinkedList;

fn main() {
    let mut list: LinkedList<i32> = LinkedList::new();
    list.push_back(1);
    list.push_back(2);
    list.push_back(3);

    let index = 1;
    if let Some(value) = list.iter().nth(index) {
        println!("索引 {} 处的值是 {}", index, value);
    } else {
        println!("索引超出范围");
    }
}
```

### **<font style="color:rgb(0, 0, 0);">性能比较</font>**
<font style="color:rgb(53, 53, 53);">与其他数据结构相比，链表在某些操作上有其独特的优势。比如，在需要频繁插入和删除操作的场景中，链表的表现优于数组。然而，由于链表的随机访问性能不佳，在需要频繁索引操作的场合，数组可能是更好的选择。</font>

## **<font style="color:rgb(34, 34, 34);">手动实现LinkedList</font>**
<font style="color:rgb(53, 53, 53);">为了更好地理解链表的内部结构，我们可以尝试手动实现一个单向链表。通过这种方式，我们能更深入地了解链表的工作原理。</font>

### **<font style="color:rgb(0, 0, 0);">单向链表的实现</font>**
<font style="color:rgb(53, 53, 53);">首先，我们定义一个节点结构，每个节点包含一个值和一个指向下一个节点的可选指针：</font>

```rust
struct Node<T> {
    value: T,
    next: Option<Box<Node<T>>>,
}

struct LinkedList<T> {
    head: Option<Box<Node<T>>>,
}

impl<T> LinkedList<T> {
    fn new() -> Self {
        LinkedList { head: None }
    }

    fn push_front(&mut self, value: T) {
        let new_node = Box::new(Node {
            value,
            next: self.head.take(),
        });
        self.head = Some(new_node);
    }

    fn pop_front(&mut self) -> Option<T> {
        self.head.take().map(|node| {
            self.head = node.next;
            node.value
        })
    }
}
```

<font style="color:rgb(53, 53, 53);">在上述代码中，我们定义了一个泛型节点结构</font>`<font style="color:rgb(248, 57, 41);">Node</font>`<font style="color:rgb(53, 53, 53);">，并定义了链表</font>`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">。链表提供了两个基本操作：在头部插入元素和从头部删除元素。</font>

### **<font style="color:rgb(0, 0, 0);">使用手动实现的链表</font>**
<font style="color:rgb(53, 53, 53);">我们可以通过以下示例代码使用我们手动实现的链表：</font>

```rust
fn main() {
    let mut list = LinkedList::new();
    list.push_front(1);
    list.push_front(2);
    list.push_front(3);

    while let Some(value) = list.pop_front() {
        println!("{}", value);
    }
}
```

<font style="color:rgb(53, 53, 53);">运行结果为：</font>

```rust
3
2
1
```

### **<font style="color:rgb(0, 0, 0);">双向链表的实现</font>**
<font style="color:rgb(53, 53, 53);">相比单向链表，双向链表具有更多的灵活性，因为它允许我们在链表的两端进行操作。我们可以进一步扩展我们的链表实现，使其成为双向链表。</font>

```rust
struct DNode<T> {
    value: T,
    next: Option<Box<DNode<T>>>,
    prev: Option<*mut DNode<T>>,
}

struct DoublyLinkedList<T> {
    head: Option<Box<DNode<T>>>,
    tail: Option<*mut DNode<T>>,
}

impl<T> DoublyLinkedList<T> {
    fn new() -> Self {
        DoublyLinkedList { head: None, tail: None }
    }

    fn push_front(&mut self, value: T) {
        let mut new_node = Box::new(DNode {
            value,
            next: self.head.take(),
            prev: None,
        });

        let node_ptr: *mut _ = &mut *new_node;

        if let Some(ref mut head) = self.head {
            head.prev = Some(node_ptr);
        } else {
            self.tail = Some(node_ptr);
        }

        self.head = Some(new_node);
    }

    fn push_back(&mut self, value: T) {
        let mut new_node = Box::new(DNode {
            value,
            next: None,
            prev: self.tail,
        });

        let node_ptr: *mut _ = &mut *new_node;

        if let Some(tail) = self.tail {
            unsafe { (*tail).next = Some(new_node) };
        } else {
            self.head = Some(new_node);
        }

        self.tail = Some(node_ptr);
    }

    fn pop_front(&mut self) -> Option<T> {
        self.head.take().map(|mut node| {
            if let Some(next) = node.next.take() {
                self.head = Some(next);
                self.head.as_mut().unwrap().prev = None;
            } else {
                self.tail = None;
            }
            node.value
        })
    }

    fn pop_back(&mut self) -> Option<T> {
        self.tail.take().map(|tail| {
            let tail = unsafe { Box::from_raw(tail) };
            if let Some(prev) = tail.prev {
                self.tail = Some(prev);
                unsafe { (*prev).next = None };
            } else {
                self.head = None;
            }
            tail.value
        })
    }
}
```

### **<font style="color:rgb(0, 0, 0);">使用双向链表</font>**
<font style="color:rgb(53, 53, 53);">我们可以通过以下示例代码使用我们手动实现的双向链表：</font>

```rust
fn main() {
    let mut list = DoublyLinkedList::new();
    list.push_front(1);
    list.push_back(2);
    list.push_front(3);

    while let Some(value) = list.pop_front() {
        println!("{}", value);
    }
}
```

<font style="color:rgb(53, 53, 53);">运行结果为：</font>

```rust
3
1
2
```

## **<font style="color:rgb(34, 34, 34);">总结</font>**
<font style="color:rgb(53, 53, 53);">链表在数据结构中占有重要地位。通过本文的讲解，我们不仅了解了Rust标准库中的</font>`<font style="color:rgb(248, 57, 41);">LinkedList</font>`<font style="color:rgb(53, 53, 53);">，还手动实现了单向链表和双向链表。掌握这些知识，可以帮助我们在实际项目中选择和实现合适的数据结构。</font>

**<font style="color:rgb(0, 0, 0);">文章精选</font>**

[Rust开发笔记 | 系统编程的守护神](http://mp.weixin.qq.com/s?__biz=MzkyMjYyODg2MQ==&mid=2247483873&idx=1&sn=220ab0a0608ea988de84b34210c9184b&chksm=c1f02673f687af65b65fa91d5d6836e19dff10f6b76b34464890ae757db4532415f134b5dfa7&scene=21#wechat_redirect)

[Rust开发笔记 | IDE选择与Rust工具链配置指南](http://mp.weixin.qq.com/s?__biz=MzkyMjYyODg2MQ==&mid=2247483878&idx=1&sn=8c4818836b47e14b5a060b93b21def5f&chksm=c1f02674f687af623366966a72042980a103d3d0c2d23805cd3c0a852a3558cb2aba3a9c1019&scene=21#wechat_redirect)

[Rust开发笔记 | Rust的交互式Shell](http://mp.weixin.qq.com/s?__biz=MzkyMjYyODg2MQ==&mid=2247483891&idx=1&sn=c181a6467d2d83f27ad82d6745c5c350&chksm=c1f02661f687af770b8706f306e61cf6a72dc3d24c362feecebb0a76a0c4293b486e0aa6f667&scene=21#wechat_redirect)

[Rust开发笔记 | 所有权系统及其对内存管理的影响](http://mp.weixin.qq.com/s?__biz=MzkyMjYyODg2MQ==&mid=2247483900&idx=1&sn=66dd7a9ffe60f3e146b0cc28c0d75dc7&chksm=c1f0266ef687af78e7887e44c1292ae65332a79c86ffd880b463199350d49bf2db985f3eb53c&scene=21#wechat_redirect)

[Rust开发笔记 | 整数类型](http://mp.weixin.qq.com/s?__biz=MzkyMjYyODg2MQ==&mid=2247483911&idx=1&sn=a422422ad0e8887cb8a74078b49efb69&chksm=c1f02595f687ac834521ba8d8cf2fc853ccab975ff472b9ba7e125b4198e9767e5e9cdfcb8f2&scene=21#wechat_redirect)

**<font style="color:rgb(0, 0, 0);">点</font>****<font style="color:rgb(0, 0, 0);">击</font>****<font style="color:rgb(0, 0, 0);">关</font>****<font style="color:rgb(0, 0, 0);">注</font>****<font style="color:rgb(0, 0, 0);">并</font>****<font style="color:rgb(0, 0, 0);">扫</font>****<font style="color:rgb(0, 0, 0);">码</font>****<font style="color:rgb(0, 0, 0);">添</font>****<font style="color:rgb(0, 0, 0);">加</font>****<font style="color:rgb(0, 0, 0);">进</font>****<font style="color:rgb(0, 0, 0);">交</font>****<font style="color:rgb(0, 0, 0);">流</font>****<font style="color:rgb(0, 0, 0);">群</font>**

**<font style="color:rgb(0, 0, 0);">领</font>****<font style="color:rgb(0, 0, 0);">取</font>****<font style="color:rgb(0, 0, 0);">「Rust </font>****<font style="color:rgb(0, 0, 0);">语</font>****<font style="color:rgb(0, 0, 0);">言</font>****<font style="color:rgb(0, 0, 0);">」</font>****<font style="color:rgb(0, 0, 0);">学</font>****<font style="color:rgb(0, 0, 0);">习</font>****<font style="color:rgb(0, 0, 0);">资</font>****<font style="color:rgb(0, 0, 0);">料</font>**<font style="color:rgb(0, 0, 0);">  
</font>

![](/images/6cdeb9c28f3296d4814fea2bf50c6455.jpeg)  


> 来自: [Rust开发笔记 | LinkedList 双向链表](https://mp.weixin.qq.com/s/R4lInJDGOfRizarSsFPefg)
>

