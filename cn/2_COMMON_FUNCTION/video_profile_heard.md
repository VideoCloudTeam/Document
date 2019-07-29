#### 

|                             信息                             | 说明                                                         |
| :----------------------------------------------------------: | ------------------------------------------------------------ |
| <a href="#配置音视频呼叫速率(接收/发送)">configBandwidth</a> | 配置音视频呼叫速率                                           |
| <a href="#配置音视频接收呼叫速率">configReceiveBandwidth</a> | 配置音视频接收呼叫速率                                       |
|  <a href="#配置音视频发送呼叫速率">configSendBandwidth</a>   | 配置音视频发送呼叫速率                                       |
|    <a href="#配置视频帧率(接收/发送)">configFrameRate</a>    | 配置视频帧率                                                 |
|    <a href="#配置视频接收帧率">configReceiveFrameRate</a>    | 配置视频接收帧率                                             |
|     <a href="#配置视频发送帧率">configSendFrameRate</a>      | 配置视频发送帧率                                             |
| <a href="#配置发送、接收的视频分辨率">configResolutionWidth</a> | 配置发送、接收的视频分辨率                                   |
| <a href="#配置发送的视频分辨率">configSendResolutionWidth</a> | 配置发送的视频分辨率                                         |
| <a href="#配置接收的视频分辨率">configReceiveResolutionWidth</a> | 配置接收的视频分辨率                                         |
|     <a href="#设置音视频质量参数">configVideoProfile</a>     | 配置音视频质量参数(设置为VCVideoProfileCustom)其他参数才有效 |

- ###### 设置音视频质量参数

|         信息         |                 说明                  |
| :------------------: | :-----------------------------------: |
|  VCVideoProfileNone  |                不限制                 |
|  VCVideoProfile720P  | 720像素 width:1280 height:720 帧率:25 |
|  VCVideoProfile480P  | 480像素 width:960 height:480 帧率:20  |
|  VCVideoProfile360P  | 360像素 width:320 height:360 帧率:15  |
| VCVideoProfileCustom |        自定义,其他的方法才有效        |

```objc
//默认VCVideoProfile480P
-(void)configVideoProfile:(VCVideoProfile )profile
```



- ###### 配置音视频呼叫速率(接收/发送)

```objc
//默认为800kbps
- (void)configBandwidth:(CGFloat ) bandwidth
```

- ###### 配置音视频接收呼叫速率 

```objc
//默认为800kbps
- (void)configReceiveBandwidth:(CGFloat ) bandwidth
```

- ###### 配置音视频发送呼叫速率

```objc
//默认为800kbps
- (void)configSendBandwidth:(CGFloat ) bandwidth
```

- ###### 配置视频帧率(接收/发送)

```objc
//默认为25fps
- (void)configFrameRate:(CGFloat ) frameRate 
```

- ###### 配置视频接收帧率

```objc
//默认为25fps
- (void)configReceiveFrameRate:(CGFloat ) frameRate
```

- ###### 配置视频发送帧率

```objc
- (void)configSendFrameRate:(CGFloat ) frameRate 
```

- ###### 配置发送、接收的视频分辨率

```objc
//960x540
- (void)configResolutionWidth:(NSInteger )width
                    andHeight:(NSInteger )heigh
```

- ###### 配置发送的视频分辨率

```objc
//960x540
- (void)configSendResolutionWidth:(NSInteger )width
                        andHeight:(NSInteger )height
```

- ###### 配置接收的视频分辨率

```objc
//960x540
- (void)configReceiveResolutionWidth:(NSInteger )width
                           andHeight:(NSInteger )height
```

