#### 常用功能实现类

所有功能均需要ZjRTC类的实例化对象完成。
```
 var rtc = new ZjRTC();
```

##  屏幕共享

- 功能描述：支持chrome73+浏览器，否则需要安装屏幕共享插件。在视频通话中，将屏幕以视频的方式分享给其他说话人或者观众看，提高沟通效率。屏幕共享在如下场景中应用广泛：

- 会议中分享自己屏幕的内容，展示自己的操作。

- 实现方法：所有示例代码均假设ZjRTC已经初始化完毕（API详情见SDK开发文档）并且已经完成环境准备（详见快速开始文档）。

### 1.发起屏幕共享

   ```
  rtc.present('screen');
  ...
   ```

### 2. 停止共享

   ```
   rtc.present(null);
    ...
   ```