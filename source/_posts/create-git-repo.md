---
layout:     post
title:      "搭建git仓库"
#subtitle:   "about-respons"
date:       2018-03-20
author:     "Citrus"
#header-img: "img/post-bg-re-vs-ng2.jpg"
#header-mask: 0.5
#catalog:    false
tags:
    - git
---
# 搭建git仓库

## 创建用户

创建一个git和用户，用来运行git服务：

    adduser git

## 创建证书登录

收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

检查权限

    chmod 755 .ssh
    chmod 644 .ssh/authorized_keys

## 初始化Git仓库

先选定一个目录作为Git仓库，假定是/gitrep/test.git
​    
    cd /
    chown -R git:git gitrep

进入gitrep目录

    git init --bare test.git

> 创建多个仓库只要在gitrep目录运行 `git init --bare xxx.git ` 修改仓库名

## 克隆仓库

修改端口之后与默认使用方法有与不同

    git clone ssh://git@ip:port/gitrep/test.git

## 初始已有仓库克隆为新仓库

    git clone remote_repo new_repo.git