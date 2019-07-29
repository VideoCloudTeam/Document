#### 共享图片

功能描述：在视频通话中将自己的图片文件分享给其他的参会人，以此提高沟通效率。

1. 开始共享图片：专属云和公有云的共享图片略有不同

   ```java
   private void sendSharePicture() {
       // 推荐使用sdk的工具裁剪一下图片，防止图片填充不了视频画面以及图片过大造成的oom的问题
       Bitmap bitmap = bitmapUtil.formatBitmap16_9(imagePathList.get(pictureIndex), 1920, 1080);
       // 专属云是以bitmap的形式发起图片共享
       vcrtc.sendPresentationBitmap(bitmap);
       // 公有云是以file文件的形式发起图片共享
       File file = new File("图片本地路径");
       zjrtc.sendPresentationImage(file);
   }
   ```

2. 关闭共享图片功能：

   ```java
   private void stopPresentation() {
           vcrtc.stopPresentation();
       }
   ```

3. 共享图片接收端的回调: 专属云和公有云的接收共享图片的回调略有不同

   ```java
   // 公有云图片共享回调，公有云是以图片的方式接收的共享图片资源，返回的是url的图片地址
   @Override
   public void onPresentationReload(String url) {
     		.......
   }
     
   // 专属云的图片共享回调，专属云是以视频的方式接受图片共享的资源，返回的是远端双流视频view或视频流，onAddView和onRemoteStream根据实际需求监听一个就好。
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

#### 共享PDF

共享PDF功能和共享图片功能类似，主要都是图片的共享，需要将PDF转化成图片资源文件再发起共享。