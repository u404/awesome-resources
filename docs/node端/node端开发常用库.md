### 命令行参数
- minimist - 解析参数为对象，极简
- yargs - 参数解析+命令解析
- commander - 参数解析+命令解析
- meow - 参数提示、命令行助手
- inquirer - 询问式命令行对话工具
- [命令行工具评测：meow, commander, yargs](https://zhuanlan.zhihu.com/p/56316812)

### 命令行开发框架
- common-bin - 将命令便捷的映射到js文件
- plop - 微生成器框架，模板代码生成器

### 命令执行
- ts-node - 直接运行.ts文件
- execa - 批量执行命令，可利用多线程并行
- cross-env - 跨平台环境变量设置
- cross-spawn - 跨平台执行脚本
- esno - 直接运行.ts和esm文件，基于esbuild增强的node运行时

### http服务开发
- koa-jwt - koa 使用 JSON Web Tokens
- jsonwebtoken - 超高下载量的node jwt的一个实现
- koa-csrf、csrf - 防csrf攻击
- get-port - 获取一个可用的TCP 端口
- gzip-size - 计算string或buffer的gzip之后的大小
- wait-port - 等待端口、等待服务器启动、等待http请求响应
- [更多参考](https://www.zhihu.com/column/c_1359460465672785920)

### 网页处理
- puppeteer - 无头浏览器引擎，网页抓取
- cheerio - 服务器端jquery，处理页面元素
- critters、critters-webpack-plugin - 使必要的css内联，其他部分延迟加载。对预渲染、服务端渲染有用。
- selenium-webdriver - 浏览器自动化库，可实现自动化测试，和脚本机械操作

### 文件处理
- fs-extra、graceful-fs - node fs的增强替代库
- chokidar - 文件观察工具，高效的实现文件变化的监听
- rimraf - 文件删除
- del、del-cli - 文件删除
- glob - glob模式匹配搜索文件
- minimatch - glob模式匹配，转化为正则表达式
- multimatch - 扩展minimatch.match，支持多个glob同时匹配
- ncp - 异步递归文件、目录复制等
- node-homedir - 获取服务器上的用户目录路径
- zip-local - zip压缩，用法简单fie-toolkit-vue中使用的
- compressing - 支持 tar gzip tgz zip，用法较为简单
- js-zip - zip压缩解压，功能强大但用法复杂
- targz - node端的tar.gz压缩和解压
- tempy - 获取随机临时文件或目录路径

### 代码分析检查
- dependency-check - 检查代码中引用的模块是否在dependencies列出，或是否有未使用的dependencies
  
### 流处理
- event-stream - 像处理事件一样，处理Stream

### 网络请求
- node-fetch - fetch库
- isomorphic-unfetch - fetch的polyfill

### 系统调用
- node-notifier - 发送mac、win、linux系统通知、对话框
- tree-kill - 杀死进程树中的所有进程，包括根进程。

### npm包处理
- pacote - 从 npm 注册表获取包清单和压缩包。

### 模板下载
- download-git-repo - 从git仓库下载资源

### 测试模拟
- faker - 随机假数据生成器，可以生成人名、地址、电话、图片等各种数据


