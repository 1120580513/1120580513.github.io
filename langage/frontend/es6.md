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

## 解构

```javascript
let [a,b,c] = [1,2,3]//完全解构
let [a] = [1,2]//不完全解构
let [a,b] = [1]// b is undefined


```

## 扩展

### String

```javascript
let s = 'hello world'
s.includes('hel')     //是否找到 字符串 
s.startsWith('h')     //是否以 字符串 开头
s.endsWith('d')		  //是否以 字符串 结尾
‘a'.repeat(3)         //将 's' 重复三次 
'a'.padStart(5,'c')	  //字符串补全
'a'.padEnd(5,'c')     //字符串补全
`abc${s}xxx`          //字符串模板
//标签模板
function tag(val, ...values){
	console.log(val)
	console.log(values)
}
let a = 3
let b = 4
tag`abc ${a} xx ${b} yy`
String.raw`asdfsdaf:\unicode`   //转义 \ 为 \\
```

### RegExp

```javascript
new RegExp(/abc/ig,'i')       //第二个参数会覆盖 ig
/^\uD83D/u 					  //增加 u 修饰符，用于正确处理 unicode 匹配中文时需要使用
/a+/y						  //增加 y 修饰符，后一次匹配必须是上一个匹配的下一个位置 粘连
/.*/s						  //增加 s 修饰符，使 . 真正的匹配任意字符
/(?<=a)|(?<!x)/				  //增加 后行断言
//增加 具名组匹配 ，且可使用解构
let {groups:{one:a,two:b}} = /(?<one>\w+):(?<two>\w+)/u.exec('abc:qwe')
/(?<word>.*):\k<word>/	      //使用 \k<组名> 引用分组
"t1t2t3".matchAll(/t\d/g)	  //匹配所有
```

### 数值

```javascript
0b111110111 === 503 		   //零b开头表示二进制
0o767 === 503 				   //零o开头表示八进制
Number(0b111110111)			   //转为10进制

//传统方法会调用Number()转为数字 而es6的方法不会
Number.isFinite(true)          //检查参数是否是值类型
Number.isNaN(true)  		   //检查参数是否是 NaN

Number.isInteger()             //判断是否是整数
Number.EPSILON                 //0 到 最接近 0 的浮点数的差
Number.isSafeInteger(Number.MIN_SAFE_INTEGER) //js 是否能精确表示 精确值的最大值
Number.isSafeInteger(Number.MAX_SAFE_INTEGER) //js 是否能精确表示 精确值的最小值
```

#### Math

```javascript
Math.trunc()                   //去除小数部分
Math.sign()					   //正数=>+1，负数=>-1，0=>0，-0=>-0，else=>NaN
Math.cbrt()                    //计算立方根
Math.clz32()                   //返回32位形式前导0的个数
.......
2 ** 2                         //2 * 2 * 2 对数运算
```

### 对象

```javascript
//对象 及 函数 简写
let str = 'abc'
var obj = {
    str,
    method(){return 'x'},
    get attr(){return this.str},
    set attr(val){this.str = val}
}
obj["a"+"bc"] = function(){return 3}         //属性名表达式
obj.abc.name							     //得到方法的名称
Object.is('foo', 'foo')                      //判断值是否一样
Object.assign(obj,{x:3},{y:4})			     //合并对象(浅拷贝) 类似 $.Extend
Object.getOwnPropertyDescriptor(obj, 'foo')  //得到对象是否可枚举
//属性的遍历
for...in                                     //+自身 +继承
Object.keys(obj)                             //+自身
Object.getOwnPropertyNames(obj)              //+不可枚举属性 +自身 +继承
Object.getOwnPropertySymbols(obj)            //+Symbol
Reflect.ownKeys(obj)                         //+ALL

Object.getOwnPropertyDescriptors(obj)        //得到属性对象的描述
obj.__proto__                                //可使用该对象，操作原型链
Object.setPrototypeOf({}, null);             //设置原型链
Object.getPrototypeOf(obj);                  //读取原型链
let obj = {find(){super.toString()}}         //super 调用原型链中的方法

Object.keys(obj)                             //对象自身可遍历的属性数组
Object.values(obj)                           //对象自身可遍历的键值
Object.entries(obj)                
```

