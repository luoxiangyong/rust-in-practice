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

