# 小知识
### cli脚本获取当前执行的npm命令
可以通过 ```process.env.npm_lifecycle_script``` 来获取当前执行的npm命令全称，如```npm run build```
### 控制台输出不换行打印内容
```ts
process.stdout.write(`内容\r`);  // 不使用console.log，结尾不带\n
```
参考：https://www.cnblogs.com/taohuaya/p/16427408.html 
