![http://gtms04.alicdn.com/tps/i4/T1i9hTFpBnXXcDeDnx-300-80.png](http://gtms04.alicdn.com/tps/i4/T1i9hTFpBnXXcDeDnx-300-80.png)

**generator-bee**是kissy简单工程构建器，跟generator-xcake和generator-clam有所不同，强调简单和快速，没有复杂的目录分级和复杂的命令功能，不是以页面作为划分维度，适用于小工程构建，适用于kissy mini工程。

generator-bee 遵循最新的kissy规范，会生成demo页面和测试用例范例。


* 作者：明河（剑平）
* [工具库地址](https://github.com/minghe/generator-bee)
* [demo工程传送门](https://github.com/minghe/bee-demo)
* [反馈bug](https://github.com/minghe/generator-bee/issues)


## 安装

安装yeoman

    npm install yo gulp -g

安装kissy-gallery目录生成器

    npm install generator-bee -g

生成组件目录

新建个工程目录，进入执行命令：

    yo bee


## 构建

打包文件

    gulp

监听文件改变实时编译

    gulp watch

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
    |      |-----gulpfile.js   // gulp打包时使用的配置信息
    |      |-----totoro-config.js       // totoro回归工具配置文件
    |      |-----package.js     // 依赖包配置

## 代码规范

模块文件使用CMD规范。

    //初始化header模块
    var header = require('./mods/header');
    header.init();
    
    //初始化article模块
    var article = require('./mods/article');
    article.init();

使用**require()**来引用模块。运行**grunt dev**时，会自动合并和编译文件到build目录。

## 工程内使用kg组件

编辑bower.json：

    "dependencies": {
        "reactive": "kg/reactive#0.2.0"
    }
    
格式为 kg/组件名#版本号 。

然后运行bower install ，会自动拉取组件到src/kg目录。

业务模块引用组件：

    var Reactive = require('./kg/reactive/index');