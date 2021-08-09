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

   
