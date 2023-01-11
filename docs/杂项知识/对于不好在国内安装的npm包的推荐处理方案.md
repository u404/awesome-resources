# 对于不好在国内安装的npm包的推荐处理方案

> 有些包在国内不好安装。现提供几个解决方案

### 使用 yarn 在 package.json 内配置(推荐)
```json
"resolutions": {
    "bin-wrapper": "npm:bin-wrapper-china"
  },

```

### 使用 npm，在电脑 host 文件加上如下配置即可

```txt
199.232.4.133 raw.githubusercontent.com
```

### 使用 cnpm 安装(不推荐)