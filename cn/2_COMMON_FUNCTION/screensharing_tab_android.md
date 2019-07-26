#### 常用功能实现类VCRTC

所有功能均需要VCRTC类的实例化对象完成。

#### 1. 屏幕共享

功能描述：android屏幕共享需要支持`Android5.0`版本以上(包括5.0)，在视频通话中，将屏幕以视频的方式分享给其他说话人或者观众看，提高沟通效率。屏幕共享在如下场景中应用广泛：

- 会议中分享自己屏幕的内容，展示自己的操作。

实现方法：所有示例代码均假设vcrtc已经初始化完毕（API详情见SDK开发文档-常用API列表）并且已经完成环境准备（详见快速开始文档）。

1. 发起屏幕共享：利用vcrtc发送屏幕共享请求

   ```java
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
       vcrtc.sendPresentationScreen();
       .....
   }
   ```

2. 发起屏幕共享回调：

   ```java
   @Override
   public void onScreenShareState(boolean isActive) {
       .....
       //isActive = true 用户允许了权限，发送成功
       //跳转到桌面
       startScreenShare（);
   }
   ```

3. 跳转桌面，开始共享：

   ```java
   private void startScreenShare() {
       //切到桌面
       Intent home = new Intent(Intent.ACTION_MAIN);
       home.addCategory(Intent.CATEGORY_HOME);
       startActivity(home);
   }
   ```

4. 停止共享：

   ```java
   private void stopPresentation() {
       vcrtc.stopPresentation();
    	.....
   }
   ```

5. 屏幕共享接收端回调，专属云和公有云的接收屏幕共享视频的回调略有不同

   ```java
   //公有云以一张张图片的形式接收其他端屏幕共享内容
   @Override
   public void onPresentationReload(String picUrl) {
       //picUrl为双流图片的链接
   }
   //专属云一视频的形式接收其他端屏幕共享内容，返回的是远端双流视频view或视频流，onAddView和onRemoteStream根据实际需求监听一个就好。
     	@Override
   public void onAddView(String uuid, VCRTCView vcrtcView, String viewType) {
       //viewType分两种。1. presentation（双流视频）2. video（正常视频）
       if (streamType.equals("presentation")) {
           ......
       }
   }
   @Override
   public void onRemoteStream(String uuid, String streamURL,String streamType) {
       // viewType分两种。1. presentation（双流视频）2. video（正常视频）
       if (streamType.equals("presentation")) {
           ......        
       }
   }
   ```