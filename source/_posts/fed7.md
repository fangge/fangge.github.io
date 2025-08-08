---
title: MacOS M1芯片 安装NVM 
categories:
  - 前端
tags:
  - 前端开发
  - HTML
  - CSS
  - Javascript
date: 2022-03-03 00:43:40

cover: /img/blog/nvm.jpeg
description: 如何在MacOS 上安装nvm包管理器
---

首先介绍一下自己电脑和系统的情况
{% note info %}
MacBook Pro（14英寸，2021年）
芯片 Apple M1 Pro
MacOs:  12.2.1 Monterey
{% endnote %}

由于在安装nvm上踩了一些坑，所以在这里和大家分享一下在M1芯片上怎么安装nvm包管理器

# 卸载node以及相关模块
```bash
# 查看已经安装在全局的模块
npm ls -g --depth=0
# 删除全局 node_modules 目录
sudo rm -rf /usr/local/lib/node_modules
# 删除 node
sudo rm /usr/local/bin/node 
# 删除全局 node 模块注册的软链
cd /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm
```

# 安装nvm

## 执行安装
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.1/install.sh | bash
```

## 打开终端，进入用户的home目录中
```bash
cd ~/
```

## 检查配置文件
使`ls -a`显示这个目录下的所有文件（夹）（包含隐藏文件及文件夹），查看有没有 `.zshrc`这个文件
```bash
ls -a
```
如果没有，就新建一个
```bash
touch ~/.zshrc
```

## 配置.zshrc文件
将 nvm 环境变量添加到 shell 中, 这里我系统默认的是的zsh而不是bash，需要配置一下，打开.zshrc文件，在最后一行添加
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

## 加载配置文件
```bash
source ~/.zshrc
```

## 测试是否安装成功
```bash
nvm -v
```
如果还是不成功，可以重新编辑.zshrc文件，在末尾添加并保存退出
```bash
source ~/.bash_profile
```

# 安装14以下的版本，先进入Rosetta shell环境
```bash
arch -x86_64 zsh
```

# 使用nvm
可以直接用help命令查看相关用法，一般有以下用法
```
nvm list  // 列出所有已经安装的版本
nvm ls-remote  // 查看所有的node可用的版本
nvm install XXX  // 下载你想要的版本
nvm use XXX  // 使用指定版本的node
nvm current   // 显示当前使用的版本
nvm alias default XXX  // 每次启动终端都使用该版本的node 
```
如果在安装node的时候出现以下提示
> cannot create directory ‘/home/ic/.nvm/alias/lts’: Permission denied
则说明我们没有权限创建这个目录，那么要做的就是修改 .nvm 目录的权限，找到 .nvm 目录位置，执行
```bash
sudo chmod -R 777 .nvm
```


