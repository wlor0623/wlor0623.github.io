1.react最好在constructor中绑定this

```js
  constructor(props) {
      super(props)
      this.handleClick = this.handleClick.bind(this)
  }
```

2.获取父组件的方法和变量全都在this.props中

```js
 handleClick() {
    const { deleteItem, index } = this.props
    deleteItem(index)
  }
```

3.setState最新写法写成函数(异步函数)

```js
    this.setState(prevState => ({
      list: [...prevState.list, prevState.inputValue],
      inputValue: ''
    }))
//	prevState为初始的state
```

4.组件传值校验

```js
//	相当于vue的type
TodoItem.prototype = {
    //	相当于vue的required:true
  content: PropTypes.string.isRequired,
  deleteItem: PropTypes.func,
    //	为number或者string
  index: PropTypes.arrayOf(PropTypes.number,PropTypes.string)
}
//	相当于vue的default
TodoItem.defaultProps = {
  test: 'hello world'
}

```

5.render

- 当父组件的render函数被运行时,他的子组件render都会被重新运行
- 当组件的state或props发生改变的时候,render函数会被重新执行(实际上就是state,因为props是父组件的state)

6.ref

```js
<input ref={(input)=>{this.input=input}}
//	访问
this.input
```

7.setState回调操作DOM

```js
thi.setState((prevState)=>({
	inputValue:''
}),()=>{
	console.log(this.input.querySelectorAll('div').length)
})
//	react setState与操作DOM 一定要在 回调里 操作
//	console.log(this.input.querySelectorAll('div').length)	放在这就不能正确获取DOM,因为虚拟算法是异步
```

8.每次父组件render渲染的时候子组件也会被渲染

```js
//	子组件
shouldComponentUpdate(nextProps,nextState){
	if(nextProps.content!==this.props.content){
		return true
	}else{
		return false
	}
}
//	如果父组件传来的content改变,就重新渲染子组件的render,否则不渲染
//	以此提高性能,当子组件需要改变才改变,否则不改变
```

9.react的虚拟DOM与同层比对

10.react动画

```js
animation:hide-item 2s ease-in forwards
//	第四个参数forwards可以保存 动画最后的效果,不复原
```

11.react-redux

```js
//	index.js
import {Provider} from 'react-redux'
import store from './store'

const App = (
	<Provider store={store}>
    	<TodoList />
    </Provider>
)

ReactDOM.render(App,document.getElementById('root'))

```

```js
//	TOdoList.jsx
import { connect } from 'react-redux'

//	组件中使用store变量或者方法 this.props.xxx

const mapStateToProps = state => {
  return {
    inputValue: state.inputValue
  }
}

const mapDispatchToProps = dispatch => {
  return {
    changeInputValue(e) {
      const action = {
        type: 'change_input-value',
        value: e.target.value
      }
      dispatch(action)
    }
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(TodoList)

```

12.*

```js
import * as constants from './constants'
//	导入constants全部内容 并且去别名为 constants
```

13.优化react

```js
//	PureComponent替换Component
//	可以让与自己组件没有影响的数据更新后,不更新render
//	即实现自动shouldComponentUpdate钩子
//	需使用immutable.js 不然会有坑
```

