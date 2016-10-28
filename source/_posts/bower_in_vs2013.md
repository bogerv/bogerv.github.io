---
title: 在 VS2013 中使用 bower
date: 2016-10-26 17:35:03
tags: [bower,VS2013,gulp]
---

## 简介
VS2015 和 Asp.Net Core 的发布, 集成了很多方便的前端工具, bower 就是其中之一. 我们使用 bower 可以很方便的安装项目所依赖的前端库.
同样在 VS2013 或者 VS2015 中创建 MVC5 项目时, 我们也可以使用 bower.

## 安装 Node 和 Bower 和 Git
1. 要安装 Node, [download and install node.js](http://nodejs.org/)

2. 使用 Node 中集成的 npm 安装 bower 
    ```
    npm install bower -g
    ```
3. 安装 Git

## 为项目添加 Bower
### 在项目中创建一个新的 bower.json 文件
1. 第一种方式

    在项目目录下打开控制台, 运行 `bower init` 命令, 按步骤初始化完成

2. 第二种方式

    使用 VS2013 或 VS2015 在项目中, 右键新建 bower.json 项

### 使用 bower
1. 第一种方式 -- 直接在控制台中运行命令
    ```
    bower install jquery --save
    ```

2. 第二种方式 -- 使用 VS 执行

    在 bower.json 文件的 dependencies 下添加所需要的前端库; 右键 bower.json 文件; Restore Packages;

> 默认情况下, bower components 被安装在 bower_components 文件夹下. 通常不需要将此文件夹包含在项目中, 也不会加入到源码管理中. 使用 `bower install` 可以轻松地恢复这些文件.

### 从 bower_components 文件夹中获取需要的文件
有些人直接将 bower_components 文件夹包含在项目中, 并且直接引用 JavaScritp/Css文件. 通常 bower_components文件夹包含很多我们不需要的东西. 最好的做法是, 把需要的文件复制到其他文件夹并且引入到项目中. 使用 client side build system, 如: [Grunt](http://gruntjs.com/) 或者 [Gulp](http://gulpjs.com/) 可以很容易做到. 我们使用 Gulp.

为我们的项目设置 Gulp, 可以参考 [How to use Gulp in Visual Studio](gulp_in_visualStudio.md)

对于 gulp 文件, 我们需要使用下面的这些 node 模块
```
npm install gulp --save-dev
npm install gulp-concat --save-dev
npm install gulp-uglify --save-dev
npm install del --save-dev 
npm install gulp-bower --save-dev 
npm install gulp-minify-css --save-dev
```

下面是 gulpfile.js 文件, MVC中由新项目模板创建的. 注释解释了每一个 gulp task 做的事情.
```
///
// include plug-ins
var gulp = require('gulp');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var del = require('del');
var minifyCSS = require('gulp-minify-css');
var copy = require('gulp-copy');
var bower = require('gulp-bower');
var sourcemaps = require('gulp-sourcemaps');

var config = {
    //JavaScript files that will be combined into a jquery bundle
    jquerysrc: [
        'bower_components/jquery/dist/jquery.min.js',
        'bower_components/jquery-validation/dist/jquery.validate.min.js',
        'bower_components/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js'
    ],
    jquerybundle: 'Scripts/jquery-bundle.min.js',

    //JavaScript files that will be combined into a Bootstrap bundle
    bootstrapsrc: [
        'bower_components/bootstrap/dist/js/bootstrap.min.js',
        'bower_components/respond-minmax/dest/respond.min.js'
    ],
    bootstrapbundle: 'Scripts/bootstrap-bundle.min.js',

    //Modernizr
    modernizrsrc: ['bower_components/modernizr/modernizr.js'],
    modernizrbundle: 'Scripts/modernizer.min.js',

    //Bootstrap CSS and Fonts
    bootstrapcss: 'bower_components/bootstrap/dist/css/bootstrap.css',
    boostrapfonts: 'bower_components/bootstrap/dist/fonts/*.*',

    appcss: 'Content/Site.css',
    fontsout: 'Content/dist/fonts',
    cssout: 'Content/dist/css'

}

// Synchronously delete the output script file(s)
gulp.task('clean-vendor-scripts', function (cb) {
    del([config.jquerybundle,
              config.bootstrapbundle,
              config.modernizrbundle], cb);
});

//Create a jquery bundled file
gulp.task('jquery-bundle', ['clean-vendor-scripts', 'bower-restore'], function () {
    return gulp.src(config.jquerysrc)
     .pipe(concat('jquery-bundle.min.js'))
     .pipe(gulp.dest('Scripts'));
});

//Create a bootstrap bundled file
gulp.task('bootstrap-bundle', ['clean-vendor-scripts', 'bower-restore'], function () {
    return gulp.src(config.bootstrapsrc)
     .pipe(sourcemaps.init())
     .pipe(concat('bootstrap-bundle.min.js'))
     .pipe(sourcemaps.write('maps'))
     .pipe(gulp.dest('Scripts'));
});

//Create a modernizr bundled file
gulp.task('modernizer', ['clean-vendor-scripts', 'bower-restore'], function () {
    return gulp.src(config.modernizrsrc)
        .pipe(sourcemaps.init())
        .pipe(uglify())
        .pipe(concat('modernizer-min.js'))
        .pipe(sourcemaps.write('maps'))
        .pipe(gulp.dest('Scripts'));
});

// Combine and the vendor files from bower into bundles (output to the Scripts folder)
gulp.task('vendor-scripts', ['jquery-bundle', 'bootstrap-bundle', 'modernizer'], function () {

});

// Synchronously delete the output style files (css / fonts)
gulp.task('clean-styles', function (cb) {
    del([config.fontsout,
              config.cssout],cb);
});

gulp.task('css', ['clean-styles', 'bower-restore'], function () {
    return gulp.src([config.bootstrapcss, config.appcss])
     .pipe(concat('app.css'))
     .pipe(gulp.dest(config.cssout))
     .pipe(minifyCSS())
     .pipe(concat('app.min.css'))
     .pipe(gulp.dest(config.cssout));
});

gulp.task('fonts', ['clean-styles', 'bower-restore'], function () {
    return
    gulp.src(config.boostrapfonts)
        .pipe(gulp.dest(config.fontsout));
});

// Combine and minify css files and output fonts
gulp.task('styles', ['css', 'fonts'], function () {

});

//Restore all bower packages
gulp.task('bower-restore', function() {
    return bower();
});

//Set a default tasks
gulp.task('default', ['vendor-scripts', 'styles'], function () {

});
```

在 command line 中, 使用 run gulp 运行 gulp tasks. 会输出一些 min.js 文件到 Scripts 文件夹、一些 css 和 font 文件到 Contents/dist 文件夹. 在 Visual Studio中, 包含这些新的文件到项目中.

### 移除 NuGet Packages
如果项目中存在 NuGet 安装的一些 script 和 css, 可以 uninstall 这些.
```
Uninstall-Package Microsoft.jQuery.Unobtrusive.Validation

Uninstall-Package jQuery.Validation

Uninstall-Package Bootstrap

Uninstall-Package jQuery

Uninstall-Package Respond

Uninstall-Package Modernizr
```

### 引用 Scripts 和 styles
使用新的脚本和样式文件替换原来的目录.

### 确保 Gulp 在运行项目之前先运行一次
打开 Task Explorer window. 选择默认的 gulp task, 右键点击, select Bindings-> Before Build.
这样默认的 gulp task 将会在每次编译之前运行了.