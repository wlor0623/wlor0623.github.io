1.模板字符串

> 里面可以写js逻辑

```js
const Jelly = {
  name: 'Jelly',
  date: '2020-3-23',
  todos: [
    { name: 'Go to Store', completed: false },
    { name: 'Watch Movie', completed: true },
    { name: 'Running', completed: true }
  ]
}


const template = `
    <ul>
      $${Jelly.todos.map(todo => `<li>${todo.name}</li>`)}
    </ul>
`
```

```js
function highlight (strings, ...values) {
  const hightlighted = values.map(value => `<span>${value}</span>`)

  let str = ''
  strings.forEach((string, i) => str += `${string}${hightlighted[i]}`)
  return str
}

const user = 'Mary'
const topic = 'Learn to user markdown'
const sentence = highlight`${user} has commented on your topic ${topic}`

console.log(sentence)
//	<span>Mary</span> has commented on your topic <span>Learn to user markdown</span>undefined
```

2.新增字符串方法

```javascript
//	startWith(value,index)		以什么开头,后面索引代表从哪开始
//	endWith(value,index)		以什么结束,后面索引代表从哪开始
//	includes(value,index)		包含了什么,后面索引标识从哪开始判断
//	repeat(time)				重复time次
```

3.箭头函数不适合场景

> 箭头函数的this是指向父级作用域,且在词法作用域中,是不会改变的

```javascript
//  1.作为构造函数,一个方法需要绑定到对象
const Person = (name, points) => {
  this.name = name  //  报错,箭头函数没有this
  this.points = points
}
const Jelly = new Person('jelly', 5)

Person.prototype.updatePoints = () => {
  console.log(this)   //  此时的this指向window
  this.points++
  console.log(this.points)
}

//  2.当你真的需要this的时候
const button = document.querySelector('.zoom')
button.addEventListener('click', () => {
  console.log(this)   //  button
  this.classList.add('in')
  setTimeout(() => {
    //  计时器中必须使用箭头函数,因为计时器中的this是window
    this.classList.remove('in')
  }, 200)
})

//  3.需要使用arguments对象
const sum = () => {
  return Array.from(arguments).reduce((prevSum, value) => prevSum + value, 0)
}

sum(1, 2, 3)  //  arguments is not defined
```

4.对象解构

```javascript
const Tom= {
    family:{
        mother:'Norah Jones',
        father:'Richard Jones',
        sister:0
    }
}
//	只有明确这个只没有,为undefined的时候默认值才会生效,否则不会(false,0,null)
const {father:f,mother:m,sister='have no sister'} = Tom.family
```

5.for...of

> 除Object之外都支持(Array,map,String,arguments)

```js
//	for...in
const fruits = ['A', 'B', 'C']
fruits.describe = 'D'

for (const index in fruits) {
  console.log(fruits[index])  //  A,B,C,D 原型中的属性也会遍历
}

//  for..of可以打断循环,且不会遍历原型上的属性
for (const fruit of fruits) {
  if (fruit === 'C') {
    break
  }
  console.log(fruit)  //  A,B
}
```

``` js
//	能够获取 值和索引值
const fruits = ['A', 'B', 'C', 'D']

for (const [index, fruit] of fruits.entries()) {
  console.log(`${fruit} ranks ${index = 1}`)
}
//	能遍历字符串
let name ='Laravise'
for (const char of name){
    console.log(char)	//	L a r a v i s e
}
```

6.Array.from of

```js
// from不是在原型上的方法,所以只能Array.from
const todos = document.querySelectorAll('li')

const names = Array.from(todos, todo => todo.textContent)

const website = 'Laravise'
console.log(Array.from(website))  //  L a r a v i s e
```

