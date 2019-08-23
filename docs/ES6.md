
## babel

- ES6是javascript语言的一个新版本
- babel可以将我们语言进行降级，让浏览器支持
- 语言迭代目的：让语言适用于复杂的大型企业项目
- 直接网站上进行转化 [1.](https://babeljs.io) [2.](https://www.babeljs.cn)

    **babel 下载 配置 运行**
    ```javascript
    下载npm install @babel/core @babel/cli @babel/preset-env

    当前目录下创建名为 .babelrc 的文件 按照json格式写入编译时需要的借助的插件
    {
        "presets":[
            "@babel/preset-env"
        ],
        "plugins":[
            ["@babel/plugin-proposal-decorators",{"legacy":true}],
            ["@babel/plugin-proposal-class-properties",{"loose":true}]
        ]
    }

    npx babel xx.js -o xx.js --watch

    webpack.config.js中配置babel
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            }
        ]
    }
    [参考babel选项](https://www.webpackjs.com/loaders/babel-loader/)
    ```

## let & const

- let & const 不能变量重复声明  不能变量声明提升  配合{}形成块级作用域、临时死区
- const 不能重复赋值 定义的空间不能被改变

## ...运算符

- 读场景、写场景
    **收集**
    ```javascript
    function test(a, ...arg){//arg是数组
        console.log(arg)
    }
    test(1, 2, 3)
    ```

    **扩展**
    ```javascript
    let arr1 = [1,2];
    let arr2 = [3,4];
    let newArr = [...arr1, ...arr2];
    ```

    **ES7 针对对象也能展开**
    ```javascript
    let obj1 = {a:1, b:2};
    let obj2 = {c:3, d:4};
    let newObj = {...obj1, ...obj2};
    ```

    **浅层克隆**
    ```javascript
    Object.assign({}, obj1, obj2, obj3)//不需要用变量去接收 第一个参数就是目标对象
    JSON.stringify & JSON.parse
    ```

## 解构

- 两边结构相似
- 取别名 默认值
- 配合...运算符收集

## 箭头函数

- 不能new只能作为普通函数使用  
- 由外围最近一层非箭头函数决定该箭头函数的arguments & this;

## Object.defineProperty

- Function.prototype//不可写
- var test = 'test';//不可配置

    **可读 可写 可配置 可枚举**
    ```javascript
    var obj = {
        val:'xw'
    }

    var obj = {//注意set get对应方法的命名
        temp:'xw',
        set val(newVal){
            this.temp = newVal;
        },
        get val(){
            return this.temp;
        }
    }
    ```

    **默认不可写 不可配置 不可枚举**
    ```javascript
    let obj = {

    };
    Object.defineProperty(obj, 'val', {
        value:'val',
        writable:false,//可写性
        configurable:false,//可配置性
        enumerable:false //可枚举性
    })

    let temp = 'temp';
    Object.defineProperty(obj, 'val', {
        configurable:false,
        enumerable:false,
        set(newVal){
            temp = newVal;
        },
        get(){
            return temp;
        }
    })
    ```

    **例一**
    ```javascript
    let obj = {
        text:{
            text:'text'
        }
    };
    inp.oninput = function(){
        obj.text.text = this.value;
    }
    function upData(){
        p.innerText = obj.text.text;
    }
    upData();
    //监控对象值属性的值有没有发生改变
    observer(obj)
    function observer(data){    
        if(data == null || typeof data != 'object'){
            return data;
        }
        Object.keys(data).forEach((ele)=>{
            defineReactive(data, ele, data[ele]);
        })
    }
    //定义响应式
    function defineReactive(obj, key, val){
        observer(val);
        Object.defineProperty(obj, key, {
            set(newVal){
                val = newVal;
                upData();
            },
            get(){
                return val;
            }
        })
    }
    ```

    **例二**
    ```javascript
    function upData(){
        console.log('更新了！！！')
    }
    let arr = [1,2,3];
    let {push} = Array.prototype;
    Object.defineProperty(Array.prototype, 'push', {
        value:(function(){
            return function (...arg){
                push.apply(this, arg);
                upData();
            }
        }())
    })
    ```

## Proxy & Reflect

- 代码量 
- 后续添加的属性也能监听
- 数组也能监听

    **例一**
    ```javascript
    let obj = {
        name:'xw'
    };
    function upData(data){
        console.log(data);
    }
    let proxyObj = new Proxy(obj, {
        set(target, key, val, origin){
            Reflect.set(target, key, val);
            upData('set');
        },  
        get(target, key, origin){
            return Reflect.get(target, key);
            upData('get');
        },
        has(target, key){
            return key.indexOf('_') != -1 ? false : key in oData;//for in
        }
    })
    console.log(oProxyData.val);//get执行
    oProxyData.val = 10;//set一定执行
    ```

    **例二**
    ```javascript
    let obj = {
        text:'text',
        user:{
            name:'xw',
            age:18
        }
    }

    function observer(data){
        for(var prop in data){
            if( typeof data[prop] == 'object' ){
                data[prop] = proxyObj(data[prop]);
            }
        }
        return proxyObj(data);
    }

    let oProxyObj = observer(obj);
    function proxyObj(data){
        return new Proxy(data, {
            set(target, key, val, origin){
                Reflect.set(target, key, val);
                console.log('setset');
            },
            get(target, key, origin){
                console.log('getget');
                return Reflect.get(target, key)
            }
        })
    }
    ```

## class

- 面向对象的编程语言需要具备 封装 继承 多态

    **原型的继承**
    ```javascript
    Object.setPrototypeOf(a.prototype, b.prototype)  
    a.prototype.__proto__ = b.prototype; 
    Object.create({
        constructor:xxx,
        __proto__:xxx
    })
    ```

    **ES6 class关键字定义类 & 类的继承**
    ```javascript
    class Plane{
        static message(){//静态类属性
            return 'message'
        }
        static message2 = 'message2';//ES7静态类属性
        like = 'qd';//ES7私有属性
        construcor(name, age){//私有属性
            this.name = name;
            this.age = age;
        }
        
        fly(){//公有属性
            this.fly = 'fly';
            return 'fly';
        }
    }
    let oPlane = new Plane('xw', '18');

    class Test extends Plane{//类的继承extends关键字
        constructor(data){
            super();//相当于call
            this.data = data;
        }
    }
    ```

    **模拟babel编译后的ES6 class结果**
    ```javascript
    function _classCallCheck(_that, constructor){
        if( !(_that instanceof constructor) ){
            throw new Error('xxx');
        }
    }   
    function _createClass(constructor, prototypeProp, staticProp){
        if(prototypeProp){
            _defineProperties(constructor.prototype, prototypeProp)
        }
        if(staticProp){
            _defineProperties(constructor, staticProp)
        }
    }
    function _defineProperties(target, prop){
        prop.forEach((ele)=>{
            Object.defineProperty(target, ele.key, {
                value:ele.value,
                writable:true,
                configurable:true
            })
        })  
    }

    var pPlane = (function pPlane(){
        //...
    }())
    var Plane = (function(pPlane){
        
        return function(name){
            Object.setPrototypeOf(Plane.prototype, pPlane.prototype);//公有属性的继承
            _classCallCheck(this, Plane);//必须new

            pPlane.call(this, name);//私有属性的继承
            this.name = name;

            //静态类属性、静态类属性的继承、公有属性不能被枚举
            _createClass(Plane, [
                {   
                    key:'fly',
                    value:function(){
                        return 'fly';
                    }
                }
            ], [
                {
                    key:'message',
                    value:function(){
                        return 'message';
                    }
                }
            ])

        }

    }(pPlane))
    ```

## @ 装饰器

**例一**
```javascript
@myReadOnly
class Test{
    static num = '静';
    @myReadOnly
    text = '私';
    @myReadOnly('123')
    fly(){
        return '公'
    }
}

function myReadOnly(mes){
    return function (proto, key, descriptor){
        //test的原型、属性、配置参数
        console.log(proto, key, descriptor)
        descriptor.writable = false;
        descriptor.initializer();   
        descriptor.initializer = function(){
            return 6;
        }
    }
}
```

## 存储数据的结构 Set & Map

- 唯一性(去重)
- 任何值都能当做属性  

    **Set**
    ```javascript
    let mySet = new Set([1, 2, 3]); => {1, 2, 3};
    mySet.add([1,2])
    mySet.delete(1)
    mySet.has(1)//有没有1
    mySet.clear();
    mySet.forEach((val)=>{
        console.log(val)//1   2   3
    })
    ```

    **Array.from 可以将 类数组 & 具有迭代属性的值 & 字符串 转为数组**
    ```javascript
    let arr1 = Array.from(mySet);
    let arr2 = Array.from('hello');
    ```

    **Map**
    ```javascript
    let myMap = new Map([ ['name', 'xw'], ['age', 18] ]); => {'name'=>'xw', 'age'=>18};
    myMap.set('name', 'xw');
    myMap.get('name');
    myMap.delete('name')
    myMap.has('name');
    myMap.clear();
    myMap.forEach((val, key, self)=>{
        console.log(val, key, self)//
    })
    ```

    **模拟Map的功能实现 链表 hash算法 桶**
    ```javascript
    function myMap(){
        this.bucketLength = 8;
        this.init();
    }
    myMap.prototype.init = function(){
        this.bucket = new Array(this.bucketLength);
        for(var i=0; i<this.bucketLength; i++){
            this.bucket[i] = {
                type:'bucket_' + i,
                next:null
            }
        }
    }

    myMap.prototype.makeHash = function(data){

        var hash = 0;
        if(typeof data != 'string'){
            if(typeof data != 'obejct'){
                //Array Object Function null
                hash = 1;
            }else if(typeof data != 'number'){
                //number NaN
                hash = Object.is(NaN, data) ? 0 : data;
            }else if(typeof data != 'boolean'){
                //true false
                hash = Number(data);
            }else if(typeof data != 'undefined'){
                //undefined
                hash = 2;
            }
        }else{
            //string
            for(var i=0; i<3; i++){
                hash += data[i] ? data.charCodeAt(0) : 0;
            }
        }
        return hash%8;
    }

    myMap.prototype.set = function(key, val){
        var hash = this.makeHash(val);
        var oTempBucket =  this.bucket[hash];
        while(oTempBucket.next){

            if(oTempBucket.next.key === key){
                oTempBucket.next.value = value;
                return value;
            }else{
                oTempBucket = oTempBucket.next;
            }

        }
        oTempBucket.next = {
            key:key,
            value:value,
            next:null
        }
    }
    myMap.prototype.get = function(key){
        var hash = this.makeHash(val);
        var oTempBucket =  this.bucket[hash];
        while(oTempBucket.next){

            if(oTempBucket.next.key === key){
                return oTempBucket.next.value;
            }else{
                oTempBucket = oTempBucket.next;
            }

        }
    }
    myMap.prototype.has = function(key){
        var hash = this.makeHash(val);
        var oTempBucket =  this.bucket[hash];
        while(oTempBucket.next){

            if(oTempBucket.next.key === key){
                return true;
            }else{
                oTempBucket = oTempBucket.next;
            }

        }
        return false;
    }
    myMap.prototype.delete = function(key){
        var hash = this.makeHash(val);
        var oTempBucket =  this.bucket[hash];
        while(oTempBucket.next){

            if(oTempBucket.next.key === key){
                oTempBucket.next = oTempBucket.next.next;
                return true;
            }else{
                oTempBucket = oTempBucket.next;
            }

        }
        return false;
    }
    myMap.prototype.clear = function(key){
        this.init();
    }
    ```

## js异步编程方案 promise

- 宏任务 比 微任务先放在任务队列中
- 微任务 比 宏任务先放在执行栈中执行

    **Promise基本使用**
    ```javascript
    new Promise((res, rej)=>{
        
    }).then((value)=>{

    }, (reason)=>{

    }).then().catch((e)=>{

    }).finally(()=>{

    })

    Promise.all([new Promise(()=>{}), new Promise(()=>{})]).then((data)=>{
        //全成功 触发成功回调 //参数为数组形式传入
    }, (reason)=>{
        //有一个失败就 触发失败的回调 //参数为数组形式传入
    })
    Promise.race([new Promise((res, rej)=>{}), new Promise((res, rej)=>{})]).then(()=>{
        //谁先成功 触发谁的成功回调
    }, ()=>{
        //谁先失败 触发谁的失败回调
    })
    ```

    **模拟babel编译后的ES6 Promise结果**

        1. executor 执行期函数同步执行
        2. 执行期函数中resolve reject throw同步触发相应的回调
        3. 执行期函数中resolve reject 异步触发相应的回调
        4. then返回一个新的Promise对象、非Promise返回值的接收
        5. 成功 失败的回调是异步执行
        6. 上一个then throw new Error('xx'),下一个then中触发失败回调
        7. 空then
        8. 返回值为Promise对象

    ```
    function myPromise(executor){
        var self = this;
        self.status = 'pending';
        self.resolveValue = '';
        self.rejectReason = '';
        self.resolveCallbackList = [];
        self.rejectCallbackList = [];
        function resolve(value){
            if(self.status === 'pending'){
                self.resolveValue = value;
                self.status = 'Completed';
                self.resolveCallbackList.forEach((ele)=>{
                    ele();
                })
            }
        }
        function reject(reason){
            if(self.status === 'pending'){
                self.rejectReason = reason;
                self.status = 'Rejected';
                self.rejectCallbackList.forEach((ele)=>{
                    ele();
                })
            }
        }
        try{
            executor(resolve, reject)
        }catch(e){
            reject(e);
        }
        
    }   
    myPromise.prototype.then = function(onCompleted, onRejected){
        var self = this;
        
        if(!onCompleted){
            onCompleted = function(value){
                return value;
            }
        }
        if(!onRejected){
            onRejected = function(reason){
                throw new Error(reason);
            }
        }

        var nextPromise = new Promise((res, rej)=>{
        
            //执行期函数resolve reject throw 处于同步触发时
            if(self.status === 'Completed'){
                setTimeout(()=>{
                    try{
                        var resolveReturnValue = onCompleted(self.resolveValue);
                        resolutionReturnPromise(resolveReturnValue, res, rej, function(){
                            res(resolveReturnValue);
                        })
                    }catch(e){
                        rej(e);
                    }
                }, 0)
            }
            if(self.status === 'Rejected'){
                setTimeout(()=>{
                    try{
                        var rejectReturnValue = onRejected(self.rejectReason);
                        resolutionReturnPromise(rejectReturnValue, res, rej, function(){
                            rej(rejectReturnValue);
                        })
                    }catch(e){
                        rej(e);
                    }
                }, 0)
            }

            //执行期函数resolve reject 处异步触发时
            if(self.status === 'pending'){

                self.resolveCallbackList.push(function(){
                    setTimeout(()=>{
                        try{
                            var resolveReturnValue = onCompleted(self.resolveValue);
                            resolutionReturnPromise(resolveReturnValue, res, rej, function(){
                                res(resolveReturnValue);
                            })
                        }catch(e){
                            rej(e);
                        }
                        
                    }, 0)
                })
                self.rejectCallbackList.push(function(){
                    setTimeout(()=>{
                        try{
                            var rejectReturnValue = onRejected(self.rejectReason);
                            resolutionReturnPromise(rejectReturnValue, res, rej, function(){
                                rej(rejectReturnValue);
                            })
                        }catch(e){
                            rej(e);
                        }
                        
                    }, 0)
                })

            }

        })
        return nextPromise;
    }

    function resolutionReturnPromise(returnValue, res, rej, fn){
        if(returnValue instanceof myPromise){
            returnValue.then((value)=>{
                res(value);
            }, (reason)=>{
                rej(reason);
            })
        }else{
            fn();
        }
    }
    ```


## 迭代器 & Symbol

- 迭代模式：提供一种方法可以顺序获得聚合对象中的各个元素，是一种最简单最常见设计模式 
- 内部迭代器：本身是函数，该函数内部定义好了迭代规则，完全接受整个迭代过程，外部只需要一次初始调用   forEach $.each
- 外部迭代器：本身是函数，执行返回迭代对象，迭代下一个元素必须显示调用，调用复杂度增加，但灵活
- 迭代器目的：从迭代模式思想中可以看出，就是要标准化迭代操作

    **模拟外部迭代器的小功能**
    ```javascript
    let arr = [1,2,3];
    function outerIterator(arr){
        let curIndex = 0;
        let next = ()=>{
            return {
                value:arr[curIndex],
                done:arr.length == ++curIndex
            }
        }
        return {
            next
        };
    }
    let oIterator = outerIterator(arr);
    console.log( oI.next() )
    console.log( oI.next() )
    console.log( oI.next() )
    ```

    **Symbol**
    ```javascript
    let os = Symbol({
        name:'xw',
        toString:function(){//方法的重写
            return 'gaixie';
        }   
    })
    console.log( Symbol.iterator, Symbol('Symbol.iterator'))
    ```

    **具有迭代属性后就可以被 for of & ... & Array.from**
    ```
    let obj = {
        0:1,
        1:2,
        length:2,
        [Symbol.iterator]:function(){
            let curIndex = 0;
            let next = ()=>{
                return {
                    value:this[curIndex],
                    done:this.length == ++curIndex, 
                }
            }
            return {//迭代对象
                next
            }
        }
    }
    ```

## 生成器函数

- *
- 生成器Generator本身是函数，执行后返回迭代对象 
- 函数内部要配合yield使用  Generator函数会分段执行 遇到yield即停止

    **生成器函数基本使用**
    ```javascript
    function *test(){//创建我们生成器函数
        yield 'a';
        yield 'b';
        yield 'c';
        return 'd';//return 可有可无
    }
    let oG = test();//返回迭代对象
    oG.next()//{value:'a', done:false}
    oG.next()//{value:'b', done:false}
    oG.next()//{value:'c', done:false}
    oG.next()//{value:'d', done:false}
    oG.next()//{value:undefined, done:false}

    function *test(){
        let val1 = yield 'a';
        console.log(val1)
        let val2 = yield 'b';
        console.log(val2)
        let val3 = yield 'c';
        console.log(val3)
        return 'd';
    }
    let oG = test();
    oG.next('111')//{value:'a', done:false}
    oG.next('222')//{value:'b', done:false} //'222'
    oG.next('333')//{value:'c', done:false} //'333'
    oG.next('444')//{value:'d', done:false} //'444'
    oG.next('555')//{value:undefined, done:false} 
    ```

    **生成器函数实现迭代器**
    ```javascript
    let obj = {
        0:1,
        1:2,
        length:2,
        [Symbol.iterator]:function *(){
            let curIndex = 0;
            while(curIndex != this.length){
                yield this[curIndex];
                curIndex++; 
            }
        }
    }
    ```

## 异步更优雅

- async 定义的函数执行后返回结果为Promise对象 

    **1. Promise & 生成器函数 & Co递归思想**
    ```javascript
    ....
    ```

    **2. async & await**
    *async 定义的函数执行后返回结果为Promise对象*

    ```javascript
    function readyFile(data){
        return new Promise((res, rej)=>{

            setTimeout(()=>{
                rej(data)
            },1000)

        })
    }
    async function oPro(val){
        let val1 = await readyFile(val);
        let val2 = await readyFile(val1);
        let val3 = await readyFile(val1);
    }
    oPro('111').then((val)=>{
        console.log(val, 'ok')
    }, (reason)=>{
        console.log(reason, 'no')
    })
    ```