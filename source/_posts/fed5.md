---
title: WakaTime Dashboard Pro
categories:
  - 前端
tags:
  - 前端开发
  - HTML
  - CSS
  - Javascript
date: 2021-06-29 10:43:35
index_img: /img/blog/fed5.jpg
banner_img: /img/blog/fed5.jpg
excerpt: 利用WakaTime 数据打造自己的开发数据图表
---

![wakatime-dashboard-pro](/img/blog/b9cc66cd-cb04-4900-afbe-3daf8d0ff0e5.png)
[项目 Demo](https://wakatime.mrfangge.com/)

---

之前一直有使用一个开源项目， **superman66**大神的[wakatime-dashboard](https://github.com/superman66/wakatime-dashboard) ，大致介绍一下这个项目，就是利用另一个同步的项目：[wakatime-sync](https://github.com/superman66/wakatime-sync)，每天将你的 WakaTime 的数据同步到你的 github 的 gist 上，然后再读取 gist 上的 json 数据，用图表去展示你的代码使用情况，非常喜欢 wakatime 这个统计工具，是因为自己很喜欢回顾之前做的项目情况，知道自己这段时间究竟做了哪些事情，也有利于自己更好地分配时间

{% note info %}
什么是[WakaTime](https://wakatime.com/): 它是一个可以统计你日常开发项目具体信息的工具。 在相应的编辑器中安装插件后，您可以获取您过去 7 天的代码信息。 如果您需要获得更多，则需要付费升级到高级版本
{% endnote %}
![WakaTime](/img/blog/20210629115606.jpg)

## 开始优化

但是个人因为用久了觉得之前的图表不能那么方便地显示一些项目具体信息，于是乎对大神原有的项目进行了重构，主要有以下这些功能的改进和增加：

### 一、功能增加

1. 增加日历选择工具，可以选择具体的某段时间，而不是只能选择前 N 天
2. 增加下载图表功能，可以方便地下载自己的项目图表数据
3. 在图表数据日期比较多的情况下，可以让用户横向滚动或出现日期筛选滚动条
4. 增加选择时间内的总的项目使用情况（原有的只是每天的项目情况）
5. 增加选择时间内的总的编程语言使用情况
6. 增加选择时间内的开发工具的使用情况
7. 增加夜间模式
8. 结合 GIthub Action 随时同步自己的修改到 master 分支，方便 Github Page 直接使用项目

### 二、功能去除

1. 去掉登录功能，直接让用户随时输入 gistid
2. 去除相关友情链接

## 技术栈和构建框架选择

之前一直想着试试[Vite](https://vitejs.dev/)开发，于是乎这次正好就用上了，主体依然是用**React**开发，但因为之前项目用的图框架是**bizcharts**，所以这次想着直接简单点直接用**[Ant Design](https://ant.design/)+Antd 的[图表框架](https://charts.ant.design/)**，整个项目使用下来，觉得 Vite 确实挺方便的

> 某些插件用的时候可能会有兼容问题，因为其强制采用了`ES Module` 来实现模块的加载，所以如果你使用的包有问题，可能要搜一下对应的一些兼容方法

{% note primary %}
Antd Charts某些图表的配置并不能通用，在官方文档上没有指出来，还好在Github上有人提供类似的issue，总算找到解决方法，如果大家在自己开发的时候，可以注意一下这点
{% endnote %}

## 安装和使用步骤

1. 登录[WakaTime](https://wakatime.com/)，按照使用方法在你的 IDE 上安装好插件和配置**wakatime key**
2. 利用[wakatime-sync](https://github.com/superman66/wakatime-sync)将你的每天的数据同步到 gist 中（*原理是利用[Github Action](https://docs.github.com/en/actions)每天定时请求 wakatime 的免费 api，获取 json 文件后再上传到 gist 中*）
3. clone 这个项目并安装

```bash
git clone https://github.com/fangge/wakatime-dashboardpro.git && cd wakatime-dashboardpro
npm i
```

4. 使用

```bash
npm run build && npm run dev
```

## 亮点功能介绍

### 1. 时间段选择

如果选择具体某段时间，则前面的前 N 天选择则失效，图表可以拖动选择的时间内的某段时间，随时定位到某段具体的时间
![时间选择](/img/blog/20210629171957.jpg)

### 2. 具体项目每日的时间变化

得益于 antd 图表中的[联动区域例子](https://charts.ant.design/zh-CN/demos/column#%E5%B8%A6%E8%81%94%E5%8A%A8%E5%8C%BA%E5%9F%9F%E7%9A%84%E7%99%BE%E5%88%86%E6%AF%94%E6%9F%B1%E7%8A%B6%E5%9B%BE)，这次鼠标移动到图表中，你可以方便地看到具体某个项目这段时间内的时间变化
![联动区域](/img/blog/20210629172546.jpg)

### 3. 选择时间内的项目总概况

利用矩形树图直观地展示你选择时间内项目的总体时间占比
![选择时间内的项目总概况](/img/blog/20210629172928.jpg)

### 4. 编程语言概况

饼图展示选择时间内的编程语言使用情况
![language](/img/blog/20210629173757.jpg)

### 5. 暗黑模式支持

项目支持根据系统主题色来变化浅色和暗色模式
![暗黑模式](/img/blog/43d1d9bf-9576-4f23-a615-557fece803bd.png)

## 计划优化的功能

- [ ] 优化直方图的展示方式，找到悬浮在具体项目信息上展示联动的时候，可以直接悬浮的是哪个项目，而不用每次都去看图例，不够清晰
- [ ] 增加使用的操作系统的概况


## 项目总结
这次的折腾，也是逼自己以产品化思维，用户角度去开发一个项目的过程，本质上是为了让自己可以多用代码去做一些自己想做的事情，找回一些写代码的激情，哈哈，代码渣渣欢迎大家fork这个项目并提交你们的改进方案呀~