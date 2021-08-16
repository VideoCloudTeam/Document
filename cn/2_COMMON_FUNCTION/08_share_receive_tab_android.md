#### 接收双流

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

