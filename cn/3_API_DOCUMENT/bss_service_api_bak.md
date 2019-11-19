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
| 请求参数(Body)   | conferenceKey | varchar | 会议是短号          | 50   | 是       |

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
| 请求参数(Body)   | conferenceKey | varchar | 会议是短号          | 50   | 是       |

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
| 请求参数(Body)   | conferenceKey | varchar | 会议是短号          | 50   | 是       |

响应data参数:

| **参数名称**  | **类型** | **说明**                                                     |
| ------------- | -------- | ------------------------------------------------------------ |
| conferenceKey | varchar  | 会议是短号                                                   |
| live          | boolean  | 直播是否开启                                                 |
| rtmpUrl       | varchar  | 直播流 live为true返回直播流，live为false返回'                |
| record        | boolean  | 录制是否开启                                                 |
| recordId      | varchar  | 录制事件的recordId record为false返回''，需要拿openRecord返回的recordId比对，是否为自己开启的录制 |
| failedReason  | varchar  | 录制开启失败的原因 录制开启失败将返回，否则不返回该字段      |

正确响应数据报格式:

```json
{
    "code": 200,
    "data": {
        "conferenceKey": "8000000",
        "live": false/true, # 直播是否开启
        "rtmpUrl": "", # 直播流 live为true返回直播流，live为false返回''
        "record": false/true, # 录制是否开启
        "recordId": 'a1e3595d-207a-4853-ba39-c576a5761952', # 录制事件的recordId record为false返回''，需要拿openRecord返回的recordId比对，是否为自己开启的录制
        "failedReason": '' # 录制开启失败的原因 录制开启失败将返回，否则不返回该字段
    },
    "message": null
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

