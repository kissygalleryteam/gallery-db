# 本地使用kg

（[xcake](http://def.taobao.net/?doc/#def-xcake)和[bee](http://gallery.kissyui.com/guide/generator-bee%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B.html)将默认支持。）

假设你的工程目录：

```
demo           
|      |---------src
|      |      |---------kg
```

请安装bower：

    npm install -g bower

## 增加 .bowerrc 文件：

文件内容如下：

    {
        "directory":"./src/kg",
        "shorthand_resolver":"http://gitlab.alibaba-inc.com/{{{owner}}}/{{{package}}}.git"
    }
    
将bower拉取的代码到 src/kg 目录。

## 增加 bower.json 文件：

    {
        "name": "demo",
        "ignore": [
            "**/*",
            "!index.js",
            "!index.css",
            "!lib/**/*"
        ],
        "dependencies": {
            "reactive": "kg/reactive"
        }
    }
    

dependencies 配置上依赖的组件，格式是 "组件名": "kg/组件名"。

默认拉取的组件master代码，如果需要拉取指定版本代码："reactive": "kg/reactive#publish/0.2.0"。

## 在工程模块中使用组件

    KISSY.add(function(S){
        var Reactive = S.require('./kg/reactive');
    })
    
这时候组件不需要版本号了，代码打包到项目模块文件中！
    



