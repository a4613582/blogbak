---
title: js运行机制
date: 2018-06-04 10:49:12
tags:
    - js
    - 原理
---
Gulp什么是Gulp,为什么使用GulpGulp是基于Node.js的前端构建工具，在我们平常web开发中，可以借助Gulp的一些插件完成很多的前端任务。如：代码的编译（sass）、压缩css，js、图片、合并js，css、es6转es5、自动刷新页面等。
前端构建工具还有: grunt、webpack、browserify.
官网
gulp常用地址：
	* 
gulp官方网址：http://gulpjs.comgulp
	* 
插件地址：http://gulpjs.com/pluginsgulp
	* 
中文API：http://www.ydcss.com/archives/424

安装Gulp
首先我们要全局安装一遍：

$ npm install gulp -g
接着我们要进去到项目的根目录再安装一遍（确保你根目录存在package.json文件，执行npm init -y生成即可）：

$ npm install gulp@3.9.1 --save-dev
	* 
推荐安装3版本的，比较稳定 。其相应的插件也较多。

创建gulp任务
使用gulp的第一步是在项目根目录创建gulpfile.js文件,引入gulp

var gulp = require('gulp');
现在使用变量gulp来创建一个任务，基本的格式是这样的。


//有三个参数 //第一个：任务名称 ，//第二个：依赖的其他任务，无依赖可省略此参数//第三个：回调函数
gulp.task('task-name',['依赖任务1，依赖任务2',...], function() {
// 任务代码
});
创建一个名为hello的任务，以下hello任务会打印hello world


gulp.task('hello', function() {
console.log('hello world');
});
在项目根目录Command Line中里执行hello任务。

$ gulp hello
结果会输出： hello world
不写任务名，默认会执行default任务。任务依赖


gulp.task('a',function(){
console.log('a任务执行');
})

gulp.task('b',function(){
console.log('b任务执行');
})

//定义一个c任务,但是依赖于a和b任务(先执行依赖的任务最后执行当前任务)
gulp.task('c',['a','b'],function(){
console.log('c任务执行');
})Gulp中的src()、dest()、pipe()方法
	* 
src()：设置源文件路径
	* 
pipe(): 管道方法，表示进行下一步的操作
	* 
dest(): 设置目标文件路径


任务1：将src目录下的index.html文件复制到dist目录中


gulp.task("copyHtml",function(){
return gulp.src('src/index.html')
.pipe( gulp.dest('dist') )
})
任务2：将lib目录下的所有的后缀为.js文件复制到dist目录中的子目录js中


gulp.task("copyJs",function(){
return gulp.src('lib/*.js')
.pipe( gulp.dest('dist/js') )
})
注意 * 和 ** 的区别
*:所有一级子目录
**:所有子孙目录


gulp.task('copyJs',function(){
// 第一个*号代表lib下的所有的一级子目录
gulp.src('./lib/*/*.js')
.pipe( gulp.dest('dist') )
})

gulp.task('copyJs',function(){
// 第一个**号代表lib下的所有的子目录中的子孙目录，同时也会获取lib目录下面的js文件
gulp.src('./lib/**/*.js')
.pipe( gulp.dest('dist') )
})

gulp.src路径的匹配模式：
可参考地址：`
http://www.ydcss.com/archives/424Gulp中的watch方法
作用：主要用于监听文件的改变
如：监听src目录下的index.html文件，如果该文件有改动，自动执行copyHtml任务


//布置一个任务，监听src目录下的index.html文件，只要一改动就同步到dist目录下的index.html中
gulp.task('watchHtml', function () {
gulp.watch('./src/index.html', ['copyHtml'])
})Gulp插件的使用
我们将要使用Gulp插件来完成我们以下任务：
	* 
sass的编译（gulp-sass）
	* 
压缩css（gulp-cssmin）
	* 
压缩js代码（gulp-uglify）
	* 
重命名（gulp-rename]）
	* 
es6转es5 (gulp-babel)
	* 
自动添加css前缀（gulp-autoprefixer）
	* 
合并js、css文件（gulp-concat）
	* 
压缩图片（gulp-imagemin）
	* 
自动刷新页面（browser-sync）


常用插件参考
上面的插件都需要在项目根目录（即package.json所在目录）先安装在使用，具体使用方法可以查看github其所在地址和所在的npmjs官方仓库