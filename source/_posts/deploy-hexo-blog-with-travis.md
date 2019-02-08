---
title: 使用 Travis 部署 Hexo 博客
description: 相信很多人都有自己的静态博客，但是部署方式可能不是采用自动化部署的方式。像我之前就是通过手动编译和部署的方式来部署我的博客到VPS上，Travis 是一款免费的自动化部署工具，接下来我将介绍如何用 Travis 来部署静态博客
cover: https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190208165724.png
date: 2019-02-04
tag: ["travis", "git", "hexo"]
---

首先我是默认看这篇博文的你是已经有自己的静态博客了，只是还缺少一套可以自动化部署博客的手段。因为我的电脑是 macOS 操作系统，所以博文中的操作都是基本 macOS 操作系统的，Windows 操作系统上不一定适用

## 注册登录 Travis

打开 Chrome 浏览器，进入 Travis 的[官网](https://travis-ci.com)，记住是 `.com` 结尾的，而不是 `.org` 结尾的。接下来凭你的直觉去添加项目

![](https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190204090319.png)

## 安装 Travis

在安装 Travis 前首先我们得重新安装 Ruby 环境，macOS 自带的 Ruby 环境太老了

```bash
brew install ruby
```

添加环境变量，由于我用的是 zsh ，所以我是在 `~/.zshrc` 中添加环境变量

```bash
# ruby
export PATH="/usr/local/opt/ruby/bin:$PATH"

# gem
export PATH="$PATH:`gem env gemdir`/bin"
```

重新加载配置

```bash
source ~/.zshrc
```

进入正戏，开始安装 Travis

```bash
gem install travis
```

执行下 `travis`，如果有反应说明就是安装完成了

## 登录 Travis

到你的博客目录执行

```bash
travis login --com --auto
```

按照它的提示进行输入即可登录成功

## 申请 GITHUB_TOKEN

打开 Github 的 [Personal access tokens](https://github.com/settings/tokens)，然后点击 `Generate new token` 按钮

![](https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190204091334.png)

权限部分我们只需要勾选 `repo`

为了防止 GITHUB TOKEN 泄露，我们添加一个环境变量。在你的 Travis 项目中对应的 Setting (More options -> Settings) 页面添加环境变量

![](https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190204092112.png)

## 配置 .travis.yml

在你的博客根目录新建 `.travis.yml`

```yaml
language: node_js
sudo: required
node_js: stable
cache:
  yarn: true
  directories:
    - node_modules
branchs:
  only:
    - master
before_script:
  - curl -o- -L https://yarnpkg.com/install.sh | bash
  - export PATH="$HOME/.yarn/bin:$PATH"
  - yarn install
  - yarn global add hexo-cli
script:
  - hexo clean
  - hexo generate
# 部署方式为 github page
# master 分支有提交的话触发 Travis 自动部署
# 编译完成后，将 public/ 目录下的代码提交到仓库的 gh-pages 分支
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_TOKEN
  keep-history: true
  on:
    branch: master
  target-branch: gh-pages
  local-dir: public/
  verbose: true
```

之后当你在 master 分支提交代码后，就会自动触发部署了

## 配置自定义域名

Github page 提供自定义域名配置，打开你的博客仓库的的 Settings 页面进行配置

![](https://tuchuang-1253782374.cos.ap-guangzhou.myqcloud.com/20190208001910.png)

然后在博客根目录的 `source` 目录下添加一个 `CNAME` 文件，然后写入你的域名

```bash
blog.helbing.cn
```

不用担心 HTTPS 证书的问题，只要你勾选了 `Enforce HTTPS`，Github 会自动帮你到 [letsencrypt](https://letsencrypt.org/) 那里申请免费的 HTTPS 证书
