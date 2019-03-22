---
layout: post
title: 通过 SSH 连接 GitHub
categories: work tech
tags: github ssh
---

GitHub 每次 pull/push 代码时都会验证用户的合法性，而 SSH 是一种安全的传输模式，可以代替用户输入账号密码的繁琐

### Global

    $ git config --list
    $ git config --global user.name 'dymyw'
    $ git config --global user.email 'dymayongwei@163.com'

### GitHub

创建一个新的 SSH Key

    $ ssh-keygen -t rsa -C 'dymayongwei@163.com'

添加 SSH Key 到你的 GitHub 账号

    $ cat ~/.ssh/id_rsa.pub

> 保存到 `GitHub - Settings - SSH and GPG keys - New SSH key - Key`

测试 SSH 连接

    $ ssh -T git@github.com

### 参考

[Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/)
