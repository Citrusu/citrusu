---
layout:     post
title:      "Web 地图 GPS 定位"
subtitle:   "实时更新位置"
date:       2017-09-08
author:     "Citrus"
icon:       "fa-globe"
tags:
    - Map
    - 百度地图
    - getCurrentPosition
---


## 背景

在地图上某城市添加多个宝藏，然后根据用户 GPS 信息获取当前位置在地图在寻找宝藏。当用户位置到达宝藏位置就打开宝藏。

## 目标/技术点

- 在 web 页面中实现定位
- 当用户移动位置刷新当前位置
- 根据 经纬度计算地球上两点距离


- 通过百度地图 API 显示地图
- 通过百度地图 API 定时刷新（有问题）

## 问题及过程

### 百度定位

在使用百度地图的 `geolocation.getCurrentPosition` 接口发现移动位置并没有更新当前位置。查找资料说要设置 `maximumAge`, 实际上把这个值设置 0 也没啥用，照样不更新位置。

```js
geolocation.getCurrentPosition(function (r) {
    
    if (this.getStatus() == BMAP_STATUS_SUCCESS) {
        var lng = r.point.lng;
        var lat = r.point.lat;
        console.log('您的位置baidu：' + lng + ',' + lat + ',时间：' + new Date().getMinutes() + ':' + new Date().getSeconds());
    }else {
        console.log('failed' + this.getStatus());
    }
}, {
    enableHighAccuracy: true,
    maximumAge: 0
});
```

经过多次测试猜测百度地图缓存了当前位置，大概需要10分钟才会更新新的位置。放弃之，下一个。

### 使用原生接口 

使用原生接口`navigator.geolocation.getCurrentPosition`

```js
if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(function(p){
        //成功获取位置
        var position = {
            lng: p.coords.longitude,
            lat: p.coords.latitude
        };
      	console.log(position);
    }, function(err){
        console.log(err);
    });
}else {
    console.log( "Geolocation is not supported by this browser.");
}
```

几乎快要搞定了，通过定时调用当前位置再转换成百度的位置显示标记到地图上，完美！✌️BUT ! 安卓一切 OK，IOS 从10开始需要 `https`协议才能使用 `geolocation`。否则报 `[blocked] Access to geolocation was blocked over insecure connection to [http://www.mydomain.cn](http://www.mydomain.cn/)` 错 WTF🤷‍♂️。 

这时候，你可以选择

- 把网站切换成 `https` ，下面就不用看啦。
- 使用 `iframe` 。如果你暂时不能换成 `https`那可以试试以下方法

### 使用 `iframe`

我的思路是使用 `iframe`加载一个`https`的页面，然后这个 `iframe`使用  `geolocation`返回位置信息。说起干就干。在这里很容易碰到另一个问题，没错，我就碰到了。😂 我是借助 `coding.net`的 `Pages`服务创建了一个页面，为嘛不用`github`呢，很简单，`coding`可以很方便一键切换成`https`协议，而且在天朝速度 sousou 的。由于用了三方页面加载 `iframe`就难免碰到跨域问题。这里推荐 [Iframe跨域通信的几种方式](https://github.com/hstarorg/HstarDoc/blob/master/前端相关/Iframe跨域通信的几种方式.md)

最后使用 `postMessage`解决跨域问题。🤣 接口详情参考 [postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage) 。



```js
// frame
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(function(p){
    //成功获取位置
    var position = {
      lng: p.coords.longitude,
      lat: p.coords.latitude
    };
    console.log(position);
    parent.postMessage(position, '*');
  }, function(err){
    console.log('GPSerr'+err)
  });
}else {
  alert( "OMG ! 您的设备不支持 GPS");
}

```

```html
// parent
<iframe id="gpsIframe" src="yourwebsite" style="display: none"></iframe>

<script>
  window.addEventListener('message', function(evt){
    if (event.origin !== "yourwebsit") return;
      console.log(evt.data);
  });
</script>
```