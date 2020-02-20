# 简介

本文档描述的是会中管理的REST API 功能补充赋能。

## 鉴权说明

接口的头部均需要传递参数auth(鉴权KEY)；

公有云：鉴权的KEY是由企业(租户)的API ID 和 API KEY构造。

专属云：鉴权的KEY是对应租户的关键字段(eg:auth=800)。

## API的使用

#### 响应参数说明:

| **参数名称** | **类型** | **说明**                        | **长度** |
| ------------ | -------- | ------------------------------- | -------- |
| data         | object   | 相关业务有效数据的信息          |          |
| code         | varchar  | 响应状态码                      |          |
| state        | int      | 1:成功 0：表示失败              |          |
| message      | varchar  | 接口错误的消息返回 ，成功为null |          |

## 开启录制

该接口用于开启会议中的录制功能<br>
请求地址:https://apiServer/meeting/openRecord<br>
请求方式:POST<br>
请求参数:"Content-Type":"application/json"

| **参数类别**     | **参数名称**  | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------- | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth          | varchar | 该企业或租户认证key | 60   | 是       |
| 请求参数(Body)   | conferenceKey | varchar | 会议室是短号        | 50   | 是       |

响应data参数:

| **参数名称** | **类型** | **说明**         | **长度** |
| ------------ | -------- | ---------------- | -------- |
| recordId     | varchar  | 本次录制的唯一ID |          |

正确响应数据报格式:

```json
{
    "code": "200",
    "data": {
        "recordId": "a1e3595d-207a-4853-ba39-c576a5761952"
    },
    "message": null,
    "state": 1
}
```

| **参数名称** | **类型** | **说明**         |
| ------------ | -------- | ---------------- |
| recordId     | varchar  | 本次录制的唯一ID |



## 关闭录制

该接口用于关闭会议中的录制功能。<br>
请求地址:https://apiServer/meeting/closeRecord<br>
请求方式:POST<br>
请求参数:"Content-Type":"application/json"

| **参数类别**     | **参数名称**  | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------- | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth          | varchar | 该企业或租户认证key | 60   | 是       |
| 请求参数(Body)   | conferenceKey | varchar | 会议室是短号        | 50   | 是       |

响应data参数:

| **参数名称** | **类型** | **说明**         | **长度** |
| ------------ | -------- | ---------------- | -------- |
| recordId     | varchar  | 本次录制的唯一ID |          |

正确响应数据报格式:

```json
{
    "code": "200",
    "data": {
        "recordId": "a1e3595d-207a-4853-ba39-c576a5761952",
    },
    "message": null,
    "state": 1,
}
```

# 获取录制文件

该接口用于通过录制的唯一ID获取录制文件<br>
请求地址:https://apiServer/meeting/getRecords<br>
请求方式:POST<br>
请求参数:"Content-Type":"application/json"

| **参数类别**     | **参数名称** | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 该企业或租户认证key | 60   | 是       |
| 请求参数(Body)   | recordId     | int     | 录制产生的唯一ID    | 15   | 是       |
| 请求参数(Body)   | origin       | varchar  | https://apiServer   | 128   | 否       |

响应data参数:

| **参数名称**  | **类型**       | **说明**                                   | **长度** |
| ------------- | -------------- | ------------------------------------------ | :------- |
| recordId      | varchar        | 本次录制的唯一ID                           |          |
| conferenceKey | varchar        | 会议号                                     |          |
| entId         | int            | 企业ID或租户ID                             |          |
| startTime     | varcharvarchar | 录制的开始时间 格式（2019-05-31 01:43:09） |          |
| fileSize      | double         | 文件大小byte                               |          |
| stopTime      | varchar        | 录制的开始时间 格式（2019-05-31 01:43:09） |          |
| downUrl       | varchar        | 录制文件下载地址                           |          |

正确响应数据报格式:

```json
{
    "code": "200",
    "data": [
        {
            "recordId": "02e555e21dc8dae6b6ea362ebbece8dd",
            "conferenceKey": "1866",
            "entId": 52,
            "startTime": "2019-05-31 01:43:09",
            "fileSize": 1027797,
            "stopTime": "2019-05-31 01:43:23",
            "downUrl": "http://xxxxxxx.cossh.myqcloud.com/2019-05-31/1866_094309.mp4?sign=tchXRYvuIsDY46D3297KGdceQllhPTEwMDQ1OTYwJmI9ZTUyJms9QUtJRGFCeG5oWW5yUkdEM29Vc3BSN3JXMVY1WjBPaDA2dlpHJmU9MTU3MTYyMTYxNSZ0PTE1NzEwMTY4MTUmcj0xNDk2NDYwMDY4JmY9"
        },
        ...
        {
            "recordId": "fef892788b675d384764e403781f1b1e",
            "conferenceKey": "1866",
            "entId": 52,
            "cid": "a15fcb02-9410-471c-af47-2b368715999f",
            "startTime": "2017-02-20 01:51:10",
            "fileSize": 102779700,
            "stopTime": "2017-02-20 02:51:10",
            "downUrl": "http://xxxxxx.cossh.myqcloud.com/2017-02-20/1866_100151.mp4?sign=/aT6y5Hbc0kD6TP%2BWqZ52LC88SthPTEwMDQ1OTYwJmI9ZTUyJms9QUtJRGFCeG5oWW5yUkdEM29Vc3BSN3JXMVY1WjBPaDA2dlpHJmU9MTU3MTYyMTYxNiZ0PTE1NzEwMTY4MTYmcj05MTc3MTM2MzgmZj0="
        }
    ],
    "message": null,
    "state": 1
}
```

## 删除录制文件

该接口用于通过录制的唯一ID删除该录制文件。<br>
请求地址:https://apiServer/meeting/delRecordFile<br>
请求方式:POST<br>
请求参数:"Content-Type":"application/json"

| **参数类别**     | **参数名称** | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 该企业或租户认证key | 60   | 是       |
| 请求参数(Body)   | recordId     | varchar | 录制产生的唯一ID    | 50   | 是       |

响应data参数:

| **参数名称** | **类型** | **说明**         | **长度** |
| ------------ | -------- | ---------------- | -------- |
| recordId     | varchar  | 本次录制的唯一ID |          |

正确响应数据报格式:

```json
{
    "code": "200",
    "data": {
        "recordId": "a1e3595d-207a-4853-ba39-c576a5761952",
    },
    "message": null,
    "state": 1,
}
```

## 获取直播录播状态

该接口用于检测直播录制的真实状态<br>
请求地址:https://apiServer/meeting/getLiveRecStatus<br>
请求方式:POST<br>
请求参数:"Content-Type":"application/json"

| **参数类别**     | **参数名称**  | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------- | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth          | varchar | 该企业或租户认证key | 60   | 是       |
| 请求参数(Body)   | conferenceKey | varchar | 会议室短号          | 50   | 是       |

响应data参数:

| **参数名称**  | **类型** | **说明**                                                     |
| ------------- | -------- | ------------------------------------------------------------ |
| conferenceKey | varchar  | 会议是短号                                                   |
| live          | boolean  | 直播是否开启                                                 |
| rtmpUrl       | varchar  | 直播流 live为true返回直播流，live为false返回空字符串""                |
| record        | boolean  | 录制是否开启                                                 |
| recordId      | varchar  | 录制事件的recordId record为false返回''，需要拿openRecord返回的recordId比对，是否为自己开启的录制 |
| failedReason  | varchar  | 录制开启失败的原因 录制开启失败将返回，否则不返回该字段      |

正确响应数据报格式:

```json
{
    "code": 200,
    "data": {
        "conferenceKey": "8000000",
        "live": false,
        "rtmpUrl": "",
        "record": false,
        "recordId": "a1e3595d-207a-4853-ba39-c576a5761952",
        "failedReason": ""
    },
    "message": null,
    "state": 1
}
```
## 获取会议信息

该接口用于查询会议是否在使用中和参会人数<br>
请求地址:https://apiServer/api/getmeetinginfo?addr={会议室短号}<br>
请求方式:GET<br>

响应data参数:

| **参数名称**  | **类型** | **说明**                                                     |
| ------------- | -------- | ------------------------------------------------------------ |
| name          | varchar  | 会议室名称                                                  |
| alias         | varchar  | 会议室短号                                                 |
| hostpwd       | varchar  | 主持密码                |
| guestpwd      | varchar  | 入会密码                                                 |
| locked        | boolean  | 会议是否锁定   |
| started       | boolean  | 会议是否开启      |
| participants  | int      | 与会人数     |

正确响应数据报格式:

```json
{
     "code": 200,
     "msg": "success",
     "name": "会议室名称",
     "alias": "8000000",
     "hostpwd": "0000",
     "guestpwd": "0000",
     "locked": false,
     "started": true,
     "participants": 0
}
```
## 预约会议

### 获取当前账号可预约的会议室信息

该接口用于获取可预约的会议室信息<br>
请求地址:https://apiServer/subscribe/getmeeting<br>
请求方式:GET<br>
请求参数:requestParam

| **参数类别**           | **参数名称** | 类型    | **说明**                  | 长度 | 是否必填 |
| ---------------------- | ------------ | ------- | ------------------------- | ---- | -------- |
| 请求头部(header)       | auth         | varchar | 该企业或租户认证key       | 60   | 是       |
| 请求参数(requestParam) | account      | varchar | 用户账号（xxxx@xxxx格式） | 50   | 是       |

响应data参数:

| **参数名称** | **类型** | **说明**                           |
| ------------ | -------- | ---------------------------------- |
| hostPin      | varchar  | 主持人入会密码                     |
| isPublicVmr  | boolean  | 是否为新会议模式 缺省为 no         |
| hostView     | varchar  | 主持人屏幕视角 eg:1:7;4:0;1:0;1:21 |
| alias        | boolean  | 会议长地址                         |
| guestView    | varchar  | 访客屏幕视角eg:1:7;4:0;1:0;1:21    |
| guestPin     | varchar  | 访客入会密码                       |
| pid          | Int      | 会议室ID                           |
| type         | Int      | 会议类型1:会议室 2:讲堂            |
| maxSquare    | varchar  | 会议室最大方数                     |
| isDispark    | varchar  | 是否为公共会议室 缺省 no           |
| sipkey       | varchar  | 会议室短号                         |
| pDesc        | varchar  | 会议室名称                         |

正确响应数据报格式:

```json
{
    "code": 200,
    "data": [
        {
            "hostPin": "123456",
            "isPublicVmr": "yes",
            "livePwd": "",
            "hostView": "1:7",
            "alias": "17815027526meet@myvmr105.cn",
            "guestView": "1:7",
            "guestPin": "1234567",
            "pid": 3364,
            "type": 1,
            "maxSquare": "100",
            "pDesc": "何宗源的会议室",
            "sipkey": 100713,
            "isDispark": "no"
        }
    ],
    "message": null,
    "state": 1
}
```

### 获取通讯录信息

该接口用于获取该企业用户信息<br>
请求地址:https://apiServer/subscribe/getcontacts<br>
请求方式:GET<br>
请求参数:requestParam

eg:https://apiServer/subscribe/getcontacts?account=xxxx@xxx.cn&pageSize=10&pageIndex=1&isAll=yes

| **参数类别**           | **参数名称** | 类型    | **说明**                                             | 长度 | 是否必填 |
| ---------------------- | ------------ | ------- | ---------------------------------------------------- | ---- | -------- |
| 请求头部(header)       | auth         | varchar | 该企业或租户认证key                                  | 60   | 是       |
| 请求参数(requestParam) | account      | varchar | 用户账号（xxxx@xxxx格式）                            | 50   | 是       |
| 分页                   | pageIndex    | int     |                                                      |      |          |
| 分页的size             | pageSize     | int     |                                                      |      |          |
| 是否获取全部           | isAll        | varchar | 缺省“no”；如果“yes” ，pageIndex，pageSize 字段无效； |      |          |

正确响应数据报格式:

```json
{
    "code": 200,
    "data": [
        {
        "id": 1429, #预约主键ID
        "theme": "会议主题", #预约主题
        "introduction": "接口会议", #预约内容介绍
        "randomHostPwd": "99999", #预约的主持人密码
        "randomVisitorPwd": "", #预约的访客密码
        "sipkey": "2516", #会议短号
        "isAutoOpen": "no", #是否开启自动拉起
        "startDate": "2020-01-30 09:00", #预约开始时间
        "endDate": "2020-01-30 13:00", #预约结束时间
        "createSubscriber": {
            "id": 2756, #uid
            "name": "蓝天" #姓名
        },# 预约发起人 信息
        "createTime": "2020-01-07 11:48:40", #创建预约的时间
        "shortAddress": "https://api.myvmr.cn/meet/share/VTZI8", #预约分享地址
        "bizProductId": 486, #会议室的主键ID
        "visitorList": [
            {
                "id": "9182",
                "name": "xxxx"
            },
            {
                "id": "9187",
                "name": "AAAAAA"
            }
        ] #参会者列表集合
    },
    "message": null,
    "state": 1
}
```

### 创建预约会议

该接口用于创建预约信息<br>
请求地址:https://apiServer/subscribe/createmeet<br>
请求方式:POST<br>
请求参数:"Content-Type":"application/json"

| **参数类别**     | **参数名称** | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 该企业或租户认证key | 60   | 是       |

请求参数(Body):

```json
{
  "account":"admin@miwei.com" #账号地址
  ,"theme":"会议主题" #预约主题
  ,"introduction":"接口会议" #内容介绍
  ,"visitor_ids":"9187,9182" #参会者的uid
  ,"startDate":"2020-01-30 09:00" 
  ,"endDate":"2020-01-30 13:00"
  ,"isAutoOpen":"yes" #是否自动开启
  ,"pid":486  #会议室 ID （空字符表示 系统随机按规则创建）
  ,"shostPwd":"99999" # 主持人密码
  ,"svisitorPwd":"888888" # 访客密码 （空字符表示访客密码空）
}
```

## 

正确响应数据报格式:

```json
{
    "code": 200,
    "data": [
        {
        "id": 1429, #预约主键ID
        "theme": "会议主题", #预约主题
        "introduction": "接口会议", #预约内容介绍
        "randomHostPwd": "99999", #预约的主持人密码
        "randomVisitorPwd": "", #预约的访客密码
        "sipkey": "2516", #会议短号
        "isAutoOpen": "no", #是否开启自动拉起
        "startDate": "2020-01-30 09:00", #预约开始时间
        "endDate": "2020-01-30 13:00", #预约结束时间
        "createSubscriber": {
            "id": 2756, #uid
            "name": "蓝天" #姓名
        },# 预约发起人 信息
        "createTime": "2020-01-07 11:48:40", #创建预约的时间
        "shortAddress": "https://api.myvmr.cn/meet/share/VTZI8", #预约分享地址
        "bizProductId": 486, #会议室的主键ID
        "visitorList": [
            {
                "id": "9182",
                "name": "xxxx"
            },
            {
                "id": "9187",
                "name": "AAAAAA"
            }
        ] #参会者列表集合
    },
    "message": null,
    "state": 1
}
```

### 取消预约会议

该接口用于取消预约记录<br>
请求地址:https://apiServer/subscribe/cancelmeet<br>
请求方式:POST<br>
请求参数:"Content-Type":"application/json"

| **参数类别**     | **参数名称** | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 该企业或租户认证key | 60   | 是       |

请求参数(Body):

```json
{
  "st_id":"xx" #与会会见主键ID
}
```

## 

正确响应数据报格式:

```json
{
    "code": 200,
    "data": null,
    "message": "操作成功",
    "state": 1
}
```

### 查询预约列表

该接口用于获取已经预约的预约会议信息<br>
请求地址:https://apiServer/subscribe/getsubmeetings<br>
请求方式:GET<br>
请求参数:requestParam

eg：https://apiServer/subscribe/getsubmeetings?account=xxxx@xxx.com

| **参数类别**           | **参数名称** | 类型    | **说明**                  | 长度 | 是否必填 |
| ---------------------- | ------------ | ------- | ------------------------- | ---- | -------- |
| 请求头部(header)       | auth         | varchar | 该企业或租户认证key       | 60   | 是       |
| 请求参数(requestParam) | account      | varchar | 用户账号（xxxx@xxxx格式） | 50   | 是       |

正确响应数据报格式:

```json
{
    "code": 200,
    "data": [
        {
        "id": 1429, #预约主键ID
        "theme": "会议主题", #预约主题
        "introduction": "接口会议", #预约内容介绍
        "randomHostPwd": "99999", #预约的主持人密码
        "randomVisitorPwd": "", #预约的访客密码
        "sipkey": "2516", #会议短号
        "isAutoOpen": "no", #是否开启自动拉起
        "startDate": "2020-01-30 09:00", #预约开始时间
        "endDate": "2020-01-30 13:00", #预约结束时间
        "createSubscriber": {
            "id": 2756, #uid
            "name": "蓝天" #姓名
        },# 预约发起人 信息
        "createTime": "2020-01-07 11:48:40", #创建预约的时间
        "shortAddress": "https://api.myvmr.cn/meet/share/VTZI8", #预约分享地址
        "bizProductId": 486, #会议室的主键ID
        "visitorList": [
            {
                "id": "9182",
                "name": "xxxx"
            },
            {
                "id": "9187",
                "name": "AAAAAA"
            }
        ] #参会者列表集合
    },
    "message": null,
    "state": 1
}
```

## 常见响应状态码

| 状态码 | 描述                           |
| ------ | ------------------------------ |
| 200001 | 会议号和当前的认证的auth不匹配 |
| 200002 | 该会议已开启直播               |
| 200003 | 该会议已开启录制               |
| 200004 | 该会议不允许录制                |
| 200005 | 该会议不允许直播                |
| 200006 | 该会议还未开启                 |
| 200007 | 该会议未开启录制               |
| 200008 | 该会议不存在               |
| 200009 | 当前时间段，此会议室已被预约 |

