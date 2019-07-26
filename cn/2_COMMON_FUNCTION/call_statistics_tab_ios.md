- 通话质量相关字段说明

|     uuid     |    参会人的唯一标识    |
| :----------: | :--------------------: |
|  mediaType   |   类型(audio/video)    |
|  direction   |  发送接收(send/recv)   |
|    codec     |        编码格式        |
|  resolution  |         分辨率         |
|  frameRate   |          帧率          |
|   bitrate    |          码率          |
|   packets    | 发送或接收数据包的数量 |
| packagesLost |        丢包数量        |
|  resolution  |         分辨率         |
| fractionLost |         丢包率         |
|    jitter    |          抖动          |

```objc
@interface VCMediaStat : NSObject <NSCoding, NSCopying>
@property (nonatomic, strong) NSString *uuid;
@property (nonatomic, strong) NSString *mediaType;
@property (nonatomic, strong) NSString *direction;
@property (nonatomic, strong) NSString *codec;
@property (nonatomic, assign) NSInteger bitrate; 
@property (nonatomic, assign) NSInteger packets;
@property (nonatomic, assign) NSInteger packetsLost;
@property (nonatomic, assign) unsigned long ssrc;
@property (nonatomic, strong) NSString *resolution; 
@property (nonatomic, assign) NSInteger frameRate;
@property (nonatomic, assign) double timestamp;
@property (nonatomic, assign) double percentageLost; 
@property (nonatomic, assign) double jitter ;
@end
```

- 获取通话质量方法

```objc
- (void)VCRtc:(VCRtcModule *)module didReceivedStatistics:(NSArray<VCMediaStat *> *)mediaStats
```

#### 