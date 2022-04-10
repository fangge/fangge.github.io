---
title: 字节面试复盘
categories:
  - 前端
tags:
  - 前端开发
  - 面试
  - Javascript
date: 2022-04-11 01:29:34
index_img: /img/blog/fed9.webp
banner_img: /img/blog/fed9.webp
excerpt: 面试复盘总结
---

最近想走出舒适圈，所以重启了一下求职之路，其实做前端这么久，自己明白自己的基础很薄弱，可能整个职业生涯也比较佛，大部分时间都是得过且过，觉得不能再浪费生命了，还是得支棱起来，好好地努力一下，争取一下更好的生活，闲话不说，主要说一下面试遇到的一些问题，还有面试官给的一些题目

## 编程题目

可能首先还是以题目切入为主吧，也说明了自己在 js 基础知识掌握方面很薄弱，其实问题都很简单，就是还是不够细心，需要面试官提醒

### 题目一

```javascript
var a = 10;
var obj = {
  a: 20,
  log0: function () {
    var a;
    if (a) {
      a = 100;
    } else {
      a = 200;
    }
    console.log(a);
    console.log(this.a);
    var a = 30;
    console.log(a);
  },
  log1: function () {
    console.log(a);
  },
};
obj.log0();
obj.log1();
```

这题非常基础，主要考的是变量提升、变量作用域和 this 的指向问题

1. 第一个`a`，因为 log0 里面刚开始有定义一个`var a`，所以刚开始 a 是 undefined，也就走到第二个`a=200`，所以打印出来<u>**200**</u>
2. 第二个`this.a`，this 指向的是 obj 里面的 a 变量，所以打印出来是<u>**20**</u>
3. 第三个`a`因为前面重新定义了`a=30`，所以打印出来是<u>**30**</u>
4. 而第四个因为没有`this`，所以指向的是全局的 a 变量，所以是最开始的<u>**10**</u>

这一题自己就犯了一个错误，第一个 a 以为 var 了之后，就会到 100，完全忘了没赋值就是 undefined，还得面试官提醒，真是太逊了，我感觉这里面试官其实就已经知道我基础不是很稳了

{% note success %}
正确答案：
200
20
30
10
{% endnote %}

### 题目二

```javascript
console.log('script start');
setTimeout(function () {
  console.log('script 1');
}, 0);
new Promise(function (resolve, reject) {
  console.log('script 2');
  resolve();
})
  .then(function () {
    console.log('script 3');
  })
  .then(function () {
    console.log('script 4');
  });
console.log('script end');
```

这题其实是蒙对的，面试官也指出了这点，这题主要考的是 js 中的宏任务和微任务，但是我之前确实没有了解过这部分的知识，这里重新巩固了一下，所有 js 中
{% note info %}
先同步后异步，先微后宏，微任务的执行时机比宏任务早
{% endnote %}

1、微任务有哪些
Promise
await 和 async

2、宏任务有哪些
setTimeout
setInterval
DOM 事件
AJAX 请求

所以我们再来看这个题目

1. `script start`肯定是第一个的这个毫无疑问
2. 接下来因为`script 1'`是包在`setTimeout`这个宏任务中，所以对比其他 console，会先放在`Promise`这个微任务后面执行
3. 来到`Promise`后，先执行里面的`script 2`，然后 resolve 之后，就会走到 then 的`script 3`，这里也考了一个 Promise 对象的知识点，就是这个 then 其实也是一个[Promise 对象](https://es6.ruanyifeng.com/#docs/promise#Promise-prototype-then)，链式调用法的缘故也就顺势执行了`script 4`
   ![Promise对象中的then链式用法](/img/blog/fed9_1.png)
4. 接着就执行`script end`
5. 最后再执行被挂起的宏任务`script 1`

{% note success %}
正确答案
script start
script 2
script end
script 3
script 4
script 1
{% endnote %}

### 题目三

```javascript
const data = [
  { id: 'R1', parentId: null },
  { id: 'R2', parentId: null },
  { id: 'R1-1', parentId: 'R1' },
  { id: 'R1-2', parentId: 'R1' },
  { id: 'R1-3', parentId: 'R1' },
  { id: 'R2-1', parentId: 'R2' },
  { id: 'R2-2', parentId: 'R2' },
  { id: 'R1-1-1', parentId: 'R1-3' },
  { id: 'R1-2-3', parentId: 'R1-1-1' },
];
```

希望输出类似下面的结构

```javascript
[
  {
    id: 'R1',
    parentId: null,
    children: [
      {
        id: 'R1-1',
        parentId: 'R1',
      },
      {
        id: 'R1-2',
        parentId: 'R1',
      },
      {
        id: 'R1-3',
        parentId: 'R1',
        children: [
          {
            id: 'R1-1-1',
            parentId: 'R1-3',
            children: [
              {
                id: 'R1-2-3',
                parentId: 'R1-1-1',
              },
            ],
          },
        ],
      },
    ],
  },
  {
    id: 'R2',
    parentId: null,
    children: [
      {
        id: 'R2-1',
        parentId: 'R2',
      },
      {
        id: 'R2-2',
        parentId: 'R2',
      },
    ],
  },
];
```

这题因为之前算法复习的不够，所以虽然最后基本写出来了，但是用的时间太久了，我觉得也给面试官一个不好的印象，回来后也是自己先试着重新写了一次

```javascript
const data = [
  { id: 'R1', parentId: null },
  { id: 'R2', parentId: null },
  { id: 'R1-1', parentId: 'R1' },
  { id: 'R1-2', parentId: 'R1' },
  { id: 'R1-3', parentId: 'R1' },
  { id: 'R2-1', parentId: 'R2' },
  { id: 'R2-2', parentId: 'R2' },
  { id: 'R1-1-1', parentId: 'R1-3' },
  { id: 'R1-2-3', parentId: 'R1-1-1' },
  { id: 'R1-2-3-5', parentId: 'R1-2-3' },
];
function getParentIndex(id) {
  return data.findIndex((pinfo) => pinfo.id == id);
}

function getResultArr(arr) {
  const inArr = arr;
  inArr.forEach((item) => {
    if (item.parentId) {
      // 获取匹配的父节点
      let parentIndex = data.findIndex((pinfo) => pinfo.id == item.parentId);
      if (parentIndex > -1) {
        let parentObj = data[parentIndex];
        // 判断是否有children属性
        if (parentObj.children) {
          if (
            // 判断chirldren中是否有已有对应的结点
            parentObj.children.findIndex((target) => target.id == item.id) == -1
          ) {
            parentObj.children.push(item);
          }
        } else {
          parentObj.children = [item];
        }
      }
    }
  });
  return inArr.filter((item) => item.children && item.parentId == null);
}
console.log('我的解法', getResultArr(data));
```
后来经过大神提醒，这道题其实就是算法中很经典的<u>**[数组转树]**</u>问题，网上已经有很多大神的分析贴了，大家也可以看看这篇[【面试了十几个高级前端，竟然连（扁平数据结构转Tree）都写不出来】](https://juejin.cn/post/6983904373508145189#heading-8)，也可以对解决这个问题的所有方法做个了解，包括怎么优化等等，看完就觉得真是学海无涯啊

## 其他面试问题总结
