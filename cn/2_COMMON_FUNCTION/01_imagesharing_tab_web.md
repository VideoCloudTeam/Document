#### 常用功能实现类

所有功能均需要ZjRTC类的实例化对象完成。
```
 var rtc = new ZjRTC();
```
### 共享图片

功能描述：在视频通话中将自己的图片文件分享给其他的参会人，以此提高沟通效率。

- 1、分享图片前准备

web端需要通过选择input选择你要共享的图片,读取到图片的File流并且转化为blob流

- 2、通知开启分享图片功能

```
rtc.present('screen_http');
```

- 3、在onScreenshareConnected回调函数中，调用发送要分享图片的blob流

```
rtc.onScreenshareConnected = function () {
    $timeout(function () {
        rtc.sendPresentationImage({
            files: [blob]
        });
    });
};
```

- 4、共享图片接收端的回调

```
// 开始或停止演示
rtc.onPresentation = function (isActive, presenter) {
    $timeout(function () {
        // isActive: true:演示流已开始  false:演示已结束
        // presenter: 演示者名字，只有在setting为true时可以设置，false时为null
    });
};

// 有新的JPEG格式 的演示帧可用
tc.onPresentationReload = function (src) {
    $timeout(function () {
        // 新的演示帧对应的图片URL
    });
};
```

   

### 共享PDF

共享PDF功能和共享图片功能类似，主要都是图片的共享，需要将PDF转化成图片资源文件再发起共享。