#### 被呼功能

##### 1. 简介

移动端被呼功能包括`点对点被呼`，和`会议室邀请被呼`两种场景。 被呼功能的实现，首先需要在APP上通过接口进行账号登录，登录成功后该账号被呼叫时，SDK会通过广播将被呼的消息推送给客户端，客户端区分是点对点模式还是会议模式，分开进行处理。

##### 2. 登录

主要为VCRegistrationUtil这个类进行操作，调用login方法

```java
VCRegistrationUtil.login(context, “账号”, “密码”);
```

传入上下文信息，用户名，密码，进行登录操作，并注册一个动态广播，监听登录的结果

```java
public class LoginReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        String message = intent.getStringExtra(VCSevice.MSG);
        switch (message) {
            case VCSevice.MSG_LOGIN_SUCCESS:
                Toast.makeText(LoginActivity.this, "登录成功", Toast.LENGTH_SHORT).show();
                finish();
                break;
            case VCSevice.MSG_LOGIN_FAILED:
                Toast.makeText(LoginActivity.this, "登录失败", Toast.LENGTH_SHORT).show();
                break;
            default:
        }
    }
}
```

##### 3.退出登录

```java
VCRegistrationUtil.logout(context);
```

##### 4. 注册静态广播接收器监听全局消息

public class MyPushReceiver extends BroadcastReceiver {

```java
private static final String TAG = "MyPushReceiver";

public static boolean isShowIncoming = false;

private Context context;

@Override
public void onReceive(Context context, Intent intent) {
    this.context = context;
    String msg = intent.getStringExtra(VCSevice.MSG);
    switch (msg) {
        case VCSevice.MSG_USER_INFO:
            String userJson = intent.getStringExtra(VCSevice.DATA_BROADCAST);
            Log.i(TAG, "用户信息" + userJson);
            break;
        case VCSevice.MSG_SESSION_ID:
            String sessionID = intent.getStringExtra(VCSevice.DATA_BROADCAST);
            Log.i(TAG, "sessionID:" + sessionID);
            break;
        case VCSevice.MSG_ONLINE_STATUS:
            boolean onlineStatus = intent.getBooleanExtra(VCSevice.DATA_BROADCAST, false);
            Log.i(TAG, "在线状态:" + onlineStatus);
            break;
        case VCSevice.MSG_LOGOUT:
            Log.i(TAG, "账号在别的端登录");
            break;
        case VCSevice.MSG_INCOMING:
            Log.i(TAG, "收到消息");
            IncomingCall incomingCall = (IncomingCall) intent.getSerializableExtra(VCSevice.DATA_BROADCAST);
            if (isInvalidCall(incomingCall)){
                Log.e(TAG, " isInvalidCall timeout 30 second ");
                return;
            }
            if (isInConference() || isShowIncoming){ //已经在通话中，直接挂掉；
                VCRegistrationUtil.hangup(context);
            } else {//没有在通话中，显示来电界面
                
            }
            break;
        case VCSevice.MSG_INCOMING_CANCELLED:
            Log.i(TAG, "呼叫端撤销了呼叫");
            finishIncomingView(context);
            break;
    }
}
}
```
