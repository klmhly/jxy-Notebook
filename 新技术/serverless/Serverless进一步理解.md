**目录**

[TOC]



# 云函数开发指南



## 函数形态(示例：Node.js， 还支持Python等...)

```javascript
exports.main_handler = (event, context, callback) => {
    console.log("Hello World")
    console.log(event)
    console.log(context)
    callback(null, event); 
};
```

执行方法：

​	形如 `index.main_hander`,    均需要指定

- 代表入口文件是` index.js`

- 执行的入口函数为 `main_handler` 函数



入参：

​	Node.js 环境下的入参包括 event 、context 和 callback，其中 callback为可选参数

- event：使用此参数**传递触发事件数据**。
- context：使用此参数向您的处理程序传递**运行时信息**。
- callback：使用此参数用于将您所希望的信息返回给调用方。如果没有此参数值，返回值为null。



返回和异常

​	callback 入参来返回信息

​	`callback(Error error, Object result);`    // 两个都是可选参数





Serverless核心组件

1. 事件源: 是发布事件（Event）的腾讯云服务或用户自定义代码，

2. SCF 函数:是事件的处理者



## 触发器

定义： 函数触发器就是管理函数和事件源对应关系的集合



触发器种类：

### 1. Api网关触发器

 API 网关会将 HTTP 请求内容，转换为请求数据结构；请求数据结构作为函数的 event 输入参数，传递给函数并进行处理

**（1） 示例： 请求数据**

```javascript
{
  "requestContext": {
    "serviceId": "service-f94sy04v",
    "path": "/test/{path}",
    "httpMethod": "POST",
    "requestId": "c6af9ac6-7b61-11e6-9a41-93e8deadbeef",
    "identity": {
      "secretId": "abdcdxxxxxxxsdfs"
    },
    "sourceIp": "10.0.2.14",
    "stage": "release"
  },
  "headers": {
    "Accept-Language": "en-US,en,cn",
    "Accept": "text/html,application/xml,application/json",
    "Host": "service-3ei3tii4-251000691.ap-guangzhou.apigateway.myqloud.com",
    "User-Agent": "User Agent String"
  },
  "body": "{\"test\":\"body\"}",
  "pathParameters": {
    "path": "value"
  },
  "queryStringParameters": {
    "foo": "bar"
  },
  "headerParameters":{
    "Refer": "10.0.2.14"
  },
  "stageVariables": {
    "stage": "release"
  },
  "path": "/test/value",
  "queryString": {
    "foo" : "bar",
    "bob" : "alice"
  },
  "httpMethod": "POST"
}
```

数据结构内容详细说明如下：

| 结构名                | 内容                                                         |
| :-------------------- | :----------------------------------------------------------- |
| requestContext        | 请求来源的 API 网关的配置信息、请求标识、认证信息、来源信息。其中：serviceId，path，httpMethod 指向 API 网关的服务 ID、API 的路径和方法。stage 指向请求来源 API 所在的环境。requestId 标识当前这次请求的唯一 ID。identity 标识用户的认证方法和认证的信息。sourceIp 标识请求来源 IP。 |
| path                  | 记录实际请求的完整 Path 信息。                               |
| httpMethod            | 记录实际请求的 HTTP 方法。                                   |
| queryString           | 记录实际请求的完整 Query 内容。                              |
| body                  | 记录实际请求转换为 String 字符串后的内容。                   |
| headers               | 记录实际请求的完整 Header 内容。                             |
| pathParameters        | 记录在 API 网关中配置过的 Path 参数以及实际取值。            |
| queryStringParameters | 记录在 API 网关中配置过的 Query 参数以及实际取值。           |
| headerParameters      | 记录在 API 网关中配置过的 Header 参数以及实际取值。          |

**（2）示例返回**

在 API 网关设置为集成响应时，需要返回类似如下内容的数据结构。

```javascript
{
    "isBase64Encoded": false,
    "statusCode": 200,
    "headers": {"Content-Type":"text/html"},
    "body": "<html><body><h1>Heading</h1><p>Paragraph.</p></body></html>"
}
```

数据结构内容详细说明如下：

| 结构名          | 内容                                                         |
| :-------------- | :----------------------------------------------------------- |
| isBase64Encoded | 指明 body 内的内容是否为 Base64 编码后的二进制内容，取值需要为 JSON 格式的 true 或 false。 |
| statusCode      | HTTP 返回的状态码，取值需要为 Integer 值。                   |
| headers         | HTTP 返回的头部内容，取值需要为多个 key-value 对象。         |
| body            | HTTP 返回的 body 内容。                                      |

### 2. COS触发器

在指定的 COS Bucket 发生对象创建或对象删除事件时，会将类似以下的 JSON 格式事件数据发送给绑定的 SCF 函数

### 3.定时触发器

在指定时间触发调用云函数时，会将类似以下的 JSON 格式事件数据发送给绑定的 SCF 云函数

```javascript
{
    "Type":"Timer",
    "TriggerName":"EveryDay",
    "Time":"2019-02-21T11:49:00Z",
    "Message":"user define msg body"
}
```

### 4. CKafka触发器    ...等











