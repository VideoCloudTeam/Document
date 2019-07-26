#### 查看通话质量

​	功能描述：本功能反应远端音视频质量以及本地音视频质量，以下是信息对照表

|     信息     |          描述          |
| :----------: | :--------------------: |
|     uuid     |    参会人的唯一标示    |
|  mediaType   |  类型（audio/video）   |
|  direcition  |  发送或接收（out/in）  |
|  resolution  |         分辨率         |
|    codec     |        编码格式        |
|  frameRate   |          帧率          |
|   bitrate    |          码率          |
|   packets    | 发送或接收数据包的数量 |
| packagesLost |        丢包数量        |
| fractionLost |         丢包率         |
|    jitter    |          抖动          |

1. 调用方法：vcrtc.getMediaStatistics()，返回为一个List

2. 事例代码：

3. ```java
   /**
   * 实体类，所有的信息都在这个类里，通过对类的解析，显示用户想要看到的信息
   **/
   public class MediaStats {
       private String uuid;
       private String mediaType;
       private String direction;
       private String resolution;
       private String codec;
       private int frameRate;
       private int bitrate;
       private int packets;
       private int packetsLost;
       private double fractionLost;
       private int jitter;
   }
   ```

```
List<MediaStats> stats = vcrtc.getMediaStatistics();

for (MediaStats mediaStats : stats) {
    if(mediaStats.getMediaType().equals("audio")
        &&mediaStats.getDirection().equals("out")) {
            ......
        }
    }
    if (mediaStats.getMediaType().equals("audio") 
        && mediaStats.getDirection().equals("in")) {
            ......
        }
    }
    if (mediaStats.getMediaType().equals("video") 
        && mediaStats.getDirection().equals("out")) {
        ......
    }
    if (mediaStats.getMediaType().equals("video") 
        && mediaStats.getDirection().equals("in")) {
        .....
    }
}
​```
```





#### 视频通话质量对照表（具体设置方法见SDK开发文档-常用API列表）

| 字段名称                               | 推荐值   | 描述                               | 推荐范围           |
| -------------------------------------- | -------- | ---------------------------------- | ------------------ |
| bandwidthUp                            | 800      | 上行带宽（大流）                   | 800~2048           |
| bandwidthDown                          | 800      | 下行带宽（全编全解模式下起作用）   | 800~2048           |
| bandwidthSmall                         | 120      | 上行带宽（小流）                   | 80~120             |
| bandwidthPresentation                  | 1200     | 双流辅流上行带宽                   | 800~1200           |
| video(Width/Height)Capture             | 1280/720 | 摄像头采集分辨率                   | 1280/720~1920/1080 |
| video(Width/Height)Up                  | 640/360  | 视频上行分辨率                     | 640/360~1920/1080  |
| video(Width/Height)Down                | 640/360  | 视频下行分辨率                     | 640/360~1920/1080  |
| video(Width/Height)Small               | 320/180  | 视屏上行小流分辨率                 | 320/180            |
| videoPresentation(Width/Height)Capture | 1280/720 | 双流辅流的采集分辨率               | 1280/720~1920/1080 |
| videoPresentation(Width/Height)Up      | 1280/720 | 双流辅流上行分辨率                 | 1280/720~1920/1080 |
| fpsCapture                             | 20       | 摄像头采集帧率                     | 20~30              |
| fpsUp                                  | 20       | 视频上行帧率                       | 20~30              |
| fpsDown                                | 20       | 视频下行帧率                       | 20~30              |
| fpsSmall                               | 15       | 小视频帧率                         | 15~20              |
| fpsMax                                 | 20       | 视频上行最大帧率                   | 20~30              |
| fpsPresentationCapture                 | 10       | 双流辅流视频采集帧率               | 5～15              |
| fpsPresentationUp                      | 10       | 双流辅流视频上行帧率               | 5～15              |
| fpsPresentationMax                     | 10       | 发送双流辅流视频最大上行帧率       | 15～20             |
| simulcast                              | true     | 就收多流（转发模式）               |                    |
| multistream                            | true     | 发送多流（一大一小两个上行视频流） |                    |
| enableH264HardwareEncoder              | true     | 使用H264硬编码                     |                    |

| disableH264hHardwareDecoder    | true    | 使用H264软解码