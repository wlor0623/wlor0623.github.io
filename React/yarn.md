#### 安装yarn

```
npm i yarn -g
```



#### yarn常用命令

| NPM                         | YARN                 | 说明                                 |
| --------------------------- | -------------------- | ------------------------------------ |
| npm init                    | yarn init            | 初始化某个项目                       |
| npm install                 | yarn install         | 默认的安装依赖操作                   |
| npm install taco --save     | yarn add taco        | 安装某个依赖，并且默认保存到package. |
| npm uninstall taco --save   | yarn remove taco     | 移除某个依赖项目                     |
| npm install taco --save-dev | yarn add taco —dev   | 安装某个开发时依赖项目               |
| npm update taco --save      | yarn upgrade taco    | 更新某个依赖项目                     |
| npm install taco -g         | yarn global add taco | 安装某个全局依赖项目                 |



#### react官方脚手架

1.安装

```
npm i create-react-app -g
```

2.生成应用项目目录

```
create-react-app my-app
```

3.启动项目

```
cd my-app
yarn start
```

4.生成上线文件

```
yarn build
```



**项目目录说明**

src:							主开发目录，里面包含所有项目的组件，开发组件都是基于此目录

public:						项目入口文件目录，目录中的文件不用动

node_modules:		项目开发依赖包文件目录，项目安装的包都会自动安装在这个文件夹中

build:							项目上线时，执行npm run build生成的目录，这里面是自动化工具生成的上线文件



#### 安装第三方包

```
yarn add axios
```

​	yarn add 相当于 npm中 npm i -S