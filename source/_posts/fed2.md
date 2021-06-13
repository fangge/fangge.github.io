---
title: 广工校徽CSS3版
date: 2014/01/1  20:46:25
excerpt: 尝试用CSS3的新属性去实现母校校徽
index_img: /img/blog/6597334748958934052.jpg
banner_img: /img/blog/6597334748958934052.jpg
categories:
  - 前端
tags:
  - 前端开发
  - HTML
  - CSS
---

因为虐心地去吃了夜宵，而且吃太多撑着了，暂时睡不着，就直接上传这个星期随便玩的一个小东西吧，也可能是大四情结在作怪，星期一起床突然就想到想用 CSS3 来弄一个校徽，然后再用 Html5 来实现，为了让自己的执行力高一些，而且最近遇到了很不愉快的事，就抓紧了时间来完成这个小计划（好了，就不要吐槽我没有实现我的大计划了，其实是有的，这几天一直都在写需求），嗯嗯，不多说，直接和大家介绍一下这次的完成步骤吧。

## 步骤一：分析校徽

首先是分析校徽，这部很关键，其实任何你想用 css3 来实现的这一类效果，其实说白了就是看你的图形学，或者说你的空间思维好不好，用什么样的方法去实现这个校徽，还有就是不同方法之间的代码实现效率等等，如果不考虑这些，会让自己的 css3 代码变得非常冗余，而且徒增自己的工作量，得不偿失。
![](/img/blog/6597201708052102870.jpg)

星期一有想法的时候就立刻做，发现越做越难做，主要就是因为我没怎么分析这个校徽，就想着直接动手试试看，这样只会越弄越糟糕，所以后来就平下心来，好好用 PS 慢慢地去测量各种尺寸，还有在草稿纸上画各种方案来实现这个校徽的效果，这里我可以给大家一个例子，就打比方说校徽中间 logo 的那个弯弯的东西我是怎么去分析它和实现它的，嗯嗯，因为这个最简单，可以和大家交流一下过程，让大家高燃起学习的兴趣，如果有更合适的方案，欢迎交流！

![](/img/blog/2536652490134271702.jpg)

其实基本上看这张图大家就很明白我想说明的是什么方法，就是用一些简单图形的叠加和相互覆盖，来实现形状效果，这其实就是用 CSS3 做图案的精髓所在，将复杂图形分解成数个简单图形，和解决问题的方法一样，将复杂问题分解成数个数个简单问题，然后逐个击破，看到上面的图，1/4 圆，半圆这两个如果接触过 CSS3 的就知道直接用 border-radius 来解决就好，其他的都是矩形，更简单。

以此类推，观察这个 logo 的其他部分，除了上图，下面那两个两条杠就分别一个矩形，两个和矩形等高的半圆形就可以；最下面的直接矩形；广东工业大学这几个字我就直接截图了，如果还用 css3 来弄我就真心找虐了；英文就直接用 css3 的旋转用 transform 属性就好；最难的就是外轮廓的花瓣，想了很久，后来使用了多个 1/4 圆，和旋转兼叠加形成，当然，还要用到一个我没过的 css 属性，就是 clip，这个属性是专门用来截断的，可能大家用的比较少，这里给一个规范：

{% note info %}
clip : auto | rect ( number number number number ) 取值：auto :　 默认值。对象无剪切
{% endnote %}

然后 number 和大多数的 css 简写一样，都是上右下左，大家可以试试看。

## 步骤二：写代码

基本上确定了要怎么画，就开始正式写代码了，过程中最纠结的就是要不断地调角度和高度等这些很微小的数据，因为用到旋转的关系，所以即使你旋转一度，所以定位上都要随之改变一些，当然像我比较懒的话，就直接开一个调试面板（其实也不是懒，如果真心调一下，刷一下，我觉得亲你就没必要继续做下去了，除非你是强迫症），然后直接修改 css 看效果，都修改后一次性改代码就好。

[项目地址](https://github.com/fangge/css3logo)
[可变颜色效果](https://www.mrfangge.com/css3logo/css3logo.html)

DEMO：

<p class="codepen" data-height="479" data-theme-id="dark" data-default-tab="css,result" data-user="fangge" data-slug-hash="PopygmM" style="height: 479px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="PopygmM">
  <span>See the Pen <a href="https://codepen.io/fangge/pen/PopygmM">
  PopygmM</a> by Mr.F (<a href="https://codepen.io/fangge">@fangge</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

微信发布了新的 jssdk 后，官方宣称的是大家以后都只能用新的代码了，可是经过测试，上面这一段旧的代码还是可以用的，只要你的页面所在的域名是在微信的“白名单”内，具体测试的话请以下的文件测试：
