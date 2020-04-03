## 一H5配合原生开发App

#### 1.页面适配

​	(1)	viewport适配

```css
<meta name="viewport" content="width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
```

​	(2)	rem

```js
html{
	font-size : 10px	//此时屏幕宽度为64rem
}

var html = document.documentElement
var rootSize = html.clientWidth / 640 * 10
html.style.fontSiz e= rootSize+'px'
```

​	(3)	vm

```css
html {
	font-size : 15.625vm
}
<div style="width:3.2rem;height=3.2rem;background-color:hotpink" >
	在任何手机屏幕上都显示为屏幕宽度为50%的正方形
</div>
```

## 二体验优化

#### 1.体验优化

​	1.图片懒加载,CDN加速,请求的img适当减少体积

​	2.骨架屏技术 

​	3.缓解焦虑,使用一些动画或彩蛋的效果吸引用户注意,减少等待时间

#### 2.APP优化

​	(1)网络状态管理

```js
catchAjaxError = (code,status){
	swith(code){
	case 0:
		//	网络错误,请检查网络连接
		break;
	}
	case 1:
		//	请求超时
		break;
	case 2:
		//	授权错误
		break;
	case 3:
		//服务器数据异常
		break;
	default:
		//服务端错误
}
```

​	(2)状态栏处理

​		引入沉浸式状态栏

​	(3)引入原生能力

​		例如打电话功能,最好引入原生功能以及有专门的官方插件库

## 三开发框架对比

1.基础框架

​	(1)Cordova

​	(2)PhoneGap

​	(3)Intel XDK(停止开发)

2.脚手架

​	(1)ionic	(root from angular)

3.原生编译框架

​	(1)React Native

​	(2)Weex

​	(3)NativeScript

​	(4)Flutter

4.开发平台

​	(1)AppCan

​	(2)ApiCloud

​	(3)DCloud

## 四H5与原生交互

​	

