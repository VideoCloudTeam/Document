

## 快速开始

### 1、前提条件

- 推荐使用Chrome 73 及以上版本
- 兼容 Safari 11+, Mozilla Firefox 64+, 360极速浏览器 11+, 360SE浏览器 10+, QQ浏览器 10+


### 2、SDK集成说明

- 下载`vcrtc.js`,并放入项目中


### 3、初始化

- 直接运用`<script>`导入
```例如
    <script src="vcrtc.js"></script>
```
- 导入后，需要实例化对象
```例如
    var rtc = new ZjRTC();
```

### 4、加入会议

#### 入会请求

- Web入会（WebRTC或RTMP）命令：makCall

```
rtc.makeCall(node, conference, name, bandwidth, call_type, flash);
// node: 会议服务器的主机名或IP地址
// conference: 会议室地址或短号
// name: 会议中显示的本人名字
// bandwidth: 上下行带宽，可以为空(null)
// call_type: 呼叫类型
// flash: (可选) flash对象，使用RTMP方式入会时使用
```
- 成功将触发onSetup 回调函数

```
rtc.onSetup = function (stream, pinStatus, conferenceExtension) {
    //stream: 一个可以应用到<video>的本地媒体流URL 。对单收或单控呼叫，该值为空
    //pinStatus: 入会密码状态
    //conferenceExtension: 只有呼叫IVR时才会设置该值。 “standard”表示呼叫的是标准的IVR；“mssip”表示呼叫的是lync/skype for business的网关
};
```

#### 完成连接的初始化处理
- 完成会议连接命令：connect(pin, extension)

```
rtc.connect(pin, extension);
// pin: 入会密码。如果会议室没有设置入会密码，参数为null
// extension: 拨打IVR后，需接驳的分机。如直拨的会议室，该参数应为null
```

- 执行成功将触发onConnect回调函数；如果提供的入会密码不正确或提供的接驳分机不存在，将再次触发OnSetup回调函数。

```
this.onConnect = function (url) {
    //url: 一个可应用到<video>的远程媒体流URL  ,可以为空值
};
```

### 5、离开会议

- 离开会议室：disconnect()

```
rtc.disconnect()
```

