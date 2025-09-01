---
title: Rust 中的 S3 魔法：s3s 实战指南与优雅代码之旅
date: '2024-12-04 22:34:36'
updated: '2024-12-23 23:16:05'
---
## <font style="color:rgb(255, 255, 255);background-color:rgb(239, 112, 96);">引言</font>
<font style="color:black;">在现代云计算环境中，Amazon S3（Simple Storage Service）是一个非常流行的对象存储服务。然而，管理 S3 存储桶中的数据同步和备份可能会变得复杂。为了简化这一过程，</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">项目应运而生。</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">是一个开源的 Rust 库，旨在帮助开发者轻松地与 S3 存储桶进行交互。</font>

<font style="color:black;">本教程将带你从零开始，通过实战的方式掌握</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">的使用方法，并提供完整的实例代码，帮助你快速上手。</font>

---

## <font style="color:rgb(255, 255, 255);background-color:rgb(239, 112, 96);">第一步：环境准备</font>
<font style="color:black;">在开始之前，你需要确保你的开发环境已经准备就绪。以下是你需要安装的工具和库：</font>

1. **<font style="color:black;">Rust 和 Cargo</font>**<font style="color:rgb(1, 1, 1);">：</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:rgb(1, 1, 1);">是基于 Rust 的库，因此你需要安装 Rust 和 Cargo。</font>
2. **<font style="color:black;">AWS CLI</font>**<font style="color:rgb(1, 1, 1);">：用于配置 AWS 凭证。</font>

### <font style="color:black;">安装 Rust 和 Cargo</font>
<font style="color:black;">如果你还没有安装 Rust 和 Cargo，可以通过以下命令安装：</font>

```rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

<font style="color:black;">安装完成后，确保 Rust 和 Cargo 已经正确安装：</font>

```rust
rustc --version
cargo --version
```

### <font style="color:black;">安装 AWS CLI</font>
<font style="color:black;">AWS CLI 是一个命令行工具，用于与 AWS 服务进行交互。你可以通过以下命令安装：</font>

```rust
# 对于 macOS 和 Linux
pip install awscli


# 对于 Windows
pip install awscli
```

<font style="color:black;">安装完成后，配置 AWS CLI：</font>

```rust
aws configure
```

<font style="color:black;">按照提示输入你的 AWS Access Key ID、Secret Access Key、默认区域和输出格式。</font>

---

<font style="color:black;">在你的 Rust 项目中，你需要将</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">添加到</font>`<font style="color:rgb(239, 112, 96);">Cargo.toml</font>`<font style="color:black;">文件中。</font>

```rust
[dependencies]
s3s = "0.1.0"
tokio = { version = "1", features = ["full"] }
```

`<font style="color:rgb(239, 112, 96);">tokio</font>`<font style="color:black;">是一个异步运行时，</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">依赖于它来处理异步操作。</font>

---

## <font style="color:rgb(255, 255, 255);background-color:rgb(239, 112, 96);">第三步：基本用法</font>
`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">的基本用法非常简单。你可以通过以下代码列出 S3 存储桶中的对象：</font>

```rust
use s3s::dto::{ListObjectsV2Input, ListObjectsV2Output};
use s3s::request::S3Request;
use s3s::response::S3Response;
use s3s::service::S3;
use s3s::auth::Credentials;
use s3s::error::S3Error;
use s3s::request::S3RequestBuilder;


#[tokio::main]
async fn main() -> Result<(), S3Error> {
    // 配置 AWS 凭证
    let credentials = Credentials::new(
        "YOUR_ACCESS_KEY_ID",
        "YOUR_SECRET_ACCESS_KEY",
        None,
        None,
        "s3s",
    );


    // 创建 S3 服务
    let s3_service = S3::new(credentials, "us-west-2");


    // 创建 ListObjectsV2 请求
    let list_objects_input = ListObjectsV2Input::builder()
        .bucket("your-bucket-name")
        .build()?;


    // 构建请求
    let request = S3RequestBuilder::new("ListObjectsV2")
        .input(list_objects_input)
        .build()?;


    // 发送请求并获取响应
    let response: S3Response<ListObjectsV2Output> = s3_service.send(request).await?;


    // 打印对象列表
    if let Some(contents) = response.output.contents {
        for object in contents {
            println!("Object: {}", object.key.unwrap_or_default());
        }
    }


    Ok(())
}
```

<font style="color:black;">将</font>`<font style="color:rgb(239, 112, 96);">YOUR_ACCESS_KEY_ID</font>`<font style="color:black;">和</font>`<font style="color:rgb(239, 112, 96);">YOUR_SECRET_ACCESS_KEY</font>`<font style="color:black;">替换为你的 AWS 凭证。</font>

---

## <font style="color:rgb(255, 255, 255);background-color:rgb(239, 112, 96);">第四步：同步 S3 存储桶</font>
`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">的核心功能之一是同步两个 S3 存储桶之间的数据。你可以通过以下代码实现同步：</font>

```rust
use s3s::dto::{ListObjectsV2Input, GetObjectInput, PutObjectInput};
use s3s::request::S3Request;
use s3s::response::S3Response;
use s3s::service::S3;
use s3s::auth::Credentials;
use s3s::error::S3Error;
use s3s::request::S3RequestBuilder;
use tokio::fs::File;
use tokio::io::AsyncWriteExt;


#[tokio::main]
async fn main() -> Result<(), S3Error> {
    // 配置 AWS 凭证
    let credentials = Credentials::new(
        "YOUR_ACCESS_KEY_ID",
        "YOUR_SECRET_ACCESS_KEY",
        None,
        None,
        "s3s",
    );


    // 创建 S3 服务
    let s3_service = S3::new(credentials, "us-west-2");


    // 列出源存储桶中的对象
    let list_objects_input = ListObjectsV2Input::builder()
        .bucket("source-bucket")
        .build()?;


    let request = S3RequestBuilder::new("ListObjectsV2")
        .input(list_objects_input)
        .build()?;


    let response: S3Response<ListObjectsV2Output> = s3_service.send(request).await?;


    // 遍历对象并同步到目标存储桶
    if let Some(contents) = response.output.contents {
        for object in contents {
            let object_key = object.key.unwrap_or_default();


            // 从源存储桶下载对象
            let get_object_input = GetObjectInput::builder()
                .bucket("source-bucket")
                .key(&object_key)
                .build()?;


            let request = S3RequestBuilder::new("GetObject")
                .input(get_object_input)
                .build()?;


            let response: S3Response<GetObjectOutput> = s3_service.send(request).await?;


            // 将对象保存到本地文件
            let mut file = File::create(&object_key).await?;
            file.write_all(&response.output.body).await?;


            // 将对象上传到目标存储桶
            let put_object_input = PutObjectInput::builder()
                .bucket("destination-bucket")
                .key(&object_key)
                .body(response.output.body)
                .build()?;


            let request = S3RequestBuilder::new("PutObject")
                .input(put_object_input)
                .build()?;


            s3_service.send(request).await?;
        }
    }


    println!("同步完成！");


    Ok(())
}
```

<font style="color:black;">将</font>`<font style="color:rgb(239, 112, 96);">YOUR_ACCESS_KEY_ID</font>`<font style="color:black;">和</font>`<font style="color:rgb(239, 112, 96);">YOUR_SECRET_ACCESS_KEY</font>`<font style="color:black;">替换为你的 AWS 凭证。</font>

---

## <font style="color:rgb(255, 255, 255);background-color:rgb(239, 112, 96);">第五步：高级用法</font>
`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">还支持一些高级功能，例如过滤特定前缀的对象、设置同步的并发数等。</font>

### <font style="color:black;">示例：同步特定前缀的对象</font>
<font style="color:black;">假设你只想同步</font>`<font style="color:rgb(239, 112, 96);">source-bucket</font>`<font style="color:black;">中以</font>`<font style="color:rgb(239, 112, 96);">logs/</font>`<font style="color:black;">开头的对象，你可以使用</font>`<font style="color:rgb(239, 112, 96);">--prefix</font>`<font style="color:black;">参数：</font>

```rust
let list_objects_input = ListObjectsV2Input::builder()
    .bucket("source-bucket")
    .prefix("logs/")
    .build()?;
```

### <font style="color:black;">示例：设置并发数</font>
<font style="color:black;">你可以通过</font>`<font style="color:rgb(239, 112, 96);">tokio</font>`<font style="color:black;">的并发机制来设置同步的并发数：</font>

```rust
use tokio::task;


#[tokio::main]
async fn main() -> Result<(), S3Error> {
    // 配置 AWS 凭证
    let credentials = Credentials::new(
        "YOUR_ACCESS_KEY_ID",
        "YOUR_SECRET_ACCESS_KEY",
        None,
        None,
        "s3s",
    );


    // 创建 S3 服务
    let s3_service = S3::new(credentials, "us-west-2");


    // 列出源存储桶中的对象
    let list_objects_input = ListObjectsV2Input::builder()
        .bucket("source-bucket")
        .build()?;


    let request = S3RequestBuilder::new("ListObjectsV2")
        .input(list_objects_input)
        .build()?;


    let response: S3Response<ListObjectsV2Output> = s3_service.send(request).await?;


    // 并发同步对象
    let tasks: Vec<_> = if let Some(contents) = response.output.contents {
        contents.into_iter().map(|object| {
            let s3_service = s3_service.clone();
            let object_key = object.key.unwrap_or_default();


            task::spawn(async move {
                // 从源存储桶下载对象
                let get_object_input = GetObjectInput::builder()
                    .bucket("source-bucket")
                    .key(&object_key)
                    .build()?;


                let request = S3RequestBuilder::new("GetObject")
                    .input(get_object_input)
                    .build()?;


                let response: S3Response<GetObjectOutput> = s3_service.send(request).await?;


                // 将对象保存到本地文件
                let mut file = File::create(&object_key).await?;
                file.write_all(&response.output.body).await?;


                // 将对象上传到目标存储桶
                let put_object_input = PutObjectInput::builder()
                    .bucket("destination-bucket")
                    .key(&object_key)
                    .body(response.output.body)
                    .build()?;


                let request = S3RequestBuilder::new("PutObject")
                    .input(put_object_input)
                    .build()?;


                s3_service.send(request).await?;


                Ok::<(), S3Error>(())
            })
        }).collect()
    } else {
        Vec::new()
    };


    // 等待所有任务完成
    for task in tasks {
        task.await??;
    }


    println!("同步完成！");


    Ok(())
}
```

---

## <font style="color:rgb(255, 255, 255);background-color:rgb(239, 112, 96);">结语</font>
<font style="color:black;">通过本教程，你已经掌握了</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">的基本用法，并学会了如何通过 Rust 代码实现 S3 存储桶的同步。</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">是一个强大且易用的库，可以帮助你轻松管理 S3 存储桶中的数据。</font>

<font style="color:black;">希望这篇教程能够帮助你快速上手</font>`<font style="color:rgb(239, 112, 96);">s3s</font>`<font style="color:black;">，并在实际项目中发挥作用。如果你有任何问题或建议，欢迎在 GitHub 上提交 Issue 或 Pull Request。</font>

---

## <font style="color:rgb(255, 255, 255);background-color:rgb(239, 112, 96);">参考资料</font>
+ **<font style="color:#ff3502;">s3s GitHub 仓库</font>****<font style="color:rgb(239, 112, 96);">[1]</font>**
+ **<font style="color:#ff3502;">AWS S3 官方文档</font>****<font style="color:rgb(239, 112, 96);">[2]</font>**
+ **<font style="color:#ff3502;">Rust 官方文档</font>****<font style="color:rgb(239, 112, 96);">[3]</font>**

---

**<font style="color:black;">祝你编程愉快！</font>**

### <font style="color:black;">参考资料</font>
<font style="color:black;">[1]</font>

<font style="color:black;">s3s GitHub 仓库:</font>_<font style="color:black;">https://github.com/Nugine/s3s</font>_

<font style="color:black;">[2]</font>

<font style="color:black;">AWS S3 官方文档:</font>_<font style="color:black;">https://docs.aws.amazon.com/s3/index.html</font>_

<font style="color:black;">[3]</font>

<font style="color:black;">Rust 官方文档:</font>_<font style="color:black;">https://doc.rust-lang.org/book/</font>_



  


> 来自: [Rust 中的 S3 魔法：s3s 实战指南与优雅代码之旅](https://mp.weixin.qq.com/s/aK7rK_n3Q3HPpyORxNm4FQ)
>

