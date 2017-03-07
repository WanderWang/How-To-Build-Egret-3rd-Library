# 白鹭引擎 4.1 如何构建第三方库


## 背景

在白鹭引擎 4.1 版本开始，我们对第三方库的机制进行了重大改变。这使得所有白鹭引擎的第三方库都需要修改项目结构后再执行 egret build

## 为何进行改变 

在白鹭引擎 4.1 中，我们重新梳理了第三方库的项目结构，核心目的在于解决以下问题：

* 降低第三方库的复杂度，使其整体结构变得更为简洁易懂
* 拥抱 es2015 / npm / tsconfig.json 等国际标准或事实标准。使白鹭引擎可以更简单的去加载任意 JavaScript 模块，而非针对白鹭引擎进行定制



## 迁移文档

* 打开一个第三方库文件夹
* 删除 ```package.json```中的 modoules 字段
* 在项目中与 ```package.json```同级创建一个 ```tsconfig.json``` 文件
* 修改 ```tsconfig.json```文件，根据 TypeScript / JavaScript 不同类型的类库，进行不同的项目配置：

```
// JavaScript 类库
{
    "compilerOptions": {
        "target": "es5",
        "outFile": "bin/libtest/libtest.js",
        "allowJs": true
    },
    "files": [
        "src/a.js",
        "src/b.js"
    ]
}
```

```
// TypeScript 类库
{
    "compilerOptions": {
        "target": "es5",
        "outFile": "bin/libtest/libtest.js",
        "declaration": true
    },
    "files": [
        "src/a.ts",
        "src/b.ts",
        "libs/egret.d.ts"
    ]
}
```

* 如果项目是 JavaScript 类库，还需要在 ```package.json```中配置一个 ```typings```字段，并设置为一个自定义的 .d.ts 文件，如下所示


```
/** 项目结构
libtest
    |-- src
    |-- bin
    |-- typings
            |-- libtest.d.ts
    |-- tsconfig.json
    |-- package.json 
*/

// package.json
{
    "name": "libtest",
    "typings": "typings/libtest.d.ts"
}
```

* 完成上述操作后，执行 egret build，就会根据 ```tsconfig.json```中的 ```outFile```字段生成库文件，压缩文件以及 .d.ts 文件
