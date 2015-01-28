# 十五分钟开发一个kissy组件

请先加入组件作者旺旺群：`600859238` ，有问题可以直接在群里面提问。

kissy版本请使用1.4.8:

````sh

    <script src="http://g.tbcdn.cn/kissy/k/1.4.8/seed-min.js" charset="utf-8"></script>

````

## Step1: 安装kpm工具

````sh
npm install yo gulp -g
````

````sh
npm install generator-kpm -g
````

请确保本地带有NodeJs和Npm环境。

mac下如果安装失败，可能是权限问题，请加 sudo 。


## Step2:提交组件信息

进入[kpm后台](http://kpm.admin.taobao.net/)，点击添加组件，填写组件信息，提交后，系统会自动在[kg](http://gitlab.alibaba-inc.com/groups/kg)中创建了组件库。

正常的话会收到邮件通知。

## Step3:创建组件目录

clone 组件库到本地，切换到daily分支，比如daily/2.0.0。

打开命令行工具，执行如下命令：

````sh
yo kpm 2.0.0
````

生成如下目录结构

```
slide           // 组件目录名, 小写, 多字符用 – 分隔
|      |---------demo                         // 用于存放demo的文件夹
|      |---------lib                       // 用于存放组件的子模块
|      |---------build                       // 用于存放组件打包后的文件
|      |---------guide                        // 用于介绍组件入门的文件夹
|           |--------index.md       // 用于介绍组件入门的文档
|      |---------test                        // 单元测试放的目录
|      |---------index.js                     // 组件入口文件
|      |-----README.md                                  	// 用于介绍组件信息和版本更新
|      |-----gulpfile.js                                 // gulp打包时使用的配置信息
|      |-----totoro-config.js                                 // totoro回归工具配置文件
```

## Step4:组件的开发和调试

模块请使用cdm规范代码：

```javascript
var $ = require('node').all;
var Base = require('base');

var Demo = Base.extend({
    initializer:function(){
        var self = this;
        var $target = self.get('$target');
    }
},{
    ATTRS:{
        $target:{
            value:'',
            getter:function(v){
                return $(v);
            }
        }
    }
});

module.exports = Demo;
```
模块文件使用CMD规范，是无法使用源码直接调试的，所以bee 内置了个本地静态服务，运行：

    gulp

会编译文件到build目录，同时会起一个本地server，http://localhost:8585/

包配置路径指向本地服务，请看**demo/index.html**

```javascript
    var S = KISSY;
    if (S.Config.debug) {
        var debugPath = "http://localhost:8585/";
        S.config({
            packages:[
                {
                    name:"kg/vc-goto/1.0.0",
                    path:debugPath,
                    charset:"utf-8"
                }
            ]
        });
    }
```

组件初始化脚本demo：

```javascript
KISSY.use('kg/demo/1.0.0/index',function (S,Demo) {
	new Demo();
});
```


## Step6:bower配置

如果你的组件有依赖其他的gallery组件，请修改bower.json：

```javascript
    "dependencies": {
       "reactive": "kg/reactive#publish/0.2.0"
    }
```


## Step5:打包文件

假设组件已经开发完成，发布前需要打包压缩下文件，运行如下命令：

````sh
npm install
````

````sh
gulp
````

打包成功后，会在`build`目录下生成`index.js`和`index-min.js`，同时`css`文件也会被构建。

## Step6:发布组件

````sh
git tag publish/2.0.0
git push origin publish/2.0.0
````
留意：非覆盖发布！

## 同步文档：

````sh
git push origin daily/2.0.0:doc
````

推送到远程分支doc会自动触发同步！so easy！