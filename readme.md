# kissy gallery组件开发规范说明

## 最低版本：kissy1.4.6

```javascript

    <script src="http://g.tbcdn.cn/kissy/k/1.4.6/seed-min.js" charset="utf-8"></script>
```

### 目录规范说明

* 统一使用 UTF-8编码；
* 模块起始必须是kg名，比如"kg/offline/2.0.0/index"
* 如果组件依赖其他组件，在bower中配置上依赖
* 组件的教程放在<code>guide</code>目录下，必须是<code>md</code>文件，不能是静态html页面！
* 打包后的文件会放在<code>build</code>目录中，发布到cdn上的只是build目录下的文件
* README.md必须存在，简单介绍下组件信息！


## 如何组织组件代码

一个组件模块代码如下

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

use时

```
    S.use('kg/demo/1.0.0/index', function (S, Demo) {
        new Demo();
    })
```

### 代码规范

* 代码基于CMD规范，KISSY.add()包裹使用工具自动生成
* 组件请继承base模块，使用get()和set()方法获取/设置属性
* 推荐使用Node方式获取节点
* 不在模块内动态<code>use()</code>js和css
* 请不要直接向KISSY全局变量注入属性或方法