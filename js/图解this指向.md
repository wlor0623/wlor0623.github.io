# 图解javascript的this指向

#简版this指向
![image.png](https://i.loli.net/2020/03/23/6Y9LeEoAkjvPZDr.png)
#升级版this指向
![8H42gH.png](https://i.loli.net/2020/03/23/hnSZA9ebX14gdzT.png)

#解释：
这里的上下文对象如下：
```js
function fn() {console.log('this指向：', this);}

let Obj = {
    fn: fn
}

window.fn();    // 上下文对象调用, 等价于直接调用 fn()
Obj.fn();       // 上下文对象调用
```
