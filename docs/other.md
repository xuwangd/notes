**滚动条的位置**

```javascript

window.pageXOffset
window.pageYOffset

IE👇
document.body.scrollLeft
document.body.scrollTop  
document.documentElement.scrollLeft
document.documentElement.scrollTop

```

**可视窗口尺寸**

```javascript

window.innerWidth
window.innerHeight

IE👇
//怪异模式 document.compatMode == 'backCompat'
document.body.clientWidth
document.body.clientWidth

//标准模式 document.compatMode == 'CSS1Compat'
document.documentElement.clientHeight
document.documentElement.clientWidth

```

**滚动条滚动**

```javascript

//滚动至
window.scroll(x, y) window.scrollTo(x, y)

//累加滚动
window.scrollBy(x, y)

dom.scrollTop / Left可读可写

```

**dom尺寸(body默认margin 8px IE 20px)**

```javascript

//视觉上的宽高
dom.offsetWidth
dom.offsetHeight


//相对最近有定位父级之间距离
dom.offsetLeft
dom.offsetTop


//返回最近有定位的父级
dom.offsetParent

```

**样式表**

```javascript

//唯一可以可写
dom.style[prop]

//IE 最终计算样式表
dom.currentStyle[prop]

//最终计算样式表
window.getComputedStyle(dom,null/'after')[prop]

```

**事件**

```javascript

//事件对象event
event.preventDefault()
event.stopPropagation()
IE👇
event.cancelBubble = true;


//句柄绑定方式 DOM1级
dom.onclick = function(){}


//可绑定多个事件处理函数，同个函数绑定多次只会执行一次 DOM2级
dom.addEventListener(事件类型, 事件处理函数, false)
dom.removeEventListener(事件类型, 事件处理函数, false)


//IE 可绑定多个事件处理函数，this指向window DOM2级
dom.attachEvent(on + 事件类型，事件处理函数)
dom.attachEvent('onclick',function(){
    demo.call(dom)
})
function demo(){}

//onmousedown onmouseup onclick
1. 先mousedown、再mouseup、最后click
2. mousedown、mouseup的event.button可以监听左 滑轮 右键
3. click的event.button只能见听左键


// mouseover/mouseenter  mousemove   mouseout/mouseleave


//oncontextmenu 右键出菜单事件
document.oncontextmenu = function(event){
    var event = event || window.event;
    var target = event.target || event.srcElement;
    event.preventDefault()
    return false;//return false适用句柄绑定方式阻止默认事件
}


//touchstart touchmove touchend
1. 移动端点击穿透问题touchstart事件300ms延迟等待双击事件再执行click事件


//keydown keypress keyup
1. 键盘按住keydown、keypress一直触发
2. keydown可响应所有按钮除了fn、keypress只响应字符类按钮
3. keypress String.fromCharCode(event.charCode)
4. keydown String.fromCharCode(event.keyCode)


//focus blur change input submit reset()

```

