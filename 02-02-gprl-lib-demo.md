## 第一个库

### 编写库代码

好啦，同志们，今天开始我们的第一Rust库的编写。确保你在`rust-in-practice-code`目录。如果没有以前的代码，可以从第一章建立的托管库下载：

```bash
$ git clone https://github.com/luoxiangyong/rust-in-practice-code.git # 请将将此处的代码库地址改成你的！
$ cd rust-in-practice-code
```



输入如下命令：

```bash
$ cd chapter01
$ cargo new --lib gprl-lib-demo
     Created library `gprl-lib-demo` package
$ cd gprl-lib-demo
$ tree .
.
├── Cargo.toml
└── src
    └── lib.rs

1 directory, 2 files

```

我们看看cargo建立的两个文件内容:

首先是`src/lib.rs`

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
}
```

其次是`Cargo.toml`：

```toml
[package]
name = "gprl-lib-demo"
version = "0.1.0"
authors = ["Luo Xiangyong <luoxiangyong@topgridcloud.com>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

`Cargo.toml`文件内容的具体意思可以参考这里：Cargo项目管理https://rustcc.gitbooks.io/rustprimer/content/cargo-projects-manager/cargo-projects-manager.html。

`Cargo.toml`文件都是针对本库的配置元信息。



### 测试库函数：单元测试



### 测试库：集成测试



### 测试库：性能测试



### 将你的库共享给世界！
