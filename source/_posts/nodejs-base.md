---
layout:     post
title:      "nodejs基本安装和配置"
#subtitle:   "about-respons"
date:       2016-05-07
author:     "Citrus"
#header-img: "img/post-bg-re-vs-ng2.jpg"
#header-mask: 0.5
#catalog:    false
tags:
    - nodejs
---
## 首先安装

- 下载安装包 [https://nodejs.org/en/](https://nodejs.org/en/)
- 查看版本

	- 进入cmd
	- 运行 `node -v`
	- 运行 `npm -v`

> 能正常显示出版本信息就说明已经安装成功

- 查看路径

        npm root -g
    或者

        npm config get

## 使用npm安装模块

		$ npm install -g gulp


## 切换到淘宝镜像
    npm install cnpm -g --registry=https://registry.npm.taobao.org
    
> cnpm跟npm用法完全一致，只是在执行命令时将npm改为cnpm

# 其他相关命令
## 删除window下目录过长的文件夹
    rm -rf 目录