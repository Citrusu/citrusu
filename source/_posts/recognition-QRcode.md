---
layout:     post
title:      "为什么微信二维码不能识别？"
data:       2017-01-16
author:     "Citrus"
icon:       "fa-weixin"
tags:
    - 微信
    - 二维码
---
### 原因分析：

*   尺寸太小
*   清晰度不够

### 测试过程

*   尺寸：100-20px
```js
    //图片大小测试
    var src = 'img/';
    var img = 'act_happy_code';
    var type = ['jpg','png','gif'];
    var size = [100,90,80,70,60,50,40,30,20];
    var box = document.querySelectorAll('body')[0];
    for(var i = 0,sizeLen = size.length; i < sizeLen; i++){
        for(var j = 0,typeLen = type.length; j < typeLen; j++){
            var imgBox = document.createElement('div');

            var ctext = document.createTextNode(type[j]+ '_' + size[i] + 'x' + size[i] + 'px');

            var cimg = document.createElement('img');
            cimg.style.width = size[i] + 'px';
            cimg.style.height = size[i] + 'px';
            cimg.setAttribute('src', src + img + '.'+ type[j] );
            imgBox.appendChild(ctext);
            imgBox.appendChild(cimg);
            box.appendChild(imgBox);
        }
    }
```
*   清晰度：100%-5% 
> 在清晰度测试上我使用了七牛的压缩接口

``` javascript
    //图片质量测试
    var src = '';
    var mode = '?imageView2/2';
    var img = 'http://ob9fno759.bkt.clouddn.com/act_happy_code';
    var type = ['jpg','png','gif'];
    var quality = [100,75,50,25,10,5];
    var size = [80,60,40,30,20];
    var box = document.querySelectorAll('body')[0];
    for(var i = 0,sizeLen = size.length; i < sizeLen; i++){
        for(var j = 0,qLen = quality.length; j < qLen; j++){
            var randNum = Math.ceil(Math.random()*1000);
            var imgBox = document.createElement('div');

            var ctext = document.createTextNode('质量:'+ quality[j] +'%' + '_' + size[i] + 'x' + size[i] + 'px');

            var cimg = document.createElement('img');
            cimg.setAttribute('src', src + img +'.gif'+ mode +'/w/'+size[i] + '/h/'+ size[i] + '/q/' + quality[j] +'#'+randNum);
            imgBox.appendChild(ctext);
            imgBox.appendChild(cimg);
            box.appendChild(imgBox);
        }
    }
```

> 经过以上测试：尺寸和清晰度都有可能影响图片识别，但是都不是最影响识别的原因。

### 原因

先看看下图，信息过于密集，为了美观在一般的活动设计中二维码都不会很大，而这才是最影响二维码识别的原因。

![enter image description here][1]

### 解决方法

*   获取图片的原链接
*   把原链接转换成短链接 [短链转换工具][2] 
*   最后把短链生成二维码

这样就能得到一张识别度高的二维码图片。

### 建议

*   考虑到目前还有不少小屏手机，建议二维码的实际显示尺寸不少于40*40px
*   如果二维码在大图上面，压缩之后可以尝试把二维码提取出来，然后再把未压缩的二维码与压缩后的背景合成一张。

 [1]: http://etui.yidake.com/help/wp-content/uploads/2017/01/2017011610024854.gif
 [2]: http://dwz.wailian.work/