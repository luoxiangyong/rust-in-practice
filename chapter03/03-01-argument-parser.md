## CLAP命令行解析

首先到https://crates.io 上用`command line`搜索，我们可以发下，下载两最大的是一个叫clap(Command Line Argument Parser for Rust)的库， 官方介绍说它是一个简单、高效并且特性丰富的库，专门用来做命令行解析，而且支持之命令。目前很多知名的工具都支持子命令，比如我们使用的cargo，用于容器的docker等等。简单看看官方的示例，使用起来还比较简单。

我们就使用这个库来开发一个简单的程序`gprl-compute`，这个程序接受命令行参数，打印计算结果，使用方式如下：

```bash
$ gprl-compute --help
gprl-compute 0.1.0
Luo Xiangyong <luoxiangyong@topgridcloud.com>
简单计算演示程序

USAGE:
    gprl-compute [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    add     compute add to integer
    help    Prints this message or the help of the given subcommand(s)
    sub     compute sub to integer
```

### 建立`gprl-compute`工程

直接上命令：

```bash
$ mkdir chapter03
$ cd chapter03
$ cargo new --bin gprl-compute
$ cd gprl-compute
```

好了，可以开工了。

接下来就是添加依赖库，参照[clap](https://clap.rs/)官方给的示例，开始撸代码了。

```toml
# Cargo.toml
[package]
name = "gprl-compute"
version = "0.1.0"
authors = ["Luo Xiangyong <luoxiangyong@topgridcloud.com>"]
edition = "2018"

[dependencies]
clap = "^2.33.0"
```

```rust
// src/main.rs
extern crate clap; 

use clap::App; 
 
fn main() { 
    App::new("gprl-compute")
       .version("0.1.0")
       .about("简单计算演示程序")
       .author("Luo Xiangyong <luoxiangyong@topgridcloud.com>")
       .get_matches(); 
}
```

先按官方的示例简单测试一下效果：

```bash
$ cargo run -- -h
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/gprl-compute -h`
gprl-compute 0.1.0
Luo Xiangyong <luoxiangyong@topgridcloud.com>
简单计算演示程序

USAGE:
    gprl-compute

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

```

从上面的输出可以发现效果还不错。继续努力，我们要添加子命令，根据不同的子命令`add|sub`对两个整数进行计算。

### 实现子命令

我们完成的`gprl-compute`可以像这样使用：

```bash
$ gprl-compute --help
gprl-compute 0.1.0
Luo Xiangyong <luoxiangyong@topgridcloud.com>
简单计算演示程序

USAGE:
    gprl-compute [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    add     compute add to integer
    help    Prints this message or the help of the given subcommand(s)
    sub     compute sub to integer
```

其中子命令`add`和`sub`分别进行两个整数的加法和减法，`help`子命令可以打印帮助。使用方式如下：

```bash
$ gprl-compute add -a 1 -b 2
$ gprl-compute sub -a 1 -b 2
```

程序比较简单，直接上代码：

```rust
// src/main.rs
extern crate clap; 

use clap::{Arg, App, SubCommand};
 
fn main() { 
    let app = App::new("gprl-compute")
                   .version("0.1.0")
                   .about("简单计算演示程序")
                   .author("Luo Xiangyong <luoxiangyong@topgridcloud.com>")
                   .subcommand(SubCommand::with_name("add")
                              .about("compute add to integer")
                              .arg(Arg::with_name("first")
                                    .required(true)
                                    .takes_value(true)
                                    .short("a")
                                    .help("第一个整数")
                                      )
                              .arg(Arg::with_name("second")
                                    .required(true)
                                    .takes_value(true)
                                    .short("b")
                                    .help("第二个整数")
                                  ))
                    .subcommand(SubCommand::with_name("sub")
                              .about("compute sub to integer")
                              .arg(Arg::with_name("first")
                                    .required(true)
                                    .takes_value(true)
                                    .short("a")
                                    .help("第一个整数")
                                      )
                              .arg(Arg::with_name("second")
                                    .required(true)
                                    .takes_value(true)
                                    .short("b")
                                    .help("第二个整数")
                                  )
                      );
                    
    let matches = app.get_matches(); 
                    
    match matches.subcommand() {
        ("add",  Some(sub_m)) => {
            if sub_m.is_present("first") {
                println!("Value for first integer is:{:?}",sub_m.value_of("first").unwrap());
            } 
            
            if sub_m.is_present("second") {
                println!("Value for second integer is:{:?}",sub_m.value_of("second").unwrap());
            } 
            
            let first = sub_m.value_of("first").unwrap().parse::<i32>().unwrap();
            let second = sub_m.value_of("second").unwrap().parse::<i32>().unwrap();
            
            println!("Add计算结果为:{}", first + second);
            
            return;   
        
        },
        ("sub",  Some(sub_m)) => {
            if sub_m.is_present("first") {
                println!("Value for first integer is:{:?}",sub_m.value_of("first").unwrap());
            } 
            
            if sub_m.is_present("second") {
                println!("Value for second integer is:{:?}",sub_m.value_of("second").unwrap());
            } 
            
            let first = sub_m.value_of("first").unwrap().parse::<i32>().unwrap();
            let second = sub_m.value_of("second").unwrap().parse::<i32>().unwrap();
            
            println!("Sub计算结果为:{}", first - second);
            
            return;   
        
        },
        _ => {
            println!("{}",matches.usage());
            return;
        }
    }    
}
```

还是比较直观的，如果对相关函数用法有疑问，请参考clap的官方网站：https://docs.rs/clap/2.33.0/clap/index.html。