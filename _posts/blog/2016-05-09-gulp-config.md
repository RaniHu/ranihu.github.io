---
layout: post
title: 前端自动化工具gulp的基本配置
categories: gulp
description: gulp和grunt都是目前流行的前端自动化构建工具，但相比grunt，gulp的配置更为简单
keywords: Java
---


gulp是前端自动化工具，它可以进行html、css压缩合并、js检查压缩、图片压缩剪裁、制作雪碧图、编译less、sass等，还可以自动完成开发过程中一些重复的的任务，使我们的工作效率大大的提高！那具体如何配置以及使用gulp呢?


## 一、安装node.js

1. 因为gulp是基于node的,所以首先我们需要搭建node环境。这一步非常简单，进入node.js官网[config-files](https://nodejs.org/en/)下载所对应的版本。下载后傻瓜式安装即可。

2. 打开命令行

   ``` node -v   查看安装的nodejs版本，出现版本号，说明已正确安装node```


## 二、安装npm

1.  npm是nodejs的包管理器，用于node插件管理（包括安装、卸载、管理依赖等）。

    ``` npm -v     查看npm版本号  ```
    
2.  由于npm安装插件是从国外服务器下载，可能出现异常，所以可以直接选择安装cnpm! cnpm与npm的用法完全一致，执行命令时可以将npm用cnpm代替。
    
    ``` npm install cnpm -g --registry=https://registry.npm.taobao.org  ```
    

## 三、全局安装gulp

1. 安装命令

   ``` npm install gulp -g ```
   
2. 查看版本

   ``` gulp -v ```   
   
   
## 四、新建pakage.json文件

package.json是基于nodejs项目必不可少的配置文件，存放在项目根目录下

1. 控制台定位到项目目录执行

   ``` npm init  ```
   
2. 然后会出现一些配置信息，可直接回车跳过，package.json就新建完成了。不过要注意的是此文件不能添加任何注释哦~
 
 
## 五、本地安装gulp

在这里要跟大家说明的是全局安装gulp是为了执行gulp任务，本地安装gulp则是为了调用gulp的插件。

1. 本地安装gulp（定位到项目文件）

   ``` npm install gulp --save-dev  ```
   
2. 安装插件  

   ``` cnpm install gulp-sass --save-dev          以安装sass插件为例  ```

3.同时安装多个插件

   ``` npm install gulp-jshint gulp-sass gulp-concat gulp-uglify gulp-rename gulp-jshint-html-reporter --save-dev            常用插件 ```
   
   
## 六.新建gulpfile.js文件
   
gulpfile.js是gulp项目的配置文件, 只需在头部require需要的模块，再使用task来定义任务。

```
//导入工具包 require('node_modules里对应模块')

var gulp = require('gulp');

var less = require('gulp-less');	                        //编译less

var sass = require('gulp-sass');                     	    //编译sass

var htmlmin = require('gulp-htmlmin');           	        //html压缩

var minifycss = require('gulp-minify-css');     	        //css压缩

var concat = require('gulp-concat');                        //文件合并

var uglify = require('gulp-uglify');                    	//js压缩

var rename = require('gulp-rename');	                    //重命名

var jshint = require('gulp-jshint');	                    //js检查

var imagemin = require('gulp-imagemin');	                //压缩图片

var cache = require('gulp-cache');                	        //图片压缩缓存

var clean = require('gulp-clean');	                        //清空文件夹

var rev = require('gulp-rev');	                            //更改版本号



//定义一个less任务（自定义任务名称）
gulp.task('less', function () {

    return gulp.src('./src/style.less')                     //需要操作的文件

        .pipe(less())	                                    //该任务调用的模块

        .pipe(gulp.dest('./dest'))	                        //文件导出的位置

})

//定义一个sass任务（自定义任务名称）
gulp.task('sass', function () {

    return gulp.src('./src/style2.scss')

        .pipe(sass())

        .pipe(gulp.dest('./dest'))

})

//语法检查
gulp.task('jshint', function () {

    return gulp.src('./src/js/*.js')

        .pipe(jshint())

        .pipe(jshint.reporter('default'));

});

//压缩html
gulp.task('htmlmin', function () {

    var options = {

        collapseWhitespace: true,

        collapseBooleanAttributes: true,

        removeComments: true,

        removeEmptyAttributes: true,

        removeScriptTypeAttributes: true,

        removeStyleLinkTypeAttributes: true,

        minifyJS: true,

        minifyCSS: true

    };

    gulp.src('./src/html/*.html')

        .pipe(htmlmin(options))

        .pipe(gulp.dest('html'));

});

//合并压缩css
gulp.task('minifycss', function () {

    return gulp.src('./src/css/*.css')

        .pipe(concat('main.css'))    	        //合并src/css下所有css到main.css

        .pipe(gulp.dest('./css'))               //将合并后的css文件输出到css文件夹

        .pipe(rename({suffix: '.min'}))         //rename压缩后的文件名

        .pipe(minifycss())                      //执行压缩

        .pipe(gulp.dest('./css'));              //输出文件

});

//压缩，合并 js
gulp.task('minifyjs', function () {

    return gulp.src('./src/js/*.js')

        .pipe(concat('event.js'))    	

        .pipe(gulp.dest('./js'))      	   

        .pipe(rename({suffix: '.min'}))     

        .pipe(uglify())    	

        .pipe(gulp.dest('./js'));  	

});

//压缩图片
gulp.task('minifyImg', function () {

    return gulp.src('./src/img/*')

        .pipe(imagemin())

        .pipe(gulp.dest('image'));

})


//执行压缩前，先删除以前压缩的文件

gulp.task('clean', function () {

    return del(['./css/all.css', './css/all.min.css', './lib/all.js', './lib/all.min.js'])

});


//定义默认任务

//压缩js需要在检查js之后操作

gulp.task('default', ['less', 'sass', 'jshint'], function () {

    gulp.start('minifycss', 'minifyjs', 'minifyImg', 'htmlmin');

});

```

## 七、运行gulp

1. 执行所有定义的任务

   ``` gulp  ```

2. 执行指定任务   

    ``` gulp 任务名  如gulp sass ```

   

## 参考

* [gulp中文网入门指南](http://www.gulpjs.com.cn/docs/getting-started/)
* [使用gulp生成sprites图片和样式表](https://www.w3ctrain.com/2015/12/09/generating-sprites-with-gulp/)
* [gulp详细入门教程](http://www.ydcss.com/archives/18)
