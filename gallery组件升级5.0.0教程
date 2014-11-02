# KISSY gallery 组件升级文档

主要分为如下几个步骤

## 第一步，将原有的代码转换成cmd的代码
这里用到的工具是 [**kmt**](https://github.com/kissyteam/kmt)，kmt是一个将KISSY 1.4版无缝升级到KISSY 5.0的工具，无需人工改写代码就能够平滑的完成KISSY升级，安装完成之后执行命令

```
kmt -s ./src -b ./kissy5.0_code -t cmd --charset gbk  
```

即可将src目录下的1.4组件的源码转换到kissy5.0_code目录下，更多个性化的使用请参考kmt的[文档](https://github.com/kissyteam/kmt)

## 第二步，将cmd代码打包
这里的打包工具是基于gulp编写，依赖的模块是[gulp-kmc](https://www.npmjs.org/package/gulp-kmc)，gulpfile.js的编写参考下面

```
var gulp = require('gulp');
var kmc = require('gulp-kmc');
var config = {
	modName:'easydialog',// 升级的组件名
	version:'5.0.0',// 版本号，即发布的分支（约定是5.0.0）
}
var src = './'+config.modName+'/kissy5.0_code',
  dest = './'+config.modName+'/build';
kmc.config({
  // depFilePath: dest + 'mods-dep.js', //全局依赖文件关系，此处配置后下面的各个模块将不会再生成
  packages: [{
    name: 'kg/'+config.modName+'/'+config.version,
    combine: true,
    base: src,
    ignorePackageNameInUri:true
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

## 第三步，发布
升级的组件为了和kissy保持一致，约定的版本号是5.0.0，在gitlab上的 **kg** group上找到相应的组件，和平时业务上的代码发布的操作流程一致即可完成
