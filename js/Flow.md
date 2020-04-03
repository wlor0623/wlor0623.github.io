#### Flow初体验

```js
function square(n:number): number{
	return n * n
}
square("2")	//Error
```

#### Flow为数据添加类型

> 需要给文件添加@flow标记,否则flow不会对文件进行类型检测

1.通过注释的方式进行添加

```js
//	@flow
var /*: number*/ = 10
a = 'abc'
console.log(a)
```

2.通过直接改写js代码结构(推荐)

```js
function sum(a:number,b:number){
	retrun a + b
}
sum("123","456")
```

#### Flow基本使用步骤

1.安装

```js
npm init -y
npm i flow-bin -D
```

2.书写代码

```js
var 变量: 数据类型 = 数据	//需要使用babel进行转码
```

3.使用flow进行类型检查

​	(1)在package.json中,添加flow命令

​	(2)为flow创建配置文件	.flowconfig

```shell
npm run flow init
```

​	(3)执行类型检查

```shell
npm run flow
```



#### Flow配合babel

1.安装babel及presets

```
npm i babel-cli babel-preset-flow -D
```

2.配置package.json添加build命令调用babel

```json
{
	"scripts"{
		"build" : "babel .src/ -d ./dist"
	}
}
```

3.执行build

```
npm run build
```



#### Flow类型

```js
number				数字,NaN,infinity都是number类型的数据
string				字符串
null				只有null是null类型
void				undefined在flow中的类型就是void
Array				数组类型
Object				对象类型
any					任意类型,建议少用
Functions			函数类型
Maybe				允许声明一个包含null和undefined两个潜在类型的值
|					类型1 | 类型2 | 类型3
类型推断			flow会尝试自行推断某个数据的类型
```

```js
//	@flow

//	number类型:	数字 NaN Infinity
let a: number = 100
let b: string = NaN		//	报错!
let c: number = Infinity

//	string类型:	字符串
let str: string = 123	//	报错!
let str: string = "123"

//	Array类型:	指定元素类型的数组
let arr: Array<number> = [1,2,3]
let arr: Array<number> = [1,2,'3]	//	报错!

//	any类型:		任意
let name: any = 123
name = "123"
let arr1: Array<any> = [1,'a',{},[]]	//	不报错

//	Function类型
function test(a: number,b: number): number{
    return a +b
}
var a: string = test(1,2)	//	报错!

let func: (a: number,b: number)  => number = test	//	指定变量赋值的函数类型

function ajax(callback: (data: Object) => void ){}
ajax(function(obj: Object))

//	Maybe类型
function test(a: ?number){	//	加?后如果不传入number则会是null或者void
    a = a ||0
    console.log(a)
}

//	或操作
let a: number|string = 10
a = "abc"
a= {}	//	报错!

//	类型推断
function test (a: number,b: number){	//	flow推断a+b会返回number
    return a + b
}
let c: string = test(1,2)

//	对象类型
function greet(obj: {sayHello: () => void}){
//	原function greet(obj)报错
    obj.sayHello()
}
var o = {
    name:'张学友'
}
greet(o)

//	option需要有url参数,type参数,success参数
function ajax(option: {url: string, type: string, success: (data :Object) => void}){
    
}

ajax()		//	报错!
ajax({})	//报错!
```



#### Flow简化代码复杂逻辑

```js
function sum (arr: Array<number>){
	let result = 0
	arr.forEach(v=>{
		result += v
	})
	return result
}
```



#### Flow小结

Static Type Checker For JavaScript

能够给JavaScript提供静态类型检查能力,其实就是为JavaScript添加了一个编译过程

```js
npm i flow-bin -D

var 变量 /*: 类型*/ = 数据
vat 变量: 类型 = 数据		//	需要使用babel转码
```

