相关资源

1. 官网： https://serverless.com/cn/framework/docs/getting-started/
2. 云函数开发指南： https://cloud.tencent.com/document/product/583/11060



概念解析

1. Faas: （Function as a service)函数即服务.  云函数是管理、运行的基本单元，通常由一系列配置与一系列可运行代码/软件包组成.
2. 不是真的没有服务器，只是用户无需关注运维的问题， 而是专注于业务



使用流程

![屏幕快照 2020-02-19 21.50.45](/Users/murphy/Desktop/屏幕快照 2020-02-19 21.50.45.png)



快速开始

1. 安装 serverless cli

   ```javascript
   # 安装 serverless cli
   npm install -g serverless
   ```

2. 创建一个新的 serverless 服务

   ```javascript
   # 创建一个新的 serverless 服务
   serverless create -t tencent-nodejs
   ```

3. 安装依赖包

   ```javascript
   # 安装依赖包
   npm install
   ```

4. 进行部署

   ```javascript
   #指定区域进行部署
   serverless deploy --stage pro --region ap-shanghai
   
   #还可以不指定区域
   ```

   

5. 去腾讯云serless下面上传部署得到的压缩包， 即完成云函数的初步操作

   上传的压缩包即是生成的云函数， 点击链接可在浏览器查看效果



> ps：以上是使用脚手架创建，也可以直接使用腾讯云控制台创建函数



云函数开发指南

1. 函数形态(示例：Node.js， 还支持Python等...)

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

   

   

   

   

   