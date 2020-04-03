#### 1.移动端300ms延迟

方法1:meta标签

```css
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1,minimum-scale=1, user-scalable=no">
```

 `width=device-width` 宽度为设备宽度

 `initial-scale=1` 初始比例为`1`

 `maximum-scale=1` 最大比例为`1`

 `minimum-scale=1` 最小比例为`1` 

`user-scalable=no` 用户不能进行放大缩小 

方法2:fastclick

```js
npm i fastclick -S
import FastClick from 'fastclick'	//	因所有页面都要引入所以写在main.js
FastClick.attach(document.body)
```



#### 2.1px像素问题

伪类+transform

```css
.border1
    height: .5rem
    position: relative
.border1:before
    position: absolute
    top:-.5rem
    left:0
    content: ''
    width:100%
    height:1px
    border-top:1px solid rgba(0,0,0,.3)
    transform: scaleY(0.5)
```

原理: 把原先元素的`border`去掉，然后利用`:before`或者`:after`重做`border` ，并`transform`的`scale`缩小一半，原先的元素相对定位，新做的`border`绝对定位 



#### 3.图片占位

```css
.icon-img{
    overflow: hidden
    width: 100%
    height: 0
    padding-bottom: 100%
}
```



#### 4.节流

手指滑动式会触发无数次handleTouchMove函数

原理:通过设定一个时间周期,只要这个周期内函数就不执行

```js
ihandleTouchMove (e) {
  if (this.touchStatus) {
    if (this.timer) {
      clearTimeout(this.timer)
    }
    this.timer = setTimeout(() => {
      const touchY = e.touches[0].clientY - 79
      const index = Math.floor((touchY - this.startY) / 20)
      if (index >= 0 && index < this.letters.length) {
        this.$emit('change', this.letters[index])
      }
    }, 10)
  }
}
//	设置的周期是10ms,10ms这个代码只会执行一次,大大优化了性能
```



#### 5.keep-alive

路由被加载一次后,就会把路由的内容放入内存之中,下次进入这个路由的时候就不需要重新渲染组件了

```vue
<keep-alive>
	<router-view/>
</keep-alive>
```

当keep-alive使用后,会自动执行activated钩子函数

```vue
activated(){
	if(this.lastCity !== this.city){
		this.lastCity=this.city
		this.goHomeInfo()
	}
}
```

```vue
<keep-alive exclude="Deatail">	//	使用exclude属性,可以设置不需要缓存的页面
	<router-view/>
</keep-alive>
```



#### 6.全局滚动事件

当在某页面滚动没问题,但是渠道其他页面滚动时,绑定在某页面的滚动也被执行了

```js
activated	//	页面展示时被执行
deactivated	//	页面被隐藏或者页面即将被替换成新的页面时执行

activated(){
	window.addEventListener('scroll',this.handleScroll)
}
deactivated(){
	window.removeEventListener('scroll',this.handleScroll)
}
```



7.每次路由切换时,回到最前面

```js
// 每次做路由切换时，让新显示的页面回到最顶部
    scrollBehavior (to, from, savedPosition) {
      return { x: 0, y: 0}
    }
```

