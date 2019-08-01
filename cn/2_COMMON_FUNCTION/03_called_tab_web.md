## 被呼功能

### 1. 简介

客户端被呼功能包括`点对点被呼`，和`会议室邀请被呼`两种场景。客户端区分是点对点模式还是会议模式，分开进行处理。

### 2. 被呼功能准备

- 需要导入[VCRegister.js](https://github.com/VideoCloudTeam/WEB-SDK/blob/master/SDK/VCRegister.js)文件

```javascript
  <script src="VCRegister.js"></script>
```

- 所有被呼功能均需要VCRegister类的实例化对象完成。

```javascript
    var vcReg = new VCRegister();
```


### 3.注册

被呼者，需要提前进行注册

```javascript
    vcReg.register("emp.abc.cn", "123@abc.com", "123@1");
```



### 4. 注册监听全局被呼消息

```javascript
vcReg.onIncoming = function (msg) {
    // msg: 呼叫的消息
}
```

