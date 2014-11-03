# KISSY gallery 组件升级文档

主要分为如下几个步骤

## 第一步，拉取代码
在gitlab上的kg组里找到相应的组件，拉取下来，新增一个daily/5.0.0的分支(约定的版本号是5.0.0)

## 第二步，将原代码转换成cmd的代码
这里用到的工具是 [**kmt**](https://github.com/kissyteam/kmt)，kmt是一个将KISSY 1.4版无缝升级到KISSY 5.0的工具，无需人工改写代码就能够平滑的完成KISSY升级，安装完成之后执行命令

```
kmt -s ./src -b ./src -t cmd --charset gbk  
```

即可将src目录下的1.4组件的源码转换到kissy5.0_code目录下，更多个性化的使用请参考kmt的[文档](https://github.com/kissyteam/kmt)

## 第三步，将cmd代码打包
这里的打包工具是基于gulp编写，依赖的模块是[gulp-kmc](https://www.npmjs.org/package/gulp-kmc)，gulpfile.js的编写参考下面

```
var gulp = require('gulp');
var kmc = require('gulp-kmc');
var config = {
    modName: 'easydialog', // 升级的组件名
    version: '5.0.0', // 版本号，即发布的分支（约定是5.0.0）
}
var src = './' + config.modName + '/src',
    dest = './' + config.modName + '/build';
kmc.config({
    // depFilePath: dest + 'mods-dep.js', //全局依赖文件关系，此处配置后下面的各个模块将不会再生成
    packages: [{
        name: 'kg/' + config.modName + '/' + config.version,
        combine: true,
        base: src,
        ignorePackageNameInUri: true
    }]
});

gulp.task('kmc', function() {
    gulp.src(src + "/**/*.js")
    //转换cmd模块为kissy模块
    .pipe(kmc.convert({
        seajs: true,
        minify: true, //是否压缩
        exclude: ['tasks'], //忽略该目录
        ignoreFiles: ['.combo.js', '-min.js'], //忽略该类文件
    }))
    //合并文件
    .pipe(kmc.combo({
        seajs: true,
        minify: true,
        // ext: "-min.js",
        files: [{
            src: src + '/index.js',
            dest: dest + '/index.js'
        }]
    }))
        .pipe(gulp.dest(dest));
})
gulp.task('default', ['kmc']);
```
## 第四步，更新demo以及相关配置文件
调试的时候，KISSY5.0.0的包配置可以参考这样
```
require.config({
	packages:[
	    {
	        name:"kg",
	        path:'../',
	        charset:"utf-8"
	    }
	]
});
```
模块的引用变成这样
```
require('kg/easydialog/2.5.0/index,node', function (Easydialog,Node) {
	
});
```
注意到，回调函数里已经没有KISSY这个变量，
之后再将package.json里的version改成5.0.0

## 第五步，发布
发布的流程和平时业务上的代码发布的操作流程一致，发布到publish/5.0.0分支即可完成
