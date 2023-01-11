## Google SEO 流程简介
1. 收录
   - 实现静态化网页——可选方式：预渲染、ssr、静态化、针对爬虫的特殊静态化。
   - [创建站点地图，并提交给google](https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap)

## 第一步：收录

#### 当前问题
当前网站技术选型是单页应用，纯客户端渲染渲染。这种方案的实质是：  
浏览器从服务器加载一个url时，只能加载到一个空的html文件，仅仅引用了css、js等脚本资源，没有任何数据内容。  
js加载执行后，通过ajax异步从api接口拉取数据，渲染出含有内容的页面。  
而搜索引擎爬虫，只会执行第一步，不会等待js执行及后续。因此对于目前的网站，无论输入什么样的url，加载出来的html都是一样的，没有内容。  

#### 解决方案探讨
要解决搜索引擎爬虫问题，唯一做法就是实现从直接从服务器（或cdn）输出含有内容的html，且针对各个url，输出指定的内容。比如，加载https://www.lyrra.io就输出带有首页内容的html，加载https://www.lyrra.io/detail?musicianId=17就输出相关音乐人的html。  

有几种实现方案可以选择：
- 预渲染——在构建工具打包编译页面时，直接生成相关页面的。最终所有页面都是静态页面。缺点较大，在内容更新时，需要重新编译发版。
- 针对爬虫机器人的预渲染——当识别到机器人时，直接输出我们的静态html。而对于普通用户，则正常输出SPA
- SSR——最干净的解决方案，在实现seo的同时，架构逻辑清晰，能够兼顾seo和用户体验。同时能够进一步做一些首屏渲染效率的优化。
- 纯静态化——最高并发的解决方案，类似于预渲染，也是直接生成静态页面，但页面更新方式主要是定时更新、配置时、发布时更新。有两种静态化的方式，一种是SSR模式，一种是页面抓取式。


## 使用prerender实现页面静态化及代理
必要条件，需要在 [在linux上下载chrome](https://www.cnblogs.com/rxysg/p/15672025.html)，
已经做了首页和nft详情页的测试，内容正常，但一次完整请求时间在十几到二十秒左右。



## 各种工具：

#### 站点地图生成工具：
https://www.xml-sitemaps.com/
https://xmlsitemapgenerator.org/free-online-sitemap-generator.aspx

#### 站点地图生成工具各种版本：
https://code.google.com/archive/p/sitemap-generators/wikis/SitemapGenerators.wiki


## 参考文档：

#### 谷歌SEO相关文档：
工具：[谷歌网页分析工具](https://search.google.com/search-console?resource_id=https%3A%2F%2Fwww.lyrra.io%2F)、[移动设备易用性测试](https://search.google.com/test/mobile-friendly)、
[谷歌官方文档：搜索引擎优化 (SEO) 新手指南](https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=zh-cn)  
[谷歌SEO最全教程，教你从0开始做谷歌SEO](https://zhuanlan.zhihu.com/p/490361939)     [谷歌SEO关键词分析操作文档（实操版）](https://zhuanlan.zhihu.com/p/556275227)  
[谷歌SEO的正确理解](https://zhuanlan.zhihu.com/p/476629225)  
[谷歌SEO入门指南：Google SEO优化怎么做](https://www.bilibili.com/read/cv17252988/)  
[谷歌SEO完整文档](https://developers.google.com/search/docs/fundamentals/get-on-google)  
[实现动态渲染](https://developers.google.com/search/docs/crawling-indexing/javascript/dynamic-rendering)  

#### 预渲染相关文档：
[prerender-spa-plugin](https://www.npmjs.com/package/prerender-spa-plugin)  
[prerender](https://www.npmjs.com/package/prerender) - 纯用于搜索引擎抓取的预渲染框架
[@prerenderer/prerenderer](https://www.npmjs.com/package/@prerenderer/prerenderer) - prerender-spa-plugin的底层核心模块
[react-snap](https://www.npmjs.com/package/react-snap) - cli 命令行处理预渲染，不依赖具体构建工具。

#### SSR相关文档：
[一文吃透React SSR服务端同构渲染](https://www.jianshu.com/p/63b7e78983ea)
[React 官方文档 - 水合——激活SSR的html结构](https://zh-hans.reactjs.org/docs/react-dom-client.html#hydrateroot)


