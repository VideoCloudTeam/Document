# 简介

本文档描述的是会中管理的REST API 功能补充赋能。

## 鉴权说明

接口的头部均需要传递参数auth(鉴权KEY)；鉴权的KEY是有企业(租户)的API ID 和 API KEY构造。



## API的使用

#### 响应参数说明:

| **参数名称** | **类型** | **说明**                        | **长度** |
| ------------ | -------- | ------------------------------- | -------- |
| data         | object   | 相关业务有效数据的信息          |          |
| code         | varchar  | 响应状态码                      |          |
| state        | int      | 1:成功 0：表示失败              |          |
| message      | varchar  | 接口错误的消息返回 ，成功为null |          |

## 开启录播

该接口用于开启会议中的录制功能<br>
请求地址:https://apiServer/meeting/openRecord<br>
请求方式:POST<br>
请求参数:

| **参数类别**     | **参数名称** | 类型    | **说明**            | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ------------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 该企业或租户认证key | 60   | 是       |
| 请求参数(Body)   | sipKey       | varchar | 会议是短号          | 50   | 是       |

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

## 关闭录播

该接口用于关闭会议中的录制功能。<br>
请求地址:https://apiServer/meeting/closeRecord<br>
请求方式:POST<br>
请求参数:

| **参数类别**     | **参数名称** | 类型    | **说明**          | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ----------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 入会短号/通讯地址 | 60   | 是       |
| 请求参数(Body)   | sipKey       | varchar | 会议是短号        | 50   | 是       |

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

# 获取录播文件

该接口用于通过录制的唯一ID获取录制文件<br>
请求地址:https://apiServer/meeting/getRecords<br>
请求方式:POST<br>
请求参数:

| **参数类别**     | **参数名称** | 类型    | **说明**          | 长度 | 是否必填 |
| ---------------- | ------------ | ------- | ----------------- | ---- | -------- |
| 请求头部(header) | auth         | varchar | 入会短号/通讯地址 | 60   | 是       |
| 请求参数(Body)   | recordId     | int     | 录制产生的唯一ID  | 15   | 是       |

响应data参数:

| **参数名称** | **类型**       | **说明**                                   | **长度** |
| ------------ | -------------- | ------------------------------------------ | :------- |
| recordId     | varchar        | 本次录制的唯一ID                           |          |
| sipkey       | varchar        | 会议号                                     |          |
| entId        | int            | 企业ID或租户ID                             |          |
| startTime    | varcharvarchar | 录制的开始时间 格式（2019-05-31 01:43:09） |          |
| fileSize     | double         | 文件大小byte                               |          |
| stopTime     | varchar        | 录制的开始时间 格式（2019-05-31 01:43:09） |          |
| downUrl      | varchar        | 录制文件下载地址                           |          |

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
            "stopTime": "2019-05-31 01:43:23",
            "downUrl": "http://xxxxxxx.cossh.myqcloud.com/2019-05-31/1866_094309.mp4?sign=tchXRYvuIsDY46D3297KGdceQllhPTEwMDQ1OTYwJmI9ZTUyJms9QUtJRGFCeG5oWW5yUkdEM29Vc3BSN3JXMVY1WjBPaDA2dlpHJmU9MTU3MTYyMTYxNSZ0PTE1NzEwMTY4MTUmcj0xNDk2NDYwMDY4JmY9"
        },
        ...
        {
            "recordId": "fef892788b675d384764e403781f1b1e",
            "sipkey": "1866",
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
请求参数:

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
    "redirectUrl": null,
    "state": 1,
    "token": null,
    "redirect": false
}
```

# 