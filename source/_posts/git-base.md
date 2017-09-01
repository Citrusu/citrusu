---
layout:     post
title:      "git 常用命令"
#subtitle:   "about-respons"
date:       2016-05-07
author:     "Citrus"
#header-img: "img/post-bg-re-vs-ng2.jpg"
#header-mask: 0.5
#catalog:    false
tags:
    - git
    - github
---
# git 常用命令

 ## 放弃本地的修改，用远程覆盖本地
    git fetch --all
    git reset --hard origin/master
    
> git fetch 只是下载远程的库的内容，不做任何的合并git reset 把HEAD指向刚刚下载的最新的版本