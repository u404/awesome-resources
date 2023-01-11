### Delete `·` eslint(prettier/prettier) 的错误
一种方案：  
默认的 VSCode TypeScript 格式化程序弄乱了大括号。在 .vscode/settings.json 添加：  
```json
{
  "javascript.format.insertSpaceAfterOpeningAndBeforeClosingEmptyBraces": false,
  "typescript.format.insertSpaceAfterOpeningAndBeforeClosingEmptyBraces": false,
}
```

参考：https://qa.1r1g.com/sf/ask/4316939301/