#### ReacrDOM.render方法

​	接收两个参数,第一个参数可以是一个JSX对象,也可以是一个react组件

​							第二个参数是一个元素dom对象,它作为显示内容的容器

```javascript
   ReactDOM.render(
        oJsx,
        document.getElementById('root')
    )   
```



#### JSX语法

​	1.单标签中要在结尾加 /

​	2.通过 {} 插入变量,表达式 或者函数调用

​	3.class属性需要写成className

​	4.style里是一个对象,将样式属性写成键值对形式

```js
{/* 定义class */}
<p className="sty01">使用样式</p>
{/* 单个标签，结尾要加“/” */}
<img src={user.avatarUrl} />
<p style={{width:'300px',height:200,color:'#ddd',backgroundColor:'hotpink'}}>这是一个使用style样式的段落</p>
```



#### ES6语法和类继承

```javascript
class Student extends Person{
    constructor(name,age,school){
        super(name,age);
        this.school = school;
    }
    showschool(){
        alert('我的学校是：' + this.school);
    }
}
```



#### 组件与函数

​	**函数式定义组件**

​		通过函数来定义一个组件，组件名称首字母要大写，函数接收一个参数props，返回一个jsx对象。其中，name属性是在渲染		组件时，通过定义属性传入进来的。

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

​	**类方式定义组件**

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

​	**组件渲染**

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

​	**组件结合**

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```



#### 绑定事件

```javascript
    // 按钮在调用方法时，此时的this默认会指向这个按钮，所以在绑定事件时，需要绑定this
        <input type="button" value="打招呼" onClick={this.fnHello.bind(this)} />
```

​	绑定this麻烦,一般使用箭头函数来绑定this

```javascript
  <input type="button" value="打招呼" onClick={this.fnHello} />
                {* 这里的fnHello2通过绑定一个箭头函数来调用需要传参的函数*}
                <input type="button" value="打招呼2" onClick={()=>{this.fnHello2('Hello,')}} />
```

#### 状态(state)

​	有state属性的组件叫做有状态的组件,没有state属性的组件叫无状态的组件

```javascript
class Increase extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            iNum:10
        }
    }
    fnAdd=()=>{                
        // 设置state里面的值用setState
        // setState里面可以直接传递一个对象，对象可以是整体或者是部分的state中的键值对
        // setState里面还可以传递一个函数，函数需要返回一个对象
        // 设置值时想引用state最新的值用setState里面传递的函数中的state
        this.setState(state=>({iNum:state.iNum+1}));
    }
    render(){
        return (
            <div>
                <p>{ this.state.iNum }</p>
                <input type="button" value="递增" onClick={this.fnAdd} />
            </div>
        )
    }
}

ReactDOM.render(
    <Increase />,
    document.getElementById('root')
);
```

​	1.不能直接修改state的值,应该用setState代替

​	2.setState是异步调用,第一个参数是state上一个状态的值,第二个是回调函数