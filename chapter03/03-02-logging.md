## 日志

log 是一个轻量级的日志库，目前在crates.io上的下载量最大，官方网站：https://docs.rs/log



在chapter03目录下，准备工程：

```bash
$ cd chapter03
$ cargo new --bin gprl-log-demo
$ cd gprl-log-demo
```

### 简单日志simple_logger

开始撸代码了:

```toml
# Cargo.toml
[package]
name = "gprl-log-demo"
version = "0.1.0"
authors = ["Luo Xiangyong <luoxiangyong@topgridcloud.com>"]
edition = "2018"

[dependencies]
log = "^0.4.8"
simple_logger = "1.6.0"
```

```rust
// src/main.rs
extern crate log;
extern crate simple_logger;
#[warn(unused_imports)]
use log::{info, warn};

fn main() {
    simple_logger::init().unwrap();
    info!("This a info log from log crate");
    warn!("This is an warn message.");

}
```

log crate提供了日志的基础设施，想要看到实际效果还需要实现log::Log trait，我们这里直接使用了官方推荐的simple_logger。看效果：

```bash
$ cargo run
   Compiling gprl-log-demo v0.1.0 (/home/luoxiangyong/writing-book/rust-in-practice-code/chapter-03/gprl-log-demo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.38s
     Running `target/debug/gprl-log-demo`
2020-02-27 10:33:53,433 INFO  [gprl_log_demo] This a info log from log crate
2020-02-27 10:33:53,433 WARN  [gprl_log_demo] This is an warn message.
```

当然还有其他的可以使用：
    - [env_logger](https://docs.rs/env_logger/*/env_logger/)
        - [simple_logger](https://github.com/borntyping/rust-simple_logger)
        - [simplelog](https://github.com/drakulix/simplelog.rs)
        - [pretty_env_logger](https://docs.rs/pretty_env_logger/*/pretty_env_logger/)
        - [stderrlog](https://docs.rs/stderrlog/*/stderrlog/)
        - [flexi_logger](https://docs.rs/flexi_logger/*/flexi_logger/)

### 高度可配置的框架log4rs

log4rs是借鉴了Java的Logback和log4j日志库模型的高度可配置日志框架。框架提供的*appenders*, *encoders*, *filters*, and *loggers*都是可以配置的。日志可以记录到文件、控制台、syslog、甚至网络上，还可以通过yaml文件进行日志配置。

在chapter03目录下，准备工程：

```bash
$ cd chapter03
$ cargo new --bin gprl-log4rs-demo
$ cd gprl-log4rs-demo
```

直接上代码：

```toml
# Cargo.toml
[package]
name = "gprl-log4rs-demo"
version = "0.1.0"
authors = ["Luo Xiangyong <luoxiangyong@topgridcloud.com>"]
edition = "2018"

[dependencies]
log = "^0.4.8"
log4rs = "0.10.0"
```

```rust
// src/main.rs
use log::LevelFilter;
use log4rs::append::console::ConsoleAppender;
use log4rs::append::file::FileAppender;
use log4rs::encode::pattern::PatternEncoder;
use log4rs::config::{Appender, Config, Logger, Root};

fn main() {
    let stdout = ConsoleAppender::builder().build();

    let requests = FileAppender::builder()
        .encoder(Box::new(PatternEncoder::new("{d} - {m}{n}")))
        .build("log/requests.log")
        .unwrap();

    let config = Config::builder()
        .appender(Appender::builder().build("stdout", Box::new(stdout)))
        .appender(Appender::builder().build("requests", Box::new(requests)))
        .logger(Logger::builder().build("app::backend::db", LevelFilter::Info))
        .logger(Logger::builder()
            .appender("requests")
            .additive(false)
            .build("app::requests", LevelFilter::Info))
        .build(Root::builder().appender("stdout").build(LevelFilter::Warn))
        .unwrap();

    let handle = log4rs::init_config(config).unwrap();

    // use handle to change logger configuration at runtime
}
```

上面的程序使用yaml格式的文件`config/log4rs.yml`进行了配置:

```yaml
# config/log4rs.yml
# Scan this file for changes every 30 seconds
refresh_rate: 30 seconds

appenders:
  # An appender named "stdout" that writes to stdout
  stdout:
    kind: console

  # An appender named "requests" that writes to a file with a custom pattern encoder
  requests:
    kind: file
    path: "log/requests.log"
    encoder:
      pattern: "{d} - {m}{n}"

# Set the default logging level to "warn" and attach the "stdout" appender to the root
root:
  level: trace
  appenders:
    - stdout

loggers:
  # Raise the maximum log level for events sent to the "app::backend::db" logger to "info"
  app::backend::db:
    level: info

  # Route log events sent to the "app::requests" logger to the "requests" appender,
  # and *not* the normal appenders installed at the root
  app::requests:
    level: info
    appenders:
      - requests
    additive: false
```

上面的配置说明，我们将输出级别设置为`trace`，将日志打印到标准输出，这样所有级别的日志都可以显示出来了。

编译并运行：

```bash 
cargo run
   Compiling gprl-log4rs-demo v0.1.0 (/home/luoxiangyong/writing-book/rust-in-practice-code/chapter-03/gprl-log4rs-demo)
    Finished dev [unoptimized + debuginfo] target(s) in 1.24s
     Running `target/debug/gprl-log4rs-demo`
2020-02-27T11:20:57.839750717+08:00 ERROR gprl_log4rs_demo - error loging
2020-02-27T11:20:57.839841861+08:00 WARN gprl_log4rs_demo - warn loging
2020-02-27T11:20:57.839916293+08:00 INFO gprl_log4rs_demo - info loging
2020-02-27T11:20:57.839953333+08:00 DEBUG gprl_log4rs_demo - debug loging
2020-02-27T11:20:57.839986843+08:00 TRACE gprl_log4rs_demo - trace loging
```

如果我们将的`root`设置为：

```yaml
root:
  level: trace
  appenders:
    - requests
    - stdout
```

那么所有日志都可以同时输出到控制台和`log/requests.log`文件中了。具体日志相关的框架原理及使用方法，请自行搜索学习！



上面是通过配置文件的方式来设置日志相关参数的，我们当然可以通过程序来设置，代码如下：

```rust
// src/main.rs
use log::{error, warn, info, debug, trace};
use log4rs;

use log::LevelFilter;
use log4rs::append::console::ConsoleAppender;
use log4rs::append::file::FileAppender;
use log4rs::encode::pattern::PatternEncoder;
use log4rs::config::{Appender, Config, Logger, Root};

fn main() {
    // log4rs::init_file("config/log4rs.yml", Default::default()).unwrap();

    let stdout = ConsoleAppender::builder().build();

    let requests = FileAppender::builder()
                    .encoder(Box::new(PatternEncoder::new("{d} - {m}{n}")))
                    .build("log/requests.log")
                    .unwrap();

    let config = Config::builder()
            .appender(Appender::builder().build("stdout", Box::new(stdout)))
            .appender(Appender::builder().build("requests", Box::new(requests)))
            .logger(Logger::builder().build("app::backend::db", LevelFilter::Info))
            .logger(Logger::builder()
                .appender("requests")
                .additive(false)
                .build("app::requests", LevelFilter::Info))
            .build(Root::builder().appender("stdout").build(LevelFilter::Trace))
            .unwrap();

    #[allow(unused_variables)]
    let handle = log4rs::init_config(config).unwrap();

    error!("error loging");
    warn!("warn loging");
    info!("info loging");
    debug!("debug loging");
    trace!("trace loging"); 
}
```

