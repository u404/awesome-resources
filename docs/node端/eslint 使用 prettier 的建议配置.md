### 1、首先是eslint自身的安装和创建配置文件
参考：https://www.npmjs.com/package/eslint

```ts
// 初始化方式 1
$ ./node_modules/.bin/eslint --init // win系统中，需要反斜线：.\node_modules\.bin\eslint --init
// 初始化方式 2
npm init @eslint/config
```

### 2、对于typescript 可以安装 @typescript-eslint/eslint-plugin
参考：https://www.npmjs.com/package/@typescript-eslint/eslint-plugin  
参考：https://typescript-eslint.io/docs/  
安装：```yarn add -D @typescript-eslint/parser @typescript-eslint/eslint-plugin```  

配置 - 使用插件建议的配置：  
```json
{
   "parser": "@typescript-eslint/parser",
   "extends": [
    "...",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "plugin:prettier/recommended"
  ],
  "plugins": ["@typescript-eslint"],
}
```

### 3、使用prettier建议的代码风格
参考：https://www.npmjs.com/package/eslint-plugin-prettier  
安装：```yarn add -D prettier eslint-config-prettier eslint-plugin-prettier```  
配置：  
```json
{
  "extends": [
    "...",
    "plugin:prettier/recommended"  // 必须放在最后
  ]  
}
```
另外可以在项目中增加 .prettierrc.json 来覆盖一些默认配置，具体可参考：  
https://prettier.io/docs/en/options.html  
https://github.com/prettier/eslint-config-prettier/  

### 最终配置文件参考

.prettierrc.json 参考
```json
{
  "bracketSpacing": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "arrowParens": "avoid",
  "printWidth": 120
}
```
.eslintrc.js 参考
```js
module.exports = {
  env: {
    es2021: true,
    node: true
  },
  extends: [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "plugin:prettier/recommended"
  ],
  plugins: ["@typescript-eslint"],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    tsconfigRootDir: __dirname,
    project: ["./tsconfig.json"],
  },
  rules: {
    "@typescript-eslint/no-unsafe-return": "off",
    "@typescript-eslint/no-unsafe-member-access": "off",
    "@typescript-eslint/no-unsafe-argument": "off"
  }
}

```