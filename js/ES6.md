#### for...of

 `for...of`语句循环/遍历集合，并为您提供修改特定项的功能。它取代了传统的`for-loop`做法。 

 <!--假设您有一个工具箱，并且要显示其中的所有工具。`for...of`迭代器很容易实现。--> 

```js
const toolBox = ['Hammer','Scredriver','Ruler']

for(const item of toolBox){
	console.log(item)
}

//	Hammer
//	Screwdriver
//	Ruler
```



#### includes

 `includes()`方法用于检查集合中是否存在特定字符串，并返回`true`或`false`。请记住，它区分大小写：如果集合中的项目是`SCHOOL`，而您搜索`school`，它将返回`false`。 

<!--假设您不知道车库中有哪些车辆，您需要一个系统来检查您想要的车是否存在。includes()来完成！-->

> 区分大小写

```js
const garage = ['BWN','AUDI','VOLVO']
const findCar = garage.includes('BWM')

console.log(findCar)

//	true
```



#### some

 some()`方法检查数组中是否存在某些元素，并返回`true`或`false`。这有点类似于`includes()`方法的概念，除了参数是函数而不是字符串。 

 <!--假设您是俱乐部老板，并不关心谁进入俱乐部。但有些人不允许进入，因为他们喝得太多。--> 

```js
const age = [16,14,18]

age.some(person=>person>=18)

//	true
```



#### every

 `every()`方法遍历数组，检查每个项，并返回`true`或`false`。概念和`some()`相同。除了每个项必须满足条件语句，否则它将返回`false`。 

 <!--最后一次允许`some()`未成年学生进入俱乐部时，有人报告此事并且警察抓住了您。这次你不会让这种情况发生，你将确保每个人`every()`都通过了年龄限制。--> 

```js
const age = [15,20,19]

age.every(person=>person>=18)

//	false
```



#### filter

 `filter()`方法创建一个包含所有通过条件的元素的新数组。 

 <!--假设想返回高于或等于`30`的价格。过滤掉所有其他价格......--> 

```js
const prices = [25,30,15,55,12,54]

prices.filter(price=>price>=30)

//	[30,54,55]
```



#### map

 `map()`方法在返回新数组方面类似于`filter()`方法。但是，唯一的区别是它用于修改项。 

 <!--假设有一个包含价格的产品列表。经理需要一个清单，以便在税率减少`25％`后显示新价格。`map()`方法可以完成。--> 

```js
const productPriceList = [200,350,1500,5000]

productPriceList.map(item=>item * 0.75)

//	[150,262.5,1125,3750]
```



#### reduce

 `reduce()`方法可用于将数组转换为其他内容，可以是整数，对象，承诺链（承诺的顺序执行）等。出于实际原因，一个简单的用例是对整数列表求和。简而言之，它将整个数组“减少”为一个值。 

 <!--假设您要查找一周的总花销，使用reduce()获得该值。--> 

```js
const weeklyExpenses = [200,350,1500,5000,450,680,350]

weeklyExpenses.reduce((first,last)=>first +last)

//	8530
```



#### set

过滤重复的值

```js
const set = new Set()

[2,3,4,5,2,3,2].forEach(item=>set.add(x))

for(let i of s){
    console.log(i)
}
```

