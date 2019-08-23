#### 浏览器私有属性 、主流浏览器 、主流浏览器内核

| 浏览器私有属性 | 主流浏览器 | 浏览器内核 |
|----------|---------------|---------|
| -webkit- | chrome/safari | webkit  |
| -moz- | firefox | gecko |
| -ms- | ie | trident |
| -o- | opera | presto |

#### CSS盒模型

1. W3C标准盒模型和IE怪异模式下盒模型
2. 盒模型由 `margin`、`border`、`padding`、`content` 组成的
3. W3C标准盒模型下设置`width` `height`分别就是`content`区的`width` `height`，IE怪异模式下盒模型设置的`width` `height`分别就是`border` + `padding` + `width`、`border` + `padding` + `height`
4. 两种盒模型的切换，IE怪异模式下盒模型`box-sizing:border-box;` W3C标准盒模型`box-sizing:content-box;`

#### BFC

1. BFC是块级格式化上下文"，它是一个独立的渲染区域
2. 生成BFC：根元素，即HTML元素（最大的一个BFC）、 `position`值为`absolute`或`fixed`、 `overflow:hidden;`、`display:inline-block;`
3. 利用BFC避免： 父子形式的`margin`重叠、浮动影响、避免文字环绕

#### `inline-block`和`inline`间隙

1. 删除两个标签间的空格， 但是这样html排版不好
2. 容器元素`font-size:0;`  然后再在里面再重新设置字体大小

#### 图片缝隙问题

1. `vertical-align: bottom;`
2. `display:block;`
3. 容器元素`font-size:0;`

#### 单行文本溢出显示省略号和多行文本溢出显示省略号

- 单行文本溢出显示省略号
    ```css
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: no-wrap;
    ```

- 多行文本溢出显示省略号
    ```css
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    ```

#### `display: none;`和`visibility: hidden;`

1. 都是让元素隐藏
2. `display:none;`会让元素完全从渲染树中消失，渲染的时候不占据任何空间
3. `visibility: hidden;`不会让元素从渲染树消失，渲染时元素继续占据空间，只是内容不可见
4. `display: none;`是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示
5. `visibility:hidden;`是继承属性，子孙节点消失由于继承了 hidden，通过设置 `visibility: visible;`可以让子孙节点显式
6. 修改常规流中元素的 display 通常会造成文档重排。修改 visibility 属性只会造成本元素的重绘
7. 读屏器不会读取 `display: none;`元素内容；会读取 `visibility: hidden` 元素内容

#### css单位

1. `px`
2. `em`
3. `rem`
4. `vh`、`vw`
5. `vmin`、`vmax`
6. `%`（`padding`、`margin`的百分比是相对于父容器`width`宽度计算的）

#### `rgba(0, 0, 0, .3)`和`opacity:.3`

opacity 作用于元素以及元素内的所有内容（包括文字）的透明度，rgba 只作用于元素自身

#### 伪元素和伪类

伪元素是特殊样式添加到选择器
伪类是配合选择器给元素添加样式

#### CSS处理器

- `postCss` 用js实现的css的抽象的语法树。
- 预处器 `sass` `less` `cssNext`用来实现一些未来的标准
- 后处理器  `autoprefixer`插件

#### CSS3新增伪元素和伪类

- 新增伪元素
    ```css
    ::placeholder
    ::selection
    ```
- 新增伪类
  ```css
  + ~
  :root（根标签选择器）
  :not(:first-of-type | :target | [type="why"])
  :nth-of-type(n+2 | even | odd) | first-of-type | last-of-type
  :target
  :disabled
  :readonly
  ```

#### `border-radius`

```css
border-radius: 50%;
border-radius: 50% 50% 50% 50%;
border-top-left-radius: 50%;
border-top-left-radius: 50% 50%;
```

#### `box-shadow`

blur是向内外同时模糊，镂刻、浮雕
```css
text-shadow:1px 1px #000, -1px -1px #fff;
box-shadow:inset 0 0 1px 0 #0ff;
```

#### `border-image`

```css
border-image-source:url;
border-image-slice:number;  上下左右怎么切割
border-image-repeat:stretch;
border-image-outset:100px;  向外延伸
border-image-width:auto;    设置宽度
border-image:url 100 round;
```

#### 渐变

```css
background-image:linear-gradient(0deg, #0ff 10%, #ff0);
background-image:linear-gradient(to top right, #0ff 10%, #ff0);
background-image:radial-gradient(circle at bottom right,#ff0, #0ff);
```

#### `background-origin`和`background-clip`

从那开始在那结束
```css
background-origin: content-box padding-box border-box;
background-clip:padding-box border-box content-box text;
```

#### `word-break`

```css
word-break:word-all;
word-break:break-word;
```

#### `flex`

- 流式布局
- 圣杯布局
- `flex-basis` width（前者决定最小值，后者决定最大值）
- `flex-grow` 按几等分分配多余空间
- `flex-shrink`设置的等分乘以自己真实的`width`/(设置的等分乘以自己真实的`width` + 其他元素设置的等分乘以自己真实的`width` ...) * 压缩空间

#### `transition`和`animation`

- 属性 过渡时长 运动曲线 延迟执行时间 状态只要改变了就会触发
    `transition:attr1 time linear duration, attr2 time linear duration;`
- 自定义动画帧的名字 过渡时长 运动曲线|steps 延迟执行时间 反向播放 播放次数 停留在最后一帧
    `animation:name time, name time linear duration alternate infinite forwards;`
    `animation:name time, name time steps(1, end) duration alternate infinite forwards;` //step-start
    `animation:name time, name time steps(1, start) duration alternate infinite forwards;` //step-end

#### `transform`

```css
rotate(n) rotatex(x) rotatey(y) rotate3d(x,y,z, 0deg)//矢量方向(作为旋转的轴)
scale(n) scalex(x) scaley(y) scalez(z) scale3d(x,y,z)
skew(n) skewx(x) skewy(y)//倾斜加拉伸
translate(n) tanslatex(x) tanslatey(y) translatez(z) translate3d(x,y,z)
```

```css
transform-origin:center center center;  //变换轴原点位置
transform-style:preserve-3d //3d空间
transform:perspective(800px); //景深

perspective:800px;//全局景深
perspective-origin:1 1;//视角位置
```

```css
backface-visibility:hidden;（图片背面影藏）

transform:matrix(-1,0,0,1,0,0 );（矩阵是transform给我们选中的计算规则）
```

#### HTML

下载解析`HTML` 下载`CSS` 下载`js` 下载`img`等等 根据cssRules构建cssRules树 根据domAPI构建dom树 两者合并构建render树 布局render树 绘制render树

---
[css参考手册1](http://css.doyoe.com)

[css参考手册2](http://www.canluse.com)

[一个收集 CSS 使用技巧的库](https://github.com/AllThingsSmitty/css-protips/tree/master/translations/zh-CN)