---
title: hexo环境搭建
date: 2018-03-06 00:06:53
tags: hexo
---

如果你没有接触了解hexo，当你看到这个页面时，可能会想这是怎么创建的呢？

首先你要知道这是GitHub+hexo，然后网上搜一下就有很多教程了。

## 如何搭建

这里引用官方教程，参考[hexo](https://hexo.io)官网。简单来讲：

1. 安装node.js
2. 安装git（如果需要使用git发布）
3. 安装hexo
4. 编写md文件并发布到github pages

#### Install Node.js 

使用[Node Version Manager](https://github.com/creationix/nvm)安装，先安装nvm

```bash
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

然后重启终端，使用nvm安装Node.js

```bash
$ nvm install stable
```

#### Install Hexo

```bash
$ npm install -g hexo-cli
```

Once Hexo is installed, run the following commands to initialise Hexo in the target `<folder>`.

```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

## Quick Start

### Create a new post

```bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

```bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

```bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

```bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)