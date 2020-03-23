# WebRTC 示例

这是一些小示例的集合，展示了[WebRTC API]（https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API）的各个部分代码。
示例可以在[GitHub 存储库](https://github.com/webrtc/samples)中找到。

大多数示例都使用 [adapter.js](https://github.com/webrtc/adapter)，用来屏蔽各个游览器之间的差异。

[https://webrtc.org/testing](http://www.webrtc.org/testing "Command-line flags for WebRTC testing")
列出了命令行标志，这些标志对于使用 Chrome 开发和测试非常有用。

## [getUserMedia():](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getUserMedia)

访问媒体设备

- [基本获取用户媒体演示](https://webrtc.github.io/samples/src/content/getusermedia/gum/)
- [将 getUserMedia 与画布一起使用](https://webrtc.github.io/samples/src/content/getusermedia/canvas/)
- [将 getUserMedia 与画布和 CSS 滤镜一起使用](https://webrtc.github.io/samples/src/content/getusermedia/filter/)
- [选择相机分辨率](https://webrtc.github.io/samples/src/content/getusermedia/resolution/)
- [仅音频获取到本地音频元素的输出](https://webrtc.github.io/samples/src/content/getusermedia/audio/)
- [仅音频获取用户媒体并显示音量](https://webrtc.github.io/samples/src/content/getusermedia/volume/)
- [录制流](https://webrtc.github.io/samples/src/content/getusermedia/record/)
- [屏幕共享与获取显示媒体](https://webrtc.github.io/samples/src/content/getusermedia/getdisplaymedia/)

## 设备:

查询媒体设备

- [选择摄像头、麦克风和扬声器](https://webrtc.github.io/samples/src/content/devices/input-output/)
- [选择媒体源和音频输出](https://webrtc.github.io/samples/src/content/devices/multi/)

## 捕获流:

从画布或视频元素进行流式传输

- [从视频元素流式传输到视频元素](https://webrtc.github.io/samples/src/content/capture/video-video/)
- [从视频元素流式传输到对等连接](https://webrtc.github.io/samples/src/content/capture/video-pc/)
- [从画布元素流式传输到视频元素](https://webrtc.github.io/samples/src/content/capture/canvas-video/)
- [从画布元素流式传输到对等连接](https://webrtc.github.io/samples/src/content/capture/canvas-pc/)
- [从画布元素录制流](https://webrtc.github.io/samples/src/content/capture/canvas-record/)
- [使用内容提示指导视频编码](https://webrtc.github.io/samples/src/content/capture/video-contenthint/)

## [RTC 对等连接:](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection)

控制对等连接

- [基本对等连接演示](https://webrtc.github.io/samples/src/content/peerconnection/pc1/)
- [仅音频对等连接演示](https://webrtc.github.io/samples/src/content/peerconnection/audio/)
- [动态更改带宽](https://webrtc.github.io/samples/src/content/peerconnection/bandwidth/)
- [升级通话并打开视频](https://webrtc.github.io/samples/src/content/peerconnection/upgrade/)
- [一次建立多个对等连接](https://webrtc.github.io/samples/src/content/peerconnection/multiple/)
- [将一台 PC 的输出转发到另一台电脑](https://webrtc.github.io/samples/src/content/peerconnection/multiple-relay/)
- [蒙格 SDP 参数](https://webrtc.github.io/samples/src/content/peerconnection/munge-sdp/)
- [设置对等连接时使用 pranswer](https://webrtc.github.io/samples/src/content/peerconnection/pr-answer/)
- [约束和统计数据](https://webrtc.github.io/samples/src/content/peerconnection/constraints/)
- [更多约束和统计数据](https://webrtc.github.io/samples/src/content/peerconnection/old-new-stats/)
- [显示各种方案的创建输出](https://webrtc.github.io/samples/src/content/peerconnection/create-offer/)
- [使用 RTCDTMFSender](https://webrtc.github.io/samples/src/content/peerconnection/dtmf/)
- [显示对等连接状态](https://webrtc.github.io/samples/src/content/peerconnection/states/)
- [ICE 候选服务器从 STUN/TURN 服务器收集](https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/)
- [执行 ICE 重新启动](https://webrtc.github.io/samples/src/content/peerconnection/restart-ice/)
- [Web 音频输出作为对等连接的输入](https://webrtc.github.io/samples/src/content/peerconnection/webaudio-input/)
- [作为 Web 音频输入的对等连接](https://webrtc.github.io/samples/src/content/peerconnection/webaudio-output/)

## [RTC 数据频道](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel)

通过对等连接发送任意数据

- [传输文本](https://webrtc.github.io/samples/src/content/datachannel/basic/)
- [传输文件](https://webrtc.github.io/samples/src/content/datachannel/filetransfer/)
- [传输数据](https://webrtc.github.io/samples/src/content/datachannel/datatransfer/)
- [消息](https://webrtc.github.io/samples/src/content/datachannel/messaging/)

## 视频聊天:

全功能 WebRTC 应用程序

- [AppRTC 视频聊天](https://apprtc.appspot.com/) 客户端由谷歌应用程序引擎提供支持
- [AppRTC URL 参数](https://apprtc.appspot.com/params.html)


{docsify-updated}
