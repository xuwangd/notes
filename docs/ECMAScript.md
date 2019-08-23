#### 浏览器发展史和主流浏览器以及内核

>V8 引擎是 JS 语法事实上的标准实现，Chrome 浏览器和 Node 的底层都用了它。它名字里面的 V 代表虚拟机（virtual machine），8 表示这是作者 Lars Bak 写的第8个虚拟机。

>Lars Bak 是一个传奇的丹麦程序员，在 V8 之前，他还写过 Java虚拟机、Smalltalk虚拟机、Dart虚拟机。下面是2009年，英国《金融时报》的报道。
>奥尔胡斯（Aarhus）是丹麦第二大城市，在该市郊外5英里的地方，有一座改造过的农舍。房子的主人叫 Lars Bak，是一个年轻的编程天才，他之所以把家安在这里是因为他非常不愿意让别人找到自己。他最近的作品 V8 是 Chrome 浏览器的一部分。
>1991年，他在 Sun 公司工作，后来成为业界最佳程序员之一，开发了 Java HotSpot。2000年初，他离开了硅谷，回到了丹麦。搬家是为了他的女儿们（他想让她们上丹麦语学校），也为了自己的身心健康。美国的工作很紧张，生活方式不健康。
>他并不特别想找新项目：他有足够的钱养家糊口，也有各种打发时间的方式，包括粉刷农舍的计划。他估计得要一年时间。这时，Google 的电话就来了。对于 Google，他是编写 JavaScript 引擎的最佳人选。巴克接受了这份工作，但不会回到加州。事实上他从没打算再次回加州，虽然谷歌的人性化办公室闻名远近，餐厅里的美食，还可以免费理发，巴克却宁可在家工作离总部5000英里，相差9个时区。

- *mosaic*(1993)
- *网景*、*微软*、*firefox*(*网景*代码开源后)
- *网景*(`ECMAScript`1996)和*sun*(`java`)合作推出`javascript`
- *IE* 、 *chrome*(v8引擎`c`语言编写、将`javascript`代码直接翻译成机械码) 、 *firefox*(路径的优化)对JS引擎优化

| 主流浏览器 | 主流浏览内核 |
|----|---------|
| ie | trident |
| chrome | webkit/blink |
| firefox | gecko |
| opera | presto |
| safari | webkit(chrome和苹果一起研发) |

#### 浏览器基本组成

1. 用户界面
2. 浏览器引擎
3. 渲染引擎（渲染页面）
4. 网络（网络请求、网络发送）
5. UI后端（弹个警告框）
6. js引擎
7. 数据存储

#### 什么是渲染、渲染引擎、渲染过程
- 渲染：在电脑绘图中是指用软件从模型生成图像的过程
- 渲染引擎：其职责就是渲染，即在浏览器窗口中显示所请求的内容
- 过程：下载解析`HTML` 下载`js`、`CSS`、`img`等等，根据`domAPI`构建DOM树、`cssRule`构建CSSRule树、两者合并构建Render树、布局Render树、绘制Render树

#### HTML、CSS 、js

- HTML：超文本标记语言，不是编程语言(变量、函数、数据结构、数据运算)只是计算机语言，组织架构、结构布局
- CSS：层叠样式表，不是编程语言(变量、函数、数据结构、数据运算)只是计算机语言，样式装饰、简单css动画
- js：人机交互

#### web标准化

1. 行为、样式、结构相分离，对应的标准也分为三方面
2. 行为标准语言主要包括文档对象模型`w3c DOM`和`ECMAScript`
3. 表现标准语言主要包括`CSS`
4. 结构化标准语言主要包括`HTML`和`XML`

#### 编译型语言、解释型语言

- 弱类型指变量的类型由值决定，变量可以随时更改类型。
- 解释型指看一行执行一行，等到运行时才能知道结果，js要运行时才发现错误。
- 脚本语言，不具备开发操作系统的能力。
- 嵌入式，嵌入大型应用程序（浏览器），达到控制程序目的。
- 不提供I/O API，依靠宿环境提供。

| 编译 | 解释 | 非编译、非解释 |
|-----|------|---------------|
| `C` 、`C++` | `javascript`、`php` | `java` |
| 快、不跨平台 | 稍慢、不存在跨不跨平台 | `.java` `.class` `jvm`编译成字节码，然后运行 |

#### 数据类型

| 原始值 | 引用值 |
|-------|--------|
| `number` `string` `boolean` `undefined` `null` `symbol` | `Array` `Object` `Function` `RegExp` `Global(window)` `Number` `String` `Boolean` `Date` `Math` `Error` |

#### 六种值的布尔类型为`false`

```javascript
false 
undefined 
null 
0 
NaN 
''
```

#### typeof的返回结果

```javascript
'number' 
'string' 
'boolean' 
'undefined'(未经声明也未赋值的变量为undefined) 
'object' 
'function' 
'symbol'
```

#### 运算符

- 字符串加任何东西都得字符串
- `+=`得字符串
- `++`和`--`走`number`数字类型转换

- `undefined` `null` `NaN` `symbol`
  1. 都不 大于 小于 等于 任何值包括自身（undefined null symbol有点特殊）
  2. undefined == null  undefined == undefined  undefined === undefined
  3. null == undefined null == null null === null
  4. let oSmybol = Symbol('Symbol.iterator'); oSmybol == oSmybol //可用来做私有属性，比如迭代器属性

- ```
  + - * / %
  ++ -- += -= *= /= %=
  > >= < <= = == === != !==
  && || !
  ```

#### 显示类型转换和`Math`

```javascript
Number(value) // false 0  null 0  true 1
String(value)
Boolean(value)
parseInt(value,2-36)
parseFloat(value)
value.toString(2-36) // undefined null没有构造函数 所有不存在有属性和方法 自然不能调用toString方法  

isNaN() // number数字类型转换后看是否是NaN
num.toFixed(n)

Math.round(num)
Math.abs(num)
Math.max(num)
Math.min(num)
Math.ceil(num)
Math.floor(num)
Math.pow(num,1/2)
Math.sqrt(num)
Math.atan2(y,x)
Math.PI
Math.random()
```

#### 语句

```javascript
const value = true;
if(value){

}

switch(value){
    case 1:
        break;
    case 2:
        break;
    case 3:
        break;// break可用于swith、循环，在循环中表示跳出整个循环
    default:
        console.log('hello world!')
        break;
}

while(!value){

}

do{

}while(!value)

for(let i=0; i<value; i++){
    continue;// continue用于循环，循环中表示跳出该次循环
}

let num = value ? 1 : 2;

```

#### 函数

每一个javascript函数都是一个对象，有些属性可以访问，有些不能访问（仅供引擎存取用）
```javascript
function transfer(){
    transfer.length;
    transfer.name;
    transfer.caller
    arguments
    arguments.length
    arguments.callee
    实参和形参相映射（相对应的实参和形参才映射，否者不会）
}
```

#### 作用域链（执行期上下文的集合）

1. 函数被定义后它的scope属性中就存着作用域链（执行期上下文的集合）了
2. 在函数内部查找变量会在该函数的作用域链（执行期上下文集合）上自顶向底查找
3. 函数每次执行都会生成自己的独一无二的执行期上下文，执行完就销毁自己的执行期上下文
4. 函数的scope属性中存着的作用域链（执行期上下文的集合）由该函数所处的环境决定

#### 函数预编译（发生在函数执行前一刻）

1. 未经声明直接赋值的变量由window对象所有，全局声明的变量为window对象的属性，GO对象就是指window
2. 函数的预编译环节发生在函数执行的前一刻
3. 函数预编译步骤
    1. 创建AO对象{}
    2. 形参、变量声明的名作为AO对象的属性名，并赋值为undefined
    3. 形参与实参相统一
    4. 函数声明的名作为AO对象的属性名，并赋值为函数体

#### 原型链

1. 原型（对象）是构造函数的一个属性
2. 原型是构造函数制造出来的对象的公有祖先
3. 通过构造函数产生的对象会继承该原型的方法和属性

#### new

```javascript
function Fun(){
    var this = {};
    AO: {
        this:{
            __proto__:Fun.prototype:{
                constructor:function Fun(){},
                __proto__:Object.prototype:{
                    //...
                }
            }
        },
        arguments
        //...
    }
    return this;
}
new Fun();
```

#### 继承方式

```javascript

//原型链方式继承了过多没用的私有属性，constructor指向错乱
G.prototype.lastName = 'xiao';
function G(){

}
F.prototype = new G()
function F(){
    var __closure = 1;
    this.hello = 'world';
    this.obj = {
        name:'xiao'
    }
    this.say = function(){
        console.log(this.hello)
        console.log(__closure)
    }
}
S.prototype = new F()
function S(){
    this.hello = 'haha';
}
var Son = new S()



//借用构造函数方式每次多执行一个函数，并未达到公有属性目的
function G(name,sex) {
    this.name = name;
    this.sex = sex;
}
function S(height,name,sex){
    G.call(this,name,sex)
    this.height = height;
}
let Son = new S(170,'xiao','male')


//共享原型方式原型上属性和方法会相互影响，constructor指向错乱
G.prototype.lastName = 'xiao';
function G(){

}
function S(){

}
S.prototype = G.prototype;
let Grand = new G()
let Son = new S()



//圣杯模式方式constructor指向错乱
let inherit = (function(){
    function F(){}
    return function(target, origin){
        F.prototype = origin.prototype;
        target.prototype = new F()
        target.prototype.constructor = target;
        target.prototype.super = origin.prototype;
    }
}())
function G() {}
function S() {}
inherit(S,G)
let Grand = new G()
let Son = new S()



//Object.setPrototypeOf
function G(){}
function S(){}
Object.setPrototypeOf(G.prototype, S.prototype)
let oG = new G();

等价于👇

function G(){}
function S(){}
G.prototype.__proto__ = S.prototype;
let oG = new G();

```

#### 闭包

- 部函数被返回到外部就会产生闭包，闭包会导致内存蟹泄露
- 利用闭包可以 1. 变量私有化 2. 变量公有化 3. 缓存 4. 模块化开发、防止污染全局变量

#### 立即执行函数

```javascript
//只有表达式才可以被执行符号执行
//函数成为表达式后会忽略掉自己的名（再找不到该函数索引）
(function () {
   
}())

let fn = function(){

}();

!function(){

}()

```

#### `call` `apply` `bind`

1. 改变`this`指向，传参列表不同
2. `call` `apply`是立即就执行
3. `bind`是返回一个全新的函数，等待被执行可以`new`

#### this

1. 函数预编译环节this指向window
2. 全局作用域里this指向window
3. call apply bind改变this指向
4. obj、Function.prototype中的函数被谁调用this就指向谁
5. new时this指向实例

#### 包装类 & 对象

```javascript

//创建对象的方式
1. 字面量
2. 构造函数 //系统构造函数、自定义构造函数大驼峰
3. Object.create(null)



//对象相关的
for(var prop in obj){}

prop in obj //该对象有没有这个属性
obj.hasOwnProperty(prop)  //该属性是不是该对象自己的属性
obj instanceof Fun  //A对象的原型链上有没有B函数的原型

Object.assign(obj, obj1, obj2, ...) //浅层克隆 obj拥有obj1 obj2 等对象的属性
Object.keys(obj)  
Object.values(obj)
Object.is(n1, n2) //n1 n2两者是不是相等

```

#### 数组

```javascript

//创建数组的方式 & 类数组
1. 字面量
2. 构造函数 //系统构造函数、自定义构造函数大驼峰

//数组相关的
Array.prototype.myForeach = function (fn) {
    var self = this,
        parmas = arguments[1] || window,
        len = self.length;
    for (var i = 0; i < len; i++) {
        fn.apply(parmas, [self[i], i, self])
    }
}

Array.prototype.myMap = function (fn) {
    var self = this,
        len = self.length,
        parmas = arguments[1] || window,
        resArr = [];
    for (var i = 0; i < len; i++) {
        resArr.push(fn.apply(parmas, [self[i], i, self]))
    }
    return resArr;
}


Array.prototype.myFilter = function (fn) {
    var self = this,
        parmas = arguments[1] || window,
        len = self.length,
        resArr = [];
    for (var i = 0; i < len; i++) {
        fn.apply(parmas, [self[i], i, self]) && resArr.push(self[i]);
    }
    return resArr;
}

Array.prototype.myEvery = function (fn) {
    var self = this,
        parmas = arguments[1] || window,
        len = self.length;
    for (var i = 0; i < len; i++) {
        var state = fn.apply(parmas, [self[i], i, self]);
        if (!state) {
            return false;
        }
    }
    return true;
}

Array.prototype.mySome = function (fn) {
    var self = this,
        parmas = arguments[1] || window,
        len = self.length;
    for (var i = 0; i < len; i++) {
        var state = fn.apply(parmas, [self[i], i, self]);
        if (state) {
            return true;
        }
    }
    return false;
}

Array.prototype.myReduce = function (fn) {
    var self = this,
        len = self.length,
        init = arguments[1],
        parmas = arguments[2] || window;
    for (var i = 0; i < len; i++) {
        init = fn.apply(parmas, [init, self[i], i, self]);
    }
    return init;
}
for(var prop in arr){

}
for(var item of arr){

}

let arr = {
    length:0,
    push:Array.prototype.push,
    splice:Array.prototype.splice
};

//改变原数组
arr.push(1,2,3)
arr.pop()
arr.unshift()
arr.shift()
arr.reverse()
arr.sort(function(a,b){
    return a-b;
})
arr.splice(0,0,1,2,3,4) //从哪开始截，截多长，在截取处填入的新数据

//不改变原数组
arr.toString()
arr.concat()
arr.slice() //从哪开始截，截到哪（这里不算在内）
arr.join('')
arr.split('')

```

#### 字符串

```javascript

//创建字符串方式
1. 字面量
2. 构造函数 //系统构造函数、自定义构造函数大驼峰


//字符串相关的

String.fromCharCode('97') //传入Unicode编码返回对应的字符
str.charCodeAt(1) //返回str字符串索引值为1对应的子串的Unicode编码，大于等于255字节长度为2，小于255字节长度为1
str.charAt(1) //返回str字符串索引值为1对应的子串
str.toUpperCase() //返回转为字母大写后结果
str.toLowerCase() //返回转为字母小写后结果
```

#### 数字和其他

```javascript
encodeURIComponent(value)
decodeURIComponent(value)
num.toLocaleString()
```

#### `Date`

```javascript

let date = new Date([2020, 1, 1]);
date.getDate()										date.setDate()
date.getDay()           							date.setDay() //0特殊处理
date.getMonth()   								    date.setMonth() //0-12
date.getFullYear()									date.setFullYear()
date.getHours()										date.setHours()
date.getMinutes()									date.setMinutes()
date.getSeconds()									date.setSeconds()
date.getTime()										date.setTime()//1970.1.1至今的毫秒数

```

#### 时间线

```javascript

// onload
script.onload = function(){
    // 异步script文件加载完成
}
img.onload = function(){
    // 图片资源加载完成
}
window.onload = function(){
    //document整个文档加载并解析完成并且document.readyState = 'complete'后
}

// onreadystatechange
document.onreadystatechange = function(){
    //'loading' 'interactive' 'complete'
    console.log(document.readyState)
}
```

1. 创建Document对象，开始解析web文档，解析HTML元素和他们的文本内容后，添加Element对象和文本节点到文档中，
2. document.readyState = 'loading';
3. 遇到link外部css，创建线程加载，继续解析文档
4. 遇到script（没有async、defer属性）外部js，浏览器加载，并阻塞，等加载完成并脚本执行完，继续解析文档
5. 遇到script（有async、defer属性）外部js，创建线程加载，继续解析文档
6. 遇到img，先正常解析dom结构，然后浏览器异步加载，继续解析文档
7. 当文档解析完成，document.readyState = 'interactive';
8. 文档解析完后，所有带async外部脚本加载完就执行，所有带有defer外部脚本按顺序执行，document对象触发DOMContentLoaded事件
9. 当所有加载完后，document.readyState = 'complete'; window对象触发onload事件
10. 整个文档进入事件驱动阶段

#### 错误捕获

```javascript

try{
    //try里代码没错误正常执行，try里代码有错误则try里的错误代码后的代码不会执行
}catch(e){
    //try里代码没错误不执行catch里代码，try里代码有错误才会执行catch里代码
    //e.name    错误名
    //e.message 错误信息
}

throw new Error('error')

window.onerror = function(){

}

```

#### 'use strict' 标准模式

```javascript

a = 6;
var a = b = 6;

arguments.callee;
fun.caller;

with(document){
    改变作用域链、简写代码、消耗效率、程序变慢
    它会把传进来的对象document做为它所圈定的代码体的作用域链的最顶端
}
eval('能改变作用域、能把字符串里的JS代码执行')

function test(){
    console.log(this)//es5里这个this必须赋值，除了全局
}

test()//undefined
new test()//test {}
test.call({})//Object {}
test.call(123)//123

```