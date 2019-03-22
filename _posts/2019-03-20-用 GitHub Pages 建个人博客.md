---
layout: post
title: 用 GitHub Pages 建个人博客
tags: github blog
---

写博客对于程序员来说，应该算一个优秀的习惯。之前一直把个人主页挂在阿里云上，最近被通知阿里云主机将被到期释放了，于是决定用 GitHub Pages 建个人博客，免费而且没有空间流量限制，彻底远离买空间、域名和备案的烦恼，而且还具有独立自主权

### 什么是 GitHub Pages 呢？

GitHub Pages 是 GitHub 提供的一个网页寄存服务，于 2008 年推出  
可以用于存放静态网页，包括博客、项目文档甚至整本书  
一般 GitHub Pages 的网站使用 github.io 的子域名，但是用户也可以使用第三方域名

### 创建仓库

[Create a new repository](https://github.com/new)

- Repository name
    - dymyw.github.io（dymyw 换成你的 github 名称）
- ✅ Public
- ✅ Initialize this repository with a README

Settings

- GitHub Pages
    - Theme Chooser

打开 [https://dymyw.github.io/](https://dymyw.github.io/) 就这么简单（dymyw 换成你的 github 名称）

### Jekyll

GitHub Pages 通过运行 [Jekyll](https://www.jekyll.com.cn/) 来构建你的站点  
它是一个简单免费的生成博客网页的工具

借助 RubyGems 安装 Jekyll、bundle

    sudo gem install jekyll bundle

> Rubyems 的功能类似于 linux 下的 apt-get，是个包管理器，可以从远程下载所需的包  
> 所有的 Jekyll 的 gem 依赖包都会被自动安装，所以你完全不用去担心  
> bundle 类似于 composer，用来管理 gem 包

安装 Jekyll 插件

    sudo gem install jekyll-paginate

> 分页插件

创建一个项目（一般来说不需要，通常会选择喜欢的主题）

    jekyll new blog

选择喜欢的主题 [Jekyll Themes](http://jekyllthemes.org/)

    // 下载、解压、进入目录

    // 创建 Gemfile 文件（如果没有）
    bundle init

        // 编辑 Gemfile 文件，添加依赖（类似于 composer 中的 composer.json）
            // 查看 _config.yml - plugins
        source 'https://rubygems.org'
        gem 'jekyll-paginate'
        gem 'jekyll-sitemap'
        gem 'jekyll-feed'

    // 安装项目下 Gemfile 文件中指定的项目依赖库
    bundle install
        // 会生成一个 Gemfile.lock（类似于 composer 中的 composer.lock）

    // 构建页面
    jekyll build

    // 开启本地浏览模式（构建页面 + web 服务，开启状态下实时构建）
    jekyll serve
        // 或者
        bundle exec jekyll server

        // 查看进程
        ps aux | grep jekyll
        // 关闭进程
        kill -9 pid

Jekyll 目录结构

    .
    ├── _config.yml         保存配置数据
    ├── _drafts             未发布的文章，没有日期（构建加 --drafts 参数）
    |   ├── begin-with-the-crazy-ideas.textile
    |   └── on-simplicity-in-technology.markdown
    ├── _includes           你可以加载这些包含部分到你的布局或者文章中以方便重用
    |   ├── footer.html
    |   └── header.html
    ├── _layouts            包裹在文章外部的模板
    |   ├── default.html
    |   └── post.html
    ├── _posts              文章目录，文件格式：YEAR-MONTH-DAY-title.MARKUP
    |   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
    |   └── 2009-04-26-barcamp-boston-4-roundup.textile
    ├── _data
    |   └── members.yml
    ├── _site               一旦 Jekyll 完成构建，就会将生成的页面放在这里（默认）
    └── index.html

> 最好将 _site 放进你的 .gitignore 文件中  
> 还有一些 css/、image/ 和 favicon.ico 等未被提及的目录和文件

_config.yml 配置文件

    // 读取文件的目录
    source: DIR

    // 写入文件的目录
    destination: DIR

    // 转换时排除某些文件夹或文件
    exclude: [DIR, FILE, ...]

    // 转换时强制包含某些文件、文件夹
    include: [DIR, FILE, ...]
        // .htaccess 是个典型的例子

    // 设置时区
    timezone: TIMEZONE
        // 比如 America/New_York 。默认值为操作系统的时区

    // 设置文件的编码，仅 Ruby 1.9 以上可用
    encoding: ENCODING

    // 开启分页功能
    paginate: number

YAML 头信息

    正是头信息开始让 Jekyll 变的很酷
    任何只要包含 YAML 头信息的文件在 Jekyll 中都能被当做一个特殊的文件来处理

    必须在文件的开始部分，并且需要按照 YAML 的格式写在两行三虚线之间

        ---
        layout: post
        title: Blogging Like a Hacker
        ---

            layout      指定使用该模板文件
                            不需要扩展名，模板文件需要放在 _layouts 目录下
            permalink   让你的博客中的 URL 地址不同于默认值
                            /year/month/day/title.html 这样
            category    设置一个分类属性
                            test
            categories  设置多个分类属性
                            [test, sample]
                            test sample
                                - github
                                - blog
            tags        增加一个或者多个标签
                            [test, sample]
                            test sample
                                - github
                                - blog

永久连接

    可以在 _config.yml、YAML 头信息上加 permalink 来实现

    模版变量
        year        文章所在文件的年份
        month       文章所在文件的月份，格式如 `01, 10`
        i_month     文章所在文件的月份，格式如 `1, 10`
        day         文章所在文件的日期，格式如 `01, 20`
        i_day       文章所在文件的日期，格式如 `1, 20`
        title       文章所在文件的标题
        categories  为文章配置的目录
                        Jekyll 可以自动将 `//` 转换为 `/` ，所以如果没有目录，会自动忽略
    已经建好的链接类型
        date        /:categories/:year/:month/:day/:title.html
        pretty      /:categories/:year/:month/:day/:title/
        none        /:categories/:title.htm

    也可以自定义，如
        permalink: /:title/

常用变量

    site        来自 _config.yml 文件，全站范围的信息 + 配置

        site.time                       当前时间（跑 jekyll 这个命令的时间点）
        site.pages                      所有 Pages 的清单
        site.posts                      时间倒叙的所有 Posts 的清单
        site.categories.CATEGORY        所有的在 CATEGORY 类别下的帖子
        site.tags.TAG                   所有的在 TAG 标签下的帖子 

    page        页面专属的信息 + YAML 头文件信息

        page.content                    页面内容的源码
        page.title                      页面的标题
        page.excerpt                    页面摘要的源码
        page.url                        帖子以斜线打头的相对路径
                                            /2008/12/14/my-post.html
        page.date                       帖子的日期
        page.id                         帖子的唯一标识码（在RSS源里非常有用）
                                            /2008/12/14/my-post
        page.categories                 这个帖子所属的 Categories
        page.tags                       这个 Post 所属的所有 tags
        page.path                       Post 或者 Page 的源文件地址

    content     被 layout 包裹的那些 Post 或者 Page 渲染生成的内容

    paginator   每当 paginate 配置选项被设置了的时候，这个变量就可用了

        paginator.per_page              每一页 Posts 的数量
        paginator.posts                 这一页可用的 Posts
        paginator.total_posts           Posts 的总数
        paginator.total_pages           Pages 的总数
        paginator.page                  当前页号
        paginator.previous_page         前一页的页号
        paginator.previous_page_path    前一页的地址
        paginator.next_page             下一页的页号
        paginator.next_page_path        下一页的地址

创建页面

    index.html 主页

    其他页面
        放在站点的根目录下
            .
            |-- _config.yml
            |-- _includes/
            |-- _layouts/
            |-- _posts/
            |-- _site/
            |-- about.html    # => http://example.com/about.html
            |-- index.html    # => http://example.com/
            └── contact.html  # => http://example.com/contact.html

        在站点的根目录下为每一个页面创建一个文件夹
        并把 index.html 文件放在每 个文件夹里
            .
            ├── _config.yml
            ├── _includes/
            ├── _layouts/
            ├── _posts/
            ├── _site/
            ├── about/
            |   └── index.html  # => http://example.com/about/
            ├── contact/
            |   └── index.html  # => http://example.com/contact/
            └── index.html      # => http://example.com/

        两种方法都可以工作，访问的 URL 不同而已

引用图片和其它资源

    一种常用做法是在工程的根目录下 创建一个文件夹，命名为 assets 或者 downloads
    将图片文件，下载文件或者其它的 资源放到这个文件夹下

    ![有帮助的截图]({{ site.url }}/assets/screenshot.jpg)

    你可以直接 [下载 PDF]({{ site.url }}/assets/mydoc.pdf)

### 文章发布

[使用 Travis CI 自动部署 Github Pages]({% post_url 2019-03-21-使用 Travis CI 自动部署 Github Pages %})

### 绑定域名

在根目录下新建一个名为 CNAME 的文本文件，写入你要绑定的域名，如 `www.mayw.tech`

DNS 要新建一条 CNAME 记录，指向 dymyw.github.io（dymyw 换成你的 github 名称）

### 评论功能设置（未完成）

登录 [Disqus](https://disqus.com/) 注册一个账号（开 vpn 比较快点）

Settings - Add Disqus To Site

- Site name
- unique disqus URL（shortname）

设置 `_config.yml` 中的 disqus，填入 shortname

在需要开启评论功能的文章的 YAML 头信息加一行 `comments: true` 就好了

### 访问统计

使用 [百度统计](https://tongji.baidu.com/)

### Todo

- 评论
- 图片（像某牛云存储之类的云存储服务）
- 源码语法高亮
