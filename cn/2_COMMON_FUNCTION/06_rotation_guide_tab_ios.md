#### 视频采集旋转

1. 要监听设备的旋转方向（把手机控制中心的竖排方向锁定关闭），设备是什么方向，视频采集方向就设置设备旋转的方向。

```objc
@property(nonatomic, assign) UIDeviceOrientation forceOrientation;
/**
 UIDeviceOrientationUnknown,
    UIDeviceOrientationPortrait,            // Device oriented vertically, home button on the bottom
    UIDeviceOrientationPortraitUpsideDown,  // Device oriented vertically, home button on the top
    UIDeviceOrientationLandscapeLeft,       // Device oriented horizontally, home button on the right
    UIDeviceOrientationLandscapeRight,      // Device oriented horizontally, home button on the left
    UIDeviceOrientationFaceUp,              // Device oriented flat, face up
    UIDeviceOrientationFaceDown             // Device oriented flat, face down
*/
```

- 初始化

```objective-c
self.vcrtc.forceOrientation = [UIDevice currentDevice].orientation
```

- 监听屏幕旋转方向

```objc
[[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(change:) name:UIDeviceOrientationDidChangeNotification object:nil]; 

-(void)change:(NSNotification*)notification
{
    UIDevice *device = notification.object;
    switch (device.orientation) {
        case UIDeviceOrientationLandscapeRight:
            self.vcrtc.forceOrientation = UIDeviceOrientationLandscapeRight;
            break;
      case UIDeviceOrientationPortrait:
        self.vcrtc.forceOrientation = UIDeviceOrientationPortrait;
        break;
            case UIDeviceOrientationLandscapeLeft:
            self.vcrtc.forceOrientation = UIDeviceOrientationLandscapeLeft;
            break;
        default:
            break;
    }
}
```

- 在ViewController中设置允许屏幕旋转

```objective-c
- (BOOL)shouldAutorotate {
    return YES;
}

- (UIInterfaceOrientationMask)supportedInterfaceOrientations {
  //此地只设置了左右横屏 没有竖屏，有竖屏的需求设置竖屏模式
    return UIInterfaceOrientationMaskLandscapeRight |  UIInterfaceOrientationMaskLandscapeLeft;
}

- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation{
    //此地只设置了左右横屏 没有竖屏，有竖屏的需求设置竖屏模式
    return ([UIDevice currentDevice].orientation == UIDeviceOrientationLandscapeRight ? UIInterfaceOrientationLandscapeLeft : UIInterfaceOrientationLandscapeRight);
}
```

