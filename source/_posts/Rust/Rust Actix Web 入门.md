---
title: Rust Actix Web 入门
date: '2024-12-25 22:52:31'
updated: '2024-12-25 23:09:01'
---
![](/images/2bd670c9f86bc9f582d074b989cdcb61.png)

目前，Actix Web 仍然是 Rust Web 后端生态系统中极其强大的竞争对手。尽管之前的事件可能对其产生影响，但它仍然很强大，并且是 Rust 中最受推荐的 [Web 框架](https://so.csdn.net/so/search?q=Web%20%E6%A1%86%E6%9E%B6&spm=1001.2101.3001.7020)之一。最初基于同名的 actor 框架 ( `actix` )，后来它已经消失，并且 `actix` 现在仅用于 websocket 端点。

本文将主要讨论 v4.4。

### Actix Web 入门
首先，使用 `cargo init example-api` 生成项目，将 `cd` 到该文件夹中，然后使用以下命令将 `actix-web` crate 添加到项目中:

```bash
cargo add actix-web
```

现在已经准备好开始了！如果想复制样板文件以直接编写您的应用程序，如下所示：

```rust
use actix_web::{web, App, HttpServer, Responder};

#[get("/")]
async fn index() -> impl Responder {
    "Hello world!"
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new().service(
            // prefixes all resources and routes attached to it...
            web::scope("/")
            // ...so this handles requests for `GET /app/index.html`
            .route("/", web::get().to(index)),
        )
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
```

### Actix Web 中的路由
使用 Actix Web 时，大多数返回 实现`actix_web::Responder` trait 的函数都可以用作路由。请参阅下面的基本 Hello World 示例：

```rust
#[get("/")]
async fn index() -> impl Responder {
    "Hello world!"
}
```

然后可以将此处理函数输入到 `actix_web::App` 中，然后将其作为参数传递给 `HttpServer` ：

```rust
#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new().service(
            // prefixes all resources and routes attached to it...
            web::scope("/")
            // ...so this handles requests for the base route
            .route("/index.html", web::get().to(index)),
        )
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
```

现在，每当您访问 `/index.html` 时，它都应该返回“Hello world!”。但是，如果您想创建多个 子路由 类型，然后最后将它们全部合并到应用程序中，您可能会发现这种方法有点缺乏。在这种情况下，您将需要 `ServiceConfig` 类型，您也可以这样写：

```rust
use actix_web::{web, App, HttpResponse};

// this function could be located in different module
fn config(cfg: &mut web::ServiceConfig) {
    cfg.service(web::resource("/test")
                .route(web::get().to(|| HttpResponse::Ok()))
                .route(web::head().to(|| HttpResponse::MethodNotAllowed()))
               );
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new().configure(config)
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
```

Actix Web 中的提取器（Extractor）正是这样的：实现类型安全的请求，当传递到处理函数时，将尝试从处理函数的请求中提取相关数据。例如， `actix_web::web::Json` 提取器将尝试从请求正文中提取 JSON。然而，要成功反序列化 JSON，您需要使用 `serde` 包 - 最好与 `derive` 函数一起使用，为您的结构添加自动反序列化和序列化派生宏。您可以通过执行以下命令来安装 serde：

```rust
cargo add serde -F derive
```

现在可以将其用作派生宏，如下所示：

```rust
use actix_web::web;
use serde::Deserialize;

#[derive(Deserialize)]
struct Info {
    username: String,
}

// deserialize `Info` from request's body
#[post("/submit")]
async fn submit(info: web::Json<Info>) -> String {
    format!("Welcome {}!", info.username)
}
```

Actix Web 还支持 paths, queries and forms。您还需要在此处使用 `serde` - 尽管对于paths，您还需要使用 Actix Web 路由宏来声明路径参数的确切位置。我们可以在下面找到所有这 3 种方法的示例：

```rust
#[derive(Deserialize)]
struct Info {
    username: String,
}

// extract path info using serde
#[get("/users/{username}")] // <- define path parameters
async fn index(info: web::Path<Info>) -> String {
    format!("Welcome {}!", info.username)
}

// data is passed in here through query parameters in the URL
// for example, google.com/?hello=world
#[get("/")]
async fn index(info: web::Query<Info>) -> String {
    format!("Welcome {}!", info.username)
}

// data is passed into the Form extractor through a HTML Form element
#[post("/")]
async fn index(form: web::Form<Info>) -> actix_web::Result<String> {
    Ok(format!("Welcome {}!", form.username))
}
```

有兴趣编写自己的提取器吗？你可以做到的！编写自己的提取器只需要实现 `FromRequest` 特征。查看 HTTP 标头提取器的这段代码，它向您展示了它的具体工作原理：

```rust
use actix_web::dev::Payload;
use actix_web::{FromRequest,
                http::header::Header as ParseHeader,
                HttpRequest, error::ParseError
               };

#[derive(Clone, PartialEq, Eq, PartialOrd, Ord, Debug)]
pub struct Header<T>(pub T);

impl<T> FromRequest for Header<T>
    where
    T: ParseHeader,
    {
        type Error = ParseError;
        type Future = Ready<Result<Self, Self::Error>>;

        #[inline]
        fn from_request(req: &HttpRequest, _: &mut Payload) -> Self::Future {
            match T::parse(req) {
                Ok(header) => ok(Header(header)),
                Err(e) => err(e),
            }
        }
    }
```

请注意， `T: ParseHeader` trait 约束 特定于此特征实现，因为为了使header有效，它需要能够成功解析为header，并在实现 `actix_web::error::Error` 。虽然我们也有 `extract` 作为提供的方法，但 `from_request` 是这里唯一需要实现的方法，它返回 `Self::Future` 。也就是说，我们需要返回一个 已准备好等待的结果（ready to be awaited） - 您可以在此处找到有关 Ready 结构的更多信息。其他提取器（例如 JSON 提取器）也允许您更改其配置 - 您可以在此文档页面中找到有关它的更多信息。

一般来说，responses只需要实现 `actix_web::Responder` trait就能够响应。尽管在实际响应类型方面已经有广泛的实现，因此您通常不需要实现自己的类型，但在某些特定的用例中这可能会很有帮助；例如，能够记录您的应用程序可能具有的所有类型的响应。

### 连接到数据库
通常，在设置数据库时，您可能需要设置数据库连接：

```rust
use sqlx::PgPoolOptions;

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    let dbconnection = PgPoolOptions::new()
        .max_connections(5)
        .connect(r#"<db-connection-string-here>"#).await;

    //... rest of your code
}
```

然后，您需要配置自己的 Postgres 实例，无论是本地安装在计算机上、通过 Docker 还是其他方式配置。但是，使用 Shuttle，我们可以消除这种情况，因为运行时会为您配置数据库：

```rust
use actix_web::{get, web::ServiceConfig};
use shuttle_actix_web::ShuttleActixWeb;

#[shuttle_runtime::main]
async fn actixweb(
    #[shuttle_shared_db::Postgres] pool: PgPool,
) -> ShuttleActixWeb<impl FnOnce(&mut ServiceConfig) + Send + Clone + 'static> {
    let state = AppState { pool };

    // .. the rest of your code
}
```

在本地，这是通过 Docker 完成的，但在部署中，有一个总体流程可以为您完成此操作！不需要额外的工作。我们还有一个 AWS RDS 数据库产品，需要零 AWS 知识才能设置 - 请访问此处了解更多信息。

### Actix Web 中的应用程序状态
路由 非常棒（连接数据库也非常容易！），但是当您需要 存储变量时，您可能想要寻找可以在应用程序中存储和使用它们的东西。这就是共享可变状态的用武之地：您在跨整个应用程序构建服务时声明它，然后您可以将它用作处理程序函数中的提取器。例如，您可能需要共享数据库池、计数器或 Websocket 订阅者的共享hashmap。您可以像这样声明和使用状态：

```rust
use sqlx::PgPool;

#[derive(Clone)]
struct AppState {
    db: PgPool
}

#[get("/")]
async fn index(data: web::Data<AppState>) -> String {
    let res = sqlx::query("SELECT 'Hello World!'").fetch_all(&data.db).await.unwrap();

    format!("{res}")
}
```

然后你可以像这样实现它：

```rust
#[actix_web::main]
async fn main() -> std::io::Result<()> {
    let db = connect_to_db();
    let state = web::Data::new(AppState { db });

    HttpServer::new(move || {
        // move app state into the closure
        App::new()
        .app_data(state.clone()) // <- register the created data
        .route("/", web::get().to(index))
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
```

### Actix Web 中的中间件
在 Actix Web 中，中间件用作能够向（一组）路由添加通用功能的介入层，方法是在处理程序函数运行之前获取请求，执行一些操作，运行实际的处理程序函数本身，然后中间件进行额外的处理（如果需要）。默认情况下，Actix Web 有几个我们可以使用的默认中间件，包括日志记录、路径规范化、访问外部服务和修改应用程序状态（通过 `ServiceRequest` 类型）。

请参阅下面的示例，了解如何实现默认 Logger 中间件：

```rust
use actix_web::{middleware::Logger, App};

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    // access logs are printed with the INFO level so ensure it is enabled by default
    env_logger::init_from_env(env_logger::Env::new().default_filter_or("info"));

    let app = App::new()
        .wrap(Logger::default());

    // ... rest of your code
}
```

此外，您还可以在 Actix Web 中编写自己的中间件！对于许多用例，我们可以使用 crate `actix-web-lab` 中方便的 `middleware::from_fn` 帮助器（在即将发布的版本中将提升为 `actix-web` 本身）。例如，在请求处理流程的不同部分打印消息，如下所示：

```rust
use actix_web::{body::MessageBody, dev::{ServiceRequest, ServiceResponse}};
use actix_web_lab::middleware::{from_fn, Next};

async fn print_before_and_after_handler(
    req: ServiceRequest,
    next: Next<impl MessageBody>,
) -> Result<ServiceResponse<impl MessageBody>, Error> {
    println!("Hi from start. You requested: {}", req.path());
    let res = next.call(req).await?;
    println!("Hi from response");
    Ok(res)
}

let app = App::new()
    .wrap(from_fn(print_before_and_after_handler))
    .route(
        "/",
        web::get().to(|| async { "Hello from handler!" }),
    );
```

为了能够编写更复杂的中间件，我们实际上需要实现两个traits - `Service<Req>` 用于实现实际的中间件本身以及 `Transform<S, Req>` 这是构建器所需的处理请求的实际服务（就我们构建服务而言，我们实际上将使用构建器，而不是中间件！外部服务将在检测到 HTTP 请求时自动调用中间件）。

对于实际的中间件实现，让我们看一下编写一个仅打印消息的中间件。我们可以通过实现 `Transform` 特征来创建中间件的构建器：

```rust
use std::{future::{ready, Ready, Future}, pin::Pin};

use actix_web::{
    dev::{forward_ready, Service, ServiceRequest, ServiceResponse, Transform},
    web, Error,
};

pub struct SayHi;

// `S` - type of the next service
// `B` - type of response's body
impl<S, B> Transform<S, ServiceRequest> for SayHi
    where
    S: Service<ServiceRequest, Response = ServiceResponse<B>, Error = Error>,
    S::Future: 'static,
    B: 'static,
    {
        // setting up the types for the middleware to work
        type Response = ServiceResponse<B>;
        type Error = Error;
        type InitError = ();
        type Transform = SayHiMiddleware<S>;
        type Future = Ready<Result<Self::Transform, Self::InitError>>;

        // this immediately returns the middleware
        fn new_transform(&self, service: S) -> Self::Future {
            ready(Ok(SayHiMiddleware { service }))
        }
    }
```

现在我们可以编写中间件本身了！在内部，中间件必须实现一个泛型类型 - 然后在 `Service` 特征中声明。请注意，我们手动重新实现了 `futures_util` 中名为 `LocalBoxFuture` 的类型 - 也就是说，未来不需要 `Send` 特征并且是安全的使用它是因为它在解除引用时实现了 `Unpin` ，这会自动取消任何先前的线程安全保证。

```rust
pub struct SayHiMiddleware<S> {
    /// The next service to call
    service: S,
}

// This future doesn't have the requirement of being `Send`.
// See: futures_util::future::LocalBoxFuture
type LocalBoxFuture<T> = Pin<Box<dyn Future<Output = T> + 'static>>;

// `S`: type of the wrapped service
// `B`: type of the body - try to be generic over the body where possible
impl<S, B> Service<ServiceRequest> for SayHiMiddleware<S>
    where
    S: Service<ServiceRequest, Response = ServiceResponse<B>, Error = Error>,
    S::Future: 'static,
    B: 'static,
    {
        type Response = ServiceResponse<B>;
        type Error = Error;
        type Future = LocalBoxFuture<Result<Self::Response, Self::Error>>;

        // This service is ready when its next service is ready
        forward_ready!(service);

        fn call(&self, req: ServiceRequest) -> Self::Future {
            println!("Hi from start. You requested: {}", req.path());

            // A more complex middleware, could return an error or an early response here.

            // we do not immediately await this, which means nothing happens
            // this future gets moved into a Box
            let fut = self.service.call(req);

            Box::pin(async move {
                // this future gets awaited now
                let res = fut.await?;

                // we can now do any work we need to after the request
                println!("Hi from response");
                Ok(res)
            })
        }
    }
```

现在已经编写了中间件，现在可以将其添加到应用程序中：

```rust
#[actix_web::main]
async fn main() -> std::io::Result<()> {
    let app = App::new()
        .wrap(SayHi);

    // ... rest of your code
}
```

### Actix Web 中的静态文件
Actix Web 中简单、简洁的静态文件服务是通过 `actix_files` 箱完成的 - 要添加它，只需通过 Cargo 添加它，如下所示：

```rust
cargo add actix-files
```

设置静态文件服务的路由如下所示：

```rust
use actix_files::NamedFile;
use actix_web::{HttpRequest, Result};
use std::path::PathBuf;

#[get("/")]
async fn index(req: HttpRequest) -> Result<NamedFile> {
    let path: PathBuf = req.match_info().query("filename").parse().unwrap();
    Ok(NamedFile::open(path)?)
}
```

该路由允许我们提供任何可以找到并与文件名匹配的文件 - 例如，如果我们有一个为该路由提供服务的基本路由，如果我们然后运行我们的应用程序并转到 `/index.html` ，则该路由将尝试在项目根目录中查找名为 `index.html` 的文件。

请注意，您在任何情况下都不应该尝试使用带有 `.*` 的路径尾部来返回 `NamedFile` - 这会产生严重的安全隐患，并且会打开您的 Web 服务以进行路径遍历！这在 Actix Web 文档中进行了记录，您可以在此处找到有关路径遍历攻击的更多信息。为了防止这种情况，您可以尝试使用 `std::fs::canoncalize` 来尝试验证路径文件是否正确或未尝试遍历目标文件夹之外。

然而，当您需要提供多个文件时，这有点笨拙 - 例如，特别是如果您需要提供 HTML 文件的文件夹。为了能够从您的网络服务提供文件文件夹，最好的方法是使用 `actix_files::Files` 并将其附加到您的 `App` ：

```rust
use actix_files as fs;
use actix_web::{App, HttpServer};

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| {
        App::new().service(
            fs::Files::new("/static", ".")
                .use_last_modified(true),
        )
    })
    .bind(("127.0.0.1", 8080))?
    .run()
    .await
}
```

请注意，您还可以使用多个选项来扩展 `Files` 结构，例如在基本路径上显示文件目录本身（用于文件服务）并允许隐藏文件，您可以在此处找到更多信息。

此外，我们还可以使用 `askama` 的 HTML 模板功能来增强我们的 HTML 文件服务！我们可以像这样开始：

```rust
cargo add askama askama-actix-web -F askama/with-actix-web
```

这会添加 `askama` 本身以及 `askama::Template` 类型的 `Responder` 特征实现。 `askama` 期望您的文件默认位于项目根目录的名为 `templates` 的子文件夹中，因此让我们创建该文件夹，然后使用以下内容创建一个 `index.html` 文件HTML 代码位于：

```rust
Hello, {{name}}!
```

要在我们的应用程序中使用 Askama，我们需要声明一个使用 `Template` 派生宏的结构体，并使用 Askama 的 `template` 宏将该结构体指向我们希望它使用的文件：

```rust
#[derive(Template)]
#[template(path = "index.html")]
struct IndexTemplate<'a> {
    name: &'a str
}

#[get("/")]
async fn index_route() -> impl Responder {
    IndexTemplate { name: "Shuttle" }
}
```

然后我们可以将其添加为我们的 Actix Web 服务中的常规处理程序函数，我们就可以开始了！当您转到返回 `index_route` 的路径时，您应该看到“Hello, Shuttle!”作为 HTML 响应。

### 部署
一般来说，由于必须使用 Dockerfile，使用 Rust 后端程序进行部署可能不太理想，尽管如果您已经有使用 Docker 的经验，这对您来说可能不是一个问题 - 特别是如果您使用 `cargo-chef` .但是，如果您使用的是 Shuttle，则只需使用 `cargo shuttle deploy` 即可完成。无需设置。

### 尾声
谢谢阅读！ Actix Web 是一个强大的框架，您可以用它来增强您的 Rust 产品组合，如果您想构建您的第一个 Rust API，那么它也是一个深入了解 Rust 的绝佳框架。

---

[原文地址:Getting Started with Actix Web in Rust](https://www.shuttle.rs/blog/2023/12/15/using-actix-rust)  


> 来自: [Rust web开发 ActixWeb 入门_actix-web-CSDN博客](https://blog.csdn.net/xuejianxinokok/article/details/135702711)
>

