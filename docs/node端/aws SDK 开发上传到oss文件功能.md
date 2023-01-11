### 参考文档：
https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/dev/UsingAWSSDK.html
https://aws.amazon.com/cn/sdk-for-node-js/

### Version2 版本demo：
https://github.com/awslabs/aws-nodejs-sample.git

### Version3 版本：
https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/getting-started-nodejs.html

### 主要参数：
{ Body, Bucket, Key, CacheControl, ContentMD5, ContentType, Metadata }  
Body: Readable | ReadableStream | Blob | string | Unit8Array | Buffer  

ContentType 需要传入mimeType，否则默认是文件流形式

### api文档：
主要api：https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/API/API_PutObject.html  
具体参数描述：https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putObject-property  

### 设置凭证：  
v3：https://docs.aws.amazon.com/zh_cn/sdk-for-javascript/v3/developer-guide/setting-credentials.html   
v2：https://docs.aws.amazon.com/zh_cn/sdk-for-javascript/v2/developer-guide/setting-credentials.html   


对于文件上传的方式，还是不要直接在web端去直接使用sdk，而是使用node服务端中转上传

### Upload 到oss 需要修改的地方：

1. 增加一个底层插件：@sc/aws-helper，统一处理上传等aws交互功能
2. fie publish  发布资源文件和页面，上传到oss
3. 图片上传vscode插件
改造方案：
增加一个新的菜单 - 上传图片（aws版），上传时overseas参数为2
引用包 node-image-upload修改 - 已确认无需修改
4. 文件上传vscode插件
改造方案：
增加一个新的菜单 - 上传到oss（aws版），上传时overseas参数为2
fe-platform 修改api/upload/file相关接口和逻辑
5. fe-platform 

### 改造方案：
- 修改api/upload/file接口及相关逻辑——支撑vscode 文件上传插件
- 新增@sc/node-image-aws，跟node-image及node-image-intl类似——封装了aws的图片压缩上传功能
- 修改api/upload/image接口及相关逻辑——支撑vscode 图片上传插件、fe-platform菜单上传、后台接口上传等功能
- 修改api/fie/_publish接口及相关逻辑——支撑fie publish等项目发布命令 

- 新增后台菜单：aws图片上传、海外图片上传
- 严重问题：aws插件不支持服务器中node版本——已解决，将aws-sdk的v3降级为v2版本

### 后台上传插件的改造：

| 组件名称 | 原接口 | 改造方案 |
| - | - | - |
| components/FileUpload | /api/upload/signature, <br> https://us-smart-java-test.oss-accelerate.aliyuncs.com <br> https://us-smart-java-prod.oss-accelerate.aliyuncs.com | 统一走/api/upload/file, <br> /api/upload/file接口引入oss库，或者统一走7001项目的/api/upload/file |
| components/ImageUpload | /api/upload/image, <br> http://172.27.3.200:7001/api/upload/image <br> http://127.0.0.1:7001/api/upload/image | 同上 |
| components/CropUpload | 同ImageUpload | 同上 |
