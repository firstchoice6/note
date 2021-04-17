## 6 WebPack

> webpack是一个现代的js应用的静态模块打包工具，依赖于node.js

### 6.1 Webpack的安装与配置

- 安装

  ```shell
  全局安装
  npm install webpack -g
局部安装
  npm install webpack@3.6.0 --save-dev
  ```
  
- 起步

  ```shell
  src 源文件夹(开发)
  dist 打包代码文件夹(发布)
  
  webpack ./src/main.js ./dist/bundle.js
  # webpack 会自动处理模块的依赖
  ```

- **配置**

  - 1. 部署npm

  ```shell
  npm init -y 
  
  1.1通过npm安装需要使用的loader
  npm install css-loader style-loader --save-dev
  # css文件
  
  npm install --save-dev less-loader less
  # less文件
  
  npm install --save-dev url-loader
  # 小文件
  
  npm install file-loader --save-dev
  # 大文件
  
  npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
  # ES6语法处理（兼容性处理)
  
  npm install vue --save
  # 不是开发时依赖，所以不用加-dev
  
  npm install vue-loader vue-template-compiler --save-dev
  # vue相关loader
  ```

  - 2. 新建webpack.config.js 

  ```js
  // webpack.config.js 文件名固定
  const path = require('path')
  // 出口路径需要绝对路径，所以请求path
  module.exports = {
      entry: './src/main.js',
      // 入口路径
      output: {
          // 出口路径及命名
          path: path.resolve(__dirname, 'dist'),
          filename: 'bundle.js',
          // publicPath: 'dist/'
          // 有了这行代码，涉及到url的地方都会自动拼接'dist/'
          // 使用HTMLWebpackPlugin插件，将html给dist也打包一份就不用这行代码了
      },
      module: {
          rules: [{
                  // ******css处理的配置*******
                  test: /\.css$/, //以.css结尾的文件
                  use: ['style-loader', 'css-loader'] //从右往左读取多个loader
                  // css-loader只负责加载
                  // style-loader负责将模块导出作为样式添加到DOM中
              },{
                  // ******less处理的配置*******
                  test: /\.less$/,
                  //从后往前读取loader
                  use: [{
                      loader: "style-loader" // creates style nodes from JS strings
                  }, {
                      loader: "css-loader" // translates CSS into CommonJS
                  }, {
                      loader: "less-loader" // compiles Less to CSS
                  }]
              },{
                  // ******文件处理的配置*******
                  test: /\.(png|jpg|gif|jpeg)$/,
                  use: [{
                      loader: 'url-loader',
                      options: {
                          // 当加载的图片小于limit的时候 图片被编译成base64字符串格式
                          // 当加载的图片大于limit的时候 需要file-loader进行加载
                          limit: 8192,
                          name: 'img/[name].[hash:8].[ext]'
                          // file-loader生成的图片文件名为32位哈希值，为了避免文件名过长且不好辨识，使用name属性对文件名进行自定义
                          // 对文件名（图片）进行命名，webpack语法里[]表示变量.表示拼接
                      }
                  }]
              },{
                  // ******ES6兼容处理的配置*******
                  test: /\.js$/,
                  exclude: /(node_modules|bower_components)/,
                  // 排除node_modules,bower_components这两个文件夹
                  use: {
                      loader: 'babel-loader',
                      options: {
                          presets: ['es2015']
                      }
                  }
              },{
                  //******vue-loader的配置******
                  test: /\.vue$/,
                  use:{
                      loader:['vue-loader','vue-template-']
                  }
              }
          ]
      },
      resolve: {
          //*****解决vue中runtime-only报错*****
          alias: {
              'vue$': 'vue/dist/vue.esm.js'
          },
          //****省略后缀名的配置****
          extensions:['.js','.css','.vue']
      }
  }
  }
  
  ```

  - 3. 修改package.json,定义脚本

  ```json
  // 在webpack.config.js中的modules关键字下进行配置
  "script" :{
  	//~~"build": "webpack"~~老版本
  	"build": "webpack --mode production"
  	// 终端里直接webpack是全局的
  	// 定义脚本之后,优先寻找本地node_modules/.bin路径中对应命令
  },
  "devDependencies"{
      vue-loader: "^13.0.0"
      // 14版本之后使用vue-loader需要额外配置插件
      // 改完之后要重新 npm install
  }
  
  ```

  - 4. 在main.js中依赖css文件(依赖loader)

  ```js
  require('./css/normal.css')
  
  4.1 在main.js中依赖less文件
  require('./css/special.less')
  
  4.2 在main.js中使用vue开发
  
  -------------------------app.js------------------------------
  /* 解决main.js文件过于冗长
  4.2.0 创建vue/app.js文件
  export default {
      template: `
  	<div>{{msg}}</div>
  	<button @click='btnClick'>click</button>
  	`,
      data(){
          return {
              msg: 'hello webpack',
              name: 'jack'
          }   
      },
      methods: {
          btnClick(){
              ...
          }
      }
  }
  模板和js代码没有分离，不用
  */
  
  ```

  ```vue
  4.2.1 创建vue/App.vue文件 (解决main.js文件过于冗长)
  4.2.2 创建多个vue文件并在App.vue中引用
  <template>
  	<!-- 模板 -->
  	<div>{{msg}}</div>
  	<button @click='btnClick'>click</button>
  	<Cpn/>
  </template>
  
  <script type="text/javascript">
  	// js代码
      import Cpn from './vue/Cpn.vue'
      // 引入别的vue文件
  	export default {
  		name:"App",
  		data(){
  	        return {
  	            msg: 'hello webpack',
  	            name: 'jack'
  	        }   
  	    },
  	    methods: {
  	        btnClick(){
  	            ...
  	        }
  	    }
  	}
  	
  </script>
  
  <style type="text/css">
  	/*样式*/
  	body {
  		background-color: green;
  	}
  </style>
  ```

  ```js
  --------------------------main.js-----------------------------    
  import Vue from 'vue'
  // 导入App.vue
  import App from './vue/app.js'
  
  
  new Vue({
      el:'#app',
      template: '<App/>',
      /* 
      Vue实例中既有el又有template的时候，
      解析的时候会把html中的<div id="app"></div>替换成模板内容
      */
      components: {
      	App
  	}
  })
  
      /*
      vue的两个版本
      1. runtime-only -> 代码中不能有任何的template
      2. runtime-compiler -> 代码中可以有template，因为有compiler可以编译
      */
  ```

  ```css
  4.3 在css中引用图片文件
  body {
  	background: url("../img/test.jpg")
  }
  
  ```

  - 打包

  ```shell
  5. npm run build
  ```
### 6.2 plugin
> loader和plugin的区别
>  - loader用域转换某些类型模块，是一个转化器
>  - plugin是插件，是对webpack本身的扩展，是一个扩展器

- 安装

```shell
npm ...
部分插件webpack已内置
```

- 添加版权的Plugin--BannerPlugin插件

```js
- 修改webpack.config.js
const path = require('path')
const webpack = require('webpack')

module.exports = {
    ...
    plugins: [
        new webpack.BannerPlugin('版权归xxx所有')
    ]
}
```

- 打包html的plugin--HTMLWebpackPlugin插件

  > 插件还会自动生成<script>并链接js文件

```shell
- 安装
npm install html-webpack-plugin@3.2.0 --save-dev

```

```js
- 配置webpack.config.js
const HTMLWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    ...
    plugins: [
        new HTMLWebpackPlugin({
        	template: 'index.html'
        })
    ]
}
```

- 压缩js的plugin-- > uglifyjs-webpack-plugin

```shell
- 安装
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev

```

```js
- 配置webpack.config.js
const UglifyjsWebpackPlugin = require ('uglifyjs-webpack-plugin')

module.exports = {
    ...
    plugins: [
        // new UglifyjsWebpackPlugin()
        // 开发阶段不需要
    ]
}
```

### 6.3 搭建本地服务器

> webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用express框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果。
>

```shell
- 安装
npm install webpack-dev-server@2.9.3 --save-dev
```

```js
- 配置webpack.config.js
module.exports = {
    ...
    devServer: [
        contentBase: './dist',
        //为拿一个文件夹提供本地服务
        inline: true
        // 页面实时刷新
        port
        // 端口号(可选,默认80)
        historyApiFallback
        // 在SPA(single page application单页富应用程序)页面中,依赖HTML5的history模式(可选)
    ]
    //开发阶段需要,发布不需要
}

-修改package.json,定义脚本
"script" :{
	"dev": "webpack-dev-server  --open"
	// 终端里直接webpack是全局的
	// 定义脚本之后,优先寻找本地node_modules/.bin路径中对应命令
    // 添加--open 运行后直接打开localhost
}
```

```shell
- 运行
npm run dev
```

### 6.4 配置分离

> 解决部分代码开发阶段需要,发布不需要

```shell
npm install webpack-merge --save-dev
```

```js
- 创建build/base.config.js
// 公共
const path = require('path')
const webpack = require('webpack')
const HTMLWebpackPlugin = require('html-webpack-plugin')
const UglifyjsWebpackPlugin = require ('uglifyjs-webpack-plugin')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.resolve(__dirname, '../dist'),
        // 修改dist为../dist 不修改会在build文件夹下创建dist文件夹
        filename: 'bundle.js',
    },
    module: {···   
    },
    plugins: [
        new webpack.BannerPlugin('版权归xxx所有'),
        new HTMLWebpackPlugin({
        	template: 'index.html'
        })
    ]
}



----------------------------------
- 创建build/prod.config.js
// 生产特有
const UglifyjsWebpackPlugin = require ('uglifyjs-webpack-plugin')
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig,{
    // 使用webpackMerge对js进行合并
    
    plugins: [
         new UglifyjsWebpackPlugin()
    ]
})



----------------------------------
- 创建build/dev.config.js
// 开发特有

const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config')

module.exports = webpackMerge(baseConfig,{
    devServer: [
        contentBase: './dist',
        inline: true
    ]
})

```

```json
-修改package.json中webpack的配置文件目录
"script" :{
	"build": "webpack --config ./build/prod.config.js",
    "dev": "webpack-dev-server --open --config ./build/dev.config.js"
}

```