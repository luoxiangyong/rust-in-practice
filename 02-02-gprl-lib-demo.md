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

在Cargo.toml中添加如下代码：

```toml
description = "一个简单的库演示"
license = "MIT"
keywords = ["demo", "rlib"]
readme = "README.md"
documentation = "本库是一些演示简单计算的函数集合。"
```



其中license字段必须是在https://spdx.org/licenses/ 表格的`Identifier`中列出的有效字符串。

`Cargo.toml`文件都是针对本库的配置元信息。

在`src/lib.rs`文件中添加如下代码:

```rust
#[allow(dead_code)]
fn compute_add(a:i32,b:i32) -> i32 {
    let s = a + b;
    println!("用Rust计算add的结果是:{}",s);
    s
}


#[allow(dead_code)]
fn compute_sub(a:i32,b:i32) -> i32 {
    let s = a - b;
    println!("用Rust计算sub的结果是:{}",s);
    s
}
```

使用`cargo build`确保编译无错误。

### 测试库函数：单元测试

在`src/lib.rs`文件中改写如下代码:

```rust
#[cfg(test)]
mod tests {
    use super::*;
    #[test]
    fn test_compute_add() {
        assert_eq!(compute_add(1000,999), 1999);
    }
    
    #[test]
    fn test_compute_sub() {
        assert_eq!(compute_sub(999,1000), -1);
    }
}
```

添加了两个单元测试函数。运行命令：

```bash
cargo test
   Compiling gprl-lib-demo v0.1.0 (/home/luoxiangyong/writing-book/rust-in-practice-code/chapter-01/gprl-lib-demo)
    Finished test [unoptimized + debuginfo] target(s) in 0.42s
     Running target/debug/deps/gprl_lib_demo-164c078fb443f2d9

running 2 tests
test tests::test_compute_sub ... ok
test tests::test_compute_add ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests gprl-lib-demo

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```



### 测试库：集成测试

在项目录下建立`tests`目录，再建立一个集成测试文件`integral_test_01.rs`，其中代码如下:

```rust
extern crate gprl_lib_demo;

#[test]
fn integral_test_compute() {
    let add = gprl_lib_demo::compute_add(1000,999);
    let sub = gprl_lib_demo::compute_sub(999,1000);
    
    assert_eq!(add + sub, 1998);
}
```

运行一下命令，可以看到，单元测试和集成测试全部完成:

```bash
$ cargo test
    Finished test [unoptimized + debuginfo] target(s) in 0.01s
     Running target/debug/deps/gprl_lib_demo-164c078fb443f2d9

running 2 tests
test tests::test_compute_add ... ok
test tests::test_compute_sub ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/integral_test_01-d08f2e845d048ae2

running 1 test
test integral_test_compute ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests gprl-lib-demo

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```



### 测试库：文档测试

代码中，以`//!`开始的注释是针对整个模块的文档，`///｀是针对单个语言项的文档。如下写注释的话:

```bash
//! 本库是一些演示简单计算的函数集合。
//!
//! # 示例
//! ```
//! assert_eq!(gprl_lib_demo::compute_add(2,2), 4);
//! ```
//!

// 

/// 计算两个函数的和
///
///　# 示例
///
/// ```
///　assert_eq!(sum::sum(1,1), 2);
/// ```
/// # 参数
///   * `a` 第一个整数
///   * `b` 第二个整数
#[allow(dead_code)]
pub fn compute_add(a:i32,b:i32) -> i32 {
    let s = a + b;
    println!("用Rust计算add的结果是:{}",s);
    s
}
```

运行一下命令，可以看到，单元测试和集成测试全部完成:

```bash
$ cargo test
   Compiling gprl-lib-demo v0.1.0 (/home/luoxiangyong/writing-book/rust-in-practice-code/chapter-01/gprl_lib_demo)
    Finished test [unoptimized + debuginfo] target(s) in 0.60s
     Running target/debug/deps/gprl_lib_demo-164c078fb443f2d9

running 2 tests
test tests::test_compute_add ... ok
test tests::test_compute_sub ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/integral_test_01-d08f2e845d048ae2

running 1 test
test integral_test_compute ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests gprl-lib-demo

running 3 tests
test src/lib.rs -  (line 4) ... ok
test src/lib.rs - compute_add (line 12) ... ok
test src/lib.rs - compute_sub (line 30) ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

可以看到添加了以上的注释代码后，注释中的三个测试也运行了。很好！



执行`cargo doc --open`,会在项目目录下的`target/doc/`目录中生成html格式的文档，并打开浏览器供你查看。



### 测试库：基准测试

基准测试可以测试你代码的性能。cargo提供了bench子命令用于此，不过现在需要在nightly版本的rust中才能使用。请使用如下指令切换到nightly版本，如果还没安装的话，它会自动安装的:

```bash
$ rustup default nightly
info: using existing install for 'nightly-x86_64-unknown-linux-gnu'
info: default toolchain set to 'nightly-x86_64-unknown-linux-gnu'
```

在`src/lib.rs`文件中头添加如下代码：

```rust
#![feature(test)]

extern crate test;

use test::Bencher;
```

在该文件的测试模块中添加如下两个基准测试函数:

```rust
#[bench]
fn bench_compute_add(b:&mut Bencher) {
    b.iter(|| compute_add(1000,999));
}

#[bench]
fn bench_compute_sub(b:&mut Bencher) {
    b.iter(|| compute_sub(1000,999));
}
```

运行如下命令:

```bash
cargo bench
   Compiling gprl-lib-demo v0.1.0 (/home/luoxiangyong/writing-book/rust-in-practice-code/chapter-01/gprl_lib_demo)
    Finished bench [optimized] target(s) in 2.58s
     Running target/release/deps/gprl_lib_demo-efbc3985baeb47b7

running 3 tests
test tests::test_compute_add ... ignored
test tests::test_compute_sub ... ignored
test tests::bench_compute_add ... bench:         216 ns/iter (+/- 21)

test result: ok. 0 passed; 0 failed; 2 ignored; 1 measured; 0 filtered out
```

命令输出显示两个基准测试都运行了，时间以纳秒统计。漂亮！:smile:



### 将你的库共享给世界！

好啦，我们已经开发了一个经过多种测试且都没发现问题的库了，现在是时候把她共享给全世界了罗！啦啦啦。。。

首先用你的github账户登录https://cratios.io，点击界面右上方的账号链接里的`Account Settings`，进入。在此页面的最下面，填写令牌(Token)名，然后点击`New Token`按钮，就会生成如下所示的令牌，将其保存到`~/.cargo/credentials`文件中，格式如下：

```ini
[registry]
token = "p2v8czieD4m6kQuH84SB****g6Z6***"
```

使用命令：

```bash
$ cargo login

$ cargo package

$ cargo publish
```



