---
layout: post
title: "oh my zsh和autojump插件"
subtitle: ""
date: 2016-05-09
author: Fengya
category: mac
tags: zsh linux mac autojump
finished: true
---

# oh my zsh和autojump插件

## oh my zsh

官方github网址：https://github.com/robbyrussell/oh-my-zsh

可以根据readme来进行安装。

readme中要求：

1.Unix-based操作系统。（OS X或者是Linux）

2.安装了zsh（可以使用homebrew安装）

3.安装了curl或者是wget

4.安装了git

这四点都满足了可以进行下一步。

如果安装了curl

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

如果安装了wget

```shell
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

即可以完成安装。

之后可以参阅网上的教程和`~/.zshrc`中的说明对风格进行进一步设置。这里我使用了`ys`主题。并只安装了autojump插件。

## 安装autojump插件

autojump插件可以用于直达常用目录。也可以手动设置权重数据库，很方便。

安装autojump可以使用homebrew，就不赘述了。

安装之后，根据安装时的提示，按照不同的系统将autojump的命令加入`~/.zshrc`中即可。

使用方法：

1.autojump [目录名字的一部分]

autojump加一部分名字即可自动跳转到数据库中对应目录去。使用了oh my zsh还可以敲tab自动补全用于直观的选择，很方便。

2.autojump有一个自带的alias：j。

因此简单的输入

```shell
j dir
```

就可以转到对应的目录去。

3.对于权重数据库的访问。

autojump可以修改目录数据库来达到自定义想要的目录的效果。

```shell
$ autojump -a [dir]
# 在数据库中添加一个目录

$ autojump -i [value]
# 提升当前目录value数目的权重

$ autojump -d [value]
# 降低当前目录的权重

$ autojump -s
# 显示数据库中的统计数据

$ autojump --purge
# 清除不再需要的目录
```

