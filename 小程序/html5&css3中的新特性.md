## html5

+ 新的元素
  + canvas 画布
  + audio  音频
  + video 视频
  + source 资源
  + embed 嵌套的内容
+ canvas ：
  +  html5 中提供的一个新标签，可以让用户在 canvas 上绘制内容
  + 结构暴露出来的 js 的API 配合使用
+ 拖放
  + 事件：
    + drag：拖 
      + ondragstart
      + ondragover
      + ondrapend
    + drop：放
      + ondrop
+ 定位
+ 视频
+ 音频
+ input 类型
  + 扩展了 html 中的input 元素
+ 元素与属性
+ web 存储
  + localstroage
  + sesssionstroage
+ web sql
  + 使用客户端直接去链接数据库
+ web 应用缓存
  + 将网站中不经常改变的内容以文件方式进行缓存
+ web works
  + 在页面的后台部分执行 js 脚本 ，这些脚本不会影响页面的状态
  + 步骤：
    + 1.0 创建一个 worker 对象
    + 2.0 在 worker 对象传入一个后台执行 js 脚本 
    + 3.0 当 js 脚本中的内容发生改变时会触发事件：
      + onmessage
    + 4.0 创建一个后台执行的 js 脚本
      + 这个脚本中返回数据的方式通过
        + postMessage(data)
+ websocket
  + 浏览器与服务器通信的一种新的方式
    + 对比：
      + https:
        + 建立连接
          + 三次握手
        + 开始请求
        + 断开连接
          + 四次挥手
      + websocket
        + 连接一旦建立，除非一方主动断开，否则会一直连接

## css3

+ 边框
+ 圆角
+ 盒阴影
+ 背景
+ 渐变
+ 文本效果
+ 转换
+ 过渡
+ 动画
+ 弹性布局
+ 媒体查询