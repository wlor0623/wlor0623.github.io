#### map()

​	可以处理数组中的成员,返回一个新的数组,也可以用于遍历数组

```javascript
let aList = ['a','b','c'];

// 第一个参数是数组的成员值，第二个参数是成员的索引值
aList.map(function(item,i){
    alert(i + ' | ' + item);
})
```



#### includes()

​	返回一个布尔值,表示某个数组是否包含给定的值

```javascript
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```



#### filter()

​	和map类似,filter方法计收一个函数但是和map不同的是,filter把传入的函数一次作用于每个函数,通过true or false 过滤

```javascript
let aList01 = [1, 2, 4, 5, 6, 9, 10, 15];
let aList02 = aList01.filter(function (x) {
    return x % 2 !== 0; 
});
alert(aList02); // 1, 5, 9, 15
```



#### find()

​	主要应用于查找第一个符合条件的数组元素,他的参数是一个回调函数,在回调函数中找元素的条件,当条件成立为true时,返回该元素,如果没有符合条件的元素,返回值为undefined

```js
const myArr=[1,2,3,4,5,6];
var v=myArr.find(value=>value>4);
console.log(v);// 5
```



#### findIndex()

​	使用方法与find相同,但是find返回的是元素,findIndex返回的是索引,如果没有符合条件的元素返回-1



#### 扩展运算符

```javascript
let arr = [1,2,3];
let arr2 = [...arr,4];
console.log(arr2)  // [1,2,3,4]
```



#### 属性名表达式

```javascript
let propKey = 'foo';
let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123
};
alert(obj.foo); // true
alert(obj.abc); // 123
```



#### 列表渲染

```javascript
let aList = ['红海','复联3','碟中谍6','熊出没'];

let el = (
    <ul>
        { 
            aList.map((item,i)=><li key={i}>{ item }</li>)
        }        
    </ul>    
);
```



#### 条件渲染

​	**组件内根据条件返回不同结构**

```javascript
function Loginstate(props){
    if(props.isLogin==true){
        return <h2>欢迎回来！</h2>
    }else{
        return <h2>请登录！</h2> 
    }
}

ReactDOM.render(<Loginstate isLogin={true}/>,document.getElementById('root'));
```

​	**组件内根据条件返回不同子组件**

```javascript
function Login(props){
    return <h2>欢迎回来！</h2>
}
function Logout(props){
    return <h2>请登录！</h2>
}
function Loginstate2(props){
    if(props.isLogin==true){
        return <Login />
    }else{
        return <Logout />
    }
}
ReactDOM.render(<Loginstate2 isLogin={true}/>,document.getElementById('root'));
```

​	**与运算符&&一同使用**

​	“&&”符号左边是条件运算式或者布尔值，右边是结构，如果左边为真，就生成右边的结构，如果为假，这一句整体忽略。

```javascript
class Notice extends React.Component{
    constructor(props){
        super(props);
        this.state={
            username:'张大山',
            messages:[
                '新邮件',
                '新的待办'
            ]
        }
    }
    render(){
        return(
            <div>
                <h2>你好，{this.state.username}</h2> 
                {
                    this.state.messages.length>0 && <p>你有{this.state.messages.length}条消息</p>
                }
            </div>
        )
    }
}

ReactDOM.render(<Notice />,document.getElementById('root'));
```



#### 表单数据绑定

在表单元件上绑定onchange事件，来将state中的值改变为表单元件中的值，同时也需要将表单的value属性值，设置为等于state中的属性值。这种表单的值和state中的值进行绑定的组件叫做受控组件。



**文本框绑定**

``` javascript
class Myform extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            username:'',
            password:''
        };
    }
    // e指的是系统自动产生的事件对象
    // e.target指的是发生事件的元素
    fnChange=e=>{
        this.setState({
            // 使用属性名称表达式
            [e.target.name]:e.target.value
        })
    }

    render(){
            return(
                <form>
                    <p>
                        <label>用户名：</label> { this.state.username } <br />
                        <input name="username" type="text" value={this.state.username} onChange=									{this.fnChange} />
                    </p>
                    <p>
                        <label>密码：</label> { this.state.password } <br />
                        <input name="password" type="password" value={this.state.password} onChange=								{this.fnChange} />     
                    </p>
                </form>
            );
        }
    }

    ReactDOM.render(
        <Myform />, 
        document.getElementById('root')
    );
```

**单选及多选绑定实例**

```javascript
class Myform extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            hobbyname:{
                'study':'学习',
                'game':'打游戏',
                'shopping':'购物',
                'movie':'看电影',
                'travel':'旅行'
            },
            hobby:['study','game','shopping','movie','travel'],
            hobbysec:['study','movie'],
            gendername:{
                'male':'男',
                'female':'女'
            },
            gender:['male','female'],
            gendersec:['male'],
        };
    }

    fnChange=e=>{
        if(this.state.hobbysec.includes(e.target.value)){            
            let newArray = this.state.hobbysec.filter(item=>item != e.target.value)
            this.setState({hobbysec:newArray});
        }else{
            let nowVal = e.target.value;
            this.setState((state)=>({hobbysec:[...state.hobbysec,nowVal]}))
        }
    }

    fnChange2=e=>{
        this.setState({
            gendersec:[e.target.value] 
        })
    }

    render(){
            let {hobbyname,hobby,hobbysec,gendername,gender,gendersec} = this.state;
            return(
                <form>
                    爱好是：{
                        hobby.map((item,i)=>{
                            return <label key={i}><input type="checkbox" value={item} checked={hobbysec.includes(item)} onChange={this.fnChange} />{hobbyname[item]}&nbsp;&nbsp;&nbsp;</label>
                        })
                    }
                    <br />
                    <br />
                    性别：{
                        gender.map((item,i)=>{
                            return (<label key={i}>
                                        <input type="radio" value={item} checked={gendersec.includes(item)} onChange={this.fnChange2} /> {gendername[item]}
                                    </label>) 
                        })
                    }
                </form>
            );
        }
}
ReactDOM.render(
    <Myform />, 
    document.getElementById('root')
);
```



#### Refs操作DOM

 Refs 是使用 React.createRef() 创建的，并通过 ref 属性附加到 React 元素。在构造组件时，通常将 Refs 分配给实例属性，以便可以在整个组件中引用它们。

```javascript
class Myform extends React.Component{
    constructor(props){
        super(props);
        this.state = {};
        // 在组件初始化的时候创建一个ref对象
        this.myref = React.createRef();
    }
    // 在组件挂载到页面之后自动执行的方法
    componentDidMount(){
        //console.log( this.myref.current );
        // 让输入框获得焦点
        this.myref.current.focus();
    }
    render(){
        return (
            // 在标签中通过ref关联创建的ref对象
            <input type="text" ref={ this.myref } />
        )
    }
}
```



#### 生命周期函数

##### constructor方法

​	这个方法是组件的构造函数，在组件初始化的时候会自动执行。

##### render方法

​	这个方法会在componentWillMount方法之后执行，也会在state和props的数据发生变化时执行，所以这个方法在组件开始会执行，在组件数据发生变化时也会执行。

##### componentDidMount方法

​	这个方法是组件在挂载到页面之后，自动执行。

##### componentDidUpdate方法

​	这个方法在组件更新之后执行。

##### componentWillUnmount方法

​	这个方法是在组件销毁之前执行。

