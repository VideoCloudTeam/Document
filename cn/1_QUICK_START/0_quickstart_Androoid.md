### 前提条件

* Android SDK API Level >= 16
* Android Studio 3.0以上版本
* APP要求Android 4.1或以上设备
* 目前不支持Androidx

### 添加SDK

1. 复制 `vcrtc-release.aar`文件到app的libs下 

2. 在项目的根目录的`build.gradle`加入以下代码

```gradle
allprojects {
   repositories {
      jcenter()
      maven { url 'https://jitpack.io' }
   }
}
```

3. 修改app下的`build.gradle`文件

```java
android {

    defaultConfig {

        ...

        ndk {
            abiFilters "armeabi-v7a"
        }
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    repositories {
        flatDir {
            dirs 'libs'
        }
    }
}
dependencies {
  	implementation fileTree(include: ['*.jar'], dir: 'libs')
    implementation 'com.squareup.okhttp3:okhttp:3.11.0'
    implementation 'org.java-websocket:Java-WebSocket:1.4.0'
    implementation 'com.google.code.gson:gson:2.8.5'
    // 用于会中图片共享的图片选择器
    implementation 'com.github.HuanTanSheng:EasyPhotos:2.4.5'
    // 动态权限请求
    implementation 'com.qw:soulpermission:1.1.7'
    implementation 'com.github.barteksc:pdfium-android:1.7.0'
    implementation(name: 'vcrtc-release', ext: 'aar')
      
}

```







### 初始化

集成完成后需要自定义一个Application，在新建的Application中的onCreate()方法进行SDK初始化，并可以做一些其他操作，然后在manifest文件中指定Application，代码如下：

```java
public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
      
      	VCRTCPreferences prefs = new VCRTCPreferences(this);
      	prefs.setPrintLogs(true);
        // 开始打印日志
        LogUtil.startWriteLog(this, false);
      	// 初始化权限请求库，具体使用可以查看sdkDemo
      	SoulPermission.init(this);
        //初始化SDK
        RTCManager.init(this);
      	// 设置开发者id
      	prefs.setDeviceId("");
        prefs.setToken("");

        //以下可不设置，设置是为了出现问题方便服务端定位
        //设备类型
        RTCManager.DEVICE_TYPE = "Android";
        //应用id
        RTCManager.APPLICATION_ID = BuildConfig.APPLICATION_ID;  
        //应用版本号
        RTCManager.VERSION_NAME = BuildConfig.VERSION_NAME;
        //合作伙伴
        RTCManager.OEM = "oem";
    }
}
```

### 初始化参数设置

通话之前，优先进行偏好设置，包括服务器地址、视频参数及编码、是否打印日志等设置，服务器地址必须设置，其他选项如果不设置，将使用默认值，偏好设置一次即可一直生效，建议在app的设置页面做配置，具体可设置选项详见api列表。

```java
VCRTCPreferences preferences = new VCRTCPreferences(this);
preferences.setServerAddress("服务器地址", new CallBack() {
            @Override
            public void success(String message) {
                
            }

            @Override
            public void failure(String reason) {
                
            }
        });
```



### 会议室号码和入会密码判断

```java
		入会前需要进行权限检测，根据检测反馈结果，设置CallType，具体见sdkDemo
		/**
     * 检测会议室号码或密码是否正确
     * @param num
     * @param apiServer
     */
    private void loadInfoAndCall(String num, String apiServer) {
        CheckUtils.checkConference(num, apiServer, new CheckUtils.CheckConferenceListener() {
            @Override
            public void onSuccess(String s) {
                // 验证成功
            }

            @Override
            public void onFail(Exception e) {
                // 验证失败
            }
        });

    }


```





### 加入会议（发起呼叫）

呼叫前首先要设置回调监听`VCRTCListener`

```java
private void makeCall(){
    vcrtc = new VCRTC(this);
  	vcrtc.setCheckdup(VCUtil.MD5(SystemUtil.getMac(context) + "显示的昵称"));
  	// 如果是被呼msgjson中会有对应信息，主动入会不需要
    if (call.getMsgJson() != null && !"".equals(call.getMsgJson())) {
            try {
                JSONObject root = new JSONObject(call.getMsgJson());
                String chanel = root.optString("conference_alias");
                String time = root.optString("time");
                String bssKey = root.optString("bsskey");
                String token = root.optString("token");
                call.setChannel(chanel);
                vcrtc.setTime(time);
                vcrtc.setBsskey(bssKey);
                vcrtc.setOneTimeToken(token);
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
    vcrtc.setVCRTCListener(listener);
    // 发起呼叫，这里的回调只是接口回调成功，还未入会，如果失败，说明入会失败，给出错误信息
    vcrtc.connect("会议室号", "会议室密码", "显示名", new CallBack() {
      @Override
      public void success(String s) {

      }

      @Override
      public void failure(String s) {
				// todo 打印错误信息
      }
    });
}
```

### 接收回调信息

连接成功后，视频view或者视频流以及会议室相关状态信息会在回调中返回，开发者可在相关回调中处理业务逻辑以及对视频画面进行展示。**注：有些回调在子线程，做UI刷新操作记得切到主线程**。

```java
VCRTCListener listener = new VCRTCListener() {

    /**
     * 本地视频回调
     * @param uuid
     * @param view
     */
    @Override
    public void onLocalVideo(String uuid, VCRTCView view) {
        vcrtcView.setMirror(true);
        flLocal.addView(vcrtcView, FrameLayout.LayoutParams.MATCH_PARENT, 	FrameLayout.LayoutParams.MATCH_PARENT);
    }

    /**
     * 转发模式下远端视频回调，如果不需要流，只是将View进行显示，可以使用
     * @param uuid
     * @param view
     * @param viewType
     */
    @Override
    public void onAddView(String uuid, VCRTCView view, String viewType) {
        llRemote.addView(vcrtcView, FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.MATCH_PARENT);
    }

    /**
     * 转发远端视频退出
     * @param s
     * @param vcrtcView
     */
    @Override
    public void onRemoveView(String uuid, VCRTCView vcrtcView) {
        llRemote.removeView(vcrtcView);
    }
  	
  // 如果不需要View，自己定义VCRTCView，需要在这个位置将本地streamUrl设置进去，本地展示需要streamType是camera，保证浏览的清晰度（默认1280 * 720），详情请看demo
  	@Override
    public void onLocalStream(String uuid, String streamURL, String streamType) {
            myUUID = uuid;
            if (mediaCallBack != null && "camera".equalsIgnoreCase(streamType)) {
                // todo 自行处理
            }
        }
  
  	// 远端视频流，streamType（video，presentation）两种，处理方式同上，详情请看demo
  	@Override
    public void onRemoteStream(String uuid, String streamURL, String streamType) {
            if (mediaCallBack != null) {
                // 自行处理
        }

    /**
     * 被服务器断开
     * @param reason
     */
    @Override
    public void onDisconnect(String reason) {
        flLocal.removeAllViews();
        flRemote.removeAllViews();
      	// 如果自己创建的View，请自行处理释放
        disconnect();
    }
    /**
    * 参会者信息以列表的形式返回,通过下面几个回调可以维护一个会中参会者列表
    */
    @Override
    public void onParticipantList(List<Participant> participantList) {}
     
    /**
    * 参会者信息以单个人的方式返回 如果设置了vcrtc.setSupportParticipantList(true)则不会回调
    */
    @Override
    public void onAddParticipant(Participant participant) {}
      
    /**
    * 参会者信息更新
    */
    @Override
    public void onUpdateParticipant(Participant participant) {}
    
    /**
    * 参会者移除
    */
    @Override
    public void onRemoveParticipant(String uuid) {}
};
```

### 退出会议

```java
vcrtc.disconnect();
```

