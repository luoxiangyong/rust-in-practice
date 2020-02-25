# 环境准备

## 操作系统准备

本书的实验环境为Ubuntu 18.04:

```bash
$ uname -a
Linux mail.topgridcloud.com 4.15.0-46-generic #49-Ubuntu SMP Wed Feb 6 09:33:07 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.1 LTS
Release:	18.04
Codename:	bionic
```

Windows环境的用户可以到[VirtualBox](https://www.virtualbox.org/)安装虚拟机，前往[Ubuntu网站](https://ubuntu.com/#download)下载18.04 LTS版本的iso镜像，自行安装操作系统环境。

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



## 安装Markdown编辑器

[Typora](https://www.typora.io/#linux)是一款非常实用的ｍarkdown文档编辑器，推荐使用。

```shell
# or run:
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

$ wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

# add Typora's repository

$sudo add-apt-repository 'deb https://typora.io/linux ./'

$ sudo apt-get update

# install typora

$ sudo apt-get install typora
```

我一般使用它编写一些文档。Typora的使用请参考这里：[Typora 完全使用详解](https://sspai.com/post/54912)。



## 安装GitBook文档编写支持

GitBook 是一个基于 Node.js 的命令行工具，支持 Markdown 和 AsciiDoc 两种语法格式，可以输出 HTML、PDF、eBook 等格式的电子书。本书就是使用GitBook写成的。

首选准备Node.js环境：

```bash
$ sudo apt-get install nodejs npm
```

验证安装：

```bash
$ node -v
v8.10.0
$ npm -v
3.5.2
```

在安装完node.js并验证成功后，打开命令行，输入如下代码安装GitBook服务端

```bash
$ sudo npm install -g gitbook-cli
```

安装完毕后，使用以下命令进行验证

```bash
$ gitbook -V
CLI version: 2.3.2
GitBook version: 3.2.3
```

注意：此处**V**必须是大写

### 创建书目录

使用如下命令创建书目录：

```bash
$ mkdir rust-in-practice
$ cd rust-in-practice
```

### 初始化GitBook

输入初始化指令  `gitbook init` 我们会在书文件夹中得到以下几个文件:

```bash
README.md
SUMMARY.md
```

其中，README文件为说明文档，可以用typora打开，添加我们创建的电子书的说明。

SUMMARY.md文件，此文件为章节目录设置文件，一般我们写博客不会定义章节，定义标题较多，而如果想把创作整合成电子书模式，我们会按章节创作，因此我们可以利用Sumary.md文件进行章节目录划分。请注意，不要用中文文件名存储md文件！

### 启动GitBook服务

输入 `gitbook serve`指令，启动gitbook服务:

```bash
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed 
info: loading plugin "livereload"... OK 
info: loading plugin "highlight"... OK 
info: loading plugin "search"... OK 
info: loading plugin "lunr"... OK 
info: loading plugin "sharing"... OK 
info: loading plugin "fontsettings"... OK 
info: loading plugin "theme-default"... OK 
info: found 1 pages 
info: found 1 asset files 
info: >> generation finished with success in 0.6s ! 

Starting server ...
Serving book on http://localhost:4000
```

看到如上界面表示GitBook服务成功启动，我们访问命令行最下方的链接:
http://localhost:4000

在所有内容编辑完毕后，在命令行中输入指令 `gitbook build`可以在当前目录的`_book`下生成html文档。如果想生成pdf文档，请安装：

```bash 
$ sudo apt-get install calibre
```

然后使用如下命令即可：

```bash
$ gitbool pdf
```



### 同步到GitHub

首先，登陆GitHub，新建仓库:`rust-in-practice`。

在`rust-in-practice`目录下，使用命令：

```bash
$ git init
```
