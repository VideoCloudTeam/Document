点对点音视频通话即两台设备之间实现音视频通话的功能,点对点呼叫包括呼叫方和被呼叫方,被呼方接收呼叫使用的是是基于VOIP协议实现的PushKit框架。

##### 被呼功能

移动端被呼功能包括`点对点被呼`，和`会议室邀请被呼`两种场景。 被呼功能的实现，首先需要在APP上通过接口进行账号登录，登录成功后该账号被呼叫时，服务器端会通过极光推送发送消息给APP，APP可根据收到的消息来建立通话，或调用接口进行拒接。

##### 具体实现

1. **登录**：要实现被呼功能首先要完成登录

   #### 验证账号密码接口

   **请求地址：** `https://{serverAddress}/api/v3/app/user/login/verify_user.shtml`

   **请求方式：** POST **请求参数：**

   | 参数类别        | 参数名称  | 类型   | 注释         | 说明                      |
   | --------------- | --------- | ------ | ------------ | ------------------------- |
   | 请求参数 (Body) | account   | String | 账号         | 登录的用户账号            |
   | 请求参数 (Body) | pwd       | String | 密码         | 登录的用户密码            |
   | 请求参数 (Body) | host      | String | 服务器地址   | 专属云需要传              |
   | 请求参数 (Body) | plat_type | String | 端类型       | webrtcios（专属云需要传） |
   | 请求参数 (Body) | login_url | String | 登录的全地址 |                           |

   ```objc
   {
   	"login_url": "https://{serverAddress}/api/v3/app/user/login/verify_user.shtml",
   	"account": "xxxx@xxxx.xxx",
   	"host": "服务器地址",
   	"pwd": "密码",
   	"plat_type": "webrtcios"
   }
   ```

   

   ** 响应参数：**

   results参数

   | 参数类别 | 参数名称   | 类型   | 注释      | 说明                  |
   | -------- | ---------- | ------ | --------- | --------------------- |
   | 响应参数 | id         | int    | 主键      | 用户主键编号          |
   | 响应参数 | account    | String | 账号      | 用户登录账号          |
   | 响应参数 | trueName   | String | 名字      | 用户真实姓名          |
   | 响应参数 | userName   | String | 用户地址  | 用户名@服务器地址形式 |
   | 响应参数 | sipkey     | String | 用户短号  | 用户短号，点对点呼叫  |
   | 响应参数 | session_id | String | sessionId | 专属云在线状态id      |
   | 响应参数 | companyId  | String | oemId     | 公司id                |

   示例：

   ```java
   {
       "code": "200",
       "timeStamp": "2018-02-09 15:25:17",
       "results": {
           "id": xxxx,
           "account": "xxxx@xxxx.com",
           "trueName": "xxxx",
           "userName": "xxxx@xxxx.com",
           "sipkey": "xxxx",
           "session_id": "xxxxxx",
           "companyId":xx
       }
   }
   ```

2. **注册接口**

   该API的作用: 登录后账号要注册到平台,用于点对点呼叫。

   **公有云请求地址：** `https://{mcuHost}/api/registrations/<account>/new_session` 

   **专属云请求地址：**`https://{serverAddress}/api/v3/app/registrations/jpush_token`

   account为登录账号,account的获取:

   如果该账号为`test@example.com`,test就是userName,example.com为domian

   account 字段为"(userName)%%40(domian)"即:"test%%40example.com"

   如果该账号为`test`

   account 字段为"test"

   **请求方式：** POST **请求参数：**

   | 参数类别          | 参数名称               | 类型   | 注释      | 说明                                              |
   | ----------------- | ---------------------- | ------ | --------- | ------------------------------------------------- |
   | 请求头部 (Header) | X-Cloud- Authorization | String | 认证信息  | 用户名和密码的Base64加密字符串； 可参考举例说明。 |
   | 请求头部 (Header) | Authorization          | String | 认证信息  | 用户名和密码的Base64加密字符串；                  |
   | 请求头部 (Header) | sessionId              | String | sessionId | 验证账号密码接口返回的sessionId                   |
   | 请求参数 (Body)   | device_id              | String | 设备id    | Voiptoken加密字段,具体加密方式参考下面            |
   | 请求参数 (Body)   | device_type            | String | 设备类型  | 必须填写 字段为ios                                |

   Header举例说明：

   账号`test@example.com`，账号密码`123456`。 用户名为test，密码为123456，编码格式：`用户名:密码` 将`test:123456`进行UTF8编码再进行Base64编码,假设编码后的字段为dGVzdDoxMjM0NTY 然后将已编码的字段和该字段`“x-cloud-basic`和`"="`进行拼接,成如下字段`x-cloud-basic ` + `编码后的字符串` + `=`

   也就是`x-cloud-basic dGVzdDoxMjM0NTY=`

   注意:在`x-cloud-basic`后有个空格符 

   最终认证信息为：

   | Header                | 消息体                         |
   | --------------------- | ------------------------------ |
   | X-Cloud-Authorization | x-cloud-basic dGVzdDoxMjM0NTY= |
   | Authorization         | x-cloud-basic dGVzdDoxMjM0NTY= |

   注意：专属云sessionId的有效时间为3分钟

   device_id 字段说明:

   ```objc
   //PushKit方法
   - (void)pushRegistry:(PKPushRegistry *)registry
   didUpdatePushCredentials:(PKPushCredentials *)credentials
                forType:(NSString *)type {
       NSString *credentialsToken = [[NSString stringWithFormat:@"%@",(credentials.token)]stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
       NSString *appVoiptoken = [credentialsToken stringByReplacingOccurrencesOfString:@" " withString:@""];
   }
   ```

   对VOIP推送的证书名进行md5,假设该证书名为:apns_exampleTest,对该字段进行md5加密

   然后appVoiptoken和所得的md5字符串进行如下拼接

   ```objc
   NSString *token_id = [NSString stringWithFormat:@"%@__%@",appVoiptoken,md5];
   ```

   ```objc
   //专属云httpBody处理方式
   dic = @{@"device_id" : token_id ,
           @"device_type" : @"ios"};
   NSData *data2 = [NSJSONSerialization dataWithJSONObject:dic options:NSJSONWritingPrettyPrinted error:nil];
   request.HTTPBody = data2 ;
   ```

   ```objc
   //公有云httpBody处理方式
   NSString *httpBody = [NSString stringWithFormat:@"device_id=%@&device_type=ios",token_id];
   request.HTTPBody = [httpBody dataUsingEncoding:NSUTF8StringEncoding];
   ```

##### 点对点呼叫

```objc
    //配置服务器域名
    self.vcrtc.apiServer = @"服务器域名";
    //遵循 VCRtcModuleDelegate方法
    self.vcrtc.delegate = self;
    self.vcrtc.groupId = @"AppleDevelop申请的groupId";
    //入会类型配置 点对点
    [self.vcrtc configConnectType:VCConnectTypeUser];
    //入会音视频质量配置
    [self.vcrtc configVideoProfile:VCVideoProfile480P];
    //入会接收流的方式配置
    [self.vcrtc configMultistream:YES];
    //用户账号配置(用户登录需配置,未登录不需要)
    [self.vcrtc configLoginAccount:@"登录账号"];
    //配置音视频 channel: 用户地址 password: 参会密码 name: 会中显示名称
    [self.vcrtc connectChannel:@"用户地址" password:@"填空字符串" name:@"会中显示昵称" success:^(id _Nonnull response) {
        
    } failure:^(NSError * _Nonnull error) {
        
    }];
```



#### 