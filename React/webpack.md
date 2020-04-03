## 基本配置

#### **初始化一个项目**

```js
npm init -y			

//	-y表示使用默认设置,这样就在文件夹中创建一个package.json包配置文件
//	并在src文件夹中创建一个index,js文件,作为webpack的入口文件
```



#### 安装webpack相关模块

```js
npm i webpack webpack-cli -D

//	-D是save-dev的简写,将包名存储到pageage.json中的devDependencies设置项中
//	-S代表上线后也用用	-D表示只在开发中使用
```



#### webpack打包

```javascript
npx webpack ./src/index.js

//	npx表示执行当前项目中安装的模块包命令,整个命令表示使用webpack打包index.js
//	打包后会在目录下生成一个dist文件,同时默认名称是main.js
```



#### webpack.config.js

```js
const path = require('path');

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'bundle.js',
        path:path.resolve(__dirname,'public')
    }
}

//	entry表示入口文件
//	output表示打包的文件名和存储地址
```



```js
npx webpack	
```

​	打包的文件放到public文件夹中 文件名为bundle.js

```js
......
"scripts": {
    "dev": "npx webpack"
},
......
//	pageage.json中的scripts就可以	npm run dev运行打包
```



在打包过程中会提示没有设置“mode”选项，这个选项有两个值，一个是“development”，设置当前模式为开发模式，此时打包出来的js文件是没有压缩和混淆的，方便开发时排错；还有一个值是“production”，设置当前模式为生产模式，此时打包出来的js文件是压缩和混淆的，方便上线。我们可以在webpack.config.js中设置如下：

```
module.exports = {
    mode:'development',
    entry:'./src/index.js',
    output:{
        filename:'main.js',
        path:path.resolve(__dirname,'public')
    }
}
```



## **loader**

#### 打包css



```js
npm i css-loader style-loader -D

//webpack.config.js
module: {
    rules: [
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader']
      }
    ]
}
//	test设置的正则匹配css文件，use设置的是使用的loader名称
//	loader的执行顺序是从右往左执行。设置完之后，就可以在index.js中导入css文件
```



#### 导入less或sass

导入less或者sass文件和导入css文件类似，需要多一个loader，将less或者sass文件转化为css文件，然后再用style-loader和css-loader，以less文件为例子，需要安装less模块和less-loader模块：

```js
npm i less less-loader -D

//webpack.config.js
module: {
    rules: [
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader']
      },
      {
          test: /\.less$/,
          use: ['style-loader', 'css-loader','less-loader']
      }
    ]
}
//	设置完后就可以在index.js中导入less文件	
//	import './sub.less'
```



#### 导入图片文件

url-loader的跟file-loader的区别是，它可以设定图片的大小限制，

大于限制的会直接使用file-loader拷贝文件，小于限制的可以将图片转化为base64的url数据。

所以使用url-loader，需要同时安装file-loader：



```js
npm i file-loader url-loader -D

//webpack.config.js
module: {
    rules: [
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader']
      },
      {
          test: /\.less$/,
          use: ['style-loader', 'css-loader','less-loader']
      },
      {
          test:/\.(jpe?g|png|gif)$/,
          use:{
              loader: 'url-loader',
              options:{
                name:'[name].[ext]',
                outputPath:'images/',
                limit:20480
              }
          }
      }
    ]
}
```



<!--其中，“[name].[ext]” 表示导出后使用原来图片文件的文件名，outputPath设置图片放置的文件夹，limit表示设置图片的大小限制。 设置完之后，就可以在index.js中导入less文件了，比如：-->

```js
// img 得到的是图片地址
import img from './8342726.jpg'

let oImg = new Image();
oImg.src = img;

document.body.appendChild(oImg);
```



## webpack插件

#### html-webpack-plugin

<!--在打包后自动在output对应的文件夹中创建一个html文件，并将打包生成的js文件引入到页面上，如果希望按照我们的模板来创建html，可以在src文件夹中创建一个html模板-->

```js
npm i html-webpack-plugin -D

//webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module:{
    ......
},
plugins:[ new HtmlWebpackPlugin({ template:'src/index.html' })]
```



#### clean-webpack-plugin

<!--打包之前，清空public文件中的文件，让public文件夹中的文件没有多余遗留的文件-->

```js
npm i clean-webpack-plugin -D

//webpack.config.js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module:{
    ......
},
plugins:[ 
    new HtmlWebpackPlugin({ template:'src/index.html' }),
    new CleanWebpackPlugin()    
    ]
```



#### HotModuleReplacementPlugin

<!--修改样式时，更新页面中标签的样式效果而不用刷新整个页面，这个需要配合webpack开发服务器一起使用。这个插件属于webpack上的插件，所以不用另外安装了，这个插件简称为HMR-->

```js
const webpack = require('webpack');
plugins:[
    new HtmlWebpackPlugin({
        template:'src/index.html'
    }),
    new CleanWebpackPlugin(),
    new webpack.HotModuleReplacementPlugin()
]
```



## 开发服务器

<!--webpack提供了一个在开发过程中使用的服务器模块，开启之后，我们修改项目代码后，项目会自动打包，而且显示页面会自动刷新显示最新的效果，这个给我们开发带来了很大的便利。-->

```js
npm i webpack-dev-server -D

//webpack.config.js
devServer:{
    contentBase:'./public',
    open:true,
    port:8080,
    hot:true
}
```

**contentBase设置服务器根目录**

**open:true 可以让我们在开启服务器时，会自动打开一个浏览器窗口来运行我们的项目**

**port设置服务器运行的端口**

**hot:true表示让浏览器开启HotModuleReplacementPlugin这个功能。**



在package.json中scripts中配置这个服务器模块

```js
"scripts": {
    "dev": "webpack",
    "start": "webpack-dev-server"
  },
  
 npm run start
```



## 打包react项目

react的项目依赖react和react-dom模块，首先需要安装这两个模块：

```
npm i react react-dom -S
```



react的项目中会编写jsx语法，这些语法是通过babel模块解析的，所以我们需要安装babel模块，同时还需要安装babel的loader。

```
npm i babel-loader @babel/core -D
```



安装完这些之后，还需要安装babel针对react的预设：

```
npm install --save-dev @babel/preset-react
```



安装完模块之后，需要在webpack.config.js中配置如下：

```js
{ 
    test: /\.js$/, 
    exclude: /node_modules/, 
    loader: "babel-loader",
    options:{
        "presets": ["@babel/preset-react"]
    }
}
```



exclude是排除node_modules中的模块代码的解析，因为node_modules中的代码已经做了解析，options里面是针对babel-loader的设置项，这些设置项可以不写在这里，可以存在外面的一个.babelrc文件中，文件的里面的内容如下：

```js
{
    "presets": ["@babel/preset-react"]
}
//设置此项,上面的options就可以去掉
```



想在react组件类里面写箭头函数，通过箭头函数来帮我们绑定this，默认类里面定义方法是不能用箭头函数的，如果需要支持这种写法，需要安装对应的解析模块：

```
npm i @babel/plugin-proposal-class-properties -D
```

还需要在.babelrc文件中添加这个解析模块的预设

```
{
    "presets": ["@babel/preset-react"],
    "plugins": ["@babel/plugin-proposal-class-properties"]
}
```