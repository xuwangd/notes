
## webpack.config.js 中配置TS

```javascript
npm install ts-loader --save-dev
npm install typescript --save-dev
module.exports = {
  entry:'./src/app.ts',
  module:{
    rules:[
      {
        test: /\.tsx?$/,
        use:'ts-loader',
        exclude:/node_modules/
      }
    ]
  }
}
tsconfig.json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5",
    "allowJs": true
  },
  "include": [
    "./src/*"
  ],
  "exclude": [
    "./node_modules"
  ]
}　　
```

## 为什么要学习typescript

- 更好开发体验
- 解决js中一些难以处理的问题

## js开发中的问题

- 使用了不存在的变量 & 函数 & 成员
- 把一个不确定的类型当做一个确定的类型处理

    **例1**
    ```javascript
    let str = '10';
    str = 20;
    str.charAt(0);
    ```

- 使用null或undefined的成员

    **例1**
    ```javascript
    const obj = undefined;
    obj.name;
    ```

## js原罪

- 弱类型 某个变量 可以随时更改类型
- 解释性 看一段执行一段 运行时才发现错误

## TS

- typescript是js的超集 是一个可选的 静态的 类型系统 => 2012年微软发布

- 超集：js有的TS都有

- 可选：类型检查可用可不用

- 静态：静态的类型检查发生在编译的时候，而不在运行时 =>  浏览器 | node环境里无法直接识别TS代码 => 使用时要借助工具转换

- 类型系统：对代码中所有标识符 变量 & 函数 & 参数 & 返回值 进行类型检查

- js有类和对象 虽然不是面向对象的语言 但能面向对象开发

## 安装TS

- node里边搭建TS开发环境

    **例一**
    ```javascript
    npm install typescript -g
    tsc index.ts
    ```

## TS配置文件

- 手动创建tsconfig.json配置文件

    **例一**
    ```javascript
    {
        "compilerOptions":{//编译选项
            "target":"es2016",//配置编译目标代码的版本标准
            "module":"commomjs  es6"，//配置比编译目标使用的模块化标准
            "lib":[
                "es2016",//node环境
                //"dom"//浏览器环境
            ],
            //"files":["./src/index.ts"]//编译某个文件
            //"outDir":"./dist",//输出目录
            "strictNullChecks":true,//针对undifined null更严格的类型检查
            "removeComments":true,//编译的结果里面移除注释
            "noImplicitUseStrict":true,//编译结果里面不出现"use strict"
            "esModuleInterop":true,//启用非ES6模块化
            "noEmitOnError":true,//错误的代码不生成在编译结果里
            "moduleResolution":"node",//模块解析策略 相对路径 非相对路径
            "strictPropertyInitialization":true//更加严格的检查属性有没有初始化
        },
        "include":["./src"],//编译整个src目录
    }
    ```

- tsc --init生成配置文件

- 安装类型库 对引入各种的库 类型描述 npm install @types/node -D

- 使用了配置文件后 tsc 编译时不能跟文件名 否则会忽略掉配置文件

## 使用第三方库简化流程 保存 编译 转化生成js文件 运行 => 将ts代码在内存中完成编译，同时完成运行

**例一**
```javascript
npm install ts-node -g  

ts-node src/index.ts

npm install nodemon -g
nodemon --exec ts-node src/index.ts //检测文件是否发生变化 改变了直接执行ts-node 命令
package.json
{
    "script":{
        "dev":"nodemon --watch src -e ts --exec ts-node src/index.ts"
    },
}
```

## 如何进行类型约束

- 仅需要在变量 函数的参数 函数的返回值位置上加上 *:类型*

    **例一**
    ```javascript
    let test:string;
    ```

- :any any *类型能赋值给任意类型*
- :number
- :string
- :boolean
- :undefined *是其它类型的子类型，它们可以赋值给其他类型   配置文件中配置选项 => 使类型检查更加严格 => 只能赋值给自身*
- :null *是其它类型的子类型，它们可以赋值给其他类型   配置文件中配置选项 => 使类型检查更加严格 => 只能赋值给自身*
- :Array<number> | :number[] *每一项是number类型数组*
- :object
- 函数相关约束 函数重载

    **例一**
    ```javascript
    函数重载：函数实现之前，对函数调用的多种情况进行申明 明确了函数只有那几种情况
    function clickMe(a: number, b: number): number;
    function clickMe(a: string, b: string): string;
    function clickMe(a: number | string, b: number | string):number | string | void{
        if ( typeof a == 'string' && typeof b == 'string'  ){
            return a+b;
        } else if (typeof a == 'number' && typeof b == 'number'){
            return a + b;
        }
    }
    ```

- :void *约束函数的返回值 表示该函数没有任何返回*
- :never *函数永远不会结束 返回*

    **例一**
    ```javascript
    function throwError( mes ):never{ throw new Error(mes) }
    function throwError():never{ while(true){} }
    ```

- 字面量类型

    **例一**
    ```javascript
    let a:"A";
    let gender:"男" | "女";  
    let arr:[];//只能是个空数组 
    let arr:[string, number]  
    let obj:{};
    let obj:{
        name:string,
        age:number
    }
    ```

- ?: 可选参数 

    **例一**
    ```javascript
    function test(a:string, b?:string):number | string{
        //...
    }
    ```

- :类型 | 类型 *联合类型*
- 元祖类型 *一个固定数组 长度确定 每一项的类型确定*

## 类型别名

**例一**
```javascript
type User = {
    name:string,
    gender:'男' | '女'
}
let u:User;
u = {
    name:'xw',
    gender:'男'
}
```

## 枚举  （逻辑含义、逻辑的值 & 真实的值） 它会出现在编译结果中

- 枚举的字段值可以是字符串 | 数字

    **例一**
    ```javascript
    enum 枚举名{
        枚举字段1 = 值1,
        枚举字段2 = 值2
    }

    let gender:枚举名;
    gender = 枚举名.枚举字段1;

    function test(lev:枚举名){

    }
    test(枚举名.枚举字段1)
    ```

## 模块化

- TS中，导入 导出模块，统一使用ES6模块化标准

    **例一**
    ```javascript
    import {sun, name} from './xxx';
    export const name = 'xw';
    export function sun(a, b){ return a+b; }
    export default{
        name:'xw',
        sun:function(){}
    }
    ```

- 编译结果

    **例一**
    ```javascript
    如何编译的结果的模块化标准是ES6，没有区别
    如何编译的结果的模块化标准是commonjs，导出的声明会变成exports的属性，默认的导出会变成exports的default属性
    ```
    **例二**
    ```javascript
    import fs from 'fs';
    //fs: module.exports = {}; fs没有去默认导出
    //import { fs } from 'fs';
    // import * as fs from 'fs';
    ```

- 模块解析

## 接口

- TS的接口：用于约束类 对象 函数的契约 => 标准

    **定义接口**
    ```javascript
    interface User{
        name:string,
        age:number,
        sayHello():void
    }
    let u:User = {
        name:'xw',
        age:18,
        sayHello(){}
    }
    ```

    **类型别名定义单个函数**
    ```javascript
    type cb = (n:number)=>boolean;
    type cb = {
        (n:number)=>boolean
    }
    function sun(num:number[], callBack:cb){

    }
    ```

    **接口定义单个函数**
    ```javascript
    interface cb{
        (n:number):boolean
    }
    ```

    **接口继承**
    ```javascript
    interface A{
        T1:string
    }
    interface C{
        T3:string
    }
    interface B extends A,C{
        T2:number
    }

    let u:B = {
        T3:'cool',
        T2:18,
        T1:'xw'
    }
    ```

    **readonly 只读修饰符，不在编译结果中**
    ```javascript
    interface User{
        readonly id:string,
        name:string,
        age:number
    }
    let u:User = {
        id:'68',
        name:'xw',
        age:18'
    }

    const arr: readonly number[] = [1, 2, 3];//arr定义好后 就改不了了
    const arr: ReadyArray<number> = [1, 2, 3];
    type User = {
        readonly arr: readonly number[],
    }
    interface User {
        readonly arr: readonly number[]
    }
    ```

## 类型兼容性 & 类型断言

- B 赋值给 A，如果能完成赋值 则B和A类型兼容 
- 鸭子辩型法 （子结构辩型法）：目标类型需要某一些特征，赋值的类型只要能满足该特征即可
- 基本类型：完全匹配
- 对象类型：鸭子辩型法 类型断言 当使用对象字面量直接赋值时，TS会更严格类型检查
- 函数类型：传递给目标函数的参数可少，但不能多。返回值要求要返回，就必须按要求返回
    **例一**
    ```javascript
    interface Duck{
        sound:'gagaga',
        swin():void
    }
    let person = {
        name:'一个伪装成duck的人',
        age:18,
        sound:'xixixi' as 'gagaga',
        swin(){
            console.log('swin')
        }
    }
    let duck:Duck = person;
    ```

## TS中的类

**属性初始化 属性可以修饰为可选的 属性可以修饰为只读的 访问修饰符 访问器**
*每个属性默认是public 私有的只有在类中可以访问private*
```javascript
class User{
    name:string
    age:number
    private gender:"男" | "女" = "男"
    public pid?:string
    readonly id:number
    constructor(name:string, age:number, gender:"男" | "女" = "男"){
        this.id = Math.random();
        this.name = 'xw';
        this.age = 18;
    }

    set age(value){
        //...
    }
    get age(){
        return this.age;
    }
}

```