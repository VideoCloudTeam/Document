## API Reference for iOS

### 会议管理

|                             方法                             |          功能          |
| :----------------------------------------------------------: | :--------------------: |
|             <a href="#初始化">sharedInstance</a>             |         初始化         |
|              <a href="#apiServer">apiServer</a>              | 加入会议配置服务器域名 |
|         <a href="#加入会议的配置">connectChannel</a>         |        加入会议        |
|        <a href="#配置会议类型">configConnectType</a>         |      设置会议类型      |
|    <a href="#加入会议失败">didDisconnectedWithReason</a>     |      加入会议失败      |
|      <a href="#当前账号退出会议">exitChannelSuccess</a>      |        退出会议        |
| <a href="#配置为专属云模型会议室">configPrivateCloudPlatform</a> | 是否是专属云模型会议室 |
|      <a href="#配置呼入的音视频类型">configCallType</a>      |  配置呼入的音视频类型  |

### 配置主叫方的呼叫账号

|                          方法                          |         功能         |
| :----------------------------------------------------: | :------------------: |
| <a href="#配置主叫方的呼叫账号">configLoginAccount</a> | 配置主叫方的呼叫账号 |

### 配置点对点接听

|                             方法                             |        功能        |
| :----------------------------------------------------------: | :----------------: |
| <a href="#配置点对点接听设置的参数">configPTPOneTimeToken</a> | 配置点对点接听参数 |



### 入会后静音麦克风

|                          方法                          |          功能          |
| :----------------------------------------------------: | :--------------------: |
| <a href="#配置默认入会后静麦克风">configSelectMute</a> | 配置默认入会后静麦克风 |



### 加入会议事件

|                         事件                          |     描述     |
| :---------------------------------------------------: | :----------: |
| <a href="#加入会议失败">didDisconnectedWithReason</a> | 加入会议失败 |



### 接收会中音视频

|                    事件                     |     描述     |
| :-----------------------------------------: | :----------: |
|   <a href="#接收远端视频">didAddView</a>    | 接收远端视频 |
|  <a href="#删除远端视频">didRemoveView</a>  | 删除远端视频 |
| <a href="#接收本地视频">didAddLocalView</a> | 接收本地视频 |



### 重新连接会议

|                        方法                        | 功能               |
| :------------------------------------------------: | ------------------ |
| <a href="#重新连接本地音视频">reloadLocalVideo</a> | 重新连接本地音视频 |
| <a href="#重新连接本地音视频">connectMediaCall</a> | 连接音视频         |
| <a href="#重建音视频">reconstructionMediaCall</a>  | 重建音视频         |

### 通话质量

|                             方法                             |                             功能                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|    <a href="#打开音视频质量参数监听">enableStatistics</a>    |                    打开音视频质量参数监听                    |
|      <a href="#配置音视频呼叫速率">configBandwidth</a>       |                      配置音视频呼叫速率                      |
| <a href="#配置音视频接收呼叫速率">configReceiveBandwidth</a> |                    配置音视频接收呼叫速率                    |
|  <a href="#配置音视频发送呼叫速率">configSendBandwidth</a>   |                    配置音视频发送呼叫速率                    |
|         <a href="#配置视频帧率">configFrameRate</a>          |                         配置视频帧率                         |
|    <a href="#配置视频接收帧率">configReceiveFrameRate</a>    |                       配置视频接收帧率                       |
|     <a href="#配置视频发送帧率">configSendFrameRate</a>      |                       配置视频发送帧率                       |
| <a href="#配置发送、接收的视频分辨率">configResolutionWidth</a> |                  配置发送、接收的视频分辨率                  |
| <a href="#配置发送的视频分辨率">configSendResolutionWidth</a> |                     配置发送的视频分辨率                     |
| <a href="#配置接收的视频分辨率">configReceiveResolutionWidth</a> |                     配置接收的视频分辨率                     |
|     <a href="#设置音视频质量参数">configVideoProfile</a>     | 配置音视频质量参数(设置为VCVideoProfileCustom)其他参数才有效 |

|                          事件                           |        描述        |
| :-----------------------------------------------------: | :----------------: |
| <a href="#获取音视频质量参数">didReceivedStatistics</a> | 获取音视频质量参数 |



### 会中媒体管理

|                     方法                      |        功能         |
| :-------------------------------------------: | :-----------------: |
| <a href="#打开/禁用本地麦克风">micEnable</a>  | 打开/禁用本地麦克风 |
| <a href="#打开/禁用本地相机">videoEnable</a>  |  打开/禁用本地相机  |
|  <a href="#切换前后摄像头">switchCamera</a>   |   切换前后摄像头    |
| <a href="#打开仅语音模式">onlyAudioEnable</a> |   打开仅语音模式    |

### 屏幕共享

|                         方法/属性                          |             功能              |
| :--------------------------------------------------------: | :---------------------------: |
|        <a href="#屏幕共享初始化">sharedInstance</a>        |        屏幕共享初始化         |
|             <a href="#groupId配置">groupId</a>             | apple developer 申请的groupId |
|           <a href="#连接到共享屏幕">connect</a>            |        连接到共享屏幕         |
|             <a href="#断开连接">disconnect</a>             |           断开连接            |
| <a href="#更新录制屏幕的数据流">didCaptureSampleBuffer</a> |     更新录制屏幕的数据流      |
|      <a href="#主动停止屏幕共享">stopRecordScreen</a>      |       主动停止屏幕共享        |



### 分享图片/PDF

|                             方法                             |                      功能                      |
| :----------------------------------------------------------: | :--------------------------------------------: |
|       <a href="#分享图片类型到会中">shareImageData</a>       |               分享图片类型到会中               |
| <a href="#分享图片类型为视频流方式到会中">shareToStreamImageData</a> |         分享图片类型为视频流方式到会中         |
| <a href="#分享图片类型为白板方式到会中">shareToWhiteOpen</a> |          分享图片类型为白板方式到会中          |
| <a href="#当前分享被其他端打断后，要自动关闭本地分享资源">shareToRemoteDisconnect</a> | 当前分享被其他端打断后，要自动关闭本地分享资源 |

|                             事件                             |                描述                 |
| :----------------------------------------------------------: | :---------------------------------: |
|      <a href="#会中有参会者发起分享">didStartImage</a>       |        会中有参会者发起分享         |
| <a href="#接收到其他参会者更新发送的图片分享">didUpdateImage</a> | 接收到其他参会者更新/发送的图片分享 |
|  <a href="#接收其他端更新发送的视频分享">didUpdateVideo</a>  |    接收其他端更新发送的视频分享     |
| <a href="#获取到其他端或版本端的停止分享事件">didStopImage</a> | 获取到其他端或版本端的停止分享事件  |
|  <a href="#接收到其他参会者开启白板">didStartWhiteBoard</a>  |      接收到其他参会者开启白板       |
|  <a href="#接收到其他参会者开启白板">didStopWhiteBoard</a>   |      接受到其他参会者关闭白板       |



### 获取会中参会者信息

|        属性        | 功能                     |
| :----------------: | ------------------------ |
|     rosterList     | 参会者信息列表           |
| layoutParticipants | 显示在视频上的布局参会者 |

|                             事件                             | 描述                                 |
| :----------------------------------------------------------: | ------------------------------------ |
|       <a href="#加入新的参会者">didAddParticipant</a>        | 加入新的参会者                       |
|      <a href="#参会者信息变更">didUpdateParticipant</a>      | 参会者信息变更                       |
| <a href="#会中参会者信息有变或者参会者数量有变">didUpdateParticipants</a> | 会中参会者信息有变或者参会者数量有变 |
|      <a href="#参会者离开会议">didRemoveParticipant</a>      | 参会者离开会议                       |
| <a href="#获取会中正在发言的参会者列表">didReceivedStageVoice</a> | 获取会中正在发言的参会者列表         |
| <a href="#获取会中正在展示视频的参会者列表">didLayoutParticipants</a> | 获取会中正在展示视频的参会者列表     |



### 会中消息

|                     方法                      |        功能        |
| :-------------------------------------------: | :----------------: |
| <a href="#在会中发送文字信息">sendMessage</a> | 在会中发送文字信息 |

|                            事件                            |           描述           |
| :--------------------------------------------------------: | :----------------------: |
| <a href="#接收到其他参会者发的消息">didReceivedMessage</a> | 接收到其他参会者发的消息 |

### 多流

|                         方法                          |         功能         |
| :---------------------------------------------------: | :------------------: |
| <a href="#配置音视频接收的方式">configMultistream</a> | 配置音视频接收的方式 |

### 获取消费ID

|                          方法                          |         功能         |
| :----------------------------------------------------: | :------------------: |
| <a href="#获取消费ID">getServiceUUID</a> | 获取消费ID |

#### 方法

###### 初始化

```objc
+ (instancetype)sharedInstance
```

###### 会议连接及各项配置

- ###### 加入会议的配置

```objc
- (void)connectChannel:(nonnull NSString *)channel
              password:(NSString *)password
                  name:(nonnull NSString *)name
               success:(void (^)(id))success
               failure:(void (^)(NSError *error))failure
```

参数

| channel  |  会议短号或地址  |
| :------: | :--------------: |
| password |     入会密码     |
|   name   | 在会中显示的名称 |

返回

status: "success" 成功

error不为空,失败

- ###### 配置会议类型

`- (void)configConnectType:(VCConnectType )connectType`

参数 connectType

| VCConnectTypeMeeting     | 音视频会议                        |
| ------------------------ | --------------------------------- |
| VCConnectTypeUser        | 点对点间呼叫                      |
| VCConnectTypeOnlyManager | 仅管理会议                        |
| VCConnectTypeHideMe      | 隐身入会 (观看会议，并不参与会议) |



- ###### 配置为专属云模型会议室

说明: YES是专属云模式,NO是公有云,默认为NO

`- (void)configPrivateCloudPlatform:(BOOL ) privateCloud`

参数

- ###### 配置音视频质量参数

`- (void)configVideoProfile:(VCVideoProfile )profile `

参数 profile

|         信息         |                 说明                  |
| :------------------: | :-----------------------------------: |
|  VCVideoProfileNone  |                不限制                 |
|  VCVideoProfile720P  | 720像素 width:1280 height:720 帧率:25 |
|  VCVideoProfile480P  | 480像素 width:960 height:480 帧率:20  |
|  VCVideoProfile360P  | 360像素 width:320 height:360 帧率:15  |
| VCVideoProfileCustom |                自定义                 |

**注意:**该方法设置为`VCVideoProfileCustom`下面有关配置音视频呼叫速率、配置音视频接收呼叫速率、配置音视频发送呼叫速率、配置音视频发送呼叫速率、配置视频帧率、配置视频发送帧率才有效

- ###### 配置音视频呼叫速率

说明:同时配置接收、发送呼叫速率,默认为800kbps

> **注意**:`- (void)configMultistream:(BOOL )multistream` 此方法设置为YES ,设置此参数无效

`- (void)configBandwidth:(CGFloat ) bandwidth `



|   参数    |           参数说明           |
| :-------: | :--------------------------: |
| bandwidth | 音视频呼叫速率 默认为800kbps |



- ###### 配置音视频接收呼叫速率

说明: 音视频呼叫速率 默认为800kbps

```objc
- (void)configReceiveBandwidth:(CGFloat ) bandwidth
```

- ###### 配置音视频发送呼叫速率

说明: 视频呼叫速率,默认为800kbps

`- (void)configSendBandwidth:(CGFloat ) bandwidth`

- ###### 配置视频帧率 

说明: 同时配置接收、发送帧率,多流模式该设置无效,默认为25fps

`- (void)configFrameRate:(CGFloat ) frameRate`

|   参数    |       参数说明       |
| :-------: | :------------------: |
| frameRate | 视频帧率 默认为25fps |



- ###### 配置视频接收帧率 

说明: 多流模式该设置无效,默认为25fps

`- (void)configReceiveFrameRate:(CGFloat ) frameRate`

- ###### 配置视频发送帧率

说明: 多流模式该设置无效,默认为25fps

`- (void)configSendFrameRate:(CGFloat ) frameRate`

- ###### 配置发送、接收的视频分辨率

说明: 多流模式该设置无效,默认为 960x540

`- (void)configResolutionWidth:(NSInteger )width andHeight:(NSInteger )height`

参数

|  参数  |   参数说明   |
| :----: | :----------: |
| width  | 视频分辨率宽 |
| height | 视频分辨率高 |



- ###### 配置发送的视频分辨率

说明: 多流模式该设置无效,默认为 960x540

`- (void)configSendResolutionWidth:(NSInteger )width andHeight:(NSInteger )height`

- ###### 配置接收的视频分辨率

说明: 多流模式该设置无效,默认为 960x540

`- (void)configReceiveResolutionWidth:(NSInteger )width andHeight:(NSInteger )height`

- ###### 配置音视频接收的方式

`- (void)configMultistream:(BOOL )multistream`

|    参数     |                           参数说明                           |
| :---------: | :----------------------------------------------------------: |
| multistream | 是否是多流模式 YES是多流模式(多流转发),NO不是多流模式(全编全解) |



- ###### 配置默认入会后静麦克风

`- (void)configSelectMute:(BOOL )mute`

| 参数 |             参数说明             |
| :--: | :------------------------------: |
| mute | YES入会静音麦克风,NO不静音麦克风 |



- ###### 配置主叫方的呼叫账号

`- (void)configLoginAccount:(NSString *)account `

|  参数   |       参数说明       |
| :-----: | :------------------: |
| account | 登录时设置登录的账号 |



- ###### 配置点对点接听设置的参数

使用场景:在接收到呼叫的时候设置,这些参数会通过下面PushKit的接收推送这个方法给用户

```objc
//payload.dictionaryPayload 包含了token、bsskey、timeStamp、owner等参数
- (void)pushRegistry:(PKPushRegistry *)registry didReceiveIncomingPushWithPayload:(PKPushPayload *)payload forType:(NSString *)type {
  
}
```



```objc
- (void)configPTPOneTimeToken:(NSString *)token
                    andBsskey:(NSString *)bsskey
                     andStamp:(NSString *)timeStamp
                        owner:(NSString *)owner
```

|   参数    |     参数说明      |
| :-------: | :---------------: |
|   token   |     平台信令      |
|  bsskey   |   平台验证key值   |
| timeStamp |   呼叫的时间戳    |
|   owner   | 真实通过的MCU地址 |



- ###### 配置呼入的音视频类型

`- (void)configCallType:(VCCallType )callType `

|       callType参数对应的值        | 描述                                              |
| :-------------------------------: | ------------------------------------------------- |
|     VCCallTypeSendAndReceive      | 建立音视频方式入会                                |
| VCCallTypeOnlySendVideoAndReceive | 建立仅视频方式入会,一般用于没有麦克风权限方式入会 |
| VCCallTypeOnlySendAudioAndReceive | 建立仅音频方式入会,一般用于没有摄像机权限方式入会 |
|        VCCallTypeOnlySend         | 建立仅发送音视频，并不接收                        |
|      VCCallTypeOnlySendVideo      | 建立仅发送视频，并不接收                          |
|      VCCallTypeOnlySendAudio      | 建立仅发送音频，并不接收                          |
|       VCCallTypeOnlyReceive       | 建立仅接收远端音视频方式入会                      |
|          VCCallTypeNone           | 不建立音视频入会 (暂时可用在 仅会议管理管理功能)  |



- ###### 重新连接本地音视频

`- (void)reloadLocalVideo `

- ###### 连接音视频

使用情景: 假如是没有通过音视频入会的，可以在会中直接创建音视频。

`- (void)connectMediaCall `

- ###### 重建音视频

  使用情景: 出现断网或者改变网络节点的情况下调用

`- (void)reconstructionMediaCall `

###### 会中媒体管理

- ###### 打开/禁用本地麦克风

`- (void)micEnable:(BOOL)enabled`

- ###### 打开/禁用本地相机

`- (void)videoEnable:(BOOL)enabled`

- ###### 切换前后摄像头

`- (void)switchCamera`

- ###### 打开仅语音模式

`- (void)onlyAudioEnable:(BOOL)enabled`

###### 邀请入会

- ###### 邀请他人入会

```objc
- (void)inviteDesitination:(NSString *)dest
    success:(successBlock)success
    failure:(failureBlock)failure
```

说明:  protocol: 专属云环境下默认是:sip 公有云默认:auto 参会者身份是访客 streaming NO            

| 参数 |  参数说明  |
| :--: | :--------: |
| dest | 通讯录地址 |



```objc
- (void)inviteDestination:(NSString *)dest
                 protocol:(NSString *)protocolType
                    alias:(NSString *)name
                     host:(BOOL)isHost
                streaming:(BOOL)isStreaming
                  success:(successBlock)success
                  failure:(failureBlock)failure
```

|     参数     |                           参数说明                           |
| :----------: | :----------------------------------------------------------: |
|     dest     |                    终端的地址/短号/手机号                    |
| protocolType | 呼叫使用的协议: "sip", "h323", "rtmp",<br/>"mssip" , “phone”(普通电话)；或者 "auto" (适用于内部终端或用户) |
|     name     |                         入会显示名称                         |
|    isHost    |                  参会身份 YES 主持人 NO访客                  |
| isStreaming  |              发送流方式 YES多流转发 NO全编全解               |

###### 发送文字信息

- ###### 在会中发送文字信息

```objc
- (void)sendMessage:(NSString *)message
            success:(successBlock)success
            failure:(failureBlock)failure
```

|  参数   |    参数说明    |
| :-----: | :------------: |
| message | 发送的消息内容 |

###### 打开音视频质量参数监听

- ###### 打开音视频质量参数监听

`- (void)enableStatistics:(BOOL)enable`

###### 当前账号退出会议

```objc
- (void)exitChannelSuccess:(successBlock)success
                   failure:(failureBlock)failure
```

###### SDK中日志写入成文件

```objc
- (void)uploadLoggerWithName:(NSString *)name
                   URLString:(NSString *)URLString
```

|   参数    |   参数说明    |
| :-------: | :-----------: |
|   name    |   写入地址    |
| URLString | 上传的URL地址 |



###### 分享相关功能

- ###### 分享图片类型到会中

```objc
- (void)shareImageData:(NSData *)imageData
                  open:(BOOL )open
                change:(BOOL )change
               success:(successBlock)success
               failure:(failureBlock)failure
```

**注意:**       open为YES  change为NO  分享图片

​                open 为YES  change为YES 分享多张图片时,更改当前显示的图片

​                open为NO  change为NO  结束自己在会中分享

|   参数    |         参数说明         |
| :-------: | :----------------------: |
| imageData |    图片转成NSData数据    |
|   open    |        是否是分享        |
|  change   | 是否是更改当前分享的图片 |



- ###### 分享图片类型为视频流方式到会中

```objc
- (void)shareToStreamImageData:(NSData *)imageData
                          open:(BOOL )open
                        change:(BOOL )change
                       success:(successBlock)success
                       failure:(failureBlock)failure
```

**注意:**       open为YES  change为NO  分享图片

​                open 为YES  change为YES 分享多张图片时,更改当前显示的图片

​                open为NO  change为NO  结束自己在会中分享

|   参数    |         参数说明         |
| :-------: | :----------------------: |
| imageData |    图片转成NSData数据    |
|   open    |        是否是分享        |
|  change   | 是否是更改当前分享的图片 |

- ###### 分享图片类型为白板方式到会中

```objc
- (void)shareToWhiteOpen:(BOOL )open success:(successBlock)success failure:(failureBlock)failure
```

| 参数 | 参数说明 |
| :--: | :------: |
| open | 打开分享 |



- ###### 当前分享被其他端打断后，要自动关闭本地分享资源

`- (void)shareToRemoteDisconnect `

###### 屏幕共享

- ###### 屏幕共享初始化

```objc
+ (instancetype)sharedInstance
```

- ###### groupId配置

说明:https://developer.apple.com 申请的groupId

```objc
@property (nonatomic, strong) NSString *groupId;
```

- ######  连接到共享屏幕

```objc
- (void)connect;
```

- ###### 更新录制屏幕的数据流

```objc
- (void)didCaptureSampleBuffer:(CMSampleBufferRef)sampleBuffer
```

- ###### 断开连接

```objc
- (void)disconnect;
```

- ###### 主动停止屏幕共享

`- (void)stopRecordScreen`

##### VCRtcModuleDelegate 协议方法

###### 加入会议

- ###### 加入会议失败

`- (void)VCRtc:(VCRtcModule *)module didDisconnectedWithReason:(NSError *)reason`

|  参数  |        参数说明        |
| :----: | :--------------------: |
| module | VCRtcModule 的实例对象 |
| reason |        错误原因        |



###### 接收音视频

- ###### 接收远端视频

使用场景:有新的参会者加入会议,显示参会者视频画面

`- (void)VCRtc:(VCRtcModule *)module didAddView:(VCVideoView *)view uuid:(NSString *)uuid`

| 参数 |            参数说明            |
| :--: | :----------------------------: |
| view |       承载远端视频的界面       |
| uuid | 当前视频所对应参会者的唯一标识 |



- ###### 删除远端视频

使用场景: 有参会者离开会议,移除该参会者视频画面

`(void)VCRtc:(VCRtcModule *)module didRemoveView:(VCVideoView *)view uuid:(NSString *)uuid`

- ###### 接收本地视频

使用场景: 自己本端的视频画面

`- (void)VCRtc:(VCRtcModule *)module didAddLocalView:(VCVideoView *)view`

###### 获取会中参会者信息

- ###### 加入新的参会者

`- (void)VCRtc:(VCRtcModule *)module didAddParticipant:(Participant *)participant`

Participant 相关字段说明

|      属性       |                    功能                     |
| :-------------: | :-----------------------------------------: |
|  callDirection  |            “in”:呼入,”out”:呼出             |
|   displayName   |                参会者的昵称                 |
|  isPresenting   | 当前参会者是否在发送双流(图片/pdf分享等等)  |
|     isMuted     |                  是否静音                   |
| isAudioOnlyCall |           参会者是否只有音频通讯            |
|   isVideoCall   |            参会者是否有视频能力             |
|    isVmuted     |        是否是全体静音 或 是否被静画         |
|   localAlias    |                 用户的账号                  |
|   overlayText   |              在会中显示的昵称               |
|  protocolType   |                  协议类型                   |
|      role       |  入会角色。"guest"：访客；"chair"：主持人   |
|    startTime    |             入会时间（UNIX秒）              |
|       uri       |                 参会者地址                  |
|      uuid       |           本参会者的唯一标识uuid            |
|     vendor      |          入会终端/浏览器的厂商标识          |
|      ssrc       |                 音视频标识                  |
|    spotlight    |              参会者为焦点事件               |
|     isChair     |                是否是主持人                 |
|    isWaiting    |                YES 等待入会                 |
|  isConnecting   |                 正在呼叫中                  |
|   isSpeaking    |              正在说话(专属云)               |
|       vad       | 讲话者指示：0=不在讲话 100=正在讲话(公有云) |



- ###### 参会者信息变更

`- (void)VCRtc:(VCRtcModule *)module didUpdateParticipant:(Participant *)participant`

- ###### 会中参会者信息有变或者参会者数量有变

`- (void)VCRtc:(VCRtcModule *)module didUpdateParticipants:(NSArray *)participants`

- ###### 参会者离开会议

`- (void)VCRtc:(VCRtcModule *)module didRemoveParticipant:(Participant *)participant`

- ###### 获取会中正在发言的参会者列表

`- (void)VCRtc:(VCRtcModule *)module didReceivedStageVoice:(NSArray *)voices`

- ###### 获取会中正在展示视频的参会者列表

`- (void)VCRtc:(VCRtcModule *)module didLayoutParticipants:(NSArray *)participants`

- ###### 接收到其他参会者发的消息

`(void)VCRtc:(VCRtcModule *)module didReceivedMessage:(NSDictionary *)message`



| message的包含的字段 |      描述      |
| :-----------------: | :------------: |
|        uuid         | 用户唯一标识符 |
|       message       |    消息内容    |



- ###### 获取音视频质量参数

`- (void)VCRtc:(VCRtcModule *)module didReceivedStatistics:(NSArray<VCMediaStat *> *)mediaStats;`

- 

| VCMediaStat包含字段 |          描述          |
| :-----------------: | :--------------------: |
|        uuid         |    参会人的唯一标识    |
|      mediaType      |   类型(audio/video)    |
|      direction      |  发送接收(send/recv)   |
|        codec        |        编码格式        |
|     resolution      |         分辨率         |
|      frameRate      |          帧率          |
|       bitrate       |          码率          |
|       packets       | 发送或接收数据包的数量 |
|    packagesLost     |        丢包数量        |
|     resolution      |         分辨率         |
|    fractionLost     |         丢包率         |
|       jitter        |          抖动          |

###### 分享功能

- ###### 会中有参会者发起分享

`- (void)VCRtc:(VCRtcModule *)module didStartImage:(NSString *)shareUuid`

|   参数    |        参数说明        |
| :-------: | :--------------------: |
| shareUuid | 分享端的参会者唯一标识 |



- ###### 接收到其他参会者更新发送的图片分享

`- (void)VCRtc:(VCRtcModule *)module didUpdateImage:(NSString *)imageStr uuid:(NSString *)uuid`

|   参数   |        参数说明        |
| :------: | :--------------------: |
| imageStr |     分享的图片链接     |
|   uuid   | 分享端的参会者唯一标识 |

- ###### 接收其他端更新发送的视频分享

`- (void)VCRtc:(VCRtcModule *)module didUpdateVideo:(NSString *)imageStr uuid:(NSString *)uuid`

- ###### 获取到其他端或版本端的停止分享事件

`- (void)VCRtc:(VCRtcModule *)module didStopImage:(NSString *)imageStr`

- ###### 接收到其他参会者开启白板

`- (void)VCRtc:(VCRtcModule *)module didStartWhiteBoard:(NSString *)shareUrl withUuid:(NSString *)uuid`

|   参数   |        参数说明        |
| :------: | :--------------------: |
|  module  | VCRtcModule的实例对象  |
| shareUrl |     分享白板的链接     |
|   uuid   | 分享端的参会者唯一标识 |


- ###### 接受到其他参会者关闭白板

`- (void)VCRtc:(VCRtcModule *)module didStopWhiteBoard:(NSString *)shareUrl withUuid:(NSString *)uuid `

- ###### 获取消费ID

`- (NSString *)getServiceUUID`


#####  属性列表

| 属性名                                 |                  描述                  |
| :------------------------------------- | :------------------------------------: |
| <a href="#apiServer">apiServer</a>     |               服务器域名               |
| <a href="#liveServer">liveServer</a>   |           开启直播、录播域名           |
| <a href="#groupId">groupId</a>         | 屏幕录制 apple developer 申请的groupId |
| bandwidth                              |                  带宽                  |
| uuid                                   |          当前用户的唯一标识符          |
| rosterList                             |             参会者信息列表             |
| layoutParticipants                     |        显示在视频上的布局参会者        |
| forceOrientation                       |              屏幕旋转方向              |
| role                                   |               参会者身份               |
| serviceUUID                            |                 消费ID                |
| isSupportLive                          |              支持直播功能              |
| isSupportRecord                        |              支持录制功能              |
| callName                               |               呼叫的名称               |
| shareImageURL                          |          会中分享的image地址           |
| localShareUuid                         |        图片、PDF分享成流时使用         |
| <a href="#callType">callType</a>       |             音视频连接类型             |
| <a href="#connectType">connectType</a> |              会议连接类型              |

- ######  apiServer 

初始化的时候必须配置服务器域名

- ###### liveServer 

开启直播或录播必须配置域名

- ###### groupId 

需要屏幕录制,必须配置groupId

- ###### callType 

设置音视频链接类型 

```objc
typedef NS_ENUM(NSInteger, VCCallType ) {
    VCCallTypeSendAndReceive,           // 建立音视频方式入会。
    VCCallTypeOnlySendVideoAndReceive,  // 建立仅视频方式入会,一般用于没有麦克风权限方式入会。
    VCCallTypeOnlySendAudioAndReceive,  // 建立仅音频方式入会,一般用于没有摄像机权限方式入会。
    VCCallTypeOnlySend,                 // 建立仅发送音视频，并不接受。
    VCCallTypeOnlySendVideo,            // 建立仅发送视频，并不接受。
    VCCallTypeOnlySendAudio,            // 建立仅发送音频，并不接受。
    VCCallTypeOnlyReceive,              // 建立仅接收远端音视频方式入会。
    VCCallTypeNone                      // 不建立音视频入会。 (暂时可用在 仅会议管理管理功能)
};
```

- ###### connectType 

会议类型

```objc
// 会议类型
typedef NS_ENUM(NSInteger, VCConnectType) {
    VCConnectTypeMeeting,//音视频会议
    VCConnectTypeUser,//点对点间呼叫
    VCConnectTypeOnlyManager,//仅管理会议
    VCConnectTypeHideMe//隐身入会 (观看会议，并不参见会议)
};
```


