# 简介

本文档描述的是会中管理的REST API 功能补充赋能。

## API的使用

#### 响应参数说明:

| **参数名称** | **类型** | **说明**                        | **长度** |
| ------------ | -------- | ------------------------------- | -------- |
| data         | object   | 相关业务有效数据的信息          |          |
| code         | varchar  | 响应状态码                      |          |
| state        | int      | 1:成功 0：表示失败              |          |
| message      | varchar  | 接口错误的消息返回 ，成功为null |          |
| redirectUrl  | varchar  | 重定向跳转的URL ，缺省null      |          |
| token        | varchar  | null                            |          |
| redirect     | boolean  | 是否需要重定向跳转页面 false    |          |

## 开启录播

该接口用于开启会议中的录制功能。
请求地址:https://apiServer/meeting/openRecordLive
请求方式:POST
请求参数:

| **参数类别**     | **参数名称** | 类型    | **说明**                                   | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ------------------------------------------ | ---- | -------- |
| 请求头部(header) | auth         | varchar | 该企业或租户认证key                        | 60   | 是       |
| 请求参数(Body)   | openType     | int     | 2:录制                                     | 15   | 是       |
| 请求参数(Body)   | sipKey       | varchar | 入会后显示姓名                             | 50   | 是       |
| 请求参数(Body)   | uuid         | varchar | 录制产生的唯一ID（缺省为系统自动生成uuid） |      | 否       |

响应data参数:

| **参数名称** | **类型** | **说明**         | **长度** |
| ------------ | -------- | ---------------- | -------- |
| data         | varchar  | 本次录制的唯一ID |          |

正确响应数据报格式:

```json
{
    "code": "200",
    "data": "a1e3595d-207a-4853-ba39-c576a5761952",
    "message": null,
    "redirectUrl": null,
    "state": 1,
    "token": null,
    "redirect": false
}
```

## 关闭录播

该接口用于关闭会议中的录制功能。
请求地址:https://apiServer/meeting/closeRecordLive
请求方式:POST
请求参数:

| **参数类别**     | **参数名称** | 类型    | **说明**          | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ----------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 入会短号/通讯地址 | 60   | 是       |
| 请求参数(Body)   | openType     | int     | 2:录制            | 15   | 是       |
| 请求参数(Body)   | sipKey       | varchar | 入会后显示姓名    | 50   | 是       |
| 请求参数(Body)   | uuid         | varchar | 录制产生的唯一ID  |      | 是       |

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
    "redirectUrl": null,
    "state": 1,
    "token": null,
    "redirect": false
}
```

# 获取录播文件

该接口用于通过录制的唯一ID获取录制文件
请求地址:https://apiServer/meeting/getRecords
请求方式:POST
请求参数:

| **参数类别**     | **参数名称** | 类型    | **说明**          | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ----------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 入会短号/通讯地址 | 60   | 是       |
| 请求参数(Body)   | recordId     | int     | 录制产生的唯一ID  | 15   | 是       |
| 请求参数(Body)   | sipKey       | varchar | 会议短号          | 50   | 否       |

响应data参数:

| **参数名称**     | **类型** | **说明**                                   | **长度** |
| ---------------- | -------- | ------------------------------------------ | :------- |
| recordId         | varchar  | 本次录制的唯一ID                           |          |
| sipkey           | varchar  | 会议号                                     |          |
| entId            | int      | 企业ID或租户ID                             |          |
| conferencetheme  | varchar  | 预约主题名称（该次录制会议被预约的主题）   |          |
| startTime        | varchar  | 录制的开始时间 格式（2019-05-31 01:43:09） |          |
| fileSize         | double   | 文件大小byte                               |          |
| stopTime         | varchar  | 录制的开始时间 格式（2019-05-31 01:43:09） |          |
| downSourceUrl    | varchar  | 文件源下载地址                             |          |
| cdnDownSourceUrl | varchar  | 文件通过cdn加速地址                        |          |

正确响应数据报格式:

```json
{
    "code": "200",
    "data": [
        {
            "recordId": "02e555e21dc8dae6b6ea362ebbece8dd",
            "sipkey": "1866",
            "entId": 52,
            "startTime": "2019-05-31 01:43:09",
            "fileSize": 1027797,
            "conferencetheme": "",
            "stopTime": "2019-05-31 01:43:23",
            "smrbId": 0,
            "downSourceUrl": "http://xxxxxxxxxx/2019-05-31/1866_094309.mp4",
            "cdnDownSourceUrl": "http://xxxxxxx.cossh.myqcloud.com/2019-05-31/1866_094309.mp4?sign=tchXRYvuIsDY46D3297KGdceQllhPTEwMDQ1OTYwJmI9ZTUyJms9QUtJRGFCeG5oWW5yUkdEM29Vc3BSN3JXMVY1WjBPaDA2dlpHJmU9MTU3MTYyMTYxNSZ0PTE1NzEwMTY4MTUmcj0xNDk2NDYwMDY4JmY9"
        },
        ...
        {
            "recordId": "fef892788b675d384764e403781f1b1e",
            "sipkey": "1866",
            "entId": 52,
            "cid": "a15fcb02-9410-471c-af47-2b368715999f",
            "startTime": "2017-02-20 01:51:10",
            "fileSize": 102779700,
            "conferencetheme": null,
            "stopTime": "2017-02-20 02:51:10",
            "smrbId": null,
            "downSourceUrl": "http://xxxxx/2017-02-20/1866_100151.mp4",
            "cdnDownSourceUrl": "http://xxxxxx.cossh.myqcloud.com/2017-02-20/1866_100151.mp4?sign=/aT6y5Hbc0kD6TP%2BWqZ52LC88SthPTEwMDQ1OTYwJmI9ZTUyJms9QUtJRGFCeG5oWW5yUkdEM29Vc3BSN3JXMVY1WjBPaDA2dlpHJmU9MTU3MTYyMTYxNiZ0PTE1NzEwMTY4MTYmcj05MTc3MTM2MzgmZj0="
        }
    ],
    "message": null,
    "redirectUrl": null,
    "state": 1,
    "token": null,
    "redirect": false
}
```

## 删除录制文件

该接口用于通过录制的唯一ID删除该录制文件。
请求地址:https://apiServer/meeting/delRecordFile
请求方式:POST
请求参数:

| **参数类别**     | **参数名称** | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 该企业或租户认证key | 60   | 是       |
| 请求参数(Body)   | recordId     | varchar | 录制产生的唯一ID    | 50   | 是       |
| 请求参数(Body)   | sipKey       | varchar | 入会后显示姓名      | 50   | 否       |

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
    "redirectUrl": null,
    "state": 1,
    "token": null,
    "redirect": false
}
```

# 