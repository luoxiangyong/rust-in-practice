## 你好，Rust！

### 准备编辑器及代码托管库

在开始开发之前请确保安装了Vim编辑器，我们的简单程序使用她来输入代码，没有安装的同学，请使用下面的命令：

```bash
$ sudo apt-get install vim
```

有如下输出，说明安装成功：

```bash
VIM - Vi IMproved 8.0 (2016 Sep 12, compiled Jun 06 2019 17:31:41)
Included patches: 1-1453
Modified by pkg-vim-maintainers@lists.alioth.debian.org
Compiled by pkg-vim-maintainers@lists.alioth.debian.org
```

在当前目录，建立目录`rust-in-practice-code`：

```bash
$ mkdir rust-in-practice-code
$ cd rust-in-practice-code
```

使用`git　init`命令初始化当前目录，便于今后的github代码托管：

```bash
$ git init
Reinitialized existing Git repository in /home/luoxiangyong/writing-book/rust-in-practice/.git/
$ ls -a
.  ..  .git
```

输入`vim .gitignore`过滤不需要代码托管的文件。进入vim后，键入`i`进入插入命令模式，输入：

```
/**/target/
/**/Cargo.lock
```

单击键盘的左上角的`ESC`,输入`:wq`，保存文件并退出。

输入如下命令，将刚才新建的`.gitignore`文件加入到git本地库中托管。

你还可以建立`Readme.md`文件，说明本代码库的主要作用，如：

```bash
$ touch Readme.md
$ typoma Readme.md
```

这样就可以使用typora书写markdown形式的readme文件了。

登陆GitHub，新建仓库:`rust-in-practice-code`

使用如下命令，添加文件到git本地仓库，并`push`到远程仓库：

```bash
$ git add .
$ git commit -m "第一次提交"
$ git remote add origin https://github.com/luoxiangyong/rust-in-practice-code.git ＃　换成你的代码仓库地址
$ git push -u origin master -f # 把本地的文件推送到github上，需要输入你的用户名和密码
$ git branch	# 查看分支
* master
```

如果不想每次提交都要输入用户名，请设置：

```bash
$ git config --global user.name "Xiangyong Luo"	# 换成你自己的名字
$ git config --global user.email example@example.com　#　换成你自己的E-mail地址，需要跟github上注册的信息一致
```

使用如下命令：

```bash
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

显示如上的输出，说明文件托管成功，你可以使用vim编辑代码，而且所有文件都已经在网络上托管好了。

假设下次你要从新目录开始你的工作，可以使用如下命令：

```bash
$ git clone https://github.com/luoxiangyong/rust-in-practice-code.git
$ cd rust-in-practice-code.git
$ ls -a
.  ..  .git  .gitignore  Readme.md
```

就是这么简单！上次的成果从线上下载下来了，你还可以接着以前的工作开始你的实践！:smile:

关于Git的详细使用，可参考这里：[Git Book](https://git-scm.com/book/zh/v2)。

关于Vim的使用，这里有一个[命令速查表](https://github.com/skywind3000/awesome-cheatsheets/blob/master/editors/vim.txt)。还不过瘾的话，我还给大家提供了一张漂亮的速查图片:

![给程序员准备的Vim命令速查表](./asserts/vim-for-programmer-01.png)

### 创建项目并运行

使用如下命令创建你好rust项目：

```bash
$ mkdir chapter-01
$ cd chapter-01
$ cargo new --bin hello_rust
     Created binary (application) `hello_rust` package
```

命令输出显示，已经创建了一个hello_rust目录及相关文件。我们使用`tree`命令查看创建的相关文件吧：

```bash
$ tree hello_rust/
hello_rust/
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files

```

其中`Cargo.toml`是本项目的配置文件，`src/main.rs`是本项目的源码文件。使用如下命令编辑`src/main.rs`：

```bash
$ cd hello_rust
$ vim src/main.rs
```

输入如下代码：

```rust
fn main() {
    println!("嗨，世界!");
}
```

使用如下命令编译并运行程序：

```bash
$ cargo run
   Compiling hello_rust v0.1.0 (/home/luoxiangyong/writing-book/rust-in-practice-code/chapter-01/hello_rust)
    Finished dev [unoptimized + debuginfo] target(s) in 5.05s
     Running `target/debug/hello_rust`
嗨，世界!
```

哈哈，你看，打印出来我们想要的结果罗！



我们使用命令`tree -L 3 .`,发现如下输出:

```bash
$ tree -L 3 .
.
├── Cargo.lock
├── Cargo.toml
├── src
│   └── main.rs
└── target
    └── debug
        ├── build
        ├── deps
        ├── examples
        ├── hello_rust
        ├── hello_rust.d
        └── incremental

7 directories, 5 files

```

其中目录`./target/debug/hello_rust`就是刚才生成的可执行程序，执行一下命令，可以得到一样的输出：

```bash
$ ./target/debug/hello_rust
嗨，世界!

```

### 提交今天的成果

使用如下命令提交今天的成果到github网站：

```bash
$ git add .
$ git commit -m "添加chapter01/hello_rust项目"
$ git push
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
$ git log
commit 4bf8a6c34340525c8f396ee474cbb4c38c34ab85 (HEAD -> master, origin/master)
Author: Luo Xiangyong <luoxiangyong@topgridcloud.com>
Date:   Tue Feb 25 17:09:45 2020 +0800

    添加chapter01/hello_rust项目

commit fbfeb8382e255e39ce59bf60ae02060174a01992
Author: Luo Xiangyong <luoxiangyong@topgridcloud.com>
Date:   Tue Feb 25 16:13:41 2020 +0800

    第一次提交

```

:ok_hand:，OK,今天的工作完成！