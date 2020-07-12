+++
title = "Hugo 入门教程"
date = 2020-07-12T00:00:00+08:00
lastmod = 2020-07-12T21:11:42+08:00
tags = ["hugo", "emacs"]
draft = false
+++

第 N 次 Hugo 入门，这次重新捡回 Hugo 主要原因是打算使用 org-mode 来管理我的笔记与博文，那么 ox-hugo 自然是不可或缺的，所以重新写了篇 Hugo 入门教程。如果想深入了解 Hugo 的话，推荐看它的[官方文档](https://gohugo.io/documentation/)

<!--more-->


## 安装 {#安装}

```shell
# macOS
brew install hugo

# archlinux
sudo pacman -Syu hugo
```

其他的安装方式推荐看官方文档，[Install Hugo](https://gohugo.io/getting-started/installing/)


## 快速入门 {#快速入门}


### 创建站点 {#创建站点}

```shell
hugo new site /path/to/blog
```


### 安装主题 {#安装主题}

单单创建了站点还是不能够使用的，还需要安装一个主题，这里推荐我使用的主题，[zozo](https://github.com/varkai/hugo-theme-zozo)

```shell
cd /path/to/blog
git clone https://github.com/varkai/hugo-theme-zozo themes/zozo
```

zozo 主题同时还需要去覆盖配置文件

```shell
cp themes/zozo/exampleSite/config.toml .
```

为了方便后面更新主题，所以最好是使用 [Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) 来管理主题，这样可以方便后续更新主题

```shell
touch .gitmodules
```

配置如下

```nil
[submodule "theme"]
path=themes/zozo
url=git://github.com/varkai/hugo-theme-zozo.git
```

配置好后，记得初始化

```shell
git submodule init
git submodule update --init --recursive
```

更新 submodule 代码

```shell
git submodule update [--remote]
```


### 创建一篇文章 {#创建一篇文章}

```shell
hugo new posts/hello-world.md
```

Hugo 会自动生成 `content/posts/hello-world.md`

```markdown
---
title: "Hello World"
date: 2020-07-12T20:49:42+08:00
draft: true
---
```

其中 draft 表示为草稿


### 本地浏览 {#本地浏览}

```shell
hugo server
```

接下来在浏览器打开 `localhost:1313` 即可，不过由于 `hello-word.md` 是草稿，所以要用下面的命令

```shell
hugo server --buildDrafts
# or
hugo server -D
```


### 编译 {#编译}

```shell
hugo
```

编译输出的静态 html 文件，默认保存到 `public` 目录下


## 部署 {#部署}
