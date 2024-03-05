---
title: 前端图片压缩方案
categories:
  - 前端
tags:
  - 前端开发
  - Javascript
  - 图片压缩
date: 2024-03-05 23:25:50
index_img: /img/blog/fed11.jpg
banner_img: /img/blog/fed11.jpg
excerpt: 纯前端压缩方案的探讨
---
> 文章灵感：感谢张鑫旭大大的[【做了个纯前端JPG/PNG尺寸缩放+压缩的在线工具】](https://www.zhangxinxu.com/wordpress/2023/09/js-jpg-png-compress-tinyimg-mini/)

之前在公司业务中，发现低代码生成的页面里面的图片总是会尺寸超标，导致页面加载速度变慢，图片上传组件即使有硬性限制死