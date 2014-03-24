![http://gtms04.alicdn.com/tps/i4/T1i9hTFpBnXXcDeDnx-300-80.png](http://gtms04.alicdn.com/tps/i4/T1i9hTFpBnXXcDeDnx-300-80.png)

**generator-bee**是kissy简单工程构建器，跟generator-xcake和generator-clam有所不同，强调简单和快速，没有复杂的目录分级和复杂的命令功能，不是以页面作为划分维度，适用于小工程构建，适用于kissy mini工程。

generator-bee遵循最新的kissy规范，会生成demo页面和测试用例范例。

阿里内部环境，使用依赖表——combo的方式，不再静态合并文件。

* 作者：明河（剑平）
* [demo工程传送门](https://github.com/minghe/bee-demo)
* [反馈bug](https://github.com/minghe/generator-bee/issues)


## 安装

安装yeoman

    npm install yo grunt-cli -g

安装kissy-gallery目录生成器

    npm install generator-bee -g

生成组件目录

新建个工程目录，进入执行命令：

    yo bee


## 构建

打包文件

    grunt

监听文件改变实时编译

    grunt dev

默认编译less和生成kissy模块名和依赖表。

## 生成的目录结构

    bee-demo           // 工程名，也是库名
    |      |-----src    // 源码目录
    |      |     |---------index.js     // index页面入口脚本
    |      |     |---------mods     // 依赖的业务模块
    |      |     |---------index.less     // index页面样式
    |      |-----build    // 发布目录
    |      |     |---------deps.js     // 模块依赖表
    |      |-----demo    // demo目录
    |      |-----test    // 测试用例目录
    |      |-----build    // 发布目录
    |      |-----README.md      // 库介绍
    |      |-----gruntfile.js   // grunt打包时使用的配置信息
    |      |-----totoro-config.js       // totoro回归工具配置文件
    |      |-----package.js     // 依赖包配置

## 调试

调试工具太多了，每个人调试习惯不同，推荐使用[httpx](https://github.com/czy88840616/httpx)。

## 代码规范

模块文件使用[AMD规范](http://www.cnblogs.com/happyPawpaw/archive/2012/05/31/2528864.html)，不再建议基于KMD规范。

    KISSY.add(function(S, require){

        //初始化header模块
        var header = require('./mods/header');
        header.init();

        //初始化article模块
        var article = require('./mods/article');
        article.init();

        return true;
    });

使用**require()**来引用模块。运行**grunt dev**时，会自动补充上模块名和依赖模块，比如：

    KISSY.add('bee-demo/index',['./mods/header', './mods/article'], function(S, require){

        //初始化header模块
        var header = require('./mods/header');
        header.init();

        //初始化article模块
        var article = require('./mods/article');
        article.init();

        return true;
    });

模块建议带有返回值。

## 单元测试