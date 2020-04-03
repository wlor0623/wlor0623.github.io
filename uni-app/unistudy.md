## 组件/标签的变化

- div 改成 [view](https://uniapp.dcloud.io/component/view)
- span、font 改成 [text](https://uniapp.dcloud.io/component/text)
- a 改成 [navigator](https://uniapp.dcloud.io/component/navigator)
- img 改成 [image](https://uniapp.dcloud.io/component/image)
- [input](https://uniapp.dcloud.io/component/input) 还在，但type属性改成了confirmtype
- [form](https://uniapp.dcloud.io/component/form)、[button](https://uniapp.dcloud.io/component/button)、[checkbox](https://uniapp.dcloud.io/component/checkbox)、[radio](https://uniapp.dcloud.io/component/radio)、[label](https://uniapp.dcloud.io/component/label)、[textarea](https://uniapp.dcloud.io/component/textarea)、[canvas](https://uniapp.dcloud.io/component/canvas)、[video](https://uniapp.dcloud.io/component/video) 这些还在。
- select 改成 [picker](https://uniapp.dcloud.io/component/picker)
- iframe 改成 [web-view](https://uniapp.dcloud.io/component/web-view)
- ul、li没有了，都用view替代
- audio 不再推荐使用，改成api方式，[背景音频api文档](https://uniapp.dcloud.io/api/media/background-audio-manager?id=getbackgroundaudiomanager)
  其实老的HTML标签也可以在uni-app里使用，uni-app编译器会在编译时把老标签转为新标签，比如把div编译成view。但不推荐这种用法，调试H5端时容易混乱。

**除了改动外，新增了一批手机端常用的新组件**

- scroll-view [可区域滚动视图容器](https://uniapp.dcloud.io/component/scroll-view)

- swiper [可滑动区域视图容器](https://uniapp.dcloud.io/component/swiper)

- icon [图标](https://uniapp.dcloud.io/component/icon)

- rich-text [富文本（不可执行js，但可渲染各种文字格式和图片）](https://uniapp.dcloud.io/component/rich-text)

- progress [进度条](https://uniapp.dcloud.io/component/progress)

- slider [滑块指示器](https://uniapp.dcloud.io/component/slider)

- switch [开关选择器](https://uniapp.dcloud.io/component/switch)

- camera [相机](https://uniapp.dcloud.io/component/camera)

- live-player [直播](https://uniapp.dcloud.io/component/live-player)

- map [地图](https://uniapp.dcloud.io/component/map)

- cover-view [可覆盖原生组件的视图容器](https://uniapp.dcloud.io/component/cover-view?id=cover-view) 

  cover-view需要多强调几句，uni-app的非h5端的video、map、canvas、textarea是原生组件，层级高于其他组件。如需覆盖原生组件，比如在map上加个遮罩，则需要使用cover-view组件

  

## JS变化

标准js语法和api都支持，比如if、for、settimeout、indexOf等。

但浏览器专用的window、document、navigator、location对象，包括cookie等存储，只有在浏览器中才有，app和小程序都不支持。

1. alert,confirm 改成 [uni.showmodel](https://uniapp.dcloud.io/api/ui/prompt?id=showmodal)

2. ajax 改成 [uni.request](https://uniapp.dcloud.io/api/request/request)

3. cookie、session 没有了，local.storage 改成 [uni.storage](https://uniapp.dcloud.io/api/storage/storage?id=setstorage)

   

## CSS变化

- 通配符选择器不支持
- body改为了page
- rem只能用于h5,推荐使用rpx全端支持

背景图和字体文件尽量不要大于40k。会影响性能。如果非要大于40k，需放到服务器侧远程引用或base64后引入



## 工程结构和页面管理

- 每个可显示的页面,都必须在page.json中注册相当于vue的路由
- 原来app.json被一拆为二。页面管理，被挪入了uni-app的pages.json；非页面管理，挪入了manifest.json
- 原来的app.js和app.wxss被合并到了app.vue中



## 目录结构

```js
┌─components            uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                存放本地网页的目录
├─platforms             存放各平台专用页面的目录
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用静态资源（如图片、视频等）的目录，**注意：**静态资源只能存放于此
├─wxcomponents          存放小程序组件的目录
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息
└─pages.json            配置页面路由、导航条、选项卡等页面类信息

// static的目录下js文件不会被编译,如果里面有es6的代码,不经过转换直接运行在手机设备上会报错

// css less/scss等资源也不要放在static目录下,建议公用的资源放在common目录下
```



## 生命周期

#### 应用生命周期

- onLaunch		当uni-app初始化完成时触发(全局只触发一次)
- onShow		   当 uni-app启动,或从后台进入前台显示
- onHide			当 uni-app从前台进入后台

应用周期仅可在App.vue中监听 ,在其他页面监听无效

#### 页面生命周期

onLoad	监听页面加载,参数为上个页面传递的参数,类型为Objecj

onShow	监听页面显示,页面每次出现在屏幕上都触发,包括从下级页面返回露出当前页面

onReady	监听页面初次渲染完成,如果 渲染速度快,会在页面进入动画完成前触发

建议用onReady代替vue的mounted,onLoad代替vue的created



## 路由

#### 页面栈

| 路由方式   | 页面栈表现                        | 触发时机                                                     |
| ---------- | --------------------------------- | ------------------------------------------------------------ |
| 初始化     | 新页面入栈                        | uni-app 打开的第一个页面                                     |
| 打开新页面 | 新页面入栈                        | 调用 API   [uni.navigateTo](https://uniapp.dcloud.io/api/router?id=navigateto)  、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator) |
| 页面重定向 | 当前页面出栈，新页面入栈          | 调用 API   [uni.redirectTo](https://uniapp.dcloud.io/api/router?id=redirectto)  、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator) |
| 页面返回   | 页面不断出栈，直到目标返回页      | 调用 API  [uni.navigateBack](https://uniapp.dcloud.io/api/router?id=navigateback)   、使用组件 [](https://uniapp.dcloud.io/component/navigator?id=navigator) 、用户按左上角返回按钮、安卓用户点击物理back按键 |
| Tab 切换   | 页面全部出栈，只留下新的 Tab 页面 | 调用 API  [uni.switchTab](https://uniapp.dcloud.io/api/router?id=switchtab)  、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator)  、用户切换 Tab |
| 重加载     | 页面全部出栈，只留下新的页面      | 调用 API  [uni.reLaunch](https://uniapp.dcloud.io/api/router?id=relaunch)  、使用组件  [](https://uniapp.dcloud.io/component/navigator?id=navigator) |



## NPM支持

- 初始化npm工程

  ​	若项目之前未使用npm管理依赖（项目根目录下无package.json文件），先在项目根目录执行命令初始化npm工程

```shell
npm init -y
```

- 安装依赖

  在项目根目录执行命令安装npm包：

```shell
npm install packageName --save
```

- 使用

  安装完即可使用npm包，js中引入npm包：

```js
import package from 'packageName'
const package = require('packageName')
```

<!--为多端兼容考虑，建议优先从 [uni-app插件市场]获取插件。直接从 npm 下载库很容易只兼容H5端。-->

<!--node_modules 目录必须在项目根目录下。不管是cli项目还是HBuilderX创建的项目。-->

<!--支持安装 mpvue 组件，但npm方式不支持小程序自定义组件（如 wxml格式的vant-weapp）-->



## 跨度兼容问题

```vue
<view class="content">
  <! -- #ifdef MP-WEIXIN -->
  <view>只会编译到微信小程序</view>
  <! -- #endif -->
  <! -- #ifdef APP-PLUS -->
  <view>只会编译到app</view>
  <! -- #endif -->
</view>
```

