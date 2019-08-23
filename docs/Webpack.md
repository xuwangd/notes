
## 工程化 & 模块化

- 工程化：前端开发流程标准化、规范化，体现在开发流程、技术选型、代码规范、创建部署，目的就是为了提高前端开发质量和效率
- 模块化：将复杂的系统分解成多个模块，降低复杂度，提高可维护性、可复用性、效率、质量
- 组件化：页面UI的抽离

## 模块化规范 ES6 & commonjs & AMD & CMD

**ES6**

```javascript
import test from 'xxx';
export default {} | function(){}

import {obj, fn} from 'xxx';
export let obj = {};
export function fn(){};

import {obj as obj, fn} from 'xxx';
let obj = {};
function fn(){};
export {
    obj as obj,
    fn
}

import * as test from 'xxx';
let obj = {};
function fn(){};
export {
    obj,
    fn
}
```

**commomjs**

- 同步加载
- 最初用在服务器node环境中，把每个文件当成一个模块
- 浏览器缺少module exports require global四个环境变量。需借助browserify工具转换成es5使用
- 导出：

  ```javascript
  module.exports = {} || '' || function(){};
  exports.message = '' || function(){} || {};
  ```

- 导入：
  `var m = require('m');`
- 引入转换后的js文件
  `<script src="./bundle.js"></script>`

**AMD**

- 异步加载
- 需借助require.js转换
- 依赖前置
- 导出

  ```javascript
  define('命名', ['m1'], function(m1){
    var msg = 'msg';
    return {msg, m1}
  })
  ```

- 导入

  ```javascript
  require.config({
    paths:{
        m1:'./m1',
        m2:'./m2'
    }
  })

  require(['m1', 'm2'], function(m1, m2){
    console.log(m1, m2)
  })
  ```

- 引入require.js data-main指定入口文件

  ```javascript
  <script src="./require.js" data-main="./index.js"></script>
  ```

**CMD**

- 异步加载
- 借助sea.js
- 按需加载，没有依赖前置
- 导出

  ```javascript
  define(function(require, exports, module){
        var m1 = require('')
        require.async('./m2', function(m2){

        })

        exports.msg = m1;
        module.exports = m1;
  })
  ```

- 导入

  ```javascript
  <script src="./sea.js"></script>
  seajs.use('./m', function(m1){

  })
  ```

## 前端自动化构建工具Gulp & Grunt & Webpack & Fis

**webpack下载 安装**

```javascript
node + npm 下载安装
git 下载安装
npm install webpack -g
npm install webpack-cli -g
npm init
npm install webpack --save-dev
npm install webpack-cli --save-dev

npm uninstall webpack -g
node_modules也删掉
```

**ps:注意项**

1. webpack4.0 支持零配置文件打包，默认找到根目录下src文件夹下index.js打包
2. webpack 默认能处理js文件，处理img css html需要借助加载器laoder 插件plugin
3. webpack 支持CommomJs ES6模块化
4. node_modules 里记录着 npm包
5. package.lock.json 里记录着 包版本信息
6. package.json 里记录着 包依赖项
7. webpack常用命令

   ```javascript
   npm uninstall webpack | 手动删除node_modules里的webpack文件夹
   npm install

   webpack ./index.js -o ./dist/bundle.js

   webpack --mode development | webpack --mode production

   webpack --config webpack-dev.config.js | webpack --config webpack-prod.config.js

   webpack-dev-server

   "dev":"webpack --mode development"
   "prod":"webpack --mode production"
   "dev":"webpack --config webpack-dev.config.js"
   "prod":"webpack --config webpack-prod.config.js"
   "server":"webpack-dev-server"
   ```

## webpack的核心 出口 & 入口 & loader & plugin & 模式

**配置文件里基本格式**

```javascript
const path = require('path');

module.exports = {
    entry:{
        index:'./src/js'
    },
    output:{
        path:path.resolve(__dirname, 'dist')
        filename:'[name][hash:5].bundle.js'
    }
    module:{
        rule:[
            {
                test:''
                use:[
                    {
                        loader:''
                        options:{}
                    }
                ]
            }
        ]
    },
    plugins:[],
    mode:'development' | 'production',
    devServer:{
        port:8080,
        contentBase:'dist'
    }
}
```

**JS文件 tree-shaking**

```javascript
npm install webpack-deep-scope-plugin --save-dev
var WebpackDeepScopePlugin = require('webpack-deep-scope-plugin').default;
plugins:[
    new WebpackDeepScopePlugin()
]
```

## CSS文件 打包 & 单独抽离 & tree-shaking & postcss-loader

**打包**

```javascript
npm install style-loader css-loader --save-dev
module:{
    rules:[
        {
            test:/\.css$/,
            use:[
                {
                    loader:'style-loader'
                },
                {
                    loader:'css-loader'
                }
            ]
        }
    ]
}
```

**单独抽离**

```javascript
npm install mini-css-extract-plugin --save-dev
var MiniCssExtractPlugin = require('mini-css-extract-plugin');
module:{
    rules:[
        {
            test:/\.css$/,
            use:[
                {
                    loader:MiniCssExtractPlugin.loader,
                    options:{
                        publicPath:'../'
                    }
                },
                {
                    loader:'css-loader'
                }
            ]
        }
    ]
},
plugins:[
    new MiniCssExtractPlugin({
        fliename:'css/[name][hash:5].css'
    })
]
```

**CSS文件 tree-shaking**

```javascript
npm purifycss-webpack purify-css --save-dev
npm install glob-all --save-dev
var PurifycssWebpack = require('purifycss-webpack');
var glob = require('glob-all');
plugins:[
    new PurifycssWebpack({
        paths:glob.sync([
            path.join(__dirname, './*.css'),
            path.join(__dirname, './src/*.js')
        ])
    })
]
```

**postcss-loader**

```javascript
npm install postcss-loader postcss autoprefixer cssnano postcss-cssnext --save-dev
module:{
    rules:[
        {
            test:/\.css$/,
            use:[
                {
                    loader:'style-loader'
                },
                {
                    loader:'css-loader'
                },
                {
                    loader:'postcss-loader',
                    options:{
                        ident:'postcss',
                        plugins:[
                            require('cssnano'),
                            require('autoprefixer'),
                            require('cssnext')
                        ]
                    }
                }
            ]
        }
    ]
}
```

**HTML单独抽离 & 清理dist目录**

```javascript
npm install html-webpack-plugin --save-dev
npm install clean-webpack-plugin --save-dev
var HtmlWebpackPlugin = require('html-webpack-plugin');
var CleanWebpackPlugin = require('clean-webpack-plugin');
plugins:[
    new HtmlWebpackPlugin({
        template:'./index.html',
        filename:'index.html'
    })
    new CleanWebpackPlugin()
]
```

**打包HTML中的img图片 & CSS中的url图片 & 压缩图片**

```javascript
npm install html-loader url-loader file-loader img-loader
module:{
    rules:[
        {
            test:/\.(png|jpeg|jpg|gif)$/,
            use:[
                {
                    loader:'html-loader',
                    options:{
                        attrs:['img:src']
                    }
                },
                {
                    loader:'url-loader',
                    options:{
                        name:'[name][hash:5].[ext]',
                        limit:100000,
                        outputPath:'img'
                    }
                },
                {
                    loader:'img-loader',
                    options:{
                        ...
                    }
                },
            ]
        }
    ]
}
```

**针对多入口文件提取公共js**

```javascript
...
```

**开启本地服务器 & 开启热更新**

```javascript
*开启本地服务器
npm install webpack-dev-server -g
npm install webpack-dev-server --save-dev
devServer:{
    port:8080,
    contentBase:'./dist'
}
```

```javascript
*热更新
var webpack = require('webpack');
plugins:[
    new webpack.HotModuleReplacementPlugin()
],
devServer:{
    port:8080,
    contentBase:'./dist',
    hot:true
}
if(module.hot){
    module.hot.accept()  
}
```