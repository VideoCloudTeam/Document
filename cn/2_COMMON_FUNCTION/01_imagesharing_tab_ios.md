<<<<<<< HEAD
### 以图片方式分享

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

### 以视频流方式分享

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

=======
### 共享图片

功能描述：在视频通话中将自己的图片文件分享给其他的参会人，以此提高沟通效率。

#### 以图片方式共享

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

#### 以流方式共享

- 共享图片

```objc
//把UIImage转成NSData
[self.vcrtc shareToStreamImageData:imageData open:YES change: NO success:^(id  _Nonnull response) {

} failure:^(NSError * _Nonnull error) {

}];
```

- 共享多张图片时,更改当前显示的图片

```objc
//把UIImage转成NSData
[self.vcrtc shareToStreamImageData:imageData open:YES change:YES success:^(id _Nonnull response) {
} failure:^(NSError * _Nonnull error) {

}];
```

- 结束自己在会中共享

```objective-c
//把UIImage转成NSData
[self.vcrtc shareToStreamImageData:imageData open:NO change:NO success:^(id _Nonnull response) {
} failure:^(NSError * _Nonnull error) {

}];
```

3. 会中有参会者发起共享

```objc
//shareUuid 发起分享的人的唯一标识符

-(void)VCRtc:(VCRtcModule *)module didStartImage:(NSString *)shareUuid{}
```

4. 接收远端其他人的图片共享

```objc
//imageStr 图片链接  uuid:发起分享的人的唯一标识符

- (void)VCRtc:(VCRtcModule *)module didUpdateImage:(NSString *)imageStr uuid:(NSString *)uuid {}
```

5. 接收远端其他人的视频共享

```objc
//imageStr视频链接 uuid:发起分享的人的唯一标识符
- (void)VCRtc:(VCRtcModule *)module didUpdateVideo:(NSString *)imageStr uuid:(NSString *)uuid{}
```

6. 停止共享

```objc
//imageStr: 分享内容 
- (void)VCRtc:(VCRtcModule *)module didStopImage:(NSString *)imageStr{}
```

### 共享PDF

共享PDF功能和共享图片功能类似，主要都是图片的共享，需要将PDF转化成图片资源文件再发起共享。
>>>>>>> c96442e6bba8d4c1722467fc5ca49c9cc79efbec
