---
title: Windows环境设置Git 快捷命令【alias】
categories:
  - 前端
tags:
  - 前端开发
  - Javascript
date: 2022-04-22 15:58:33
index_img: /img/blog/git.jpg
banner_img: /img/blog/git.jpg
excerpt: window环境设置git快捷命令，且修改Vscode的默认终端
---

笔者最近想着设置一下git的快捷命令，之前在mac环境就直接设置``.gitconfig``文件即可，然后来到windows发现找到同样的文件后设置没有反应，后来搜了相关教程，踩了一些坑，终于找到对的方法了，和大家分享一下

## 设置alias

找到Git的安装目录下的``bash.bashrc``文件
```bash
Git安装目录\etc\bash.bashrc
```
直接用编辑器打开并插入如下代码

```bash
alias g='git'
alias ga='git add'
alias gaa='git add --all'
alias gapa='git add --patch'
alias gau='git add --update'
alias gav='git add --verbose'
alias gap='git apply'
alias gcp='git add . && git commit --m "修改问题" && git push'

alias gb='git branch'
alias gba='git branch -a'
alias gbd='git branch -d'
alias gbda='git branch --no-color --merged | command grep -vE "^(\+|\*|\s*($(git_main_branch)|development|develop|devel|dev)\s*$)" | command xargs -n 1 git branch -d'
alias gbD='git branch -D'
alias gbl='git blame -b -w'
alias gbnm='git branch --no-merged'
alias gbr='git branch --remote'
alias gbs='git bisect'
alias gbsb='git bisect bad'
alias gbsg='git bisect good'
alias gbsr='git bisect reset'
alias gbss='git bisect start'

alias gc='git commit -v'
alias gc!='git commit -v --amend'
alias gcn!='git commit -v --no-edit --amend'
alias gca='git commit -v -a'
alias gca!='git commit -v -a --amend'
alias gcan!='git commit -v -a --no-edit --amend'
alias gcans!='git commit -v -a -s --no-edit --amend'
alias gcam='git commit -a -m'
alias gcsm='git commit -s -m'
alias gcb='git checkout -b'
alias gcf='git config --list'
alias gcl='git clone --recurse-submodules'
alias gclean='git clean -id'
alias gpristine='git reset --hard && git clean -dffx'
alias gcm='git checkout $(git_main_branch)'
alias gcd='git checkout develop'
alias gcmsg='git commit -m'
alias gco='git checkout'
alias gcount='git shortlog -sn'
alias gcs='git commit -S'

alias gd='git diff'
alias gdca='git diff --cached'
alias gdcw='git diff --cached --word-diff'
alias gdct='git describe --tags $(git rev-list --tags --max-count=1)'
alias gds='git diff --staged'
alias gdt='git diff-tree --no-commit-id --name-only -r'
alias gdw='git diff --word-diff'
```
> 大家可以自行定义喜欢的快捷命令

## 设置默认终端
到了这一步我以为完成了，然后发现windows的默认cmd或者powershell都无法识别到新加的命令
![无法识别命令](/img/blog/fed10.png)

后来才了解到加的命令只有**Git Bash**能使用，之前我的Vscode一直都是默认使用cmd来作为默认终端的，所以需要修改默认终端为Git Bash才行

### 进入Vscode的JSON setting中添加配置
1. ``Ctrl+Shift+p``输入``setting``
![](/img/blog/fed10_1.png)
2. 找到``terminal.integrated.profiles.windows``修改配置如下，请注意path那里就是填你git安装目录下的``bin``目录下的bash.exe
```json
  "terminal.integrated.profiles.windows": {
    "PowerShell": {
      "source": "PowerShell",
      "icon": "terminal-powershell"
    },
    "Command Prompt": {
      "path": [
        "${env:windir}\\Sysnative\\cmd.exe",
        "${env:windir}\\System32\\cmd.exe"
      ],
      "args": [],
      "icon": "terminal-cmd"
    },
    "Git-Bash": {
      // "source": "Git Bash",
      "path": "D:\\software\\Git\\bin\\bash.exe",
      "args": []
    }
  },
  "terminal.integrated.defaultProfile.windows": "Git-Bash"
```
3. 重启vscode

## 完美
之后``Ctrl+~``你就可以看到已经切换终端成功了，顺便舒服地用上快捷命令

