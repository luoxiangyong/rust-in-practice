## 安装Rust开发环境

Rust语言的官方网站为：[https://www.rust-lang.org](https://www.rust-lang.org)，有兴趣的可以都去逛逛。下面介绍Rust开发环境的安装：

一条命令搞定：

```shell
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

安装过程默认选择即可。使用如下命令测试是否安装正确：

```
$ cargo --version
cargo 1.41.0 (626f0f40e 2019-12-03)
```

更具体的安装过程可参考：https://www.rust-lang.org/learn/get-started。

