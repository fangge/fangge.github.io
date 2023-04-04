---
title: WakaTime Dashboard Pro - 更新日志
categories:
  - 前端
tags:
  - 前端开发
  - HTML
  - CSS
  - Javascript
date: 2023-04-04 14:17:10
index_img: /img/blog/fed5.jpg
banner_img: /img/blog/fed5.jpg
excerpt: 利用WakaTime 数据打造自己的开发数据图表
---
![wakatime-dashboard-pro](/img/blog/fed11.png)
[项目 Demo](https://wakatime.mrfangge.com/)

---

时隔那么久，想着再次更新一下自己的这个项目
> 温馨提示: 此项目中的你自行设置的action里面的`ACCESS_TOKEN`可能会过期，大家记得去[【Github的token页】](https://github.com/settings/tokens)去重新生成更新一下

先说一下这次的修改点
1. 改为pnpm包管理，安装依赖更加快速
2. 折线图改成虚线，并增加对应时间点的样式
3. 图表如果刚好当天是没有任何项目的（也就是对应结构中的projects没有值），依然添加对应的时间点
4. 图表增加项目图例，可以方便筛选每个项目的开发时间
5. 增加时间范围内的项目总结：总共项目时长、花费时间最长的一天、花费时间最长和最短的项目

最近ChatGpt很火，刚好一直觉得Ant Design Chart的文档不算很好阅读，就想着直接通过问Chatgpt的方式直接修改代码，可以看看我是是怎么修改的

![问答参考](/img/blog/chat1.png)
![问答参考](/img/blog/chat2.png)
![问答参考](/img/blog/chat3.png)
![问答参考](/img/blog/chat4.png)
![问答参考](/img/blog/chat5.png)
![问答参考](/img/blog/chat6.png)

就这么简单的几个问答，就把这次我想要更新的功能更新好了，所以说这次chatgpt的对于生产力的变革，可以说是颠覆性的，在不远的未来会影响到我们的方方面面，完全可以让一个不熟悉行业的人，快速上手行业，如果有恰当的关键词和问答训练，得到的答案会更加专业

