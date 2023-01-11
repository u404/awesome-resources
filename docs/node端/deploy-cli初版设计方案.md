# deploy-cli初版设计方案
> 部署静态化页面项目

## 全局安装deploy-cli

```bash
# 全局安装deploy-cli
npm install -g deploy-cli
# 或
yarn global add deploy-cli
```

## 配置项目的package.json
- 首先要确保其中配置了正确的“name”、“version”字段
  - name —— 按照标准package.json的要求，name应为英文小写或数字，可以用中划线或下划线作为连字符，可以使用“@xxx/”开头作为scope。
  - version —— 标准的三段式版本号，如：0.0.1
- 然后新增配置项“deploy”，作为一个对象，有以下可配置字段：
  - pages —— （必要）对象数组形式，对象必须具有以下几个属性：
    - name —— （必要）页面名称，可以为
    - source —— （必要）页面源文件路径，相对于dist目录的路径
    - target —— （必要）目标部署路径，部署到oss或cdn的远程path路径
  - distPath —— （可选）指定构建结果所在目录，即待发布的目录，默认为dist目录。
  - buildCommand —— （可选）构建命令定制。字符串或对象形式，默认为 npm run build。
    - 可以配置为字符串，举例："buildCommand": "yarn build"
    - 可以配置为对象形式，对象的key为部署环境的代码，值为命令字符串，对象形式必须同时配置所有环境。如：
      - "buildCommand": { "dev": "npm run build:dev", "test": "npm run build:test", "pre": "npm run build:pre", "prod", "npm run build:prod"  }
  - processEnvKeys —— （可选），构建时环境变量名定制，对象形式，key为以下值，value为环境变量名：
    - publicUrl —— （可选）会将动态生成的cdn路径，存入指定的环境变量，用于构建配置的publicPath（这是webpack中的定义，其他构建工具中大同小异），默认为"PUBLIC_URL"。即在构建工具的上下文中，可以使用process.env.PUBLIC_URL，来获取动态生成的cdn路径。
      - process.env.PUBLIC_URL具体值举例："https://res.lyrra.io/project/soundsright-admin/0.0.1/"
  - deployEnv —— （可选）会将部署环境代码放置于指定的环境变量中，默认为"DEPLOY_ENV"。即在构建工具的上下文中，可以使用process.env.DEPLOY_ENV，来获取部署环境代码。

## json配置示例：
```json
{
  "name": "mywebsite",
  "version": "0.0.1",
  // 新增配置项
  "deploy":{
    // 必要的配置
    "pages": [
      {
        "name": "页面名称",
        "source": "index.html",
        "target": "path/to/remoteUrl.html"
      }                             
    ],
    // "distPath": "dist",  // 指定构建结果所在目录
    // "buildCommand": "npm run build",  // 构建命令定制
    // 或 "buildCommand": { "dev": "npm run build:dev", "test": "npm run build:test", "pre": "npm run build:pre", "prod", "npm run build:prod" },
    // "processEnvKeys": { "publicUrl": "PUBLIC_URL", "deployEnv": "DEPLOY_ENV" } // 构建时环境变量名定制
  }
}
```

## 执行部署命令
```shell
# 部署项目 - 进入到项目目录中
deploy

# 选择环境
```