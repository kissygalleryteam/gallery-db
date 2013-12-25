# 十五分钟开发一个kissy组件

发布和同步系统：[http://kpm.f2e.taobao.net/](KPM)(阿里内网才能访问)

请先加入组件作者旺旺群：`600859238`。

组件review和fork：@伯才

答疑：@伯方、@翰文


> 阿里内网用户：可以自主发布和同步文档，前提是`abc.json`中的邮箱必须使用内网邮箱，不要使用`github`邮箱。
>
> 非阿里内网用户：发布和同步文档，请联系@伯才。

## Step0: 在github中创建个组件库

比如，你的组件名称是[Slide](https://github.com/jayli/slide)，创建个库名叫[Slide](https://github.com/jayli/slide)。并Fork至`@kissygalleryteam`（若无权限，联系@翰文）。

![](http://gtms01.alicdn.com/tps/i1/T10QceFnheXXa2RzfA-605-435.png)

然后clone这个库到你本地。

> 库名必须是组件名，fork到官方库后，如果有修改需求，请联系翰文，不然会出现无法发布的问题！

## Step1: 安装kissy gallery组件工具

````sh
npm install yo grunt-cli -g
````

````sh
npm install generator-kissy-gallery -g
````

请确保本地带有NodeJs和Npm环境。

## Step2:创建组件目录

进入之前clone的库目录，比如`slide/`。

打开命令行工具，执行如下命令：

````sh
yo kissy-gallery 1.0
````

`1.0`为组件起始版本目录，可以自由设置。

工具会询问您组件的作者和email，这是必须输入的：

![](http://gtms01.alicdn.com/tps/i1/T1uBTvFdJeXXX0p5cq-425-367.png)

构建成功后的目录如下：

	./slide/
	├── 1.0
	│   ├── build
	│   ├── demo
	│   │   └── index.html
	│   ├── guide
	│   │   └── index.md
	│   ├── index.js
	│   ├── meta
	│   │   ├── alias.js
	│   │   └── modules.js
	│   ├── plugin
	│   └── spec
	├── abc.json
	├── .gitignore
	├── Gruntfile.js
	├── node_modules
	├── package.json
	└── README.md

目录和各个文件的用途可以看[Kissy Gallery组件开发规范说明](http://gallery.kissyui.com/guide)。

## Step3:打包文件

假设组件已经开发完成，发布前需要打包压缩下文件，运行如下命令：

````sh
grunt
````

> `grunt`默认只打包`index.js`，如果组件有其他需求，请修改[gruntfile.js](https://github.com/jayli/slide/blob/master/Gruntfile.js)的打包配置

打包成功后，会在`build`目录下生成`index.js`和`index-min.js`，同时`css`文件也会被构建。

## Step4:组件的调试

<strong>源码开发</strong>

本地Demo调试：无需HTTP服务器支持，直接打开`demo/index.html`，带上`?ks-debug`后缀即可调试代码。

<strong>线上代码调试</strong>

组件源码自带一个本地服务，包含代理服务器（[代理服务器原理](https://github.com/jayli/grunt-flexcombo)），让你能够方便调试任意线上（使用你的组件的页面）组件代码。比如，我要`debug`一个引用了[slide组件](https://github.com/jayli/slide)的页面

第一步：将组件代码clone在任意目录：`git clone Git地址`

第二步：补全`node_modules`：在组件目录`./slide/`内执行`npm install`

第三步：绑定Host & 绑定代理（推荐） 二选一

1. 将cdn配向开发机`127.0.0.1 g.tbcdn.cn a.tbcdn.cn`
1. 将浏览器或者设备HTTP代理配置到本机的反向代理服务的端口（推荐此方法）

比如在手机终端（无法设置Host）的环境中，设置代理的方法：

![](http://gtms01.alicdn.com/tps/i1/T1bePRFlVXXXXhb4nD-502-341.png)

第四步：基于代码启动服务：在`./slide/`目录中执行：

	grunt debug

第五步：修改`./slide/`中的源码（比如`./slide/1.2/index.js`），打开使用了slide的页面，可开始调试

![](http://gtms01.alicdn.com/tps/i1/T1.ZhxFvXbXXb0CP6.-596-364.png)

## Step5:补充组件描述、教程、demo

打开abc.json，修改组件信息，比如像下面的内容：


```javascript
{
    "name": "slide",
    "version":"1.2",
    "author":{
		"name":"拔赤",
		"email":"bachi@taobao.com",
		"page":"https://github.com/jayli"
	},
    "cover":"http://img02.taobaocdn.com/tps/i2/T1C1X_Xs8gXXcd0fwt-322-176.png",
    "desc":"幻灯组件"
}
```

> 想要在首页显示头像，email字段需要是[gravatar](http://cn.gravatar.com/)上的，没有的话可以到gravatar上传一个。

- page 字段指向你的个人主页，
- cover字段为组件封面图片。
- desc字段为组件描述
- version字段为最新的组件代码版本
- author字段为作者信息

### 组件使用教程和demo

组件的所有的教程，必须放在guide目录下，而且必须是md文件，服务器会自动渲染成html页面。

demo必须放在demo目录，所有的demo请仿照着demo/index.html来写，特别是gallery包的配置。

guide的访问地址示例：[http://gallery.kissyui.com/slide/1.2/guide/index.html](http://gallery.kissyui.com/slide/1.2/guide/index.html)。

demo的访问地址示例：[http://gallery.kissyui.com/slide/1.2/demo/index.html](http://gallery.kissyui.com/slide/1.2/demo/index.html)。

请把demo的入口放在guide的md文件内。

## Step6:发布组件

### 第一次发布在[kpm](https://github.com/kissygalleryteam/kpm/issues)下建一个issue

issue的内容可以参考：[editorUploader组件](https://github.com/kissygalleryteam/kpm/issues/26)，正文带上你用户名下的组件库路径。

issue标题统一为:add 组件名称。

新建issue的目的有二个：

* 通知官方库管理员fork你的组件库(审核)
* 当你的组件发布成功后，系统会反馈发布消息到你建的issue。


### 向官方库发送pull request

前提是管理员已经fork你的组件库。

将你的代码提交到自己的github库后，发一个pull request到kissygalleryteam用户名下的库。

### 使用kpm发布和同步文档

进入[http://kpm.f2e.taobao.net/](KPM)，如下图：

![kpm](http://img01.taobaocdn.com/tps/i1/T1xK9lFk8bXXcOe6rm-462-259.png)

如果您没有找到自己的组件，请把邮箱发送给翰文，让他配下权限。

![action](http://img01.taobaocdn.com/tps/i1/T1wsKlFbtbXXarLv7e-420-160.png)

点击*"prepub"*会触发如下功能：

* 官方库自动merge你的更新
* 同步组件文档和组件信息
* 预发你的代码

点击*"publish"*正式发布组件！

（请尽量不要覆盖式发布！以免背上故障单！）

点击*"view log"*可以查看同步和发布日志！

还不清楚的同学可以看右上角的帮助入口。

发布成功后，系统返回的消息类似如下：

![发布消息](http://img03.taobaocdn.com/tps/i3/T1jc9mXpNiXXbmmmfY-272-368.png)


