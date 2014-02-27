# kissy gallery组件开发规范说明

## 安装工具

```javascript
npm install generator-kpm -g
```

generator-kpm 由明正开发和维护，工具的详细使用说明请看[generator-kpm](https://github.com/kissygalleryteam/generator-kpm)。

工具依赖于yoeman，如果本机没有安装，请先安装yeoman：

```javascript
npm install yo -g
```

## 使用工具生成组件目录

```javascript
yo kpm 1.0
```

打包运行：

```javascript
grunt
```

## 目录规范

拿 offline 举例，具体的可以参考[example](https://github.com/kissygalleryteam/offline "例子") :

```
offline           // 组件目录名, 小写, 多字符用 – 分隔
|      |-----1.0    // 版本名字, 两个数字表示 x.x
|      |     |---------demo                         // 用于存放demo的文件夹
|      |     |---------plugin                       // 用于存放组件的插件，可以没有
|      |     |---------build                       // 用于存放组件打包后的文件
|      |     |---------guide                        // 用于介绍组件入门的文件夹
|      |     |         |--------index.md       // 用于介绍组件入门的文档
|      |     |---------test                        // 单元测试放的目录
|      |     |---------index.js                     // 组件入口文件
|      |-----README.md                                  	// 用于介绍组件信息和版本更新
|      |-----abc.json                                 // 组件信息（用于首页显示信息）
|      |-----gruntfile.js                                 // grunt打包时使用的配置信息
|      |-----totoro-config.js                                 // totoro回归工具配置文件
```

### 规范说明

* 统一使用 UTF-8编码；
* 模块起始必须是gallery名，比如"gallery/offline/1.0/index"
* index.js为组件入口文件，必须存在
* abc.json为组件信息，用于首页的组件信息显示，必须存在（有工具后会自动生成）
* 组件的教程放在<code>guide</code>目录下，必须是<code>md</code>文件，不能是静态html页面！
* 可以在package.json中指定打包发布模块
* 打包后的文件会放在<code>build</code>目录中，发布到cdn上的只是build目录下的文件
* README.md必须存在，简单介绍下组件信息！

### abc.json内容

```javascript
{
    "name": "auth",
    "version": "1.6",
    "desc": "最强大的表单校验组件",
    "tag":"表单,表单验证",
    "author": {
        "name": "张挺/明河",
        "email": "zhangting@taobao.com",
        "page": "https://github.com/czy88840616"
    }
    "cover":""
}
```

想要在首页显示头像，email字段需要是[gravatar](http://cn.gravatar.com/)上的，没有的话可以到gravatar上传一个。

page字段指向你的个人主页。

cover字段为组件封面图片

## 组件demo和本地调试

请看**demo/index.html**

配置组件路径到本地：

```javascript
    var srcPath = "../../";
    S.config({
        packages:[
            {
                name: "gallery/auth",
                path: srcPath,
                charset: "utf-8",
                ignorePackageNameInUri: true
            }
        ]
    });
```

ignorePackageNameInUri这个配置参数，可以不让路径带上gallery目录，这样本地就不需要创建一个gallery目录。


组件初始化脚本demo：

```javascript
KISSY.use('gallery/offline/1.0/index',function (S,Offiline) {
	var a = new Offiline();
});
```


## 组件发布

我们需要把组件发布到淘宝cdn上，方便用户直接引用。

非阿里员工，发布请找剑平。

### 添加新组件

进入[kpm发布系统](http://kpm.f2e.taobao.net/)，点击申请加入新组件：

![http://gtms02.alicdn.com/tps/i2/T1H_SNFAtXXXb8_2DG-366-264.png](http://gtms02.alicdn.com/tps/i2/T1H_SNFAtXXXb8_2DG-366-264.png)

填写上你的组件库名称和你的github账户名称。

通知剑平或伯方审核组件。

### 发布组件

审核通过了，进入kpm系统，发布的按钮就会点亮。

![http://gtms02.alicdn.com/tps/i2/T10ReMFDlaXXc26ILI-474-143.png](http://gtms02.alicdn.com/tps/i2/T10ReMFDlaXXc26ILI-474-143.png)

点击预发，预发代码并同步文档，文档同步成功后，组件就会在gallery网站中出现。

最后点击发布，正式发布组件。

发布是否成功可以点击日志按钮查看。

(PS:使用工具的yo kpm refer命令可以查看组件被哪些页面引用)

### 旧组件发布

老的组件接入，只需要修改一下gitname就行，如下图所示：

![https://github-camo.global.ssl.fastly.net/04b790cfb6574a47b3e9139e3baffe727e98ae14/687474703a2f2f67746d7330312e616c6963646e2e636f6d2f7470732f69312f54316430305f464234615858586c326349772d3734362d3430362e706e67](https://github-camo.global.ssl.fastly.net/04b790cfb6574a47b3e9139e3baffe727e98ae14/687474703a2f2f67746d7330312e616c6963646e2e636f6d2f7470732f69312f54316430305f464234615858586c326349772d3734362d3430362e706e67)

把kissygalleryteam改成你自己的github用户名即可。

## 运行单测

//待补充

## 如何组织组件代码

一个组件模块代码如下

```javascript
/**
 * @fileoverview 文件用户
 * @author 作者的
 **/
KISSY.add(function(S, Node, Base) {
    var EMPTY = '';
    var $ = Node.all;
    /**
     * @name ImageZoom
     * @class 图片放大器
     * @since 1.4
     * @constructor
     * @extends Base
     */
    function ImageZoom(config) {
        var self = this;
        //调用父类构造函数
        ImageZoom.superclass.constructor.call(self, config);
    }
    S.extend(ImageZoom, Base, /** @lends ImageZoom.prototype*/{

    }, {ATTRS : /** @lends ImageZoom*/{

    }});
    return ImageZoom;
}, {requires : ['node','base']});
/**
 * changes:
 * 明河：1.4
 *           - XXXX
 */
```

use时

```
KISSY.use('gallery/uploader/1.4/plugins/imageZoom/imageZoom', function(S, ImageZoom) {
   new ImageZoom();
});
```

### 代码规范

* 代码基于kissy1.4.x，无需兼容kissy1.1.6
* add()模块时，不要加上模块名称，使用工具自定生成
* 组件请继承base模块，使用get()和set()方法获取/设置属性
* 推荐使用Node方式获取节点
* 不在模块内动态<code>use()</code>js和css
* 复杂组件请继承RichBase
* 组件注释符合YUIDoc规范
* js中需要模版的请使用xtemplate模块
* 请不要直接向KISSY全局变量注入属性或方法