---
layout:     post
title:      "linux常用命令"
#subtitle:   "about-respons"
date:       2016-05-07
author:     "Citrus"
#header-img: "img/post-bg-re-vs-ng2.jpg"
#header-mask: 0.5
#catalog:    false
tags:
    - linux
    - 系统
---
## 寻找文件和目录 `find`
    find pathname -options [-print -exec -ok ...]
    
> `find / -name mydir`

## 创建软链接/快捷方式
    ln -s 源文件 目标文件
    
>  当前目录是`/local`，而我经常要访问`/usr/local/linux/work`
那么我就可以使用在`local`下建立一个文件`linkwork`，
然后`ln -s /usr/local/linux/work  /local/linkwork`