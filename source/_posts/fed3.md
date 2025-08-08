---
title: 微信jssdk使用流程
date: 2017/04/21 09:46:25
updated: 2021/06/13 01:01:00
description: 本篇适合从没有接触过微信jssdk的同学

cover: /img/blog/banner.jpg

categories:
  - 前端
tags:
  - 前端开发
  - Javascript
---

> 本篇适合从没有接触过微信 jssdk 的同学

## 旧的分享代码

在经历了一段时间普通用户的鬼哭狼嚎后（微信的分享突然不能用了），微信终于推出了正式的 jssdk，再重新贴一下旧的微信的分享代码

```javascript
//给微信设置分享文案
var onBridgeReady = function () {
  var appId = '';
  // 发送给好友;
  WeixinJSBridge.on('menu:share:appmessage', function (argv) {
    var imgUrl = sharePic;
    var link = shareUrl;
    var title = shareTitle;
    var shareDesc = shareTxt;
    WeixinJSBridge.invoke(
      'sendAppMessage',
      {
        img_url: imgUrl,
        img_width: '640',
        img_height: '640',
        link: link,
        desc: shareDesc,
        title: title,
      },
      function (res) {
        //分享成功后回调函数
      }
    );
  });
  // 分享到朋友圈;
  WeixinJSBridge.on('menu:share:timeline', function (argv) {
    var imgUrl = sharePic;
    var link = shareUrl;
    var title = shareTitle;
    var shareDesc = shareTxt;
    WeixinJSBridge.invoke(
      'shareTimeline',
      {
        img_url: imgUrl,
        img_width: '640',
        img_height: '640',
        link: link,
        desc: shareDesc,
        title: shareDesc,
      },
      function (res) {
        //分享成功后回调函数
      }
    );
  });
};
if (document.addEventListener) {
  document.addEventListener('WeixinJSBridgeReady', onBridgeReady, false);
} else if (document.attachEvent) {
  document.attachEvent('WeixinJSBridgeReady', onBridgeReady);
  document.attachEvent('onWeixinJSBridgeReady', onBridgeReady);
}
```

微信发布了新的 jssdk 后，官方宣称的是大家以后都只能用新的代码了，可是经过测试，上面这一段旧的代码还是可以用的，只要你的页面所在的域名是在微信的“白名单”内，如果加上以上代码后可以正常分享，那么就证明你的域名是在微信的白名单内

## 新的 jssdk 使用方法

[微信 JS-SDK 说明文档](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html)

新的方式比之前的方式复杂一些，如果你想要你的页面可以在微信内分享，必须满足以下条件：
\{% note %\}
1. 你有一个已备案的域名
2. 你有一个微信公众号（服务号或订阅号都行）
3. 你有相关的后台服务来生成签名（php、java、nodejs 以及 python）
   {% endnote %}

前两个比较容易理解，这里我就不展开了，后面所谓的签名到底是什么呢，这里我用一个浅显易懂的流程来向大家说明一下微信 jssdk 是怎么运作的：

1. 获取当前时间（10 位，精确到秒）、公众号 AppID 和 AppSecret、页面地址、随机字符串
2. 通过 https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=**公众号AppID**&secret=**公众号AppSecret**取得 ACCESS_TOKEN
3. 通过 https://api.weixin.qq.com/cgi-bin/ticket/getticket?access_token=**ACCESS_TOKEN**&type=jsapi取得 jsapi_ticket（有效期为 7200s，也就是 2h）
4. 用获得的当前时间、页面地址、随机字符串和 jsapi_ticket 生成签名
5. 用公众号 AppID、当前时间、随机字符串和签名来配置 jssdk 并使用

---

那么来详细解释一下流程中的一些点：

### （一）已备案的域名和公众号

注册公众号并且认证这里就不展开了，大家直接去https://mp.weixin.qq.com/ 了解即可，可是为什么需要已备案的域名呢？因为如果你需要使用 jssdk，需要登录微信公众平台进入“公众号设置”的“功能设置”里填写“JS 接口安全域名”，这里的域名所需要的是已备案的域名，设置完成后，才能开通相对应的接口权限。

> 服务号只有两个域名白名单，订阅号有三个

对于权限来说，服务号比订阅号多了一些权限（因为服务号可以验证，订阅号不能），比如微信支付、卡券等等，还有就是读取用户的相关信息等，这个大家可以自行根据需求选择公众号类型

### （二）随机字符串

可以用以下函数生成任意长度的字符串

```javascript
//生成随机字符串
function randomString(len) {
  len = len || 32;
  var chars =
    'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678'; /****默认去掉了容易混淆的字符oOLl,9gq,Vv,Uu,I1****/
  var maxPos = chars.length;
  var pwd = '';
  for (i = 0; i < len; i++) {
    pwd += chars.charAt(Math.floor(Math.random() * maxPos));
  }
  return pwd;
}
```

### （三）签名

{% note info %}
**签名算法**：参与签名的字段包括 noncestr（随机字符串）, 有效的 jsapi_ticket, timestamp（时间戳）, url（当前网页的 URL，不包含#及其后面部分） 。对所有待签名参数按照字段名的 ASCII 码从小到大排序（字典序）后，使用 URL 键值对的格式（即 key1=value1&key2=value2…）拼接成字符串 string1。这里需要注意的是所有参数名均为小写字符。对 string1 作 sha1 加密，字段名和字段值都采用原始值，不进行 URL 转义。
{% endnote %}

在上述介绍的 jssdk 运作流程中，2、3、4 步骤其实都是直接放在后台实现的，这也是为什么我说需要后台服务的缘故，具体的后台代码 demo 微信文档里面已经给出了，下面贴一个链接：
[http://demo.open.weixin.qq.com/jssdk/sample.zip](http://demo.open.weixin.qq.com/jssdk/sample.zip)

{% note warning %}
备注：链接中包含 php、java、nodejs 以及 python 的示例代码供第三方参考，第三方切记要对获取的 accesstoken 以及 jsapi_ticket 进行缓存以确保不会触发频率限制，另外注意生成的签名有效时间为 2 小时，2 小时后就要重新生成
{% endnote %}

### （四）使用 jssdk

配置的代码如下：

```javascript
wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '', // 必填，公众号的唯一标识
    timestamp: , // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名，见附录1
    jsApiList: [] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
});
```

jsApiList 请大家直接在[微信 JS-SDK 说明文档](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html)里面找到对应的使用方法即可，这里就不再赘述
