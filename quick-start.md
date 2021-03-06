# 十五分钟开发一个kissy组件

发布和同步系统：[http://kpm.f2e.taobao.net/](KPM)(阿里内网才能访问)，[changelog](https://github.com/kissygalleryteam/kpm/issues/238)

请先加入组件作者旺旺群：`600859238`。

组件review和fork：@伯才，@剑平，@拔赤

答疑：@伯方、@翰文、@明正

> 阿里内网用户：可以自主发布和同步文档，前提是`abc.json`中的邮箱必须使用内网邮箱，不要使用`github`邮箱。
>
> 非阿里内网用户：发布和同步文档，请联系@伯才。

## Step0: 在github中创建个组件库

比如，你的组件名称是[Slide](https://github.com/jayli/slide)，创建个库名叫[Slide](https://github.com/jayli/slide)。<del>并Fork至`@kissygalleryteam`（若无权限，联系@翰文）</del>。

然后clone这个库到你本地。

> 库名必须是组件名。

## Step1: 安装kissy gallery组件工具

````sh
npm install yo grunt-cli -g
````

````sh
npm install generator-kpm -g
````

请确保本地带有NodeJs和Npm环境。

## Step2:创建组件目录

进入之前clone的库目录，比如`slide/`。

打开命令行工具，执行如下命令：

````sh
yo kpm 1.0
````

`1.0`为组件起始版本目录，可以自由设置。

工具会询问您组件的作者和email，这是必须输入的：

![](http://gtms01.alicdn.com/tps/i1/T1uBTvFdJeXXX0p5cq-425-367.png)

其中有两项重要配置：flexComboPort（默认81）和reserveServerPort（默认8080），含义如下：

1. flexComboPort：设置[Grunt-FlexCombo](http://github.com/jayli/grunt-flexcombo)服务的端口
1. reserveServerPort：设置Grunt-flexcombo中的反向代理的端口。

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

打包成功后，会在`build`目录下生成`index.js`和`index-min.js`，同时`css`文件、`less`和`scss`文件也会被构建。

## Step4:组件的开发和调试

组件源码自带一个本地服务，包含代理服务器（[代理服务器原理](https://github.com/jayli/grunt-flexcombo)），让你能够方便调试任意线上（使用你的组件的页面）组件代码。

<strong>源码开发</strong>

STEP.1 在组件目录中启动服务：`grunt demo`

SETP.2 浏览器（或手机设备）中绑定本地代理（8080端口）

![](http://gtms01.alicdn.com/tps/i1/T1KzNFFrxbXXcrSyrN-372-175.png)

SETP.3 打开浏览器，访问`http://demo/demo/index.html`即可

<strong>线上代码调试</strong>

比如我要`debug`一个引用了[slide组件](https://github.com/jayli/slide)的页面（淘宝首页）

第一步：将[组件代码](https://github.com/jayli/slide)clone在任意目录：`git clone https://github.com/jayli/slide.git`

第二步：补全`node_modules`：在组件目录`./slide/`内执行`npm install`

第三步：给浏览器（或手机设备）绑定代理（本机8080端口）

比如在手机终端（无法设置Host）的环境中，设置代理的方法：

![](http://gtms01.alicdn.com/tps/i1/T1bePRFlVXXXXhb4nD-502-341.png)

第四步：基于代码启动服务：在`./slide/`目录中执行：

	grunt debug

第五步：修改`./slide/`中的源码（比如`./slide/1.2/index.js`），打开使用了slide的页面，断点生效

![](http://gtms01.alicdn.com/tps/i1/T1.ZhxFvXbXXb0CP6.-596-364.png)

## Step5:补充组件描述、教程、demo

打开abc.json，修改组件信息，比如像下面的内容：


```javascript
{
    "name": "slide",
    "version":"1.2",
	"flexComboPort":"81",
	"reserveServerPort":"8080",
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
- flexComboPort为flexcombo的服务端口
- reserveServerPort为反向代理的服务端口

### 组件使用教程和demo

组件的所有的教程，必须放在guide目录下，而且必须是md文件，服务器会自动渲染成html页面。

demo必须放在demo目录，所有的demo请仿照着demo/index.html来写，特别是gallery包的配置。

guide的访问地址示例：[http://gallery.kissyui.com/slide/1.2/guide/index.html](http://gallery.kissyui.com/slide/1.2/guide/index.html)。

demo的访问地址示例：[http://gallery.kissyui.com/slide/1.2/demo/index.html](http://gallery.kissyui.com/slide/1.2/demo/index.html)。

请把demo的入口放在guide的md文件内。

## Step6:发布组件

直接登录[http://kpm.f2e.taobao.net/](http://kpm.f2e.taobao.net/)，新增组件，由管理员（剑平、拔赤、韩文）审核

### 使用kpm发布和同步文档

进入[http://kpm.f2e.taobao.net/](KPM)，新增组件，或者在老的组件基础上添加github库的前缀，如下图：

![](http://gtms01.alicdn.com/tps/i1/T1d00_FB4aXXXl2cIw-746-406.png)

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

