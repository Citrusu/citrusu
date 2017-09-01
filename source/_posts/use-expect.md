---
layout:     post
title:      "使用 expect"
#subtitle:   "about-respons"
date:       2017-08-22
author:     "Citrus"
#header-img: "img/post-bg-re-vs-ng2.jpg"
#header-mask: 0.5
#catalog:    false
tags:
    - expect
    - linux
---
# 安装expect
> expect 可以完成一些自动化的任务，举个粟子：进入 a 服务器更新代码，退出再进入 b 服务器更新代码……

## 检查是否已经安装 expect
    
    whereis expect

## 安装tcl
下载地址：http://nchc.dl.sourceforge.net/sourceforge/tcl/tcl8.4.11-src.tar.gz

    tar -zxvf tcl8.4.11-src.tar.gz
    cd tcl8.4.11/unix/
    ./configure
    make && make install
    
## 安装expect

下载地址：http://sourceforge.net/projects/expect/files/Expect/5.45/expect5.45.tar.gz/download

    tar -zxvf expect5.45.tar.gz 
    cd expect5.45
    ./configure
    make && make install

> 安装参考 

- http://blog.csdn.net/silenceray/article/details/54582345
- http://www.cnblogs.com/daojian/archive/2012/10/10/2718390.html
    
## 使用expect 示例

    #!/usr/bin/expect
    # 上面的路径为 whereis expect 的路径
    
    set host 192.168.0.1
    set password abcd123
    set username root
    set port 22
    set script "ls"
    
    set timeout -1
    spawn ssh -p $port $username@$host
    expect {
        "(yes/no)?"
        {send "yes\r";exp_continue}
        "password:"
        {send "$password\r";exp_continue}
        "#*"
        {send "$script && exit\r"}
    }
    
    interact
