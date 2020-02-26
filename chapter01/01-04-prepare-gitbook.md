## 安装GitBook文档编写支持

###　本地书写环境

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

#### 创建书目录

使用如下命令创建书目录：

```bash
$ mkdir rust-in-practice
$ cd rust-in-practice
```

####  初始化GitBook

输入初始化指令  `gitbook init` 我们会在书文件夹中得到以下几个文件:

```bash
README.md
SUMMARY.md
```

其中，README文件为说明文档，可以用typora打开，添加我们创建的电子书的说明。

SUMMARY.md文件，此文件为章节目录设置文件，一般我们写博客不会定义章节，定义标题较多，而如果想把创作整合成电子书模式，我们会按章节创作，因此我们可以利用Sumary.md文件进行章节目录划分。请注意，不要用中文文件名存储md文件！

#### 启动GitBook服务

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



关于gitbook的编写几使用，请参考这里：[使用 Gitbook 打造你的电子书](https://zhuanlan.zhihu.com/p/34946169)。

### 代码托管与在线发布

#### 同步到GitHub

首先，登陆GitHub，新建仓库:`rust-in-practice`。

在`rust-in-practice`目录下，使用命令：

```bash
$ git init
```

初始化本地git仓库。

```bash
$ vim .gitignore
```

添加不需要版本控制的文件，如:

```bash
_book/
*.pdf
```

使用如下命令，添加文件到git本地仓库，并`push`到远程仓库：

```bash
$ git add .
$ git commit -m "第一次提交"
$ git remote add origin https://github.com/luoxiangyong/rust-in-practice.git ＃　换成你的代码仓库地址
$ git push -u origin master -f
$ git branch
* master
```

#### 发布到[gitbook.com](https://www.gitbook.com/)上

详细方法请参考这里：[用github做出第一本书](https://blog.csdn.net/hk2291976/article/details/51173850?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)。