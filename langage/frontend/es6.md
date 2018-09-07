## ES6

```shell
#node 开启 ES6 支持
$ node --v8-options | grep harmony
```

## let 和 const

```javascript
// let 和 var 类似，不过 let 只能在代码块内有效
{
    let a = 3
    var b = 4
}
a // a is not defined
b // 4
/*
var 会存在变量提升，而 let 不会
let 不允许在同一代码块内声明相同的变量，会报错
 */

// const 声明一个只读的常量，一旦声明，值无法改变
// const 在声明的同时必须赋值，否则会报错
```



## Babel


```shell
# babel 是一个广泛使用的 ES6 转换工具
# babel-preset-latest 最新转码规则
npm install --save-dev babel-preset-latest
# babel-preset-stage-[0-3] 不同阶段语法提案的转码规则，选装一个(0最广)
npm install --save-dev babel-preset-stage-2
# babel-cli 用于命令行转码，会附带 babel-node REPL环境
npm install -save-dev babel-cli
 # 文件转码
 babel example.js --o compiled.js
 # 目录转码
 babel src -d lib
# babel-register 对 require 引入的文件转码，对当前文件不转码，且要 require('babel-register')
npm install --save-dev babel-register
# babel-core 提供 babel API
npm install --save babel-core
# babel-polyfill 由于 babel 默认只转换 javascript 语法，所以需要此插件转换新的 API
npm install --save-dev babel-polyfill
```

### .babelrc

```javascript
{
  "presets": [
    "latest",
    "react",
    "stage-2"
  ],
  "plugins": []
}
```

