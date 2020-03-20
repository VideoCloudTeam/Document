####  被呼功能

##### 1. 简介

移动端被呼功能包括`点对点被呼`，和`会议室邀请被呼`两种场景。 被呼功能的实现，首先需要在APP上通过接口进行账号登录，登录成功后该账号被呼叫时，服务器端会通过推送发送消息给APP，APP可根据收到的消息来建立通话，或调用接口进行拒接。

iOS 13.0以前使用的是`VoIP Push Notification` iOS 13.0以后由于苹果的要求用的是普通推送。

##### 2.登录

```objective-c
    [self.vcrtc loginWithAccount:@"登陆账号" password:@"登录密码" voipToken:@"推送devicetoken" success:^(id  _Nonnull response) {
        NSLog(@"======%@",response);
    } failure:^(NSError * _Nonnull error) {
        
    }];
```

##### 3.退出登录

```objective-c
    [self.vcrtc logoutAccountSuccess:^(id  _Nonnull response) {
    } failure:^(NSError * _Nonnull error) {
        
    }];
```

