#### 视频采集旋转

1. 固定画面的横竖屏，不允许动态切换，摄像头会根据当前是横屏或者是竖屏来默认采集视频信息，但是如果发送到对方时，如果当前本地时竖屏，对方时横屏，并且并没有配置VCRTCView的setObjectFit（"cover"）方法，视频信息在对方现实的时候不会被填充，而是会出现黑边，配置了这个方法，会将视频截取，然后填充满整个画面。
2. 竖屏画面可以设置本地视频采集的分辨率为720x1280，通过VCRTCPreferences的setCaptureVideoSize(int width, int height),
3. 横屏画面可以通过设置本地视频采集的分辨率1280x720，如果本地是竖屏，想发送对方是横屏，可以通过VCRTCPreferences.setVideoSize的方法来控制发送的视频分片率，在通过VCRTCView的setObjectFit方法进行显示的设置，是屏幕截取还是完全填充可以自由设定。