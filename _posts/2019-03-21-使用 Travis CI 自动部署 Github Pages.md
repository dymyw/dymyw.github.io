---
layout: post
title: 使用 Travis CI 自动部署 Github Pages
tags: 
    - github
    - blog
    - travis ci
---

写博客更应该关注的是文章内容。每次写好文章后，还要手动运行命令来构建页面，操作过于繁琐。可以使用 Travis CI 或是 Jenkins 这样的自动化部署工具来实现，最后我选择的是前者，原因是免费

### Travis CI

编写代码只是软件开发的一小部分，更多的时间往往花在构建（build）和测试（test）  
为了提高软件开发的效率，构建和测试的自动化工具层出不穷。`Travis CI` 就是这类工具之中 [市场份额](https://github.blog/2017-11-07-github-welcomes-all-ci-tools/) 最大的一个

### 持续集成

Travis CI 提供的是持续集成服务（Continuous Integration，简称 CI）。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。

持续集成指的是只要代码有变更，就自动运行构建和测试，反馈运行结果。确保符合预期以后，再将新代码"集成"到主干。

持续集成的好处在于，每次代码的小幅变更，就能看到运行结果，从而不断累积小的变更，而不是在开发周期结束时，一下子合并一大块代码。

### 准备工作

Travis CI 只支持 Github，不支持其他代码托管服务。这意味着，你必须满足以下条件，才能使用 Travis CI

使用 Github 账户登入 [Travis CI](https://travis-ci.org/)  
激活需要 Travis 帮你构建的仓库

.travis.yml

    Travis 要求项目的根目录下面，必须有一个 .travis.yml 文件
    必须保存在 Github 仓库里，一旦代码仓库有新的 Commit，Travis 就会去找这个文件，执行里面的命令

    这个文件采用 YAML 格式

        language            指定了默认运行环境
        sudo                是否需要 sudo 权限
                                sudo: required 需要 sudo 权限
        install             用来指定安装脚本
                                一个
                                    install: ./install-dependencies.sh
                                多个
                                    install:
                                        - command1
                                        - command2
                                跳过
                                    install: true
        script              指定要运行的脚本
                                script: true 表示不执行任何脚本，状态直接设为成功

        7 个钩子
        before_install      install 阶段之前执行
        before_script       script 阶段之前执行
        after_failure       script 阶段失败时执行
        after_success       script 阶段成功时执行
        before_deploy       deploy 步骤之前执行
        after_deploy        deploy 步骤之后执行
        after_script        script 阶段之后执行

运行流程

- install 阶段：安装依赖
- script 阶段：运行脚本

> 注意，script 与 install 不一样  
> 如果 script1 失败，script2 会继续执行。但是，整个构建阶段的状态是失败  
> 如果 command2 只有在 command1 成功后才能执行，就要写成下面这样
>> script: command1 && command2

完整的生命周期

1. before_install
2. install
3. before_script
4. script
5. aftersuccess or afterfailure
6. [OPTIONAL] before_deploy
7. [OPTIONAL] deploy
8. [OPTIONAL] after_deploy
9. after_script

运行状态

- passed：运行成功，所有步骤的退出码都是0
- canceled：用户取消执行
- errored：before_install、install、before_script有非零退出码，运行会立即停止
- failed ：script有非零状态码 ，会继续运行

### 具体操作

在博客根目录下建立并配置 `.travis.yml` 文件

    language: ruby
    install: gem install bundler:2.0.1
    script:
      - bundle install
      - bundle exec jekyll build

### 参考

[持续集成服务 Travis CI 教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)
