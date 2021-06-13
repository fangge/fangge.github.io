---
title: Hexo+Github Action构建Github Page博客
index_img: /img/blog/fed4.jpg
banner_img: /img/blog/fed4.jpg
categories:
  - 前端
tags:
  - 前端开发
  - Github
  - Hexo
  - HTML
  - CSS
  - Javascript
date: 2021-06-13 15:17:14
---

最近重拾了Github page，之前有了解到结合[**Github Action**](https://github.com/features/actions)这个利器，现在发文章更流畅了，正好这段时间想着重新写写博客，对自己技术做一些反思和沉淀，所以就重新折腾了一次，在这里做一个生成Github Page的总结，可以避免各位踩坑

{% note primary %}
本篇适合基础入门者从头学习怎么建立github page
{% endnote %}

## Github项目

1. 首先Github建一个**你的github用户名.github.io**的项目，比如我的github是github.com/fangge，fangge就是我的用户名，所以要建一个fangge.github.io的项目
2. 建好后，进入项目中的**Setting**中，点击左侧的**Pages**，选择你要显示的你的Page的分支和目录，保存后你的github page就已经可以访问了，github本身还提供了几款简单的主题供你选择
![GitHub Page](/img/blog/gpsetting.jpg)

到这里了如果你觉得已经可以满足你的个人需要了，那你就可以尽情在你设置的分支里自己写各种你所需要的html文件，然后githubpage就会自动去读取里面的文件，但我们当然不能止步于此，加油继续折腾

## Hexo
{% note info %}
Hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Heroku上，是搭建博客的首选框架。。Hexo同时也是GitHub上的开源项目，参见：[hexojs/hexo](https://github.com/hexojs/hexo) 如果想要更加全面的了解Hexo，可以到其官网 Hexo 了解更多的细节，因为Hexo的创建者是台湾人，对中文的支持很友好，可以选择中文进行查看。
{% endnote %}

- 毕竟总是手写html不方便，我们写博客，程序员当然最喜欢用Markdown来写，而Hexo就是其中最好的选择之一，我们首先全局安装hexo命令
```bash
npm install hexo-cli -g
```

- 然后clone 刚才你建好的github.io的项目，输入命令``git branch``来建一个新的分支并``checkout``
> **这一步记得做，之后用Github Action会涉及到**
```bash
git branch xxx
git checkout xxx
```
- 在你新建好的``xxx``目录下，开始初始化hexo博客
```bash
hexo init
```

- 初始完成后，直接start命令就可以本地预览你的博客了
```bash
hexo s
```

- 写作的话都是在``source``里面的``_posts``去编辑对应的文章markdown，你可以用方便的hexo命令来生成文章
```bash
hexo new xxx
```
xxx就是生成的markdown的文件名，hexo的写作还有一些文档和注意事项，可以在文档[Front-matter](https://hexo.io/zh-cn/docs/writing)中查看，如果你希望用不同的模板来新建文章，可以在``scaffolds``中新建一个markdown模板，然后新建文章的时候，可以直接写``(比如aaa就是你新建的aaa.md的模板)``
```bash
hexo new aaa xxx
```
- 预览完毕后，可以直接用generate命令来生成博客，也就是hexo会将你的markdown，生成githubpage所需要的html文件，生成目录是``public``
```bash
hexo g
```

那么到这一步，其实你就可以手动将public中的文件，复制到你的master分支，提交后访问你的Github Page地址，就可以看到Hexo生成好的博客了
> 复制到你GithubPage设置的分支去，不一定是master

## Github Action

但这样每次都要复制也太麻烦了，有没有什么方法可以只要我们提交当前分支，Github就自动帮我们生成博客呢，答案当然是肯定的，下面有请神奇的Github Action
{% blockquote Github Action -  https://github.com/features/actions %}
大家知道，持续集成由很多操作组成，比如抓取代码、运行测试、登录远程服务器，发布到第三方服务等等。GitHub 把这些操作就称为 actions。

很多操作在不同项目里面是类似的，完全可以共享。GitHub 注意到了这一点，想出了一个很妙的点子，允许开发者把每个操作写成独立的脚本文件，存放到代码仓库，使得其他开发者可以引用。

如果你需要某个 action，不必自己写复杂的脚本，直接引用他人写好的 action 即可，整个持续集成过程，就变成了一个 actions 的组合。这就是 GitHub Actions 最特别的地方。 - 阮一峰
{% endblockquote %}

所以我们现在的目标就是，只要提交我们的hexo代码后，也就是提交到我们的博客分支后，action自动帮我们运行``hexo c && hexo g``生成博客文件，然后把生成的产物推送到master分支，那以后我们只要关注我们的markdown代码即可，其他的都是action帮我们处理好，那么现在开始action教程

### 新建一个Access Token

进入[个人设置页](https://github.com/settings/tokens)新建一个Access Token
![AccessToken](/img/blog/ghat.jpg)

直接全选所有选项后，点击**Generate token**
![](/img/blog/20210613175425.jpg)

生成后请自行保存好生成的token，如果关掉后就不再显示了

### 设置个人Secrets
回到github.io项目的Settings中，新建一个token，名字是**ACCESS_TOKEN**，值是刚才你保存的access token，如图
![](/img/blog/20210613175950.jpg)

### 新建action的yml文件
回到刚才博客源码的那个分支，新建目录``.github/workflows``，然后新建一个**main.yml**文件，到时候Action就会读取这个文件来做对应的操作，将以下代码复制进去
```
name: Blog CI/CD

# 触发条件：在 push 到 hexo-blog-backup 分支后触发
on:
  push:
    branches: 
      - hexo-backup

env:
  TZ: Asia/Shanghai

jobs:
  blog-cicd:
    name: Hexo blog build & deploy
    runs-on: ubuntu-latest # 使用最新的 Ubuntu 系统作为编译部署的环境

    steps:
    - name: Checkout codes
      uses: actions/checkout@v2

    - name: Setup node
      # 设置 node.js 环境
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'

    - name: Cache node modules
      # 设置包缓存目录，避免每次下载
      uses: actions/cache@v1
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

    - name: Install hexo dependencies
      # 下载 hexo-cli 脚手架及相关安装包
      run: |
        npm install -g hexo-cli
        npm install

    - name: Generate files
      # 编译 markdown 文件
      run: |
        hexo clean
        hexo generate

    - name: Deploy hexo blog
      env: 
        # Github 仓库
        GITHUB_REPO: github.com/fangge/fangge.github.io
      # 将编译后的博客文件推送到指定仓库
      run: |
        cd ./public && git init && git add .
        git config user.name "mrfangge"
        git config user.email "fangge-sun@163.com"
        git add .
        git commit -m "GitHub Actions Auto Builder at $(date +'%Y-%m-%d %H:%M:%S')"
        git push --force --quiet "https://${{ secrets.ACCESS_TOKEN }}@$GITHUB_REPO" master:master
```
### 万事俱备，只欠push

操作万上面所有东西后，你就可以直接push你这个博客源码分支了，然后你可以去项目的action那里看到对应的进程详情，成功与否都能知道是什么问题
![](/img/blog/20210613180630.jpg)

当成功后，master就能看到新生成的blog文件