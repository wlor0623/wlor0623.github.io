1.遍历数组最好用forEach方法

2.js类型判断

```js
1==true		//true
123=="123"	//true
123==="123"	//false
1===true	/fasle
```

3.一般将字符串转化为数字,推荐parseInt(),直接用+如果遇到不是数字的字符串会NaN

4. Array.length是比数组最大索引多一的数

```js
var a =['dog','cat','hen']
a[100]='fox'
a.length	//	101
```

5.项目中最好不要操作原数据,包括array,object

```
数组用slice(),对象用扩展运算符或者Object.assign
```

6.React事件绑定箭头函数需要显示传入event

```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>

//	React 的事件对象 e 会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过 bind 的方式，事件对象以及更多的参数将会被隐式的进行传递。
```

7.所有React组件必须像纯函数一样保护它们的props不被更改

```js
//	纯函数
function sum(a, b) {
  return a + b;
}
//	非纯函数
function sum(a, b) {
  return a + b;
}
```

8.阻止组件渲染

```js
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }
  return (
    <div className="warning">
      Warning!
    </div>
  );
}
//	在组件的 render 方法中返回 null 并不会影响组件的生命周期
```

9.元素中的key

```js
//	一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用数据中的 id 来作为元素的 key
//	数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。当我们生成两个不同的数组时，我们可以使用相同的 key 值
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
//	官网推荐在map()方法中的元素设置key属性

```

```js
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
)
//	key 会传递信息给 React ，但不会传递给你的组件
//	Post 组件可以读出 props.id，但是不能读出 props.key
```

10.提取组件

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />

      )}
    </ul>
  );
}
//	如果一个 map() 嵌套了太多层级，那可能就是你提取组件的一个好时机
```

11.input输入框判断为纯数字时,可isNaN(value) return ''

12.props.children

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
//	props.children里面包括FancyBorder组件里面所有的内容
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

13.组件多类名

```jsx
<li classNmae={`tab-item ${item.id==id?'has':''}`}
```

14.数据不可变性

拷贝(常见)

```js
const state = { filter: 'completed', todos: ['Learn React'] }
const newState = { ...state, todos: [...state.todos, 'Learn Redux'] }
const newState2 = Object.assign({}, state, { todos: [...state.todos, 'Learn Redux'] })	//	效率最高
```

immutability-helper(适合深层次的节点)

```js
import update from 'immutability-helper'
const state = { filter: 'completed', todos: ['Learn React'] }
const newState = update(state, { todos: { $push: ['Learn Redux'] } })
```

immer(新玩法,性能不高,试用小app)

```js
import produce from 'immer'

const state = { filter: 'completed', todos: ['Learn React'] }

const newState = produce(state, draftState => {
  draftState.todos.push('Learn Redux')
})
```

15.React-Router核心API

```js
<Link>		//	普通链接,不会触发浏览器刷新
    <Link to="/about">About</Link>

<NavLiNK>	//	类似Link但是会添加当前选中状态
    <NavLink to="faq" activeClassNmae="selected">FAQs</NavLink>

<Prompt>	//	满足条件时提示用户是否离开当前页面
    <Prompt	when={formIsHalfFilledOut message="Are you sure you want to leavt?"}
<Redirect>	//	重定向时提示用户是否李凯当前页面
    <Route exact path="/" render={()=>(loggedIn?(<Redirect to="dashboard"/>):(<PublicHomePage />)}
    
<Route>		//	路由配置的核心标记,路径匹配时显示对应组件(如果匹配多个,匹配到的路由会全部显示)
    <div>
    	<Route exact path="/"  component={Home} />
        <Route path="/news"	component={NewsFeed} />
    </div>
   
<Switch>	//	只显示第一个匹配的路由
    <Switch>
         <Route exact path="/" component={Home} />
         <Route path="/about"  component={About} />
         <Route path="/:user"  component={User} />
         <Route component={NoMatch} />
    </Switch>
```

16.React-Route通过URL传递参数

> 页面状态尽量通过URL参数定义(语义化的参数信息,例如一个表单的时间,随着时间不同内容也不同)

```jsx
<Route path="/topic/:id" component={Topic} />	 //	传递
this.props.match.params						   //  获取

const Topic = ({match})=>(<h1>Topic {match.params.id}</h1>)		//	原理是通过高阶组件实现
```

17.布局使用绝对相对定位,是一种很好的layout方式

```css
    position: relative;
    position: absolute;
```

18.窗口缩放

```js
//	手动实现拖放逻辑
//	使用local storage 存储宽度的位置
```

19.useMemo与useCallback

> useMemo		缓存 [函数的返回值]
>
> useCallback	缓存函数
>
> 高阶函数 ,建议一律使用useMemo

```js
//	在组件内部,那些会成为其他useEffect依赖项的方法,建议用useCallback包裹,或者直接编写在引用它的useEffect中

//	如果function作为props传递给子组件,一定使用useCallback包裹,对于子组件来说如果每次render都会导致你传递函数的函数发生变化,可能会造成很大的困扰,同时也不利于react做优化
```



