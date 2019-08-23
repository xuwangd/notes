#### BOM

1. `BOM` （browser object model） 简称浏览器对象模型
2. 主要处理浏览器窗口`window`和框架`iframe`，描述了与浏览器交互的接口和方法，可以对浏览器窗口访问和操作
3. `window`是`BOM`的顶层核心对象，`window`有着双重角色即时全局对象又是js层级中顶级对象

#### `window`（浏览器窗口对象）

```javascript

window.screenX/Y === window.screenLeft/Top
window.confirm('hello world')
window.prompt('hello world')
window.alert('hello world')
window.onbeforeunload = function(){
    return 'hello world';
}

let ruselt = window.open('https://baidu.com','name','width=300,height=300');
window.close()
window.parent
window.top
window.self
window.name

```

#### `navigator`（客服端浏览器信息）

```javascript

navigator.cookieEnabled //是否启用cookie
navigator.onLine //是否处于脱机状态
navigator.userAgent //客服端发送服务器user-agent头部值

```

#### `history`（浏览器访问过的URL历史记录）

```javascript

history.length
history.back()
history.forward()
history.go()

```

#### `location`（当前URL信息）

```javascript

location.protocol
location.hostname
location.host
location.pathname
location.search
location.hash
location.href
location.assign('http://xxx')
location.reload(false)

```

#### screen（客服端显示屏信息）

```javascript

screen.width
screen.height

```