---
title: 深入理解Rust闭包三大特征
date: '2024-12-04 22:15:35'
updated: '2024-12-23 23:07:53'
---
原创 作者:瀛洲在线编程之道 公号:黑客编程之道 发布时间:2024-10-31 16:52 发表于吉林

原文地址：[深入理解Rust闭包三大特征：从实战到原理，让你彻底搞懂Fn、FnMut、FnOnce！](https://mp.weixin.qq.com/s/-l00_EvV6v9pQrsr-QRAHQ)

在 Rust 中，闭包是一个强大而灵活的特性，而理解 Fn、FnMut、FnOnce 这三个特征对于掌握闭包至关重要。今天，让我们深入了解这三个特征的方方面面。

前面一篇文章[震惊！Rust大神都在用的闭包技巧，让你的代码瞬间升级](http://mp.weixin.qq.com/s?__biz=MzI0NDYwOTU4NQ==&mid=2247484306&idx=1&sn=424b0d400ad534fdda34a47085f19350&chksm=e95a64ccde2dedda2a2d56dfe68bd0c688707ac574bcfeb3930b7f41e64b1fd01e7809bf7675&scene=21#wechat_redirect)介绍了闭包，但是介绍的不够完善，内容不够全面，所以本篇文章作为补充。  

### <font style="color:rgb(255, 104, 39);">🔍</font><font style="color:rgb(255, 104, 39);"> 什么是闭包？</font>
<font style="color:rgba(0, 0, 0, 0.9);">闭包可以看作是一个匿名函数，但它比普通函数更强大，因为它能够捕获其定义时所在环境中的变量。从本质上讲，闭包是将函数和其环境的状态绑定在一起的对象。</font>

<font style="color:rgba(0, 0, 0, 0.9);">示例：</font>

<font style="color:rgb(51, 51, 51);">let x = 10;let add = |y| x + y;  // 闭包捕获了环境中的xprintln!("结果: {}", add(5));  // 输出：15</font>

### <font style="color:rgb(255, 104, 39);">🎯</font><font style="color:rgb(255, 104, 39);"> 三大特征深度解析</font>
#### <font style="color:rgb(255, 104, 39);">Fn特征</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">通过不可变引用（&T）捕获环境变量</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">可以被重复调用</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">不会改变环境中的值</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">最严格的约束</font>

#### <font style="color:rgb(255, 104, 39);">FnMut特征</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">通过可变引用（&mut T）捕获环境变量</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">可以被重复调用</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">可以修改环境中的值</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">中等严格的约束</font>

#### <font style="color:rgb(255, 104, 39);">FnOnce特征</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">通过所有权（T）捕获环境变量</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">只能被调用一次</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">消耗掉捕获的值</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">最宽松的约束</font>

### <font style="color:rgb(255, 104, 39);">🎯</font><font style="color:rgb(255, 104, 39);"> 特征之间的关系</font>
```rust
FnOnce ← FnMut ← Fn
```

+ <font style="color:rgba(0, 0, 0, 0.9);">Fn实现了FnMut和FnOnce</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">FnMut实现了FnOnce</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">这是一个继承关系，越往右要求越严格</font>

## <font style="color:rgb(255, 104, 39);">💻</font><font style="color:rgb(255, 104, 39);"> 实战示例详解</font>
### <font style="color:rgb(255, 104, 39);">1. 简单计数器（FnMut示例）</font>
```rust
fn make_counter() -> impl FnMut() -> i32 {
    let mut count = 0;
    move || {
        count += 1;
        count
    }
}


fn main() {
    let mut counter = make_counter();
    println!("第一次调用: {}", counter()); // 1
    println!("第二次调用: {}", counter()); // 2
    println!("第三次调用: {}", counter()); // 3
}
```

解释：

+ 这是一个典型的FnMut示例
+ 闭包通过可变引用捕获count
+ 每次调用都修改count的值
+ 使用move关键字获取count的所有权

### <font style="color:rgb(255, 104, 39);">2. 事件监听器（Fn示例）</font>
```rust
struct EventListener<F>
    where
        F: Fn(i32),
{
    callback: F,
}


impl<F> EventListener<F>
    where
        F: Fn(i32),
{
    fn new(callback: F) -> Self {
        EventListener { callback }
    }


    fn notify(&self, value: i32) {
        (self.callback)(value);
    }
}


fn main() {
    let data = vec![1, 2, 3];


    let listener = EventListener::new(|x| {
        println!("接收到事件: {}", x);
        // 这里可以读取data，但不能修改它
        println!("当前数据: {:?}", data);
    });


    listener.notify(42);
    listener.notify(100);
    // data仍然可用
    println!("原始数据: {:?}", data);
}
```

解释：

+ 使用Fn特征因为回调需要多次调用
+ 回调函数只需要读取环境变量
+ 不需要修改任何捕获的值



### <font style="color:rgb(255, 104, 39);">3. 资源清理（FnOnce示例）</font>
```rust
struct Resource {
    data: String,
}


impl Resource {
    fn new(data: &str) -> Self {
        Resource {
            data: data.to_string(),
        }
    }
}


fn process_resource<F>(resource: Resource, cleanup: F)
    where
        F: FnOnce(Resource),
{
    // 使用资源
    println!("使用资源: {}", resource.data);
    // 调用清理函数
    cleanup(resource);
}


fn main() {
    let resource = Resource::new("重要数据");


    process_resource(resource, |r| {
        println!("清理资源: {}", r.data);
        // 资源在这里被消耗
    });


    // 这里不能再使用resource
}
```

解释：

+ 使用FnOnce因为cleanup只需要执行一次
+ cleanup获取resource的所有权
+ 资源在cleanup执行后被释放



### <font style="color:rgb(255, 104, 39);">4. 数据转换管道（Fn示例）</font>
```rust
struct Pipeline<T, U> {
    transform: Box<dyn Fn(T) -> U>,
}


impl<T, U> Pipeline<T, U> {
    fn new<F>(transform: F) -> Self
        where
            F: Fn(T) -> U + 'static,
    {
        Pipeline {
            transform: Box::new(transform),
        }
    }


    fn process(&self, input: T) -> U {
        (self.transform)(input)
    }
}


fn main() {
    // 创建一个字符串处理管道
    let string_pipeline = Pipeline::new(|s: String| {
        let uppercase = s.to_uppercase();
        format!("处理后的数据: {}", uppercase)
    });


    let result = string_pipeline.process("hello world".to_string());
    println!("{}", result);  // 输出：处理后的数据: HELLO WORLD
}
```

解释：

+ 使用Fn因为转换函数需要被重复调用
+ 转换函数不需要修改环境
+ 可以多次处理不同的输入

### <font style="color:rgb(255, 104, 39);">5. 状态更新器（FnMut示例）</font>
```rust
#[derive(Debug)]
struct GameState {
    score: i32,
    level: i32,
}


struct StateUpdater {
    updates: Vec<Box<dyn FnMut(&mut GameState)>>,
}


impl StateUpdater {
    fn new() -> Self {
        StateUpdater {
            updates: Vec::new(),
        }
    }


    fn add_update<F>(&mut self, update: F)
        where
            F: FnMut(&mut GameState) + 'static,
    {
        self.updates.push(Box::new(update));
    }


    fn update_state(&mut self, state: &mut GameState) {
        for update in &mut self.updates {
            update(state);
        }
    }
}


fn main() {
    let mut state = GameState {
        score: 0,
        level: 1,
    };


    let mut updater = StateUpdater::new();


    // 添加分数更新逻辑
    updater.add_update(|state| {
        state.score += 100;
    });


    // 添加等级更新逻辑
    updater.add_update(|state| {
        if state.score >= 100 {
            state.level += 1;
        }
    });


    println!("更新前: {:?}", state);
    updater.update_state(&mut state);
    println!("更新后: {:?}", state);
}
```

解释：

+ 使用FnMut因为更新函数需要修改状态
+ 每个更新函数都可以多次调用
+ 状态可以被多个更新函数修改



### <font style="color:rgb(255, 104, 39);">6. 配置构建器（FnOnce示例）</font>
```rust
#[derive(Debug, Default)]
struct Config {
    host: String,
    port: u16,
    max_connections: u32,
}


struct ConfigBuilder {
    config: Config,
    finalizers: Vec<Box<dyn FnOnce(&mut Config)>>,
}


impl ConfigBuilder {
    fn new() -> Self {
        ConfigBuilder {
            config: Config::default(),
            finalizers: Vec::new(),
        }
    }


    fn with_host(mut self, host: String) -> Self {
        self.config.host = host;
        self
    }


    fn with_port(mut self, port: u16) -> Self {
        self.config.port = port;
        self
    }


    fn add_finalizer<F>(&mut self, finalizer: F)
        where
            F: FnOnce(&mut Config) + 'static,
    {
        self.finalizers.push(Box::new(finalizer));
    }


    fn build(mut self) -> Config {
        for finalizer in self.finalizers {
            finalizer(&mut self.config);
        }
        self.config
    }
}


fn main() {
    let mut builder = ConfigBuilder::new()
        .with_host("localhost".to_string())
        .with_port(8080);


    builder.add_finalizer(|config| {
        if config.max_connections == 0 {
            config.max_connections = 100;
        }
    });


    let config = builder.build();
    println!("最终配置: {:?}", config);
}
```

解释：

+ 使用FnOnce因为每个终结器只需要运行一次
+ 终结器在build时消耗掉
+ 提供了灵活的配置方式



## <font style="color:rgb(255, 104, 39);">存在的意义</font>
1. <font style="color:rgb(255, 104, 39);">安全性保证</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">编译时检查捕获变量的使用方式</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">防止数据竞争</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">保证内存安全</font>
2. <font style="color:rgb(255, 104, 39);">灵活性</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">支持函数式编程范式</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">提供多样的抽象能力</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">方便代码复用</font>
3. <font style="color:rgb(255, 104, 39);">性能优化</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">零成本抽象</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">编译时优化</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">最小运行时开销</font>

## <font style="color:rgb(255, 104, 39);">🔮</font><font style="color:rgb(255, 104, 39);"> 使用建议</font>
1. <font style="color:rgba(0, 0, 0, 0.9);">选择最严格的特征</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">能用Fn就不用FnMut</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">能用FnMut就不用FnOnce</font>
2. <font style="color:rgba(0, 0, 0, 0.9);">注意生命周期</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">考虑闭包和捕获变量的生命周期</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">合理使用move关键字</font>
3. <font style="color:rgba(0, 0, 0, 0.9);">性能考虑</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">避免不必要的Box</font>
    - <font style="color:rgba(0, 0, 0, 0.9);">合理使用引用</font>

## <font style="color:rgb(255, 104, 39);">🎁</font><font style="color:rgb(255, 104, 39);"> 总结</font>
<font style="color:rgba(0, 0, 0, 0.9);">Rust的闭包系统通过Fn、FnMut、FnOnce三个特征，提供了：</font>

+ <font style="color:rgba(0, 0, 0, 0.9);">精确的控制能力</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">出色的安全性</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">优秀的性能</font>
+ <font style="color:rgba(0, 0, 0, 0.9);">优雅的抽象</font>

<font style="color:rgba(0, 0, 0, 0.9);">掌握这些特征，将帮助你写出更好的Rust代码！</font>



## 
