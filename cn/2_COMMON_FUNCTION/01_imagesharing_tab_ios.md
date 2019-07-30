#### 

1. 分享图片类型到会中

- 分享图片

```objc
//把UIImage转成NSData
[self.vcrtc shareImageData:imageData open:YES change:NO success:^(id  _Nonnull response) {

} failure:^(NSError * _Nonnull error) {

}];
```

- 分享多张图片时,更改当前显示的图片

这个地方不太好理解,举个例子吧,加入会议后,你一次性在会议中分享了多张图片,默认当前显示的是第一图片,你想查看第二张图片,同时也让会议中远端的其他人也看第二张图片,图片滑动后,调用下面的方法

```objc
//把UIImage转成NSData

[self.vcrtc shareImageData:imageData open:YES change:YES success:^(id _Nonnull response) {
} failure:^(NSError * _Nonnull error) {
}];

```

- 结束自己在会中分享

使用场景:当你在会中分享了几张图片,你想结束当前的分享

```objc 
[self.vcrtc shareImageData:[NSData data] open:NO change:NO success:^(id _Nonnull response) {
} failure:^(NSError * _Nonnull error) {
}];

```

2. 分享图片类型为视频流方式到会中

- 分享图片

```objc
//把UIImage转成NSData
[self.vcrtc shareToStreamImageData:imageData open:YES change: NO success:^(id  _Nonnull response) {

} failure:^(NSError * _Nonnull error) {

}];
```

- 分享多张图片时,更改当前显示的图片

```objc
//把UIImage转成NSData
[self.vcrtc shareToStreamImageData:imageData open:YES change:YES success:^(id _Nonnull response) {
} failure:^(NSError * _Nonnull error) {

}];
```

- 结束自己在会中分享

```objective-c
//把UIImage转成NSData
[self.vcrtc shareToStreamImageData:imageData open:NO change:NO success:^(id _Nonnull response) {
} failure:^(NSError * _Nonnull error) {

}];
```

3. 会中有参会者发起分享

```objc
//shareUuid 发起分享的人的唯一标识符

-(void)VCRtc:(VCRtcModule *)module didStartImage:(NSString *)shareUuid{}
```

4. 接收远端其他人的图片分享

```objc
//imageStr 图片链接  uuid:发起分享的人的唯一标识符

- (void)VCRtc:(VCRtcModule *)module didUpdateImage:(NSString *)imageStr uuid:(NSString *)uuid {}
```

5. 接收远端其他人的视频分享

```objc
//imageStr视频链接 uuid:发起分享的人的唯一标识符
- (void)VCRtc:(VCRtcModule *)module didUpdateVideo:(NSString *)imageStr uuid:(NSString *)uuid{}
```

6. 远端和本端分享停止

```objc
//imageStr: 分享内容 
- (void)VCRtc:(VCRtcModule *)module didStopImage:(NSString *)imageStr{}
```

