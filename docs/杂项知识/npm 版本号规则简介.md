### 模块版本安装的规则* ^ ~ 以及锁定版本：

```
// * 意味着安装最新大版本的依赖包

// ^ 指的是只要不修改 [major, minor, patch] 三元组中，最左侧的第一个非0位，都是可以的
^1.2.3 匹配 >=1.2.3 < 2.0.0
^0.2.3 匹配 >=0.2.3 <0.3.0
^0.0.3 匹配 >=0.0.3 <0.0.4

// ~ 简而言之就是允许修改最后一位被指定的版本号
~1.2.3 匹配 >=1.2.3 <1.3.0
~1.2 匹配 >=1.2.0 <1.3.0 (Same as 1.2.x)
~1 匹配 >=1.0.0 < 2.0.0
~0.2.3 匹配 >=0.2.3 <0.3.0
~1.2 匹配 >=1.2.0 <1.3.0 (Same as 0.2.x)
~0 匹配 >=0.0.0 < 1.0.0(Same as 0.x)

// 不带标志，1.2.3则意味着指定版本的依赖包,即锁定版本号
```