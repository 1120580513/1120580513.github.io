## ES6

> [ECMAScript6入门](http://es6.ruanyifeng.com/)
>
> ES6的模块自动采用严格模式，不管是否在顶部加 use strict

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

Object.keys(obj)                             //对象自身可遍历的k
Object.values(obj)                           //对象自身可遍历的v
Object.entries(obj)                          //对象自身可遍历的kv  

let{x,y,..z} = {a:1,b:2,c:3,d:4}             //扩展运算符(取得可遍历但未被取值的值)
```

## Symbol

> 为防止属性值重复，导致覆盖。每个Symbol都不会相等

```javascript
//用于做属性名
const a = Symbol()     //由于是类型，不能使用new
let obj = {            //给对象赋值
    [a] = 3
}
obj[a]                 //只能通过[]取出不能 . 出
Object.getOwnPropertySybols(obj)   //遍历 Symbol
//值用于做常量
const type = {
    status = Symbol()
}

/***
Symbol.for('abc') 会登记，Symbol('abc')不会登记
Symbol.keyFor(a)  只能取出登录过的 Symbol 对象的值
***/
let a = Symbol.for('abc')
let b = Symbol.for('abc')
Symbol.keyFor(a)     // abc
//内置 Symbol 值
Symbol.hasInstance   //当对象调用 instanceof 时会调用此方法
Symbol.isConcatSpreadable //用于 Array.prototype.concat()时是否可展开
Symbol.species       //指向构造函数
Symbol.match         //指向str.match(myObject)
//当访对象被String.prototype.{0}调用时会返回该方法的值
Symbol.replace  
Symbol.search
Symbol.split

Symbol.iterator     //指向遍历器方法
Symbol.toPrimitive  //当转为原始数据类型时会调用此方法
Symbol.toStringTag  //调用 toString 时该返回值表示对象的类型
Symbol.unscopables  //指示使用with时哪些属性会被with排除
```

## Set & Map

### Set

> 类似数据，但成员唯一
>
> 可用来去除重复元素

```javascript
add(value)、delete(value)、has(value)、clear()
keys()、values()、entries()、forEach()
```

#### WeakSet

> 类似 Set ，成员只能是对象，弱引用 

```javascript
add(value)、delete(value)、has(value)
```

### Map

> k/v 集合，k 可以是任何类型

```javascript
set(k,v)、get(k)、has(k)、delete(k)、clear()
size
keys()、values()、entries()、forEach()
var map = new Map([
    [k,v]          //注意：数组表示 k/v
])
```

#### WeakMap

> k/v 集合，k 只能是对象，弱引用

## Proxy

> 对操作做拦截，可以对语言进行编程

```javascript
var obj = {proxy:new Proxy(target,handler)}
```

## Reflect

> 语言内部的 API ，与 Proxy 方法一一对应

```javascript

Reflect.apply(target, thisArg, args)
Reflect.construct(target, args)
Reflect.get(target, name, receiver)
Reflect.set(target, name, value, receiver)
Reflect.defineProperty(target, name, desc)
Reflect.deleteProperty(target, name)
Reflect.has(target, name)
Reflect.ownKeys(target)
Reflect.isExtensible(target)
Reflect.preventExtensions(target)
Reflect.getOwnPropertyDescriptor(target, name)
Reflect.getPrototypeOf(target)
Reflect.setPrototypeOf(target, prototype)
```

## Promise

> 用于解决异步编号，回调地狱

```javascript
let promise = new Promise(function(resolve, reject){
    if(/成功/){
        resolve(/成功返回值/)
    }else{
        reject(/失败返回值/)
    }
}).then(function(data){
    /data 就是 resolve 方法传的值/
},function(err){
    /err 就是 reject 方法传的值/  
}).catch(function(err){// catch 就是 then(null,function(err))
    /err 就是 reject 方法传的值/ 
}).finally(function(){//最后都会执行这个方法
})
//-----------
Promise.race([promiseObj1,promiseObj2])    //将多个 Promise 包装成一个，返回第一个改变状态的 Promise 对象
Promise.resolve()                //将现有对象包装成 Promise 对象
Promise.reject()                 //包装成 reject 状态的 Promise 对象
```

## Iterator & for ... of

> Iterator 表示一种可遍历的接口，主要供 for ... of 使用

```javascript
let obj = {
    [Symbol.iterator](){
        return {
            next(){
                return true
            }
        }
    }
}
```

## Generator

> Generator 是一个状态机，封闭了多个内部状态

```javascript
function * generator(){
    yield 'a'
    yield 'b'
    return 'c'
}
generator.next()       //得到下一个状态
generator.next(val)    //将上一次 yield 的值设为 val
generator.throw()      //在外部抛出错误并在内部捕获
generator.return(val)  //返回 val 并终结 Generator 函数 
```

### Thunk 函数

> 编译器传名调用时，通常将参数放到一个临时函数中，再将这个函数传入函数休，这个临时函数就叫 thunk函数
>
> Thunk 函数是自动执行 Generator 函数的一种方法

## Async

> Async 是 Generatior 的语法糖

## Class

> Class 基本相当于语法糖
>
> 类和模块的内部，默认是严格模式
>
> 类不存在变量提升

```javascript
let bar = new Symbol('bar')
const Point = class MyPoint{//Point 才是类的名称，MyPoint 是在内部使用的
    constructor(){// === Point.prototype.constructor
        if(new.target === undefined){
            //不是通过 new 调用的
        }
        //构造函数 可以为空
        this.x = 3    //只有显示使用 this 时，是定义在 this 对象上的
    }
    _privateFunc(){ } //私有方法，只是名称约定
    [bar](){ }        //使用 Symbol 外部无法访问
    get prop(){ return 'xxx' }
    set prop(val){ console.log(val) }
    * [Symbol.iterator](){ yield 'xx' }
    static staticFunc(){ }//静态方法
    static staticProp = 42//静态属性
	prop = 0              //实例属性
}
//继承
class ColorPoint extends Point{
    constructor(x, y, color){
        super(x, y)       //调用父类方法
    }
}
//__proto__
ColorPoint.__proto__ === Point //子类的 __proto__ 指向父类
ColorPoint.prototype.__proto__ === Point.prototype //子类原型属性的__proto__ 指向父类的原型
```

## Decorator

> 修饰器 | 特性 | 注解

```javascript
@testable
class MyTestableClass{
    @log
    add(a, b){return a + b}
}
function testable(target){
    target.isTestable = true
}
function log(target, name, descrptor){
    descriptor.val = //...
    return descriptor
}
```

### core-decorators.js

> core-decorators.js 是一个第三个模块，

```javascript
@autobind                 //将方法的 this 绑定原始对象
@readonly                 //只读
@override                 //检查子类的方法，是否正确覆盖了父类
@deprecate                //会在控制台显示一条警告，表示该方法将废除
```

## ArrayBuffer

> 以数据处理二进制