

## 一、Nuxt环境搭建

#### 1.nuxt.js搭建项目

```js
npx create-nuxt-app	your-project			//	npx创建项目

Project name                                //  项目名称
Project description                         //  项目描述
Use a custom server framework               //  选择服务器框架
Choose features to install                  //  选择安装的特性
Use a custom UI framework                   //  选择UI框架
Use a custom test framework                 //  测试框架
Choose rendering mode                       //  渲染模式
Universal                                   //  渲染所有连接页面
Single Page App                             //  只渲染当前页面
```



#### 2.nuxt.config.js

```js
const pkg = require('./package')
module.exports = {
  mode: 'universal',    //  当前渲染使用模式
  head: {       //  页面head配置信息
    title: pkg.name,        //  title
    meta: [         //  meat
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: pkg.description }
    ],
    link: [     //  favicon，若引用css不会进行打包处理
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }
    ]
  },
  loading: { color: '#fff' },   //  页面进度条
  css: [    //  全局css（会进行webpack打包处理）
    'element-ui/lib/theme-chalk/index.css'  
  ],
  plugins: [        //  插件
    '@/plugins/element-ui'
  ],
  modules: [        //  模块
    '@nuxtjs/axios',
  ],
  axios: {},
  build: {      //  打包
    transpile: [/^element-ui/],
    extend(config, ctx) {       //  webpack自定义配置
    }
  }
}
```

#### 3.Nuxt生命周期

```js
export default {
  middleware () {}, //服务端
  validate () {}, // 服务端
  asyncData () {}, //服务端
  fetch () {}, // store数据加载
  beforeCreate () {  // 服务端和客户端都会执行},
  created () { // 服务端和客户端都会执行 },
  beforeMount () {}, 
  mounted () {} // 客户端
}

```



## 二、常用配置项

#### 1.plugins

<!--nuxtjs不会自动提取插件中的文件,因此需要手动添加component.js文件到plugins-->

```js
module.exports = { 
	plugins: 
		[ '~/plugins/components' ]
}
```

#### 2.端口与IP

开发中遇到端口被占用或指定IP的情况,在package.json中添加以下

```js
"config":{
    "nuxt":{
      "host":"127.0.0.1",
      "port":"1000"
    }
}
```

#### 3.修改SEO

```js
 head: {
    title: 'wfaceboss',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: 'Nuxt.js project' }
    ],
    link: [
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }
    ]
  },
```

#### 4.全局CSS

```css
//	common.css
html{
	color:red
}
```



```js
//	nuxt.config.js
css:['~/assets/css/normailze.css']
```

#### 5.错误页面

在layout下建立error.vue文件

```js
<template>
  <div>
      <h2 v-if="error.statusCode==404">404页面不存在</h2>
      <h2 v-else>500服务器错误</h2>
      <ul>
          <li><nuxt-link to="/">HOME</nuxt-link></li>
      </ul>
  </div>
</template>

<script>
export default {
  props:['error'],
}
</script>

//	错误需要在<script>里声明,否则程序找不到err.statusCode
```

#### 6.meta设置

> 需求:制作一个新闻页面,为了搜索引擎对新闻的收录,需要在每个新闻页面都有不同的title和meta

​	(1)在pages/news/index.vue的链接中,传入一个title,目的为了在新闻具体页面接收title,形成文章标题

```vue
<li><nuxt-link :to="{name:'news-id',params:{id:123,title:'nuxt.com'}}">News-1</nuxt-link></li>
```

​	(2)修改pages/news/_id.vue,让他根据传递值变成独特的meta和title标签

```vue
<template>
  <div>
      <h2>News-Content [{{$route.params.id}}]</h2>
      <ul>
        <li><a href="/">Home</a></li>
      </ul>
  </div>
</template>

<script>
export default {
  validate ({ params }) {
    // Must be a number
    return /^\d+$/.test(params.id)
  },
  data(){
    return{
      title:this.$route.params.title,
    }
  },
//独立设置head信息
  head(){
      return{
        title:this.title,
        meta:[
          {hid:'description',name:'news',content:'This is news page'}
        ]
      }
    }
}
</script>
```

**!!!:为了避免自组件中的meta标签不能正确覆盖父组件相同的标签而产生重复的现象,建议利用hid键为meta标签配一个唯一的标识编号**



## 三、Nuxt路由

#### 1.基本路由

<!--nuxt是根据pages目录结构自动生成vue-router模块的路由配置-->

```js
└─pages
    ├─index.vue
    └─user
      ├─index.vue
      ├─one.vue
```

```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'user',
      path: '/user',
      component: 'pages/user/index.vue'
    },
    {
      name: 'user-one',
      path: '/user/one',
      component: 'pages/user/one.vue'
    }
  ]
}
```

#### 2.页面跳转

- 不能写成a标签,因为是重新获取一个新的页面,并不是SPA
- <nuxt-link to="/users"></nuxt-link>
- this.$router.push('/users')

#### 3.动态路由

nuxt中的动态路由,需要创建**对应的以下划线作为前缀**的Vue文件或目录

获取动态参数{{$route.params.id}}

4.跳转路由传递参数并且取值

(1)使用nuxt传递参数

```vue
<template>
  <div>
    <ul>
      <li><nuxt-link :to="`informa/${item.newsCode}-${item.newsType}`"></li>
    </ul>
  </div>
</template>
```

**name其实指向的是路由,而路由区分大小写,所以to后面区分大小写,建议文件夹都写成小写的**

(2)使用nuxt接收参数

```js
async asyncData(context) {
    let newsCode = context.route.params.code.split('-')[0]
    let newsType = context.route.params.code.split('-')[1]
},
```

(3)使用this.$router.push的query传递参数

```js
传递参数  -- this.$router.push({path: ' 路由 ', query: {key: value}})
参数取值  -- this.$route.query.key

注: 使用这种方式，传递参数会拼接在路由后面，出现在地址栏

```

(4)使用this.$router.push的params传递参数

```js
传递参数  -- this.$router.push({name: ' 路由的name ', params: {key: value}})
参数取值  -- this.$route.params.key

注: 使用这种方式，参数不会拼接在路由后面，地址栏上看不到参数

注意: 由于动态路由也是传递params的，所以在 this.$router.push() 方法中 path不能和params一起使用，否则params将无效。需要用name来指定页面。

```

#### 4.路由参数校验

```js
export default {
  // nuxt中使用validate方法进行路由参数校验，这个方法必须返回一个布尔值，为true表示校验通过，为false表示校验失败。注意validate不能写到methods属性中。
  validate(obj) {
    // console.log(obj);
    // return true
    return /^\d+$/.test(obj.params.id)
  }
}

```

#### 5.嵌套路由

1. 添加一个Vue文件,作为父组件
2. 添加一个与父组件同名的文件夹来存放子视图组件
3. 在父文件中,添加组件,用于展示匹配到的子视图



## 四、Nuxt路由动画效果

#### 1.全局路由动画

​	(1)添加样式文件

```css
//	assets/css/normailze.css(没有请自行建立)
.page-enter-active, .page-leave-active {
    transition: opacity 2s;
}

.page-enter, .page-leave-active {
    opacity: 0;
}
```

​	(2)文件配置

```js
//	nuxt.config.js
css:['assets/css/main.css']
```

​	(3)使用<nuxt-link>组件来跳转链接

```css
<li><nuxt-link :to="{name:'news-id',params:{id:123}}">News-1</nuxt-link></li>
```



## 五、Nuxt的默认模板和布局

#### 1.默认模板

> 需求在每个页面都加入"nuxt.js",一般为根目录下app.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
   {{ HEAD }}
</head>
<body>
    <p>学习nuxt.js</p>
    {{ APP }}
</body>
</html>

//	{{HEAD}}读取的是nuxt.config.js里的信息
//	{{APP}}就是pages文件夹下的主体页面
```

#### 2.默认布局

> 主要对于页面的统一布局使用,一般放在layouts/default.vue

```vue
<template>
  <div>
    <p>学习nuxt.js</p>
    <nuxt/>
  </div>
</template>

//	<nuxt/>即我们每个页面的内容
```

#### 3.区别

<!--默认模板,可以定制很多头部信息,包括IE版本的判断-->

<!--默认布局,只能定制<template>里的内容-->

<!--建立默认模板,需重启服务器,默认布局则不需要-->



## 六、Nuxt插件的使用

#### 1.ElementUI

​	(1)npm i element-ui -S

​	(2)在plugins文件夹,创建ElementUI.js

```js
import Vue from 'vue'
import ElementUI from 'element-ui'
Vue.use(ElementUI)
```

​	(3)nuxt.config.js添加配置

```js
css: [
  'element-ui/lib/theme-chalk/index.css'
],
plugins: [
  {src: '~/plugins/ElementUI', ssr: true }		//ssr,表示这个插件只在服务端起作用
],
build: {
  vendor: ['element-ui']					//防止element-ui被多次打包
}
```

#### 2.axios

​	(1)npm i axios -S

​	(2)使用

```js
import axios from 'axios'

asyncData(context, callback) {
  axios.get('http://localhost:3301/in_theaters')
    .then(res => {
      console.log(res);
      callback(null, {list: res.data})
    })
}
```

​	(3)防止重复打包,在nuxt.config.js添加配置

```js
module.exports = {
  build: {
    vendor: ['axios']
  }
}
```

#### 3.跨域与拦截

###### (1)跨域

​	1. npm i @nuxtjs/axios -D

​	2. nuxt.config.js

```js
module.exports = {
    modules: [
        // Doc: https://axios.nuxtjs.org/usage
        '@nuxtjs/axios',
    ],
    axios: {
        proxy:true  //  代理
    },
    axios: {
        proxy: true,
        prefix: '/api', // baseURL
        credentials: true,
    },
    proxy: {
        "/eia/":"http://192.168.0.97:8181/",    //  key(路由前缀)：value(代理地址)
        changeOrigin: true, // 是否跨域
        pathRewrite: {
          '^/api': '' //路径重写
        }
    }
}
```

###### (2)拦截

 1. npm i @nuxtjs/axios @nuxtjs/proxy -S

 2. nuxt.config.js

    ```js
    module.expores{
      plugins: [
        {
          src: "~/plugins/axios",
          ssr: false
        },
      ],
      modules: [
        // Doc: https://axios.nuxtjs.org/usage
        '@nuxtjs/axios',
      ],
    }
    
    ```

3.plugins/axios.js

```js
export default ({ $axios, redirect }) => {
  $axios.onRequest(config => {
    console.log('Making request to ' + config.url)
  })

  $axios.onError(error => {
    const code = parseInt(error.response && error.response.status)
    if (code === 400) {
      redirect('/400')
    }
  })
}

export default function (app) {
  let axios = app.$axios; 
 // 基本配置
  axios.defaults.timeout = 10000
  axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'

  // 请求回调
  axios.onRequest(config => {})

  // 返回回调
  axios.onResponse(res => {})

  // 错误回调
  axios.onError(error => {})
}
```



## 七、asyncData

- asyncData 方法会在组件(限于页面组件)每次加载之前被调用
- asyncData 可以在服务端或路由更新之前被调用
- 第一个参数被设定为当前页面的上下文对象
- Nuxt会将 asyncData 返回的数据融合到组件的data方法返回的数据一并返回给组件使用
- 对于 asyncData 方式实在组件初始化前被调用的，所以在方法内饰没办法通过`this`来引用组件的实例对象

```js
asyncData(context,callback){
	callback(null,data)
}

	context.route.params.xxx获取参数

	callback(new Error(),data)
```



#### 1.promise

```js
<script>
import axios from 'axios'
export default {
  data(){
     return {
         name:'hello World',
     }
  },
  asyncData(){
      return axios.get('https://api.myjson.com/bins/1ctwlm')
      .then((res)=>{
          console.log(res)
          return {info:res.data}
      })
  }
}
</script>

```

**!!!:	一定要return出去获取到的对象，这样就可以在组件中使用，这里返回的数据会和组件中的data合并。这个函数不光在服务端会执行，在客户端同样也会执行。**

#### 2.promise并发

```js
async asyncData(context) {
  let [newDetailRes, hotInformationRes, correlationRes] = await Promise.all([
    axios.post('http://www.huanjingwuyou.com/eia/news/detail', {
      newsCode: newsCode
    }),
    axios.post('http://www.huanjingwuyou.com/eia/news/select', {
      newsType: newsType, // 资讯类型： 3环评资讯 4环评知识
      start: 0, // 从第0条开始
      pageSize: 10,
      newsRecommend: true
    }),
    axios.post('http://www.huanjingwuyou.com/eia/news/select', {
      newsType: newsType, // 资讯类型： 3环评资讯 4环评知识
      start: 0, // 从第0条开始
      pageSize: 3,
      newsRecommend: false
    })
  ])
  return {
    newDetailList: newDetailRes.data.result,
    hotNewList: hotInformationRes.data.result.data,
    newsList: correlationRes.data.result.data,
    newsCode: newsCode,
    newsType: newsType
  }
},
```

#### 3.await

```js
<script>
import axios from 'axios'
export default {
  data(){
     return {
         name:'hello World',
     }
  },
  async asyncData(){
      let {data}=await axios.get('https://api.myjson.com/bins/8gdmr')
      return {info: data}
  }
}
</script>
```

