1.Hook

作用:可以让不别写class的情况下,使用state以及其他React特性

```js
1.只能在 函数最外层 调用Hook,不要在循环 条件判断 或子函数中调用
2.只能在 React函数组件 调用Hook,不要再其他JS函数中调用
3.Hook能准确运行的原因,是因为Hook是由顺序决定的,固定好顺序
```

**什么时候会用Hook?**

如果编写函数组件需要向其添加一些state,以前做法是必须转化了class组件

2.useState

如果需要使用多个state变量,可以使用多次Hook

```jsx
import React, { useState } from 'react';
   function Example() {
       //	数组结构语法	useState接收唯一参数,state初始值
       //	返回当前state与修改state的函数
       //	useState可以传递函数,且只会初次渲染时起作用
     const [count, setCount] = useState(0);
       
     return (
      <div>
             `读取State`
        <p>You clicked {count} times</p>
             `修改State`
         <button onClick={() => setCount(count + 1)}>
         Click me
        </button>
             //	如果新的state需要通过使用先前的state算出,那么可以将回调函数作为参数传递
        <button onClick={()=>setNumber(number+1)}>+</button>
      </div>
    );
  }
```

3.useEffect

相当于React class组件的周期函数 componentDidMount,componentDidUpdate,componentWillUnmount结合

且useEffect调度的effect不会阻塞浏览器更新屏幕



**无需清除的effect**

应用场景:React更新DOM之后运行一些额外的代码(发送网络请求,手动变更DOM,记录日志)

```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

    //	初始渲染组件以及每次DOM更新后的时候会设置document的title属性
  useEffect(() => {
      //	在组件内部调用useEffect可以直接访问 count,而不需要特殊的API访问
    document.title = `You clicked ${count} times`;
      //	
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```



**需要清除的effect**

应用场景:有些副作用是需要清除的,例如订阅外部数据源,组件卸载的时候需要取消订阅

注意:每次组件渲染都会执行清除函数,因为保证每次的数据都是最新的

```jsx
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
    //	引入useState Hook
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
      //	订阅数据时处理方法,引入useState修改的函数
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
      //	订阅数据源
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // 组件卸载时,会执行cleanup函数,清除effect(每次effect渲染时都会执行,清除上一个effect)
    return function cleanup() {
        //	组件卸载时,取消订阅
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

```jsx
	//	组件渲染后实现各种不同的副作用,有些副作用需要清除,所以需要返回一个函数
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

	//	其他的eddect可能不比清除,所以不需要返回
useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
```

**effect优化**

注意:确保数组中包含了所有**外部作用域会随时间变化并且在effect中的变量**,否则代码会引用先前渲染的就变量

如果想执行只运行一次的effect(仅在组件挂载和卸载时执行),可以传递一个空数组[]作为第二个参数

```jsx
	//	class优化
componentDidUpdate(prevProps, prevState) {
  if (prevState.count !== this.state.count) {
    document.title = `You clicked ${this.state.count} times`;
  }
}
	//	hook优化(不需清除的effect)
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新

	//	hook优化(需要清除的effect)
useEffect(() => {
  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
  return () => {
    ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
  };
}, [props.friend.id]); // 仅在 props.friend.id 发生变化时，重新订阅
```



4.useReducer

useState的替代方案

应用场景:state逻辑较复杂且包含多个子值或者下个state依赖之前的state

```jsx
//	接收形如(state,action)=>newState 的reducer,并返回当前的state已经与其配套的dispatch方法
const initialState = 0;
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {number: state.number + 1};
    case 'decrement':
      return {number: state.number - 1};
    default:
      throw new Error();
  }
}
function init(initialState){
    return {number:initialState};
}
function Counter(){
    const [state, dispatch] = useReducer(reducer, initialState,init);
    return (
        <>
          Count: {state.number}
          <button onClick={() => dispatch({type: 'increment'})}>+</button>
          <button onClick={() => dispatch({type: 'decrement'})}>-</button>
        </>
    )
}
```



5.useContext

```jsx
//	声明一个Context
const CountContext = createContext()

//	子级函数组件中消费Countext 直接使用useContext
function Counter() {
  const count = useContext(CountContext)
  return <h1>{count}</h1>
}

//	子级类组件 需要静态声明一个contextType,然后自己用this.context获取
class Bar extends Component {
  static contextType = CountContext
  render() {
    const count = this.context
    return <h1>{count}</h1>
  }
}

function App() {
  const [count, setCount] = useState(0)
//	父级
  return (
    <div>
      <button
        onClick={() => {
          setCount(count + 1)
        }}
      >
        Click({count})
      </button>
      <CountContext.Provider value={count}>
        <Counter />
        <Bar />
      </CountContext.Provider>
    </div>
  )
}
```



6.useRef

类组件/React元素用React.createRef,函数组件用useRef

useRef返回一个可变的ref对象,其**current**属性被初始化为传入的参数(initialValue)

```jsx
const refContainer = useRef(initialState)
//	useRef返回的ref对象在整个生命周期内保持不变
//	即每次重新渲染函数组件的实时,返回的ref对象都是同一个(使用React.createRef时每次渲染都会重新创建ref)
```



7.useMemo useCallback

```jsx
useMemo(()=>fn)	===	useCallback(fn)
```

