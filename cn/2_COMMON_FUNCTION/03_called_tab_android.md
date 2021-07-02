#### 被呼功能

##### 1. 简介

移动端被呼功能包括`点对点被呼`，和`会议室邀请被呼`两种场景。 被呼功能的实现，首先需要在APP上通过接口进行账号登录，登录成功后该账号被呼叫时，SDK会通过广播将被呼的消息推送给客户端，客户端区分是点对点模式还是会议模式，分开进行处理。

##### 2. 登录

主要为VCRegistrationUtil这个类进行操作，调用login方法

```java

/**
* platType 传""/webrtcand即可
	account 账号
	password 密码
*/
VCRegistrationUtil.login(Context context,String platType, String account, String password);
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
                break;
            case VCSevice.MSG_LOGIN_FAILED:
                Toast.makeText(LoginActivity.this, "登录失败", Toast.LENGTH_SHORT).show();
                break;
            case VCService.MSG_USER_INFO:
            		// 返回等候成功后的用户信息
                String userJson = intent.getStringExtra(VCService.DATA_BROADCAST);
            default:
        }
    }
}
```

##### 3.退出登录

```java
VCRegistrationUtil.logout(Context context);
```

##### 4. 注册动态广播接收器监听全局消息



```java

public class ContentReceiver extends BroadcastReceiver {
	  // 挂断原因 1:在会中/已经被呼 2: 主动挂断 3: 超时挂断
		public final static int BUSY = 1;
    public final static int HANG_UP = 2;
  	// 专属云独有 原因3
    public final static int OUT_TIME = 3;
    @Override
    public void onReceive(Context context, Intent intent) {
        String message = intent.getStringExtra(MSG);
        final String TAG = "ContentReceiver";
        switch (message){
            case VCService.MSG_LOGIN_SUCCESS:
                Log.i(TAG, "登录成功");
                break;
            case VCService.MSG_LOGIN_FAILED:
                String reason = intent.getStringExtra(VCService.DATA_BROADCAST);
                Log.i(TAG, "登录失败" + reason);
                break;
            case VCService.MSG_USER_INFO:
                String userJson = intent.getStringExtra(VCService.DATA_BROADCAST);
                Log.i(TAG, "用户信息" + userJson);
                break;
            case VCService.MSG_SESSION_ID:
                String sessionID = intent.getStringExtra(VCService.DATA_BROADCAST);
                Log.i(TAG, "sessionID:" + sessionID);
                break;
            case VCService.MSG_ONLINE_STATUS:
                boolean onLine = intent.getBooleanExtra(VCService.DATA_BROADCAST, false);
                Log.i(TAG, "在线状态:" + onLine);
                break;
            case VCService.MSG_LOGOUT:
                Log.i(TAG, "账号在别的端登录");
                break;
            case VCService.MSG_INCOMING:
                IncomingCall incomingCall = (IncomingCall) intent.getSerializableExtra(VCService.DATA_BROADCAST);
                //已经在通话中，直接挂掉
                if (isInConference(context)) {
                    VCRegistrationUtil.hangup(context, BUSY);
                    break;
                }
                //显示来电界面，需要自己开发界面，并把incomingCall传过去展示相应信息
                //在来电界面，单机接听的话可以执行下面代码
//                joinConference(context, incomingCall);
                // 如果是拒绝，需要执行hangup方法
                break;
            case VCService.MSG_INCOMING_CANCELLED:
                // 公有云会收到对方取消通话的消息
                // 还没接听的情况下，收到此消息把来电界面关闭
                break;
            default:
        }
    }

    //判断是否正在开会
    public boolean isInConference(Context context) {
        ActivityManager am = (ActivityManager) context.getSystemService(ACTIVITY_SERVICE);
        ComponentName cn = am.getRunningTasks(1).get(0).topActivity;
        return cn.getClassName().equals(ZJConferenceActivity.class.getName());
    }

    private void joinConference(Context context, IncomingCall incomingCall) {
        Call call = new Call();
//        call.setAccount("");//登录的账号
        call.setChannel(incomingCall.getChannel());
        call.setMsgJson(incomingCall.getMsgJson());
        call.setNickname("我的入会名称");
        Intent intent = new Intent(context, ZJConferenceActivity.class);
        intent.putExtra("call", call);
        context.startActivity(intent);
    }
}

```

