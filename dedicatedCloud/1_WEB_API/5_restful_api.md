# 简介

本文档描述的是会中管理的REST API。可以通过这个API实现对云平台上正在召开的会议或讲堂实现外呼、踢人、静音、锁会、Layout、录播等灵活的会中管理。

# API的使用

## 验证用户信息并获取会议服务器地址

该接口用于验证用户入会信息是否合法,返回验证成功后的入会信息,其中mcuHost为目标会议节点地址,后续会议请求请使用该地址。
请求地址:https://apiServer/api/v3/meet/checkJoin.shtml
请求方式:POST
请求参数:

| **参数类别**    | **参数名称** | 类型                                        | **说明**          | 长度 | 是否必填 | **备注说明** |
| ---------------| ------------ | ------------------------------------------ | ---------------- | ---- | -------- | ------------ |
| 请求参数(Body)  | joinAccount     | varchar      | 入会短号/通讯地址                              | 60 | 是   |          |
| 请求参数(Body)  | joinPwd         | varchar      | 入会认证密码,<br />点对点呼叫时,此参数无效。    | 15 | 否   |           |
| 请求参数(Body)  | participantName | varchar      | 入会后显示姓名                                 | 50 | 是   |          ||

响应参数:

| **参数名称**    | **类型** | **说明**                                                     | **长度** |
| --------------- | -------- | ------------------------------------------------------------ | -------- |
| isDot           | Boolean  | 是否是点对点呼叫:true、false                                 |          |
| sipkey          | Varchar  | 会议室/用户入网/终端入网短号                                 |          |
| alias           | Varchar  | 通讯地址,格式为email格式                                     |          |
| password        | Varchar  | 认证密码,点对点呼叫时,此参数默认为空字符.                   |          |
| participantName | Varchar  | 参会者显示名称                                               |          |
| roleType        | Varchar  | 参会者角色类型:<br />主持人(host)、访客(guest),点对点呼叫时,此参数默认为空字符. |          |
| mcuHost         | Varchar  | 资源池请求地址                                               |          |
| companyId       | Int      | 所属企业ID                                                   |          |
| isRecord        | Boolean  | 是否开通录制权限,点对点呼叫时,此参数默认为false             |          |
| isLive          | Boolean  | 是否开通直播权限,点对点呼叫时,此参数默认为false             |          |
| conferenceName  | Varchar  | 会议室/用户姓名/终端登记名称                                 |          |
| allowGuest      | Varchar  | 是否允许访客,允许(yes),不允许(no),默认为yes                  |          ||

正确响应数据报格式:
```json
{
    "code": "200",
    "timeStamp": "2017-05-24 15:41:30",
    "results": {
        "isDot": xxxx,
        "sipkey": "xxxx",
        "alias": "xxx@xxx.com",
        "password": "xxxx",
        "participantName": "xxxx",
        "roleType": "xxxx",
        "mcuHost": "xxx.xxx.cn",
        "companyId": xxx,
        "isRecord": xxxx,
        "isLive": xxxx
    }
}
```
# REST API接口地址

## API命令调用的前缀规则
https://mcuHost/api/services/<会议室>/
其中会议室是召开视频会议所在会议室的地址或id。云平台为每个会议室分配一个email格式全局唯一的地址,便于记忆;同时为每个会议室分配一个全局唯一的全数字的会议号id便于终端或手机通过遥控器/键盘输入。
>例如:https://mcuHost/api/services/1234/new_session 或 https://mcuHost/api/services/abc@email.cn/new_session

## REST API 的认证
除了最初用于申请令牌(token)的new_session命令,客户端所发起的所有其他请求都需要将申请到的token作为认证凭据,以获得调用该请求的权限。令牌通过HTTP Header中的关键字token提交,如:
>token : xxxxxxxxxxxxxxxxxxxxxxxxxx
token有效期为两分钟。在失效之前,需要调用刷新命令refresh_session获取新的token进行后续通讯。

## REST API 的数据格式
除非另有规定,所有REST请求和应答的数据格式都是类型为application/json 的JSON对象。应答由"status"和"result"两个字段组成

>status:接口调用状态,成功或失败。<br>
result :返回的结果

# REST API详述 
会中管理API按功能大致分为:访问控制、会议控制、参会者控制以及云平台发送给控制端的各类事件。

## 访问控制API
请求的REST URL格式:
https://mcuHost/api/services/<会议室地址或id>/<请求>

| ***请求命令***    | **GET/POST** |           **说明**             |
| --------------- | -------- | ----------------------------------  |
| new_session     | POST     | 从云平台获取通讯用的令牌（token）        |
| refresh_session | POST     | 刷新并获取新的通讯令牌（token）          |
| end_session     | POST     | 释放通讯令牌,有效断开与会议室的连接       |

### new_session命令
本命令用于从云平台获取通讯用的令牌。

#### 请求消息体参数
| ***参数***    | **Value示例** |           **说明**             |
| --------------- | -------- | ----------------------------------  |
| display_name     | Jiang     | 参会者名字        |
| hideme           | "yes"     | "yes":隐藏与会者;"no":不隐藏;<br/>不在与会者列表中显示当前参会者。|

#### 请求消息体实例
```json
{
    "display_name": "jiang", 
    "hideme":  "no"
}
```
#### 请求头部参数
new_session除了传递的json格式的消息体以外,还需在请求头部包含以下几个信息:

| **Key** | **Value示例** | **说明**                                 |
| ---- | ------------- | ---------------------------------------- |
| pin  | 123456789        | 如果设置了入会密码,这是主持人或访客密码 |

#### 应答格式及示例
```json
{
    "status": "success",
    "result": {
        "token": "SE9TVAltZ...etc...zNiZjlmNjFhMTlmMTJiYTE%3D",
        "expires": "120",
        "participant_uuid": "2c34f35f-1060-438c-9e87-6c2dffbc9980",
        "display_name": "Alice",
        "stun": [{
            "url": "stun:adc.bdc.cn"
        }],
        "analytics_enabled": true,
        "version": {
            "pseudo_version": "25010.0.0",
            "version_id": "12"
        },
        "role": "HOST",
        "service_type": "conference",
        "chat_enabled": true,
        "current_service_type": "conference"
    }
}
```
上例应答中包含令牌（token）,用于后继通讯;
expires是token的有效时间120,单位为秒,在两分钟内必须通过refresh_session重新获取通讯令牌。
以下是详细的应答字段列表:

| **应答字段名**     | **含义**  |
| ----| ----------   |
| token       | 用于后继通讯认证使用的令牌（token）             |
|expires    |令牌的有效时间,单位为秒;失效前必须调用refresh_session更新令牌    |
|participant_uuid   |在云平台新生成的唯一标识本参会者的uuid         |
|version    |连接到的会议节点版本号         |
|role       |本参会者的入会身份。 "HOST":主持人;"GUEST":访客  |
|chat_enabled   |会中是否允许文字聊天。true:允许;false:不允许     |
|service_type   |本次呼叫的服务类型。"conference":多点的会议服务;"gateway":点到点呼叫|
|stun           |云平台指定的WebRTC/SIP通讯防火墙穿透判断使用的stun服务器               |
|display_name   |参会者在会议中的名字                                                   |
|analytics_enabled  |是否自动发送音视频通讯统计信息                     |
|current_service_type   |当前已连接上的服务。‘conference’:会议服务中;‘gateway’:点到点通讯中;’waiting_room’: 锁会了,正在等待主持人授权;’ivr’:等待输入pin    |

### refresh_session命令
本命令用于从云平台更新通讯用的令牌。

#### 请求消息体参数
无

#### 请求头部参数
refresh_session需要在头部传递token用于认证。

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 请求头部参数
```
{
    "status": "success",
    "result": {
        "token": "SE9TVAltZ...etc...jQ4YTVmMzM3MDMwNDFlNjI%3D",
        "expires": "120"
    }
}
```
应答中包含令牌（token），用于后继通讯； expires是新token的有效时间，单位为秒

| **应答字段**     | **含义**  |
| ----| ----------   |
| token | 用于后继通讯的新令牌     |
| expires | 新令牌的有效时间     |

### end_session命令
本命令用于释放在云平台获取的令牌，在断开与云平台连接时调用。

#### 请求消息体参数
无

#### 请求头部参数
end_session需要在头部传递token用于认证。

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
应忽略所有应答

## 会议控制API
请求的REST URL格式:<br/>
https://mcuHost/api/services/<会议室地址或id>/<请求>

| **请求**     | **GET/POST**  | **描述**  |
| ----| ----------   |-----------------|
| dial |   POST   | 外呼，将终端/用户加入会议   |
|service_status |GET    |获取会议室状态     |
|lock/unlock    |POST   |锁会/解锁。锁会后，新参会者只有主持人才能入会  |
|start_service  |POST   |开启会议，并让等待中的所有访客入会         |
|muteguests     |POST   |静音会议中的所有访客                       |
|unmuteguests   |POST   |取消静音，允许访客发言                      |
|disconnect     |POST   |将包括调用者自己在内的所有与会者踢出会议室     |
|message        |POST   |向会议中的所有与会者发送文字信息               |
|participants   |GET    |返回会议中的全部参会者列表                     |

### dial命令
该命令用于将某个终端或用户呼入本会议，支持SIP、H323、RTMP、Phone等呼叫。只有会议主持人才能使用本命令。

#### 请求消息体实例
```
{
    "role": "guest",
    "destination": "bob@example.com",
    "protocol": "sip",
    "source_display_name": "Alice"
}
```

#### 请求消息体参数
| **请求字段名**     | **含义** |
| ----| ----------   |
| role          |入会角色。"guest"：访客；"chair"：主持人   |
|destination |拟呼入终端的地址或短号、手机号    |
|protocol |呼叫使用的协议: "sip", "h323", "rtmp", "mssip" (lync), "phone"(普通电话)；或者 "auto" (适用于内部终端或用户)    |
| streaming           | 适用于RTMP呼叫。是否将RTMP呼叫作为流媒体呼叫。缺省值为"no".RTMP呼叫请设置为"yes". |
| dtmf_sequence       | （可选）呼叫连接建立后立即向目标终端发送的DTMF序列           |
| source_display_name | （可选）在被呼终端看到的会议室名字                           |
| source              | （可选）指定呼叫时使用的会议室地址或短号                     |
| call_type           | （可选）呼叫使用的音视频能力，缺省为video<br/> - "video": 主流+辅流，即双流<br/> - "video-only": 只有主流<br/> - "audio": audio-only<br/> |

#### 请求头部参数
需要在头部传递token用于认证。

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": ["977fcd1c-8e3c-4dcf-af45-e536b77af088"]
}
```
如果外呼初始化成功，返回值是云平台生成的参会者uuids列表，在绝大部分情况下，一次外呼只会产生一个新的参会者uuid,除否云平台上配置了fork呼叫。新生成的参会者会立即在参会者列表中出现,service_type为"connecting",呼叫建立成功，参会者的service_type将变为正常的"conference"；如果被呼终端拒绝了本呼叫或者30秒内没有收到任何应答，云平台将从参会者列表中删除该准参会者。

### service_status命令
该GET命令用于获取会议状态。当前，云平台只提供会议是否锁定和访客入会缺省是否被静音的信息。

#### 请求消息体实例
无。这是一个GET请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "tatus": "uccess",
    "result": {
        "guests_muted": false,
        "locked": false
    }
}
```
| **应答字段**     | **含义**  |
| ----| ----------   |
| guests_muted | true/false:访客是否被静音     |
| Locked | true/false:本会议是否被锁了   |

<!-- lock/unlock命令 -->

### lock/unlock命令
这两个POST命令由主持人用于锁会或解锁会议。当会议被锁后，新的访客参会者入会时将先看到一个"等待主持人"的画面，直至主持人授权入会。

#### 请求消息体实例
无。这是一个POST请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- start_service命令 -->
### start_service命令
只有有主持人身份的参会者入会时会议才能开始，否则所有入会的访客只能处于"等待主持人入会"状态。该命令可以让以"会议控制"（没有音视频通讯）身份入会的主持人开启会议，让所有等待的访客入会，无需其他有音视频通讯的主持人入会即可开会。

#### 请求消息体实例
无。这是一个POST请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- participants命令 -->
### participants命令
该GET命令用于获取参会者列表。当前，云平台只提供会议是否锁定和访客入会缺省是否被静音的信息。请参考partcipant_create事件获取更多参会者详情。

#### 请求消息体实例
无。这是一个GET请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": […]
}
```
返回参会者列表，每个参会者的详情请请参考partcipant_create事件。

<!-- Override_layout命令 -->
### override_layout命令
该override_layout命令用于控制主持人或访客布局。命令的格式举例。

#### 请求消息体实例
{‘layouts’:[layout1, layout2]}，这是一个POST请求。
```
{
    "layouts": [{
            "audience": "hosts",
            "actors": [],
            "vad_backfill": true,
            "layout": "2:21",
            "plus_n": "auto",
            "actors_overlay_text": "auto"
        },
        {
            "audience": "",
            "actors": [],
            "vad_backfill": true,
            "layout": "2:21",
            "plus_n": "auto",
            "actors_overlay_text": "auto"
        }
    ]
}
```

#### 请求消息体参数
| **Key**     | **Value示例**  |
| ----| ----------   |
| audience | "hosts"：主持人; "":访客。     |
| Actors | "[uuids]"<br />公有云场景：指定屏幕上需要显示的与会者;[]表示所有人均可以显示在屏幕上。<br />专属云场景：<br />1、设置自动轮训填'-2'<br />2、设置显示演讲人'1010'<br />3、设置自动指定'0'<br />4、设置指定与会者'$uuid'，指定屏幕上需要显示的与会者 |
| Vad_backfill | "true/false":actors数组中指定的与会将按照actors顺序优先显示在屏幕上，当个别与会者退出时，屏幕上将产生空位。如果屏幕上有空位，剩余与会者是否根据其语音激励顺序填满空位。     |
| Layout | 公有云支持的布局类型："1:0"、"4:0"、"1:7"、"1:21"、"2:21"<br />专属云支持的布局类型："1:0"、"2:0"、"3:0"、"4:0"、"1:5"、"1:7"、"1:12" |
| plus_n | "off":actors不为空时;"auto":其它情况     |
| actors_overlay_text | 设置为"auto"    |
#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": […]
}
```
返回参会者列表，每个参会者的详情请请参考partcipant_create事件。

<!-- 参会者API -->
## 参会者API
参会者API命令的REST URL格式:<br/>
https://mcuHost/api/services/<会议室地址或id>/participants/<participant_uuid>/<request>
<br/>
participant_uuid是云平台中某参会者在会议中的的内部唯一标识，由云平台生成并维护，会议控制API中的参会者列表请求可以获得每个参会者的uuid。<br/>

| **Request**     | **GET/POST**  | **Description**  |
| ----| ----------   |-----------------|
| disconnect |POST      | 将指定参会者踢出会议   |
| mute/unmute |POST      | 静音该参会者/取消静音   |
| allowrxpresentation |POST      | 允许该参会者接受辅流(演示)   |
| denyrxpresentation |POST      | 禁止该参会者接受辅流（演示）   |
| spotlighton |POST      | 允许该参会者获取“焦点”   |
| spotlightoff |POST      | 禁止该参会者获取“焦点”   |
| unlock |POST      | 将该访客放行加入已锁定的会议中   |
| dtmf |POST      | 向该与会者发送DTMF数字序列   |
| calls |POST      | 给该WEBRTC/RTMP参会者新增音视频信道(子呼叫)   |
| role |POST      | 改变该参会者的角色   |
| transfer |POST      | 将该参会者转到另一个会议   |

<!-- disconnect命令 -->
### disconnect命令
该命令由主持人用于将指定的与会者踢出会议。

#### 请求消息体实例
无。这是一个POST请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- mute/unmute命令 -->
### mute/unmute命令
该命令由主持人用于将指定的与会者静音/取消其静音。

#### 请求消息体实例
无。这是一个POST请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- allowrxpresentation / denyrxpresentation命令 -->
### allowrxpresentation / denyrxpresentation命令
这两个命令由主持人用于控制指定参会者能否接受辅流(演示)，缺省为允许;allowrxpresentation：允许接受辅流；denyrxpresentation：禁止接受辅流。
#### 请求消息体实例
无。这是一个POST请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- spotlighton / spotlightoff命令 -->
### spotlighton / spotlightoff命令
这两个命令由主持人用于将某个参会者变成“焦点”/”去焦”，从而影响会议视频布局中的参会者及其位置。缺省是基于语音激励自动调整视频布局中的参会者及其图像位置。Spotlight特性可以将“焦点“参会者锁定在视频布局的主要位置。当有多个”焦点“的参会者时，第一个被”焦点”的参会者将拥有主讲者的位置，第二个被”焦点”的参会者在第二重要的位置：1+7布局7中最左边的，四等分屏布局中右上的，依次类推。 

#### 请求消息体实例
无。这是一个POST请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- unlock命令 -->
### unlock命令
这命令由主持人用于将指定参会者放进已锁定的会议中 

#### 请求消息体实例
无。这是一个POST请求

#### 请求消息体参数
无

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- dtmf命令 -->
### spotlighton / spotlightoff命令
这命令由主持人用于向指定的参会者发送dtmf数字序列

#### 请求消息体实例
```
{
    "digits": "1234"
}
```

#### 请求消息体参数
| **请求字段**     | **含义**  |
| ----| ----------   |
| Digits |拟发送的dtmf数字序列      |

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- role命令 -->
### role命令
该命令由主持人用于改变指定参会者的角色。

#### 请求消息体实例
```
{
    "role": "chair"
}
```

#### 请求消息体参数
| **请求字段**     | **含义**  |
| ----| ----------   |
| role |chair:主持人；guest:访客      |

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- transfer命令 -->
### transfer命令
该命令由主持人用于将指定参会者从当前会议转接到另一个会议室。

#### 请求消息体实例
```
{
    "role": "guest",
    "conference_alias": "meet@example.com",
    "pin": "1234"
}
```

#### 请求消息体参数
| **请求字段**     | **含义**  |
| ----| ----------   |
| role |chair:主持人；guest:访客      |
| conference_alias |目标会议室地址或短号      |
| pin |目标会议室对应角色的入会密码      |

#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
```
{
    "status": "success",
    "result": true
}
```
操作成功，result返回true，否则返回false

<!-- calls命令 -->
### calls命令
该命令由主持人用于增加指定参会者的音视频通讯信道（子呼叫）。该命令作用于WebRTC和RTMP参会者，并各自有不同的请求参数以及应答。

#### 请求消息体实例
```
{
    "call_type": "WEBRTC",
    "sdp": "…"
}
```

#### 请求消息体参数
针对WebRTC
| **请求字段**     | **含义**  | **含义**  |
| ----| ----------   | ----------------  |
| call_type |呼叫类型：WebRTC或RTMP      |  |
| sdp |sdp格式的音视频通讯请求握手信息      |webRTC |
| present |可选。Send或receive本次呼叫增加的是辅流，而不是主流      |RTMP   |
| streaming |true/false，是否将本次呼叫作为流媒体处理      |RTMP |
| Bandwidth |可选项。用于限定本次新增信道的最大带宽      |RTMP |


#### 请求头部参数
需要在头部传递token用于认证

| **Key**     | **Value示例**  | **说明**  |
| ----| ----------   |-----------------|
| token |      | 当前通讯正在使用的认证用有效令牌（token）   |

#### 应答格式及示例
##### WebRTC呼叫:
```
{
    "status": "success",
    "result": {
        "call_uuid": "50ed679d-c622-4c0e-b251-e217f2aa030b",
        "sdp": "…"
    }
}
```
在收到包含了sdp格式的握手应答信号后，必须给call_uuid代表的呼叫发送ack后才能启动音视频媒体通讯。

| **应答字段**     | **Value示例**  | **说明**  |
| -------| ----------   |-----------------   |
| call_uuid |50ed679d-c622-4c0e-b251-e217f2aa030b | 用于唯一标识本次呼叫的ID，可用于后继控制该呼叫   |
| sdp       |.....| sdp格式的音视频通讯握手应答信息  |

##### RTMP呼叫:
```
{
    "status": "success",
    "result": {
        "call_uuid": "50ed679d-c622-4c0e-b251-e217f2aa030b",
        "url": "rtmp://10.0.0.1:40002/example/50ed679d-c622-4c0e-b251-e217f2aa030b",
        "secure_url": "rtmps://hostname.mcuHost:40003/example/50ed679d-c622-4c0e-b251-e217f2aa030b"
    }
}
```
除了唯一标识本次呼叫的call_uuid ,应答中包含RTMP通讯的URL链接：一个是普通的RTMP链接，一个是安全加密的RTMPS链接。

| **应答字段**     | **Value示例**  | **说明**  |
| -------| ----------   |-----------------   |
| call_uuid |50ed679d-c622-4c0e-b251-e217f2aa030b | 用于唯一标识本次呼叫的ID，可用于后继控制该呼叫  |
| url       |rtmp://live.abc.cn/example/50ed679d-c622-4c0e-b251-e217f2aa030b| sdp格式的音视频通讯握手应答信息  |
| secure_url |rtmps://live.abc.cn/example/50ed679d-c622-4c0e-b251-e217f2aa030b| 进行加密音视频通讯的RTMPS URL  |

<!-- 云平台发送的事件 -->
## 云平台发送的事件
使用以下GET命令“订阅”云平台该会议相关的事件:<br/>

https://mcuHost/api/services/<会议地址或id>/events?token=<token_id><br/>

下表是云平台可能发送的与本次会议相关的所有事件列表。

| **事件**     | **描述**  |
| -------------   | ------------   |
|presentation_start     |有新的演示(辅流)，消息体中包含辅流的相关描述信息    |
|presentation_stop     |演示 (辅流)结束    |
|presentation_frame     |有新的演示（辅流）帧    |
|participant_create     |有新的参会者加入了会议    |
|participant_update     |参会者属性有变    |
|participant_delete     |有参会者离开了会议    |
|participant_sync_begin     |用于同步参会者列表。开始同步参会者列表    |
|participant_sync_end     |用于同步参会者列表。参会者列表同步结束    |
|service_update     |会议属性有更新    |
|layout     |视频布局有更新    |
|Message    |有新的（聊天）文字消息     |
|Stage      |讲话者有变，返回按“讲话”顺序排列的参会者列表   |
|call_disconnected  |有参会者的子通讯被断开及其原因:比如结束屏幕共享    |
|Disconnect     |有参会者被云平台“踢出“了会议       |

<!-- presentation_start -->
### presentation_start
标志新的演示开始，并包含演示者的相关信息。

#### 消息体实例
```
{
    "presenter_name": "Bob",
    "presenter_uri": "bob@example.com"
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------| ----------   |
| presenter_name |演示者姓名 |
| presenter_uri       |演示者地址   |

<!-- presentation_stop -->
### presentation_stop
标志演示结束。

#### 消息体实例
无

#### 消息体字段
无

<!-- presentation_frame -->
### presentation_frame
有新的演示帧产生，可访问以下链接获取:<br/>
https://mcuHost/api/services/<会议室地址或短号>/presentation.jpeg


#### 消息体实例
无

#### 消息体字段
无

<!-- participant_create -->
### participant_create
有新的参会者加入了会议。

#### 消息体实例
```
{
    "api_url": "/participants/50b956c8-9a63-4711-8630-3810f8666b04",
    "call_direction": "in",
    "display_name": "Alice",
    "encryption": "On",
    "has_media": false,
    "is_audio_only_call": "NO",
    "is_external": false,
    "is_muted": "NO",
    "is_presenting": "NO",
    "is_streaming_conference": false,
    "is_video_call": "YES",
    "local_alias": "meet.alice",
    "overlay_text": "Alice",
    "presentation_supported": "NO",
    "protocol": "api",
    "role": "chair",
    "rx_presentation_policy": "ALLOW",
    "service_type": "conference",
    "spotlight": 0,
    "start_time": 1441720992,
    "uri": "CloudConnect_10.44.21.35",
    "uuid": "50b956c8-9a63-4711-8630-3810f8666b04",
    "vendor": "CloudConnect/2.0.0-25227.0.0 (Windows NT 6.1; WOW64) nwjs/0.12.2 Chrome/41.0.2272.76"
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|call_direction     |“in”:呼入；”out”:呼出    |
|display_name       |该参会者的显示名           |
|Encryption         |on/off:参会者的媒体流是否加密了    |
|has_media          |true/false:参会者是否有音视频媒体能力  |
|is_audio_only_call |YES:如果参会者只有音频通讯             |
|is_external        |true/false:是否是外部参会者，如来自lync    |
|is_muted           |YES:是否已被静音                       |
|is_presenting      |YES:是否是当前演示者（双流发送者）         |
|is_streaming_conference    |true/false:是否是流媒体（录播）参会者  |
|is_video_call          |YES:参会者是否有视频能力               |
|Protocol           |参会者入会使用的通讯协议               |
|role               |参会者角色:chair(主持人),guest(f)      |
|rx_presentation_policy |ALLOW/DENY:是否允许接受辅流（演示）        |
|service_type   |服务类型:<br/>- “connecting” — 外呼入会中 <br/> - “waiting_room” — 等待主持人批准入会（锁会）<br/> - “ivr” — 等待输入入会密码 <br/> - “conference” — 已入会 <br/> - “gateway” — 点到点呼叫|
|Spotlight      |如果“被焦点”，该参会者被“焦点”的时间（UNIX秒） |
|start_time     |入会时间（UNIX秒）     |
|uuid           |本参会者的唯一标识uuid         |
|uri            |参会者地址             |
|vendor            |入会终端/浏览器的厂商标识             |


<!-- participant_update -->
### participant_update
参会者的属性发生了改变。

#### 消息体实例
同participant_create

#### 消息体字段
同participant_create


<!-- participant_delete -->
### participant_delete
有参会者离开了会议室

#### 消息体实例
```
{
    "uuid": "65b4af2f-657a-4081-98a8-b17667628ce3"
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|uuid     |离开会议的参会者对应的唯一标识uuid   |

<!-- participant_sync_begin/participant_sync_end -->
### participant_sync_begin/participant_sync_end
参会者向云平台发出“订阅”事件列表请求，云平台将将先后发送这两个事件，以便参会者获取/同步参会者清单。Participant_sync_begin表示云平台将开始以participant_create事件方式发送参会者清单，一个参会者对应一个participant_create事件，每个参会者属性参见《participant_create》；participant_sync_end事件表示已发送（同步）完毕参会者清单。

<!-- Service_update -->
### Service_update
会议属性有更新。当前，云平台只提供会议是否锁定和访客入会缺省是否被静音的信息。

#### 消息体实例
```
{
    "locked": false,
    "guests_muted": false
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|locked     |true/false:是否锁会   |
|guests_muted     |true/false:是否静音了所有访客   |

<!-- layout -->
### layout
会议视频布局方式有更新。

#### 消息体实例
```
{
    guest_view: "1:0",
		host_view: "1:0",
		missing: [],
		"participants": ["a0196175-b462-48a1-b95c-f322c3af57c1", "65b4af2f-657a-4081-98a8-b17667628ce3"],
		quality: "high",
		rtx_ssrcs_map: {},
		ssrcs: [],
		time: 1573702690,
		view: "1:0"
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|view     |当前视频布局：1:0-只显示主讲人；1:7-主讲人+至多7个以往的发言者；1:21-主讲人+至多21个以往的发言者 |
|Participants     |屏幕上显示的以uuid标识的参会者/发言者顺序列表。第一个是当前主讲人，依次是上个主讲人…   |
|guest_view |访客布局 |
|host_view |主持人布局 |
|quality |网络质量 high：“好”，low：“差” |


<!-- message -->
### message
有群发的文本信息。

#### 消息体实例
```
{
    "origin": "Alice",
    "type": "text/plain",
    "payload": "Hello World",
    "uuid": "eca55900-274d-498c-beba-2169aad9ce1f"
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|origin     |群发文字信息者的显示名 |
|uuid     |群发文字信息的参会者对应的唯一标识uuid  |
|type     |群发的文字信息MIME类型  |
|payload     |群发的文字内容  |

<!-- stage -->
### stage
“舞台”布局（“视频显示”）有更新

#### 消息体实例
```
[{
        "stage_index": 0,
        "participant_uuid": "a0196175-b462-48a1-b95c-f322c3af57c1",
        "vad": 0
    },
    {
        "stage_index": 1,
        "participant_uuid": "65b4af2f-657a-4081-98a8-b17667628ce3",
        "vad": 0
    }
]
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|participant_uuid     |参会者唯一标识uuid |
|stage_index     |参会者在“舞台”上的编号：0是最近的主讲人，1次之…  |
|vad     |讲话者指示：0=不在讲话 100=正在讲话  |

<!-- call_disconnected -->
### call_disconnected
有子呼叫被断开（例如：由于演示权被其他参会者接管导致屏幕共享被关闭等）

#### 消息体实例
```
{
    "call_uuid": "50ed679d-c622-4c0e-b251-e217f2aa030b",
    "reason": "API initiated participant disconnect"
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|call_uuid     |子呼叫ID，用于唯一标识子呼叫 |
|reason     |子呼叫被断开的原因  |

<!-- disconnect -->
### disconnect
参会者被云平台踢出会议

#### 消息体实例
```
{
    "reason": "API initiated participant disconnect"
}
```

#### 消息体字段
| **字段**     | **含义**  |
| -------------   | ------------   |
|reason     |参会者被剔出会议的原因 |

## 公有云和专属云接口差异

| 接口                                             | 接口                                    | 公有云 | 企业专属云 |
| ------------------------------------------------ | --------------------------------------- | ------ | ---------- |
| 获取token                                        | new_session                             | 支持   | 支持       |
| refresh_session                                  | refresh_session                         | 支持   | 支持       |
| 结束token                                        | end_session                             | 支持   | 支持       |
| 获取会议状态                                     | Service_status                          | 支持   | 支持       |
| 将某个终端或用户呼入本会议                       | dial                                    | 支持   | 支持       |
| 锁会或解锁会议                                   | lock/unlock                             | 支持   | 支持       |
| 开启会议                                         | start_service                           | 支持   | 不支持     |
| 获取参会者列表                                   | participants                            | 支持   | 支持       |
| 控制布局                                         | Override_layout                         | 支持   | 不支持     |
| 将指定的与会者踢出会议                           | disconnect                              | 支持   | 支持       |
| 将指定的与会者静音/取消其静音                    | mute/unmute                             | 支持   | 支持       |
| 控制指定参会者能否接受辅流                       | allowrxpresentation /denyrxpresentation | 支持   | 支持       |
| 将某个参会者变成“焦点”/”去焦”                    | spotlighton / spotlightoff              | 支持   | 不支持     |
| 将指定参会者放进已锁定的会议中                   | unlock                                  | 支持   | 不支持     |
| 改变指定参会者的角色                             | role                                    | 支持   | 不支持     |
| 将指定参会者从当前会议转接到另一个会议室         | transfer                                | 支持   | 不支持     |
| 给该WEBRTC/RTMP参会者新增音视频信道(子呼叫)      | calls                                   | 支持   | 支持       |
| 修改字幕                                         | override_text                           | 支持   | 支持       |
| 有新的演示(辅流)，消息体中包含辅流的相关描述信息 | presentation_start                      | 支持   | 支持       |
| 演示 (辅流)结束                                  | presentation_stop                       | 支持   | 支持       |
| 有新的演示（辅流）帧                             | presentation_frame                      | 支持   | 不支持     |
| 有新的参会者加入了会议                           | participant_create                      | 支持   | 不支持     |
| 参会者属性有变                                   | participant_update                      | 支持   | 支持       |
| 有参会者离开了会议                               | participant_delete                      | 支持   | 支持       |
| 会议属性有更新                                   | service_update                          | 支持   | 支持       |
| 视频布局有更新                                   | layout                                  | 支持   | 支持       |
| 有新的（聊天）文字消息                           | message                                 | 支持   | 支持       |
| 讲话者有变，返回按“讲话”顺序排列的参会者列表     | stage                                   | 支持   | 支持       |
| 有参会者的子通讯被断开及其原因:比如结束屏幕共享  | call_disconnected                       | 支持   | 支持       |
| 有参会者被云平台“踢出“了会议                     | disconnect                              | 支持   | 支持       |