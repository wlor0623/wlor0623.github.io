

#### TS初体验

1.安装

```shell
npm i typescript -g
```

3.使用

```typescript
function test (a: number,b:number): number{
	return a +b
}
```

2.全局会提供一个`tsc`命令使用

```typescript
tsc index.ts
```



#### TS配置文件

1.创建配置文件

```
tsc --init
```

2.设置配置项

```js
target:	转换成哪个版本的js代码	es5	es6
module:	使用的模块化标准是什么
outDir:	最终js代码存放的文件夹路径
rootDir:ts代码的存放路径
strict:	是否将ts代码转换成严格模式的js代码
```

3.使用配置文件

```velocity
tsc -p ./tsconfig.json
```



#### TS数据类型

```typescript
//	number
let a: number = 10
let a: number = NaN
let c: number = Infinity
let d: number = 0xA12
let e: number = 0b101001
let f: number = 0o75

//	string
let str: string = "这是一个字符串"
let str1: string = '这也是一个字符串'
let str2: string = `这是一个模板字符串${a}`

//	boolean
let flag: boolean = true
let flag: boolean = false

//	Array<数据类型>
let arr: Array<number> = [1,2,3,4]
let arr1: number[] = [1,2,3,4]

//	元组
let arr2: [number,string] = [1,'a']
arr2[0] = 'a'	//	报错!
arr2[0] = 100
arr2[2] = 'a'	//	没有索引的元素,则代表是新增,但是只能是声明的两种类型
arr2[2] = []	//	非number,string

//	void
let res: void = undefind

//	undefined
//	null
let res1: undefined = undefined
let res2: null = null

//	any
let somevar: any = 10
somevar = 'abc'
somevar = []

//	never
//	一般用在不可能返回内容的函数的返回值类型设置
function test(): never{
    while(true){
        //	死循环不可能有返回值
    }
}

//	object
let o: object = {}
let o: {name:string,age:number} = {name:'张三',age:12}

//	enum	枚举,给一组数据赋值友好的名字
enum Gender{
    male = 1,
    famale = 0,
    unknow = -1
}
let gender: Gender = Gender.male
let o ={
    gender:Gender.male
}

//	类型断言
let str1: any = 'abc'
let len: number = (<string>str1).length
```



#### TS中的类

```typescript
class Person{
    //	与ES6不同的是,TS属性必须声明,需要指定类型
    name: string
    //	声明好属性之后,属性必须赋值一个默认值或者在构造函数中进行初始化
    //  age:number = 10
    age: number
	constructor(name: number){
		this.name = name
        this.age = age
	}
    
    sayHello(msg: string): void{
        console.log(msg)
    }
}
```



#### TS类继承

```typescript
class Animal{
	age: number
	constructor(age: number){
		this.age = age
	}
	eat(){
		console.log('吃鸡腿')
	}
}

class Dog extends Animal{
    type: string
    constructor(type: string,age: number){
        super(age)		//	继承必须要super父类过来的属性
        this.type = type
    }
    //	子类出现与父类同名的方法,则会进行覆盖
    eat(){
        console.log('狗对象中的eat方法')
    }
}
var dog = new Dog('哈士奇',18)
dog.eat()
```



#### TS类成员访问修饰符

在类的成员前添加关键字来设置当前成员的访问权限

- public:	公开默认,所有人都可以访问
- private:	私有的,只能在当前类中进行访问
- protected:	受保护的,只能在当前类或者子类中进行访问

```typescript
enum Color{
	red,
	yellow,
	blue
}

class Car{
    //	如果不加访问修饰符,则当前成员默认是公开的,所有人都可以访问
	color:	Color
    constructor(){
        this.color = Color.red
    }
    private run(){
        console.log('当前成员只能在当前类中使用')
    }
    protected loadPeople(){
        console.log('在当前类中或子类中方位')
    }
}
let byd = new Car()
byd.color	//	红色
byd.run()	//	报错!
byd.loadPerson()	//	报错,只能在当前类中或者子类中

class Audi extends Car{
    sayHi(){
        console.log(this.color)
        this.loadPeople()	//	正常调用
    }
}
let audi = new Audi()
audi.color	//	红色
```



#### TS只读属性与参数属性

```typescript
class Cat{
	readonly name: string
    type: string
    //	type: string	public代替了这两句
    //	构造函数中给参数前面加上修饰符,就相当于声明了一个属性
    constructor(public type: string){
        this.name = '加菲'
    //  this.type = type	public代替了这两句
    } 
}
var cat = new Cat("橘猫")
cat.name		//	正常访问
cat.name = 123	//	报错1
```



#### TS类成员存取器

```typescript
class People{
		//	name: string = ''
    private _name: string = ""
    	//	属性的存取器
    get name(): string{
        return this._name
    }
    set name(value: string){
        //	设置器中可以添加相关的校验逻辑
        if(value.length < 2 || value.length > 5){
            throw new Error('非法昵称,禁止使用!')
        }
        this._name = value
    }
}
var p = new People
p.name = '111111'	//	报错
p.name = '123'	
```



#### TS接口

接口使用interface进行声明

```typescript
interface AjaxOptions{
	url: string,
	type?: string,		//	可选属性加?
	data?: object,		//	可选属性加?
     success(data: object): void
}
```



```typescript
//	options参数中 需要包含 url type data success
function ajax(options: AjaxOptions){
	
}

ajax({
    url: 'www.baidu.com',
    type: 'get',	//	可不选
    data: {},		//	可不选
    success(data){
        
    }
})
```



接口只读属性

```typescript
interface Point{
	readonly x: number,
	y: number,
    [propName: string]: any	//	加了额外属性检查propName不报错
}
let poi: Point = {
    x:10,
    y:10,
    z:100		//	加了额外属性检查propName不报错
}
poi.x = 100		//	报错,x是可读
```



#### TS函数类型接口

```typescript
interface SumInterFace{
    (a: number,b: number): number
}
let sum: SumInterFace = function(a: string,b: number){	//	报错,接口规范为number类型
    return a + b
}
```



#### TS类类型接口

```typescript
interface PersonInterFace{
    name: string,
    age: string,
    eat(): void
}
class XiaoMing implements PersonInterFace{
    name: string = '小明'
    age: number = 18
    eat(){
        console.log('吃饭')
    }
}
class XiaoHong implements PersonInterFace{
    name: string = '小红'
    age: number = 18
    eat(){
        
    }
}
var xh = new XiaoHong()
xh.name		//	放心使用数据,因为都遵守PersonInterFace接口
xh.age		//	放心使用数据,因为都遵守PersonInterFace接口
xg.eat()	//	放心使用数据,因为都遵守PersonInterFace接口
```



#### TS接口继承

1.接口继承接口

```typescript
interface TwoDPoint{
	x: number,
	y: number
}
interface ThreeDPoint{
    z: number
}
interface FourDPoint extends ThreeDPoint,TwoDPoint{
    time: Data
}
let poi1: FourDPoint = {
    z : 100,
    x : 100,
    y : 100,
    time : new Data
}
```

2.接口继承类

```typescript
class Bird{
	type: string = '画眉鸟'
	fly(): void{
		
	}
}
interface Fly extends Bird{
    
}
let flyingBird: Fly = {
    type:'啄木鸟',
    fly():void{
        
    }
}
```

