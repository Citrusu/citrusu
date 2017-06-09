---
layout:     post
title:      "gulp的基本使用"
#subtitle:   "about-respons"
date:       2016-05-07
author:     "Citrus"
#header-img: "img/post-bg-re-vs-ng2.jpg"
#header-mask: 0.5
#catalog:    false
tags:
    - nodejs
    - gulp
---
> Gulp.js 是一个自动化构建工具，开发者可以使用它在项目开发过程中自动执行常见任务。Gulp.js 是基于 Node.js 构建的，利用 Node.js 流的威力，你可以快速构建项目并减少频繁的 IO 操作。Gulp.js 源文件和你用来定义任务的 Gulp 文件都是通过 JavaScript（或者 CoffeeScript ）源码来实现的。

## 1.全局安装 gulp
    npm install --global gulp

## 2.作为项目的开发依赖（devDependencies）安装
    npm install --save-dev gulp
    
## 3.安装依赖组件
    npm snstall xx

## 4.在项目根目录下创建一个名为 gulpfile.js 的文件,引入依赖组件：
    
    // 引入 gulp及组件
    var gulp=require('gulp'),  //gulp基础库
        cleanCSS = require('gulp-clean-css'),   //css压缩
        concat=require('gulp-concat'),   //合并文件
        uglify=require('gulp-uglify'),   //js压缩
        rename=require('gulp-rename'),   //文件重命名
        jshint=require('gulp-jshint'),   //js检查
        imagemin = require('gulp-imagemin'),
        del = require('del'),            //清除文件
        cache = require('gulp-cache'),   //图片缓存，只有图片替换了才压缩
        notify=require('gulp-notify');   //提示
    
    
    //css
    gulp.task('minifycss',function(){
       return gulp.src('etui/css/*.css')      //设置css
           .pipe(concat('etui.css'))          //合并css文件到"etui"
           .pipe(gulp.dest('dist/styles'))    //设置输出路径
           .pipe(rename({suffix:'.min'}))     //修改文件名
           //.pipe(cleanCSS())                //压缩文件
           //.pipe(gulp.dest('dist/styles'))         //输出文件目录
           .pipe(notify({message:'css task ok'}));   //提示成功
    });
    
    // Images
    gulp.task('minifyimg', function() {
      return gulp.src('Images/double/**/*')
        .pipe(cache(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true })))
        .pipe(gulp.dest('dist/images'))
        .pipe(notify({ message: 'Images task complete' }));
    });
    
    // Scripts
    gulp.task('minifyjs', function() {
        return gulp.src('Js/**/**/*.js')
            //.pipe(concat('main.js'))    //合并所有js到main.js
            //.pipe(gulp.dest('dist/scripts'))    //输出main.js到文件夹
            .pipe(rename({suffix: '.min'}))   //rename压缩后的文件名
            .pipe(uglify())    //压缩
            .pipe(gulp.dest('dist/scripts'));  //输出
    });
    
    gulp.task('default', ['minifycss', 'minifyimg','minifyjs']);

## 运行
    gulp
或者

    gulp <taskname>