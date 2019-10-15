

#### link标签和CSS的`@import`语法规则

link为标签 `@import`为CSS的规则，`@import`引入的样式要等HTML页面或它所在的文件加载完再加载

#### HTML和DOM的关系

HTML只是字符串，DOM由HTML解析而来，JS可以操作DOM

#### `property`和`attribute`

input为例

#### alt和title

鼠标移上后显示title信息
图片不输出信息时，会显示alt信息，反之则不显示alt信息

#### table表格

服务器把代码加载到本地服务器的过程中，本来是加载一行执行一行，但是在table标签里面的内容全都加载完才显示出来

#### 厂商定制

>同样分享页面到QQ的聊天窗口，有些页面直接就是一个链接，但是有些页面有标题，图片，还有文字介绍。为什么区别这么明显呢？其实就是看有没有设置下面这三个内容
```HTML
<meta itemprop="name" content="这是分享的标题"/>
<meta itemprop="image" content="http://imgcache.qq.com/qqshow/ac/v4/global/logo.png" />
<meta name="description" itemprop="description" content="这是要分享的内容" />
```

#### 移动端项目需要注意的4个问题

1. meta中设置viewport
```HTML
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
```
2. 样式重置
3. 一像素边框问题
4. 300毫秒点击延迟问题，引入fastclick.js模块

#### HTML5

- 新增属性
    - `::contenteditable`
    - `hidden`
    - `data-val`
    - `::draggable、ondragstart、ondrag、ondragend、ondragenter、ondragover（e.preventDefault()）、ondrop、 ondragleave`

- 新增标签
    - canvas
    - svg
    - audio
    - video
    - （语义化标签，一群类似DIV东西）header、footer、nav、section

- 新增API
    1. 定位（需要地理位置的功能）
    2. 重力感应（陀螺仪）检验平衡的
    3. `requestAnimationFrame`（动画优化）
    4. `history`（控制当前页面历史记录）
    5. `localStorage` `sessionStorage`（存储信息，比如历史最高记录）
    6. `WebSocket`（在线聊天， 聊天室）
    7. `FileReader`（文件读取、预览）
    8. `Worker`（文件的异步 提升性能，提升交互体验）
    9. `Fetch`（传说中要替代AJAX的东西）

#### Example

| svg | canvas |
|-----|------- |
| 矢量图放大不会失真模糊  |  基于像素 |
| css和标签都可以画  |  js操作 |
| 适合大面积贴图  | 适合用于小面积绘图  |
|  通常动画较少 |  适合动画 |

- canvas

```javascript
    let ctx = canvas.getContext('2d');
    ctx.moveTo(x, y)
    ctx.lineTo(x, y)
    ctx.lineWidth = 10;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    ctx.strokeStyle = 'red';
    ctx.stroke();
    ctx.fillStyle = 'red';
    ctx.fill()
    ctx.font = '2px 微软雅黑';
    ctx.strokeText('why', x, y);
    ctx.fillText('why', x, y);
    ctx.closePath();
    ctx.beginPath();

    ctx.rect(x, y, w, h);
    ctx.fillRect(x, y, w, h);
    ctx.strokeRect(x, y, w, h);
    ctx.clearRect(x, y, w, h);

    ctx.arc(x, y, r, srad, erad, 0);
    ctx.arcTo(x, y, x2, y2, r);

        ctx.translate(w, h)
        ctx.rotate(Math.PI)
        ctx.scale(2, 2)

    ctx.save();
    ctx.restore();

        var bg = ctx.createPattern(imgDom, 'no-reapet');（根据坐标原点进行填充的）
        ctx.fillStyle = bg;

        var bg = ctx.createLinearGradient(sx, sy, ex, ey)//起点 终点（根据坐标原点进行填充的）
        bg.addColorStop(0.5, '#000')
        bg.addColorStop(1, '#333')
        ctx.fillStyle = bg;

        var bg = ctx.createRadialGradient(x1, y1, r1, x2, y2, r2)//圆心点1 半径1 圆心点2 半径2
        bg.addColorStop(0.5, '#000')
        bg.addColorStop(1, '#333')
        ctx.fillStyle = bg;

        ctx.shadowColor = 'red';
        ctx.shadowBlur = 10;
        ctx.shadowOffsetX = 10;
        ctx.shadowOffsetY = 10;

    ctx.drawImage(dom, x, y, w, h)
    ctx.toDataURL();
```

- svg

```javascript
    <style>
        line.line1{
            stroke-width:5px;
            stroke:#000;
            stroke-dasharray:10px 20px 30px;/* 每隔10切一刀刀宽是20*/
            stroke-dashoffset:300px; /*基于stroke-dasharray向左偏移*/
        }
        polyline{
            fill:transparent;
            stroke:#000;
            stroke-opcity:0.5;
            fill-opcity:0.5;
        }
    </style>
    <svg width="500px" height="500px" viewBox="0,0, 250, 250">
        <!-- 划线 -->
        <line class="line1" x1="100" y1="100" x2="200" y2="200"></line>

        // 在SVG中闭合的图形天生填充
        <rect x="0" y="0" height="100" width="100" rx="10" ry="10"></rect>

        <circle r="20" cx="50" cy="220"><circle>

        <ellipse rx="20" ry="30" cx="50" cy="220"></ellipse>

        <polyline points="0 0, 50 50, 100 100, 200 200..."></polyline>//画到哪就停le   折线
        <polygon points="0 0, 50 50, 100 100, 200 200..."></polygon>//首位会连接    多边形
            
        <text x="300" y="50">why</text>

        <path d="M 100 100 L 200 100 l 200 100 H 200 W 200 A 100 50 90 1 1 200 200 z"></path>//大写绝对位置 小写相对位置


    </svg>
```

- audio、video

```javescript
    controls 控制台
    play()
    paused()
    pause()
    duration 总的时间
    currentTime 当前时间 可读可写做跳转
    playbackRate = 1; 倍速
    volumn = 1; 声音 [0,1 ]
```
