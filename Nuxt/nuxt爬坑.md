#### 1.在head里引入js文件

​	Nuxt.js通过vue-meta实现头部标签管理(主要为移动端项目引入flexible.js适配问题)

```js
head: {
  script: [{ innerHTML: require('./assets/js/flexible'), type: 'text/javascript', charset: 'utf-8'}],
  __dangerouslyDisableSanitizers: ['script']
}
```

#### 2.nuxt使用less,sass等预处理器

```js
npm i node-sass sass-loader -D
npm i less less-loader -D
```

#### 3.使用px2rem

在nuxt.config.js文件添加配置 也可在postcss.conf.js中添加

```js
build: {
  postcss: [
    require('postcss-px2rem')({
      remUnit: 75 // 转换基本单位
    })
  ]
}
```





>  https://juejin.im/post/5cc81e1a6fb9a032414f695b#heading-59 