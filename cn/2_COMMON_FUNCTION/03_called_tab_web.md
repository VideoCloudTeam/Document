## 被呼功能

### 1. 简介

客户端被呼功能包括`点对点被呼`，和`会议室邀请被呼`两种场景。客户端区分是点对点模式还是会议模式，分开进行处理。

### 2. 被呼功能准备

- 需要导入ZjRegister.JS文件

```
  <script src="ZjRegister.js"></script>
```

- 所有被呼功能均需要ZjRegister类的实例化对象完成。

```
    var zjReg = new ZjRegister();
```


### 3.注册

被呼者，需要提前进行注册

```
    zjReg.register("emp.abc.cn", "123@abc.com", "123@1");
```



### 4. 注册监听全局被呼消息

```
zjReg.onIncoming = function (msg) {
    // msg: 呼叫的消息
}
```

