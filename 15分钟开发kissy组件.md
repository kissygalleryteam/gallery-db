# 十五分钟开发一个kissy组件

kpm（组件后台系统）：[http://kpm.f2e.taobao.net/](KPM)(阿里内网才能访问)，[changelog](https://github.com/kissygalleryteam/kpm/issues/238)

请先加入组件作者旺旺群：`600859238`。

不再使用gallery包名！使用**kg**（千克~）

旧的kissy需要配置下包：

````sh

   S.config({
        packages:[
            {
                name:"kg",
                path:"http://g.tbcdn.cn/kg/",
                charset:"utf-8",
                ignorePackageNameInUri:true
            }
        ]
    });

````

## Step1: 安装kpm工具

````sh
npm install yo grunt-cli -g
````

````sh
npm install generator-kpm -g
````

请确保本地带有NodeJs和Npm环境。

## Step2:提交组件信息

进入[http://kpm.f2e.taobao.net/](KPM)，点击添加组件，填写组件信息，提交后，系统会自动在[http://gitlab.alibaba-inc.com/groups/kg](kg)中创建了组件库。

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
|      |-----gruntfile.js                                 // grunt打包时使用的配置信息
|      |-----totoro-config.js                                 // totoro回归工具配置文件
```

## Step4:组件的开发和调试

请看**demo/index.html**

配置组件路径到本地：

```javascript
var S = KISSY;S.config({
        packages:[
            {
                name:"kg",
                path:"http://g.tbcdn.cn/kg/",
                charset:"utf-8",
                ignorePackageNameInUri:true
            }
        ]
    });

    if (S.Config.debug) {
        var srcPath = "../";
        S.config({
            packages:[
                {
                    name:"kg/demo/2.0.0",
                    path:srcPath,
                    charset:"utf-8",
                    ignorePackageNameInUri:true
                }
            ]
        });

    }
```

ignorePackageNameInUri这个配置参数，可以不让路径带上gallery目录，这样本地就不需要创建一个gallery目录。


组件初始化脚本demo：

```javascript
KISSY.use('gallery/slide/2.0.0/index',function (S,Slide) {
	var a = new Slide();
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
grunt
````

打包成功后，会在`build`目录下生成`index.js`和`index-min.js`，同时`css`文件、`less`和`scss`文件也会被构建。

## Step6:发布组件

````sh
git tag publish/2.0.0
git push origin publish/2.0.0
````
留意：非覆盖发布！

同步文档的方式：

````sh
git push origin daily/2.0.0:doc
````

推送到远程分支doc会自动触发同步！so easy！