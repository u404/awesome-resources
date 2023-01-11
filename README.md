<h1 align="center">资源库</h1>

> 整理汇总项目、文档、笔记，陆续更新中...

- [web3](#web3)
  - [项目](#项目)
  - [资料](#资料)
  - [笔记](#笔记)
- [vue](#vue)
  - [项目](#项目-1)
  - [资料](#资料-1)
  - [笔记](#笔记-1)
- [react](#react)
  - [资料](#资料-2)
  - [笔记](#笔记-2)
- [c端](#c端)
  - [资料](#资料-3)
  - [笔记](#笔记-3)
- [node端（服务、cli、框架、插件）](#node端服务cli框架插件)
  - [项目](#项目-2)
  - [资料](#资料-4)
  - [笔记](#笔记-4)
- [特殊项目](#特殊项目)
  - [项目](#项目-3)
- [seo的研究](#seo的研究)
  - [笔记](#笔记-5)
- [杂项知识](#杂项知识)
  - [文档](#文档)
  - [笔记](#笔记-6)




## web3

### 项目

- [soundsright](README.md) - @soundsright基础功能monorepo仓库
  - @soundsright/sdk - 整合了以下所有功能的sdk包
  - @soundsright/connector - 客户端钱包连接管理模块
  - @soundsright/service - 网关api封装请求底层模块
  - @soundsright/user - 用户模块，提供google/facebook/twitter/instagram等第三方登录、钱包登录等功能
  - @soundsright/auth - 用户授权模块，也是用户模块的底层模块，用于仅获取授权的场景
  - @soundsright/chain - 封装了区块链合约交互的具体功能
  - @soundsright/share - 实现分享到facebook、twitter的基础功能
  - @soundsright/invite - 邀请注册机制的封装
  - @soundsright/contracts - 主要c端合约abi的汇总
- [api-service](README.md) - 供服务端调用的代理执行合约交易的服务。主要技术栈：midway、ethers.js、ethereumjs-wallet、mysql（sequelize）、redis。

### 资料

- [以太坊官方文档](https://ethereum.org/zh/developers/docs/) - 系统性入门
- [ethers.js](https://docs.ethers.org/) - 最新的区块链客户端开发、dapp开发的基础库。web3.js的替代者
- [web3.js](https://web3js.readthedocs.io/) - 优秀的区块链客户端开发、dapp的基础库。目前已逐步被ethers.js所替代
- [web3-react](https://github.com/u404/web3-react) - 最流行的react版钱包连接组件，其包装了几乎所有类型的钱包连接方式，开发和学习必备
- [web3-react相关资料](https://note.youdao.com/s/2X0OY5HK)
- eip-1193 provider - 钱包与网页交互的桥梁，遵循 [eip-1193 以太坊标准](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md)，所有钱包的实现都是大同小异的。详细的api可参考[metamask中的实现](https://docs.metamask.io/guide/ethereum-provider.html)
- 客户端实现的规范标准
  - [eip-1193 以太坊标准](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md) - 钱包与网页交互的桥梁。详细的api可参考[metamask中的实现](https://docs.metamask.io/guide/ethereum-provider.html)
  - [EIP-1474规范](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1474.md) - 定义了JSON-RPC请求的标准。也可参考[metamask更友好的文档](https://docs.metamask.io/guide/rpc-api.html)
  - [JSON-RPC文档](https://ethereum.org/zh/developers/docs/apis/json-rpc/) - 以太坊官方文档中JSON-RPC的部分
- 以太坊api服务提供商infura相关文档
  - [什么是 Infura？](https://www.qklw.com/top/20201111/139470.html)
  - [Infura的使用](https://www.cnblogs.com/wanghui-garcia/p/9719399.html)
  - [Infura官方](https://www.infura.io/)
  - [Infura开发文档（汇智网）](http://cw.hubwiz.com/card/c/infura-api/)
  - [使用truffle、opensea ipfs、polygon部署nft合约](https://dev.to/yournewempire/deploy-nfts-with-truffle-ipfs-opensea-polygon-5581)
  - [以太坊ethereum nonce的理解](https://medium.com/swlh/ethereum-series-understanding-nonce-3858194b39bf)
  - [昆仑链](https://www.kunlunlian.com/)

### 笔记
- [以太坊基础概念](docs/web3/以太坊基础概念.md)
- [区块链基础知识](https://note.youdao.com/s/ASXRGN29)
- [区块链开发笔记(1)](docs/web3/区块链开发笔记(1).md)
- [Solidity合约开发研究](https://note.youdao.com/s/3Z8ycoSB)
- [Solidity 开发总结](https://note.youdao.com/s/UkFO5MJK)
- [OpenZeppelin 合约研究](https://note.youdao.com/s/51bEF9pg)
- [以太坊提案EIP系列介绍](docs/web3/以太坊提案EIP系列介绍.md)
- [区块链上的event、logs、topics](docs/web3/区块链上的event、logs、topics.md)
- [gas费的一些概念](https://note.youdao.com/s/UovmSPyL)
- [MetaMask插件源码分析](docs/web3/MetaMask插件源码分析.md)
- [根据合约内部验签构造TypedData签名、ethers.js执行合约并获取日志](docs/web3/根据合约内部验签构造TypedData签名、ethers.js执行合约并获取日志.md)
- [对比ethers.js和solidity的签名方法](docs/web3/对比ethers.js和solidity的签名方法.md)
- [检查交易状态的情况总结](https://note.youdao.com/s/GrBSgVcq)
- [gusto使用的基础技术栈整理汇总](https://note.youdao.com/s/YfDjJSmq)
- soundsright sdk 设计笔记
  - [【草稿】Node端 SDK设计文档](docs/web3/soundsright%20sdk设计笔记/【草稿】Node端%20SDK设计文档.md)
  - [【草稿】前端 SDK设计文档](docs/web3/soundsright%20sdk设计笔记/【草稿】前端%20SDK设计文档.md)
  - [【草稿】lyrra游戏中使用的sdk设计](docs/web3/soundsright%20sdk设计笔记/【草稿】lyrra游戏中使用的sdk设计.md)
  - [【草稿】从使用者的角度考虑sdk的设计](docs/web3/soundsright%20sdk设计笔记/【草稿】从使用者的角度考虑sdk的设计.md)
  - [sdk技术栈选型](https://note.youdao.com/s/XW5Iw9w8)
  - [SoundsRight monorepo 创建笔记](https://note.youdao.com/s/3CN0KL2o)
- [web3-react 源码分析](https://note.youdao.com/s/LvlDsFoz)
- [WalletConnect 钱包连接开发及测试](https://note.youdao.com/s/4xlA2fww)
- [钱包签名登录的网络安全问题](https://note.youdao.com/s/XSaNktiV)
- [Node端代理合约交易的执行流程](https://note.youdao.com/s/A14OdptG)
- [node服务操作链时序图](https://note.youdao.com/s/JxFDxIi7)
- [Node链交易流程多实例版](https://note.youdao.com/s/9vQbGFVV)
- [Node区块链交易存在的问题](docs/web3/Node区块链交易存在的问题.pdf)
- [懒铸造交易流程图](https://note.youdao.com/s/79yr25yu)
- [代币购买NFT的详细逻辑流程](https://note.youdao.com/s/FmriNgVZ)
- [法币支付订单系统时序图](docs/web3/法币支付订单系统时序图.drawio.png)
- [sendwyre 法币支付研究总结](docs/web3/sendwyre%20法币支付研究总结.pdf)
- [NFT购买产品流程图](https://note.youdao.com/s/56pijoSY)
- [ipfs开发文档](docs/web3/ipfs开发文档.md)
- [opensea Node端执行实现笔记](https://note.youdao.com/s/3IHI9tH4)
- [opensea中合约的集成指南](https://note.youdao.com/s/Rfkfc7Fi)
- [Opensea NFT市场研究](https://note.youdao.com/s/Q4wCyNzb)
- [可升级合约的部署原理、OpenZeppelin 可升级合约研究](https://note.youdao.com/s/a8sYv5l6)
- [全新的NFT购买方案 - 先铸造，后上传](https://note.youdao.com/s/N1vhMYOl)
- [懒铸造跟先铸造的NFT交易流程](https://note.youdao.com/s/GNwwVBzz)
- [NFT 中 MetaData（元数据）的格式规范](https://note.youdao.com/s/23qrzTpf)
- [rarible NFT Market 项目调研分析](https://note.youdao.com/s/3pUv3ycV)
- [Rarible 合约研究](https://note.youdao.com/s/TtNBLoAh)
- [rarible SDK nft 源码研究](https://note.youdao.com/s/TZQDZUP0)
- [rarible SDK APIs 整理](https://note.youdao.com/s/59CemLeL)
- [Finger NFT项目研究](https://note.youdao.com/s/FCMqGfA8)
- [finger NFT Market的用户和服务需要修改的部分](https://note.youdao.com/s/7U2TiehN)
- [NFT市场设计](https://note.youdao.com/s/VCGxD5LG)
- [Polygon 扫描链的方式探究](https://note.youdao.com/s/9dwsOzk1)
- [用svg绘制基于钱包地址的多色块头像](https://note.youdao.com/s/FTFHerDg)
- [游戏内登录流程图](https://note.youdao.com/s/5eIqDc6F)
- [算法稳定币](https://note.youdao.com/s/6ClZXgWq)
- [链开发 - 全链路](https://note.youdao.com/s/DSsqEPbK)
- [uniswap 研究](https://note.youdao.com/s/1DiotfWZ)
- [pancake-frontend 源码调研](https://note.youdao.com/s/crsnUg0e)
- [ethers中contract on的listener 中event对象的格式](https://note.youdao.com/s/JVxXzrnO)
- [区块链中转账的两种方式](https://note.youdao.com/s/6XJr3Rwa)
- [BSC（Binance Smart Chain）基础知识](https://note.youdao.com/s/7gHCLp1i)
- [什么是代币？如何发币？](https://note.youdao.com/s/2lOfozbM)
- [币圈知识](https://note.youdao.com/s/ID32shoE)
- [Rarible跨链NFT交易协议 - 知乎](https://note.youdao.com/s/BZYDqXwf)


## vue

### 项目
- [vue-lucky-card](https://github.com/u404/vue-lucky-card) - vue 组件，用于实现翻卡抽奖、塔罗牌抽奖等特效。
### 资料
- pinia - 一个简单灵活的创建和使用store的库
- [vue-vben-admin](https://github.com/vbenjs/vue-vben-admin) - vue3后台项目模板。主要技术栈：Vue3 + vite2 + ts + antd。项目功能极其完善，但其中对大量的antd组件进行了二次封装，具有较高的学习成本。[demo预览](https://vvbin.cn/next/#/dashboard/analysis)
- [vue-manage-system](https://github.com/lin-xin/vue-manage-system) - vue3后台项目模板。主要技术栈：Vue3 + pinia + Element Plus。vue2时期比较热门的一个后台项目，全面升级到了vue3。使用较为简单。[demo预览](https://lin-xin.gitee.io/example/work/#/dashboard)

### 笔记
- [vue常用的一些插件](docs/vue/vue常用的一些插件.md)
- [vue3基础理解](docs/vue/vue3基础理解.md)


## react

### 资料
- react-window - 用于react下大型列表和表格高效渲染
- [React 18 中新的 SSR 架构](https://github.com/stereobooster/react-snap/blob/HEAD/doc/alternatives.md)

### 笔记
- [vite2 + React + Antd 踩坑](https://note.youdao.com/s/SOdr5kBr)

## c端

### 资料
- [前端性能优化的最佳实践](https://csspod.com/frontend-performance-best-practices/) - [为什么速度很重要？](https://web.dev/why-speed-matters/) [自动化 Web 性能优化分析方案](https://juejin.cn/post/6844903933580673032)
- [MDN 关于Content-Encoding的说明](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding)
- [vite文档](https://github.com/vitejs/vite)
- [spriteJS 动画库](http://spritejs.com/#/zh-cn/index?id=%e7%ae%80%e4%bb%8b) - [demo](http://spritejs.com/demo/#/shader_and_pass/pass_wave)
- [fetch api 使用](https://www.ruanyifeng.com/blog/2020/12/fetch-tutorial.html)


### 笔记
- [c端常用模块](docs/c端/c端常用模块.md)
- [通用npm包、常用js库](docs/node端/通用npm包、常用js库.md)
- [PWA相关知识](docs/c端/pwa相关知识.md)
- [在javascript中学习数据结构与算法](https://note.youdao.com/s/dkqVGOjd)
- [从浏览器多进程到js单线程，js运行机制最全面的一次梳理](https://note.youdao.com/s/MEB3qvtd)
- [css3硬件加速注意事项](https://note.youdao.com/s/AQcRHpsn)
- [css 中的网格布局：display grid](docs/c端/css%20中的网格布局：display%20grid.md)
- [跨域问题的解决  iframe 跨域嵌套问题](docs/c端/跨域问题的解决%20%20iframe%20跨域嵌套问题.md)
- [TypeScript 笔记 new](https://note.youdao.com/s/1LyLsFsc)
- [css3 实现 渐变字体 和 镂空字体](docs/c端/css3%20实现%20渐变字体%20和%20镂空字体.md)
- [html 页面元素动画库](https://note.youdao.com/s/blREH5bg)
- [邀请注册机制的c端实现](https://note.youdao.com/s/Qbfbp8TV)
- [canvas 制作高特效页面](https://note.youdao.com/s/dUFKkGLd)
- [GraphQL 学习](https://note.youdao.com/s/8kBeV7B9)
- [分享到第三方参考资料](https://note.youdao.com/s/V9d64xUd)
- [Lyrra分享功能设计](https://note.youdao.com/s/KdPJ9ySL)
- [各个浏览器&服务器URL最大长度限制](https://note.youdao.com/s/PU7gyuIW)
- [wyre支付 java测试的问题](https://note.youdao.com/s/BACjd18n)
- [前端开发 多端框架](https://note.youdao.com/s/G7enMx02)
- [Http Cache Control - 缓存控制、缓存策略](https://note.youdao.com/s/4I6VdEAc)
- [项目初始化的配置](https://note.youdao.com/s/PbQJp575)
- [React和Vue使用中的区别](https://note.youdao.com/s/P5967Qgp)


## node端（服务、cli、框架、插件）

### 项目
- [api-service](README.md) - 供服务端调用的代理执行合约交易的服务。主要技术栈：midway、mysql(sequelize)、redis、ethers.js、ethereumjs-wallet、crypto(aes/des)、lark消息api。
- fe-service - 面向前端的服务，包括：身份校验、上传文件到aws、静态网页项目的发布、查询、管理等接口。主要技术栈：midway、mysql（sequelize）、redis、compressing(服务端压缩/解压)、crypto(sha256)、fs-extra、minimatch、nanoid、moment。
- internal-service - 内部接口服务，当前主要配合nginx实现针对google bot爬虫的网页伪静态化seo支持。
- [gusto-upload](README.md) - vscode插件，用于开发中快速上传文件到oss。后端部分由fe-service提供接口支持。
- [deploy-cli](README.md) - 实现快速纯静态前端项目部署的cli工具，结合fe-service服务，构成了完整的项目发布及回滚系统
- [smartcinema-file-upload](https://github.com/u404/smartcinema-file-upload) - vscode插件，早期制作的移动电影院内部用的上传插件。
- [fie-toolkit-frame](https://github.com/u404/fie-toolkit-frame) - 用于创建UI库的脚手架。能够快速创建出类似于element-ui的仓库。基于阿里[fie](https://github.com/fieteam/fie)工程工具的插件
- [fie-toolkit-vue-frame](https://github.com/u404/fie-toolkit-vue-frame) - 用于创建vue静态项目的脚手架。基于阿里[fie](https://github.com/fieteam/fie)工程工具的插件
- [fie-plugin-v](https://github.com/u404/fie-plugin-v) - 用于检索远程和本地版本，自动递增版本号。基于阿里[fie](https://github.com/fieteam/fie)工程工具的插件
- [fie-tookit-comp](https://github.com/u404/fie-toolkit-comp) - 用于创建公共vue组件的脚手架。基于阿里[fie](https://github.com/fieteam/fie)工程工具的插件
- [egg-vue-framework](https://github.com/u404/egg-vue-framework) - egg框架插件，用于构建一个egg服务端 + vue前端的一体化项目。
- [egg-vue-manage-system](https://github.com/u404/egg-vue-manage-system) - egg + vue-manage-system 制作的同时具有前端、后端功能的一体化项目的模板。

### 资料
- [yarn使用文档](https://yarnpkg.com/cli/install)
- [npx 使用教程](http://www.ruanyifeng.com/blog/2019/02/npx.html)
- [ESM 和 CommonJs 模块系统深入理解](https://mp.weixin.qq.com/s/Be-hUjPbtOPCdIpHIJX4ow)
- [Node 最新 Module 导入导出规范](https://juejin.cn/post/6972006652631318564#heading-9)
- [dts-bundle]() - 用于将项目中的.d.ts，打包到一起输出
- [NextJS处理服务器server端的npm模块](https://zhuanlan.zhihu.com/p/421441144)
- [redis教程](https://www.redis.net.cn/tutorial/3505.html)
- [Node 如何开发一个命令行工具](https://mbd.baidu.com/ma/s/wYyGcuaT)
- [关系型数据库表设计规范](https://blog.csdn.net/u013164931/article/details/79692402?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-2-79692402.pc_agg_new_rank&utm_term=bcnf%E8%8C%83%E5%BC%8F%E7%9A%84%E5%AE%9A%E4%B9%89&spm=1000.2123.3001.4430) - 第一范式，第二范式，第三范式，BCNF范式理解
- [git commit前检测husky与pre-commit](https://www.jianshu.com/p/f0d31f92bfab)

### 笔记
- [前端工程化工具设计](docs/node端/前端工程化工具设计.md)
- [deploy-cli初版设计方案](docs/node端/deploy-cli初版设计方案.md)
- [静态站点部署工具的执行流程](https://note.youdao.com/s/U67GRPoj)
- [node端开发常用库](docs/node端/node端开发常用库.md)
- [通用npm包、常用js库](docs/node端/通用npm包、常用js库.md)
- [node.js 常用的模块](https://note.youdao.com/s/KTrtAQ8M)
- [Chrome 插件开发笔记](https://note.youdao.com/s/FfK5MItz)
- [babel体系](https://note.youdao.com/s/CeVC6JZp)
- [Node Crypto模块加密解密](https://note.youdao.com/s/ISRa2eRS)
- [aws SDK 开发上传到oss文件功能](docs/node端/aws%20SDK%20开发上传到oss文件功能.md)
- [aws REST api 上传文件](docs/node端/aws%20REST%20api%20上传文件.md)
- [eslint 使用 prettier 的建议配置](docs/node端/eslint%20使用%20prettier%20的建议配置.md)
- [webpack打包分析](docs/node端/webpack打包分析.md)
- [配置文件命名rc的由来](docs/node端/配置文件命名rc的由来.md)
- [npm 安装依赖，安装前置依赖 peerdeps](docs/node端/npm%20安装依赖，安装前置依赖%20peerdeps.md)
- [利用RabbitMQ来更新Node服务的合约执行机制](docs/node端/利用RabbitMQ来更新Node服务的合约执行机制.md)
- [puppeteer 使用相关问题](docs/node端/puppeteer%20使用相关问题.md)
- [预渲染prerender的研究](https://note.youdao.com/s/RLVXZwwF)
- [脚手架工具 及其概念](https://note.youdao.com/s/SbPFLJXf)
- [lerna 包管理器](https://note.youdao.com/s/3nZS9o8K)
- [lerna publish发布npm包](https://note.youdao.com/s/ASHMEDXe)
- [rollup打包ts](https://note.youdao.com/s/PAunIHnp)
- [webpack打包异步模块的原理研究](https://note.youdao.com/s/KIqh32os)
- [在 typescript项目中配置eslint](https://note.youdao.com/s/GFP2f8gM)
- [mysql索引机制](https://note.youdao.com/s/KDuwMBcK)
- [第三方登录及钱包登录流程图](https://note.youdao.com/s/3uNsgJrK)
- [第三方登录及钱包登录的接口交互实现](docs/node端/第三方登录及钱包登录的接口交互实现.pdf)
- [第三方登录和钱包登录注册和登录用户的流程](https://note.youdao.com/s/GxPrA0nl)
- [海外第三方登录初步实现](https://note.youdao.com/s/dKZuB21H)
- [实现单点登录的方案有哪些？](https://note.youdao.com/s/1GPdLCd4)
- [MongoDB使用 - 高级查询](http://cw.hubwiz.com/card/c/543b2f3cf86387171814c026/1/1/9/)
- [Node 多进程模式 或 集群环境下定时任务执行方案](https://note.youdao.com/s/6AuNksks)
- [aws 集群redis中不可用的指令](https://note.youdao.com/s/L4AV6Eno)
- [SSR 框架项目](https://note.youdao.com/s/YGnpBXQj)
- [node服务器端mvc框架](https://note.youdao.com/s/TXhW0vPf)
- [TypeScript学习笔记](https://note.youdao.com/s/cuHCZNOS)
- [react项目中 typescript 编译配置说明](https://note.youdao.com/s/8qTIsvsB)
- [Node 端提高并发的做法](https://note.youdao.com/s/DGloTcys)

## 特殊项目

### 项目
- [dashjs-mpd-loader](https://github.com/u404/dashjs-mpd-loader) - 【视频 - 预加载研究】用于将mpd文件内容，转化为dash.js 支持的manifest对象。可以用来实现mpd文件的预加载。
- [MVP项目开发相关记录文档](https://note.youdao.com/s/J9RF71xW)




## seo的研究

### 笔记
- [网站seo改版方案分析以及相关资料](docs/seo的研究/网站seo改版方案分析以及相关资料.md)
- [seo 网页内部优化](docs/seo的研究/seo%20网页内部优化.md)

## 杂项知识

### 文档
- [代理翻墙原理](https://superxlcr.github.io/2018/07/01/%E4%B8%8A%E7%BD%91%E9%99%90%E5%88%B6%E5%92%8C%E7%BF%BB%E5%A2%99%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86/)
- [法币支付 VMade 支付对接](https://uspay.vmadeglobal.com/docs/cn/) - VMade 跨境支付服务
- [npm包趋势查看及对比](https://npmtrends.com/cross-spawn-vs-execa-vs-nice-try)

- [markdown 代码块(```code)支持的语言列表](https://www.jianshu.com/p/20f893a43960)
- [google分析的使用](https://zh.wikihow.com/%E4%BD%BF%E7%94%A8%E8%B0%B7%E6%AD%8C%E5%88%86%E6%9E%90%E7%BD%91%E7%AB%99)
- [aws 对象存储 s3 裁剪图片、压缩图片](http://www.wmmzz.com/awsduixiangcunchus3jiyulambdashixiantupiancaijian/) - [更多参考](https://www.google.com/search?q=aws+%E5%9B%BE%E7%89%87%E8%A3%81%E5%89%AA&oq=aws+%E5%9B%BE%E7%89%87&aqs=chrome.1.69i57j0i12i512l2j0i512l2.4684j0j15&sourceid=chrome&ie=UTF-8)
- [Git line endings （脚本换行问题）](https://www.jianshu.com/p/cee6312ac328) - git 取消自动修改crlf 、lf 行结尾

### 笔记
- [js常用的设计模式](https://note.youdao.com/s/8OXRAsYq)
- [monorepo 方案介绍](https://note.youdao.com/s/EoAsonUq)
- [git分支及CICD部署规则设计](docs/杂项知识/git分支及CICD部署规则设计.md)
- [海外付款平台对接开发指南](docs/杂项知识/海外付款平台对接开发指南.md)
- [sendwyre支付文档](https://note.youdao.com/s/YQGAkxn2)
- [stripe 支付](https://stripe.com/docs/payments/quickstart)
- [法币支付设计文档](https://note.youdao.com/s/LDzXG1Dg)
- [刷新hosts文件](docs/杂项知识/刷新hosts文件.md)
- [对于不好在国内安装的npm包的推荐处理方案](docs/杂项知识/对于不好在国内安装的npm包的推荐处理方案.md)
- [vscode格式化 解决 eslint(prettier prettier) 错误](docs/杂项知识/vscode格式化%20解决%20eslint(prettier%20prettier)%20错误.md)
- [nginx 配置手册](docs/杂项知识/nginx%20配置手册.md)
- [js音频可视化研究](docs/杂项知识/js音频可视化研究.md)
- [音频处理](https://note.youdao.com/s/5AWCS9z6)
- [npm 版本号规则简介](docs/杂项知识/npm%20版本号规则简介.md)
- [使用 npm init、yarn create、npx create-xxx 初始化项目](https://note.youdao.com/s/BNxeIKx8)
- [HTTP 状态码 301 和 302 的区别详解](https://blog.csdn.net/qq_43968080/article/details/107355758)
- [内部npm仓库搭建笔记 - 利用verdaccio](https://note.youdao.com/s/OcnvuaV1)
- [内部npm仓库搭建笔记 - 利用 cnpmjs](https://note.youdao.com/s/RDAxZVKe)
- [bit.dev 组件开发和共享](https://note.youdao.com/s/WagqAByQ)
- [乐观更新与保守更新](https://note.youdao.com/s/6VPhh28U)
- [window下安装node-sass](https://note.youdao.com/s/3QESZ5Am)
- [yarn create 报错](https://note.youdao.com/s/WHZDXVLU)
- [git 默认忽略文件名大小写](https://note.youdao.com/s/T9OmNgYK)
- [前端招聘简历初步筛选](https://note.youdao.com/s/THwmDBPq)
- [电话面试流程及题目](https://note.youdao.com/s/ZFmOBeWX)
- [mac下安装redis 客户端管理工具](http://www.jianshu.com/p/214baa511f2e)
- [vscode format 格式化自动 import 排序问题](https://note.youdao.com/s/7hn6TKFd)
- [git 无法使用ssh的问题排查](https://note.youdao.com/s/8jy3NSUT)
- [npx 安装报错 EPERM operation not permitted, mkdir ‘CUsersxx‘ 解决方法](https://note.youdao.com/s/Q24KYB5M)
- [同步vscode 本地配置 settings-sync的使用](https://note.youdao.com/s/HvBG0shZ)
- [递归实现阶乘](https://note.youdao.com/s/UrVgRabu)
- [js原理性问题](https://note.youdao.com/s/QmPsmdxd)
- [app内嵌网页、网页唤起app的原理](https://note.youdao.com/s/DHrUnBXn)

