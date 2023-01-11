### 1、基本反向代理配置
```
location /web {
    proxy_pass https://h5.smartcinemausa.com/web.html
}

// 正则语法
location ~* /tv/(.+)  {
  proxy_pass  https://h5.smartcinemausa.com/tv.html?code=$1;
}
```
### 2、Nginx 配置 path 详解、path穿透
https://blog.csdn.net/u010433704/article/details/99945557  
https://blog.csdn.net/lgxzzz/article/details/121722316  

.xml$  



### 配置指南：
https://www.kancloud.cn/louis1986/nginx-web/526912  

### 常见正则用法：
https://cloud.tencent.com/developer/article/1500808  
http://www.cocoachina.com/articles/43312  