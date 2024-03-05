---
title: WakaTime Dashboard Pro - 更新日志
categories:
  - 前端
tags:
  - 前端开发
  - HTML
  - CSS
  - Javascript
date: 2021-11-20 23:51:16
index_img: /img/blog/fed5.jpg
banner_img: /img/blog/fed5.jpg
excerpt: 利用WakaTime 数据打造自己的开发数据图表
---

前篇WakaTime Dashboard介绍：
> [WakaTime Dashboard Pro](/2021/06/29/fed5/)

[项目地址](https://github.com/fangge/wakatime-dashboard-pro)
[项目 Demo](https://wakatime.mrfangge.com/)

距离上一次更新项目已经有一段时间，这次主要重新介绍一下这个项目可能会遇到的一些问题和做出的一些更新

## 使用问题

1. github中的gist同步是有限制的，笔者自测同步到300天多一些的时候，github的api，也就是**https://api.github.com/gists/xxx**这个核心接口，只会展示部分内容，比如我的gist现在每天都同步，但是接口只是返回到2021-10-27的json，也就是后面的json单纯用请求github的api的方式，是没办法看到的，解决方法要么是大家自行clone一下代码然后改成获取本地json（也就是自己找方法把gist中的json同步到本地或其他地方），要么就只能删掉前面一些月份的json来让后面同步的东西接口能返回过来
> 利用[wakatime-sync](https://github.com/superman66/wakatime-sync)将你的每天的数据同步到 gist 中（原理是利用[Github Action](https://docs.github.com/en/actions)每天定时请求 wakatime 的免费 api，获取 json 文件后再上传到 gist 中）

{% note info %}
如果有需要的朋友，也可以直接看我的另一个修改为[<u>获取本地json的分支</u>](https://github.com/fangge/wakatime-dashboard-pro/tree/local)
{% endnote %}

2. 依然是gist同步的问题，大神现在的wakatime-sync的做法是利用github Action去同步的，但是Action有个问题是如果你不去主动更新项目或者不主动点一下开启，在运行了一段时间后就会自动停止，所以如果大家自己有服务器的话，**wakatime-sync**这个项目可以找到这个[提交](https://github.com/superman66/wakatime-sync/tree/9eba97a683ab1289d2c7f9ecd3b50e35bdb0b7b3)，用旧的方式去同步你的wakatime文件

## 相关更新
因为工作原因时长需要写周报，所以给项目底部加上了项目周报功能，周报显示的就是你当前选择日期范围内的项目汇总，时间段取的就是开发过这个项目的第一天和最后一天，而且顺便为表格增加了编辑功能，大家可以方便更新周报中的项目详情，欢迎大家继续对我的渣代码提出[宝贵意见](https://github.com/fangge/wakatime-dashboard-pro/pulls)
![周报生成器你懂的](/img/blog/1637425411607.jpg)

# 项目更新日志
1. [2023.4.4更新](/2023/04/04/fed11/)
2. [2021.11.20更新](/2021/11/20/fed6/)
3. [WakaTime Dashboard Pro](/2021/06/29/fed5/)