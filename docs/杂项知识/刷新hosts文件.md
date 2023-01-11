### 1、window环境：

hosts文件位置：C:\windows\system32\drivers\etc

刷新方式：

win+r，输入CMD，回车

在命令行执行: ```ipconfig /flushdns```     #清除DNS缓存内容。  
ps: ```ipconfig /displaydns```    //显示DNS缓存内容

 

### 2、linux环境

文件位置：/etc/hosts

刷新命令：```systemctl restart nscd```