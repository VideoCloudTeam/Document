#### 打印log

设置参数，可以在自定义的Application中初始化log功能，打印log日志



```java
VCRTCPreferences prefs = new VCRTCPreferences(this);
prefs.setPrintLogs(true);
```

开启打印日志功能

1. 打印普通日志，存放位置是app包名下的com.包名AppLogs文件夹下。
2. 打印入会日志，存放位置是com.包名ConferenceLog下，可以保存十次入会信息。

