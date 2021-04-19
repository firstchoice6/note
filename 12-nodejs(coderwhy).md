[toc]

## Node.js

### 0 终端的基本操作

```shell
md #创建目录
rd(rmdir) #删除目录，目录内没有文档
echo on a.txt #创建空文件
del #删除文件
rm 文件名 #删除文件
cat 文件名 #查看文件内容
cat > 文件名 #向文件中写上内容
cls #清屏
```

### 1 基本操作

- node安装

- node查看版本

  ```shell
  node -v
  ```

- 进入 REFL

  ```shell
  node
  ```

- 传递参数

  ```shell
  node index.js jack env=dev
  ```

  - 接收

  ```js
  console.log(process.argv)
  // [
    'C:\\Program Files\\nodejs\\node.exe',
    'F:\\codefc\\vue\\learnNode\\传递参数\\index.js',
    'jack',
    'env=dev'
  ]
  
  // argv arg + vector 表示数据结构
  ```

- node的输出

  ```js
  console.log() // 打印
  
  console.clear() // 清空控制台
  
  console.trace() // 打印函数的调用栈
  
  ...
  ```

### 2 全局对象

- 特殊的全局对象

  >- 这些全局对象实际上是模块中的变量 只是每个模块都有 看起来象是全局变量
  >- 在命令行交互中不可使用
  > - `__dirname`
  > - `__filename`
  > - `exports`
  > - `module`
  > - `require()`

- 常见的全局对象

  - `process` - 提供了node进程中相关的信息

  - `console`

  - `global` - 类似浏览器的`window`对象

    > 定义的变量属于模块 不属于global

  - 定时器函数

    - setImmediate
    - setTimeout
    - setInterval
    - process.nextTick

### 3 js模块化

#### 3.1 commonjs

> node对commonjs的实现本质上是对象的引用赋值

- 导出

```js
const name = "zhangsan"

const age = 18

function foo(){
    console.log(1123)
}

exports.name = name
exports.foo = foo
```

- 导入

```js
const {name,foo} = require(./bar)

conosle.log(name)

foo()
```

- `module.exports`

  > module.exports = exports 本质上是module.exports在导出

```js
const name = "zhangsan"

const age = 18

function foo(){
    console.log(1123)
}

module.exports = {
    name:"abc"
}
```

- `require`的细节

  > `require`是一个函数 引入一个文件(模块)中导入的对象
  >
  > `require`加载过程是同步的

  - 语法:` require(x)`

  - `require`的查找规则

    - x是核心模块

      > 直接返回核心模块 停止查找

    - x是以 `./` 或`../`或 `/`(根目录)开头的

      - 有后缀名

        > 按照后缀名查找

      - 无后缀

        > 1. 直接查找文件x
        > 2. 查找x.js
        > 3. 查找x.json
        > 4. 查找x.node

    - 没有文件,有目录x

      > 1. 查找x/index.js
      > 2. 查找x/index.json
      > 3. 查找x/index.node

    - 没有找到 报错

    - 直接require(x) 没有路径

      > 1. 查找x是不是核心模块
      > 2. 逐层往上在node_modules 文件夹里寻找

- 模块的加载过程

  - 模块在被第一次引入时，模块中的js代码会被运行一次

  - 模块被多次引入时，会缓存，最终只加载（运行）一次

    > 每个模块对象module都有一个属性：loaded。
    >  为false表示还没有加载，为true表示已经加载

  - 循环引入的加载顺序按照深度优先算法

- 缺点

  - CommonJS加载模块是同步的
  - 适合服务端 不适合浏览器端

#### 3.2 AMD（异步模块定义）

#### 3.3 CMD（通用模块定义）

#### 3.4 ES Module

> 使用了import和export**关键字**
>
> 它采用编译期的静态分析，并且也加入了动态引用的方式
>
> 采用ES Module将自动采用严格模式：use strict
>
> 在html中使用 添加 `type="module"`
>
> 本地测试 — 如果你通过本地加载Html 文件 (比如一个 file:// 路径的文件), 你将会遇到 CORS 错误，因为Javascript 模块安全性需要。你需要通过一个服务器来测试。Live Server插件

- `export`

  ```js
  // 方式一 直接导出
  export const name = ""
  
  // 方式二 统一导出
  const name = ""
  export {
  	name // 语法 不是对象 不同于commonjs的exports 放置导出变量的引用列表
  }
  // 方式三 起别名
  export {
  	name as myname
  }
  ```

  

- `import`

  ```js
  // 方式一
  import {name} from "./index.js" // 非webpack环境不能省略.js
  
  // 方式二
  import {name as myname} from "./index.js"
  
  // 方式三
  import * as foo from './index.js'
  ```

- `import`和`export`结合使用

  ```js
  export {name} from "./index.js" 
  ```

  - 使用场景: 第三方库统一导出

- `default`

  > 一个模块只能有一个default

  ```js
  export default function foo(){
      
  }
  ```

  ```js
  import Foo from "./index.js"
  ```

  

- `import`不可以将其放到逻辑代码中

  - 原因: 解析时已经确定了依赖关系 这个时候代码还没有运行
  
  - `webpack`环境下可以使用`require()`函数替代
  
  - 纯esmodule环境 使用`import()`函数异步加载 返回一个`promise`
  
    ```js
    if(true){
        import("./index,js").then(a=>{
            a.a()
        })
    }
    ```
  
- **加载过程**

  -  ES Module加载js文件的过程是编译（解析）时加载的，并且是异步的
  - JS引擎在遇到import时会去获取这个js文件，但是这个获取的过程是异步的，并不会阻塞主线程继续执行；
  -  ES Module通过export导出的是变量本身的引用：
    - export在导出一个变量时，js引擎会解析这个语法，并且创建模块环境记录（module environment record）;
    - 模块环境记录会和变量进行 绑定（binding），并且这个绑定是实时的；
    - 在导入的地方，我们是可以实时的获取到绑定的最新值的；

#### 3.5 node中的ESmodule

- 一个js是一个commonjs的模块 斌不是es6的模块

  ```js
  import {name} from "./index.mjs" // .js不能省略
  // 报错
  ```

- 解决方案

  - 方式一: `package.json`中设置`type:"module"`
  - 方式二: 修改后缀名为`.mjs`

#### 3.6 ES和CJS的相互调用

- 通常情况 commonjs不能加载ES module
  - 因为CommonJS是同步加载的，但是ES Module必须经过静态分析等，无法在这个时候执行JavaScript代码；
  - 但是这个并非绝对的，某些平台在实现的时候可以对代码进行针对性的解析，也可能会支持； 
  - Node当中是不支持的；

- 多数情况下，ES Module可以加载CommonJS
  - ES Module在加载CommonJS时，会将其module.exports导出的内容作为default导出方式来使用；
  -  这个依然需要看具体的实现，比如webpack中是支持的、Node最新的Current版本也是支持的

### 4 常见的内置模块

#### 4.1 path

```js
const path = require('path')

1. 获取路径的最后一部分
path.basename('/foo/bar/qu.html','.html')
// 无第二额参数时返回qu.html 
// 第二个参数可选 有第二个参数的时候返回qu

2. 获取路径
__dirname  (node自带，不用require)
// 全路径
path.dirname('/abc/s/d/index')
//只有路径 返回'/abc/s/d'

3. 获取文件的扩展名
path.extname('index.html')
//返回'.html' .结尾返回. 没有返回''

4. 路径的格式化处理(url也有一样的用法)
path.format()
// obj转化为string
// 'F:\codefc\node'
path.parse()
// string转化为obj
/*
{
  root: 'F:\\',根目录
  dir: 'F:\\codefc\\node',全路径
  base: 'text.js',文件名称
  ext: '.js',扩展名
  name: 'text'文件名称
}
*/

5. 判断是否为绝对路径
path.isAbsolute()
//返回true/false

6. 拼接路径(..表示上层路径 .表示当前路径)
path.join('/foo','bar','ba/sadsa','../../')
// 返回'\foo\bar\'

7. 规范化路径
path.normalize('C:\\temp\\\\foo\\bar\\..\\')
// 返回'C:\temp\foo\'

8. 计算相对路径
path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb')
 // 返回..\..\impl\bbb

9. 解析路径
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')
// 返回F:\codefc\node\wwwroot\static_files\gif\image.gif

10. 两个特殊属性
  path.sep
  //表示路径分隔符（windows是\ Linux是/）
  path.delimiter
  //环境变量分隔符(windows中使用; linux中使用:)
```

- 在webpack中的使用

  ```js
  const path = require("path")
  
  const resolve = dir => path.resolve(__dirname,dir)
  
  module.export = {
      webpack:{
          alias:{
              "@":resolve("src"),
              ...
          }
      }
  }
  ```

#### 4.2 fs

- API大多数都提供三种操作方式

  - 方式一: 同步操作文件：代码会被阻塞，不会继续执行；

  - 方式二：异步回调函数操作文件：代码不会被阻塞，需要传入回调函数，当获取到结果时，回调函数被执行；

  - 方式三：异步Promise操作文件：代码不会被阻塞，通过 fs.promises 调用方法操作，会返回一个Promise，可以通过then、catch进行处理；

  - 案例

    ```js
    const fs = require("fs")
    
    // 方式一 同步操作
    const filepath = './hello1.txt'
    const info = fs.statSync(filepath)
    console.log(info)
    
    // 方式二 异步操作
    
    fs.stat(filepath,(error,res)=>{
      if(error){
        return console.log(error)
      }
      console.log(res)
    })
    
    // 方式三 promise
    fs.promises.stat(filepath).then(
      console.log
    ).catch(
      console.log
    )
    ```

- 文件描述符

  ```js
  // 获取文件描述符 并调用
  fs.open(filepath,(error,res)=>{
    if(error){
      return console.log(error)
    }
    fs.fstat(res,(error,info)=>{
      console.log(info)
    })
  }) 
  
  ```

- 文件的读写

  ```js
  // 读文件 默认返回的是Buffer格式
  fs.readFile(path[, options], callback)
  
  eg:
  	fs.readFile("./hello.txt",{encoding:"utf8"},(err,info)=>{
        console.log(info)
      })
  // 写文件
  fs.writeFile(file, data[, options], callback)
  
  eg: 
  	fs.writeFile("./hello.txt","\n你好",{flag:"a"},console.log)
  ```

  - option选项

    - flag选项

      |  值  |                            含义                            |
      | :--: | :--------------------------------------------------------: |
      |  w   |                   打开文件写入，默认值；                   |
      |  w+  |          打开文件进行读写，如果不存在则创建文件；          |
      |  r+  |         打开文件进行读写，如果不存在那么抛出异常；         |
      |  r   |               打开文件读取，读取时的默认值；               |
      |  a   | 打开要写入的文件，将流放在文件末尾。如果不存在则创建文件； |
      |  a+  | 打开文件以进行读写，将流放在文件末尾。如果不存在则创建文件 |

    - encoding选项

      > 默认utf-8

- 文件夹的操作

  - 创建文件夹

    ```js
    const fs = require("fs")
    
    const diranme
    ```

  - 读取文件

    ```js
    fs.readdir("f:\\codefc\\node\\why\\核心模块", { withFileTypes: true }, (_, file) => {
      console.log(file)
    })
    ```

  - 重命名

    ```js
    fs.rename("../why","../coder",err=>{
        console.log(err)
    })
    ```

  - 复制
  
    - 案例 拷贝脚本
  
    ```js
    const fs = require("fs")
    const path = require("path")
    
    const srcDir = process.argv[2]
    const destDir = process.argv[3]
    
    let i = 0
    while(i++< 30){
        const num = 'day' + (i + '').padStart(2,0)
        const srcPath = path.resolve(srcDir,num)
        const destPath = path.resolve(destDir,num)
        if(fs.existsSync(destPath)) contiue
        fs.mkdir(destPath,err=>{
            if(!err) console.log("start copy",num)
            
            const srcFiles = fs.readdirSync(srcPath)
            for(const file of srcFiles){
                if(file.endWith(".mp4")){
                    const srcFile = path.resolve(srcPath,file)
                    const destFile = path.resolve(destPath,file)
                    fs.copyFileSync(srcFile,destFile)
                    console.log(file,"copy success")
                }
            }
        })
    }
    ```
  
    

#### 4.3 events模块

- node中的核心api都是异步事件驱动的

  - 某些对象(Emitters 发射器)发出某一个事件
  - 监听事件(Listensers) 并且传入的回调函数 会在监听到时间的时候调用

- 发出时间和监听事件都是通过`EventEmitter`类完成的 都属于`events`对象

  - `emitter.on(name,listener)`
    - 监听事件 也可以使用`addListener`
  - `emitter.off(name,listener)`
    - 移除事件监听,也可以使用`removeListener`
  - `emitter.emit(name[,...args])`
    - 发出时间 可以携带参数

- 常用方法

  ```js
  const EventEmitter = require("events")
  // 创建发射器
  const emitter = new EventEmitter()
  
  // 监听事件
  
  const event1 = (args)=>{console.log(args)}
  
  emitter.on("click",event1)
  
  // 发出事件
  emitter.emit("click","123")
  // 取消监听事件
  emitter.off("click",event1)
  
  ```
  
- 不常用方法

  ```js
  
  // 将本次监听放到最前面
  emitter.prependListener("click",event1)
  
  // 监听一次
  emiitter.once("click",event1)
  
  // 移除所有
  emitter.removeAllListeners([eventname]) // eventname可选
  ```

  

### 5 包管理工具(npm)

- 初始化

  ```shell
  npm init 
  
  npm init -y # 忽略所有
  ```

- 必填属性

  - name
  - version

- 选填属性
  - description
  - author
  - license(开源协议)
  - private(是否私有)

- 其他属性

  - main 项目入口(发布时使用 前端用不到)

  - script(自定义脚本)

    ```shell
    npm run start
    
    # start test stop等几个特殊命令可以省略 run
    ```

  - `dependencies ` 依赖

  - `devDependencies` 开发时依赖

    ```shell
    npm i name --save-dev
    ```

- 版本控制

  ```js
  "dependencies": {
      "axios": "^0.21.1"
    }
  ```

  ```shell
  npm i name@1.1.1
  ```

- `npm install`

  ```shell
  # 全局安装 一般都是工具
  npm install yarn -g
  
  # 局部安装 
  npm install axios
  npm i axios
  
  # 开发者环境
  npm install webpack --save-dev
  npm install webpack -D
  npm i webpack -D
  
  ```

- `package.lock.json`

  ```js
  name //项目的名称；
  version //项目的版本；
  lockfileVersion //lock文件的版本；
  requires //使用requires来跟着模块的依赖关系；
  dependencies//项目的依赖
  // 当前项目依赖axios，但是axios依赖follow-redireacts；
  // axios中的属性如下：
  	version // 表示实际安装的axios的版本；
      resolved // 用来记录下载的地址，registry仓库中的位置；
  	requires //记录当前模块的依赖；
  	integrity // 用来从缓存中获取索引，再通过索引去获取压缩包文件；
  ```

- ###### 安装过程

  - 没有lock文件
    - 分析依赖关系，这是因为我们可能包会依赖其他的包，并且多个包之间会产生相同依赖的情况；
    - 从registry仓库中下载压缩包（如果我们设置了镜像，那么会从镜像服务器下载压缩包）；
    -  获取到压缩包后会对压缩包进行缓存（从npm5开始有的）；
    -  将压缩包解压到项目的node_modules文件夹中（前面我们讲过，require的查找顺序会在该包下面查找）
  - 有lock文件
    -  检测lock中包的版本是否和package.json中一致（会按照semver版本规范检测）；
      - 不一致，那么会重新构建依赖关系，直接会走顶层的流程；
    -  一致的情况下，会去优先查找缓存
      - 没有找到，会从registry仓库下载，直接走顶层流程；
    - 查找到，会获取缓存中的压缩文件，并且将压缩文件解压到node_modules文件夹中；

- 其他命令

  - 卸载

    ```shell
    npm uninstall package
    npm uninstall package --save-dev
    npm uninstall package -D
    ```

  - 重新build

    ```shell
    npm rebuild
    ```

  - 清缓存

    ```shell
    npm cache clean
    ```

  - [更多]: https://docs.npmjs.com/cli-documentation/cli

- `yarn`

  | npm                                                          | yarn                               |
  | ------------------------------------------------------------ | ---------------------------------- |
  | npm install                                                  | yarn install                       |
  | npm install [package]                                        | yarn add [package]                 |
  | npm install --save [package]                                 | yarn add--save [package]           |
  | npm install --save-dev [package]                             | yarn add--save [package] [--dev-D] |
  | npm rebuild                                                  | yarn install --force               |
  | npm uninstall package<br/>npm uninstall package --save-dev<br/>npm uninstall package -D | yarn remove [package]              |
  | npm cache clean                                              | yarn cache clean                   |
  | rm -rf node_modules && npm install                           | yarn upgrade                       |

- cnpm

  - 查看镜像

    ```shell
    npm config get registry
    ```

  - 设置镜像

    ```shell
    npm config set registry https://registry.npm.taobao.org
    ```

  - 使用cnpm

    ```shell
    npm install -g cnpm --registry=https://registry.npm.taobao.org
    cnpm config get registry # https://r.npm.taobao.org/
    ```

- npx

  > 调用项目中某个模块的指令

  ```shell
  ./node_module/.bin/webpack --version
  
  npx webpack --version
  ```



### 6 开发脚手架工具

- 创建入口文件

  - 可以编写一些简单的代码

    ```js
    #!/usr/bin/env node 
    // shebang 可以找到node运行本文件
    console.log("my cli")
    ```

    

- 创建`package.json`

  ```shell
  npm init -y
  ```

- 修改`package.json`

  > 这个地方尽量不要起短的名字 

  ```json
  {
      "bin": {
          "fc4ever": "index.js"
       },
  }
  ```

- 配置环境变量

  ```shell
  npm link
  ```

  - 运行

    ```shell
    fc4ever
    # my cli
    ```

- 安装`commander`

  ```shell
  npm i commander
  ```

- 修改`index.js`

  ```js
  #!/usr/bin/env node
  
  const program = require("commander")
  
  program.version(require('./package.json').version[,'-v','--version']) 
  // 后面的写了会覆盖默认的-V
  
  program.parse(process.argv) // 放在后面
  ```

  ```shell
  # 查看版本号
  fc4ever --version
  
  fc4ever -V
  ```

- 增加options

  ```js
  #!/usr/bin/env node
  
  const program = require("commander")
  
  program.version(require('./package.json').version)
  // 添加option
  program.option('-f --framework',"framework type,exp: vue")
   // 参数名 dest
  program.option('-d --dest <dest>',"a destination folder,exp: -d /src/components")
  
  program.parse(process.argv)
  
  
  // 监听指令
  program.on('--help',function(){
    console.log("-----------------")
    console.log("other:")
    console.log(" other options~")
  })
  
  console.log(program._optionValues.dest)
  ```

- 封装

  - index.js

    ```js
    #!/usr/bin/env node
    
    const program = require("commander")
    
    const {helpOption} = require('./lib/core/help')
    
    // 查看版本
    program.version(require('./package.json').version)
    
    // 帮助
    helpOption()
    
    
    program.parse(process.argv)
    ```

  - help.js

    ```js
    const program = require("commander")
    
    const helpOptions = () => {
      // 添加option
      program.option('-f --framework', "framework type,exp: vue")
      // 参数名 dest
      program.option('-d --dest <dest>', "a destination folder,exp: -d /src/components")
    
      // 监听指令
      program.on('--help', function () {
        console.log("-----------------")
        console.log("other:")
        console.log(" other options~")
      })
    }
    
    module.exports = {
      helpOptions
    }
    ```

- 自定义指令

  - 新建create并导入

    ```js
    const program = require('commander')
    
    const createCommands = ()=>{
        //  project 参数名
        //  [others...] 其他参数
      program
        .command('create <project> [others...]')
        .description("clone a repository into a folder")
        .action((project,others)=>{
          console.log(project,others)
        })
    }
    
    module.exports =  createCommands
    ```

  - 调用

    ```shell
    fc4ever create demoName abc
    # demo [ 'abc' ]
    ```

  - 对actions进行封装

    ```js
    const createProjectAction = (project) =>{
      console.log(project)
    }
    
    module.exports = {
      createProjectAction
    }
    ```

    ```js
    const program = require('commander')
    
    const {createProjectAction} = require('./action')
    
    const createCommands = ()=>{
      program
        .command('create <project> [others...]')
        .description("clone a repository into a folder")
        .action(createProjectAction)
    }
    
    module.exports =  createCommands
    ```
    
  - 拉取代码实现`create`

    ```shell
    npm i download-git-repo
    ```

    - action.js

    ```js
    const {promisify} = require('util')
    const download = promisify(require('download-git-repo'))
    const {cesiumRepo} =require("../config/repo-config.js")
    const createProjectAction = async (project) =>{
      // clone项目
      await download(cesiumRepo,project,{clone:true})
    }
    
    module.exports = {
      createProjectAction
    }
    ```

    - repo-config

    ```js
    let cesiumRepo = 'direct:https://github.com/firstchoice6/vue-cesium.git'
    
    module.exports = {
      cesiumRepo
    }
    ```

  - 实现剩余步骤

    - 封装`terminal`相关指令

      ```js
      /**
       * 执行终端命令相关的代码
       */
      
      const { spawn } = require('child_process')
      
      const commandSpawn = (...args) => {
        return new Promise((resolve, rejects) => {
          // 返回一个子进程 子进程的打印信息默认不会打印在当前进程
          const childProcess = spawn(...args)
          // 将子进程的输出和错误信息打印到主进程
          childProcess.stdout.pipe(process.stdout)
          childProcess.stderr.pipe(process.stderr)
          // 监听结束
          childProcess.on('close', () => { resolve() })
        })
      }
      module.exports = {
        commandSpawn
      }
      ```

    - actions.js调用

      ```js
      const {commandSpawn} = require('../util/terminal.js')
      
      const createProjectAction = async (project) => {
        console.log('plz wait')
        // clone项目
        await download(cesiumRepo, project, { clone: true })
        // npm install - windows上要改成npm.cmd
        const command = process.platform === 'win32' ? 'npm.cmd':'npm'
        await commandSpawn(command,['install'],{cwd:`./${project}`})
        // npm run serve  -> await 会阻塞进程 所以不用await
        commandSpawn(command, ['run', 'serve'], { cwd: `./${project}` })
        // 打开浏览器
        
      }
      ```

    - 安装open库

      ```shell
      npm i open
      ```

      ```js
      const open = require('open')
      const createProjectAction = async (project) => {
        // ...
        // 打开浏览器 vue 默认8080 react默认3000
        open('http://localhost:8080')
      }
      ```

- 添加组件功能

  - create.js
  
  ```js
  program
      .command('add <cpn>')
      .description('add components')
      .action(name => addCpnAction(name, program.dest || 'src/components'))
  ```
  
  - action.js
  
  ```js
  /**
  ```
 * 
   * @param {name} name 组件名称
 * @param {dest} dest 目标路径
   */
    const addCpnAction = (name,dest)=>{
    // 1. ejs模板
    // 2. 编译ejs模板 -> 返回result
    // 3. 将result写入vue文件中
    // 4. 放到对应的文件夹中
}
  ```
  
  - ejs模板
  
    ```shell
    cd lib
    mkdir template
    cd template
    New-item component.vue.ejs
  ```

    ```ejs
    <template>
      <div class="<%= data.lowerName %>">
        <h1>{{ message }}</h1>
      </div>
    </template>
    
    <script>
    export default {
      name: "<%= data.name %>",
      props: {
        msg: String,
      },
      components: {},
      mixins: [],
      data() {
        return {
          message: "<%= data.name %>",
        };
      },
    methods: {},
      computed: {},
  };
    </script>
    
    <style scoped>
    .<%= data.lowerName %> {
    }
  </style>
    
    ```

  - 编译ejs模板
  
    ```shell
    npm i ejs
    cd ./lib/util
    New-item util.js
    ```
  
    ```js
    const ejs = require('ejs')
    const path = require('path')
    
    const compile = (template,data)=>{
      const position = `../${template}`
      const templatePath = path.resolve(__dirname,position) 
      ejs.renderFile(templatePath)
    }
    
    module.exports = {
      compile
    }
    ```
  
    ```js
    /**
     * 
    ```
   * @param {name} name 组件名称
     * @param {dest} dest 目标路径
      */

    const addCpnAction = async (name, dest) => {
      // 1.ejs模板
        // 2. 编译模板
          const result = await compile('vue-component.ejs',{name,lowerName:name.toLowerCase()})
          console.log(result)
    }
    ```

  - 封装写入文件
  
    - util.js
    
    ```js
    const writeToFile = (path, content) => {
      return fs.promises.writeFile(path, content)
    }
    module.exports = {
        writeToFile
    }
    ```
    
    
    
    ```js
    const {writeToFile} = require "../util.js"
    
    const addCpnAction = async (name, dest) => {
      // 1.ejs模板
      // 2.编译模板
      const result = await compile('vue-component.ejs',{name,lowerName:name.toLowerCase()})
      // console.log(result)
      // 3.写入文件
      const targetPath = path.resolve(dest,`${name}.vue`)
      writeToFile(targetPath,result)
    
    }
    ```


### 7 Buffer

- 创建

  ```js
  const str = "hello buffer"
  // 过期(不推荐)
  new Buffer(str)
  // 推荐
  Buffer.from(str[,编码方式])
  
  //alloc方式
  Buffer.alloc(size[,fill[,编码方式]])
  ```

  - utf8里面 一个中文三个字节 一个字母一个字节

- 转化成字符串

  ```js
  Buffer.from(str).toString([解码方式(可选)])
  ```

- 赋值

  ```javascript
  const buffer = Buffer.alloc(8)
  
  buffer[0] = 88
  
  buffer[1] = 0x88
  ```

- 应用:文件操作

  ```js
  // 读取文本
  const fs = require("fs")
  
  fs.readFile('./hello.txt',(err,text)=>{
    console.log(text.toString())
  })
  
  // 操作图片 - 复制
  const fs = require("fs")
  
  fs.readFile('./org.jpg',(err,data)=>{
    fs.writeFile('./copy.jpg',data,console.log)
  })
  
  ```

  - `sharp`的使用

    ```shell
    npm run sharp
    ```

    ```js
    const sharp = require('sharp')
    
    sharp(buffer or path)
    	.resize()
    	....
        .toFile('./bar.jpg')
    
    sharp(buffer or path)
    	.resize()
    	....
        .toBuffer()
    	.then(data =>{
            fs.writeFile('./copy.jpg',data,console.log)
        })
    ```

- buffer的创建过程

  > 事实上我们创建Buffer时，并不会频繁的向操作系统申请内存，它会默认先申请一个8 * 1024个字节大小的内存，也就是**8kb**

### 8 事件循环和异步IO

#### 事件循环

>  事件循环是什么？
>
> - 事实上我把事件循环理解成我们编写的JavaScript和浏览器或者Node之间的一个桥梁。
>
> 浏览器的事件循环
>
> - 是一个我们编写的JavaScript代码和浏览器API调用(setTimeout/AJAX/监听事件等)的一个桥梁,桥梁之间他们通过回调函数进行沟通。
>
> Node的事件循环
>
> - 是一个我们编写的JavaScript代码和系统调用（file system、network等）之间的一个桥梁, 桥梁之间他们通过回调函数进行沟通的.

#### 宏任务和微任务

> 事件循环中并非只维护着一个队列，有两个队列
>
> - 宏任务队列（macrotask queue）：ajax、setTimeout、setInterval、DOM监听、UI Rendering等
> - 微任务队列（microtask queue）：Promise的then回调、 Mutation Observer API、queueMicrotask()等

- 规则
  - main script中的代码优先执行（编写的顶层script代码）；
  - 在执行任何一个宏任务之前（不是队列，是一个宏任务），都会先查看微任务队列中是否有任务需要执行
    - 宏任务执行之前，必须保证微任务队列是空的
    - 如果不为空，优先执行微任务队列中的任务（回调）；

- 案例

  ```js
  setTimeout(() => {
    console.log('set1')
    new Promise((resolve, reject) => {
      resolve()
    }).then(() => {
      new Promise((resolve, reject) => {
        resolve()
      }).then(() => {
        console.log('then4')
      })
      console.log('then2')
    })
  })
  new Promise((resolve, reject) => {
    console.log('pro1')
    resolve()
  }).then(() => {
    console.log('then1')
  })
  
  setTimeout(() => {
    console.log('set2')
  })
  
  console.log(2)
  
  
  queueMicrotask(() => {
    console.log('queueMicrotask1')
  })
  
  new Promise((resolve, reject) => {
    resolve()
  }).then(() => {
    console.log('then3')
  })
  ```

  - 分析

    1. set1代码放入宏任务队列

       ```text
       宏:set1
       微:
       已打印:
       ```

    2. promise中的代码立即执行 打印`pro1`  `then1`加入微任务队列

       ```text
       宏:set1
       微:then1
       已打印:pro1
       ```

       

    3. set2放入宏任务队列

       ```text
       宏:set1 set2
       微:then1
       已打印:pro1
       ```

       

    4. 打印`2`立即执行

       ```text
       宏:set1 set2
       微:then1
       已打印:pro1 2
       ```

       

    5. `queueMicrotask1`加入微任务队列

       ```text
       宏:set1 set2
       微:then1 queueMicrotask1 
       已打印:pro1 2 
       ```

    6. `then3` 加入微任务

       ```text
       宏:set1 set2
       微:then1 queueMicrotask1 then3
       已打印:pro1 2 
       ```

       

    7.  执行微任务 打印`then1 queueMicrotask1 then3`

       ```text
       宏:set1 set2
       微:
       已打印:pro1 2 then1 queueMicrotask1 then3
       ```

    8. 执行宏任务set1,打印`set1` `then2`加入微任务队列

       ```text
       宏:set2
       微:then2
       已打印:pro1 2 then1 queueMicrotask1 then3 set1
       ```

       

    9. 执行微任务then2 打印then2 `then4`加入微任务队列

       ```text
       宏:set2
       微:then4
       已打印:pro1 2 then1 queueMicrotask1 then3 set1 then2 
       ```

    10. 执行微任务 打印then4

        ```text
        宏:set2
        微:
        已打印:pro1 2 then1 queueMicrotask1 then3 set1 then2 then4
        ```

        

    11. 执行set2 打印`set2`

        ```text
        宏:
        微:
        已打印:pro1 2 then1 queueMicrotask1 then3 set1 then2 then4 set2
        ```

- node架构

- 阻塞IO和非阻塞IO

  > 如果我们希望在程序中对一个文件进行操作，那么我们就需要打开这个文件：通过文件描述符。
  >
  > - 我们思考：JavaScript可以直接对一个文件进行操作吗？
  > - 看起来是可以的，但是事实上我们任何程序中的文件操作都是需要进行系统调用（操作系统的文件系统）；
  > - 事实上对文件的操作，是一个操作系统的IO操作（输入、输出）；
  >
  > 操作系统为我们提供了阻塞式调用和非阻塞式调用：
  >
  > - 阻塞式调用： 调用结果返回之前，当前线程处于阻塞态（阻塞态CPU是不会分配时间片的），调用线程只有
  >   在得到调用结果之后才会继续执行。 
  > - 非阻塞式调用： 调用执行之后，当前线程不会停止执行，只需要过一段时间来检查一下有没有结果返回即可。
  >
  > 所以我们开发中的很多耗时操作，都可以基于这样的 非阻塞式调用：
  >
  > - 比如网络请求本身使用了Socket通信，而Socket本身提供了select模型，可以进行非阻塞方式的工作；
  > - 比如文件读写的IO操作，我们可以使用操作系统提供的基于事件的回调机制；

- 非阻塞IO的问题

  - 轮询

  - 线程池

    > libuv提供了一个线程池（Thread Pool）：
    >
    > - 线程池会负责所有相关的操作，并且会通过轮训等方式等待结果； 
    > - 当获取到结果时，就可以将对应的回调放到事件循环 （某一个事件队列）中；
    > - 事件循环就可以负责接管后续的回调工作，告知Js应用程序执行对应的回调函数；

- 阻塞和非阻塞，同步和异步的区别

  - 阻塞和非阻塞是对于被调用者来说的
    - 操作系统为我们提供了阻塞调用和非阻塞调用
  - 同步和异步是对于调用者来说的
    - 发起调用之后，不会进行其他任何的操作，只是等待结果，这个过程就称之为同步调用；
    - 发起调用之后，并不会等待结果，继续完成其他的工作，等到有回调时再去执行，这个过程就是异步调用；
  - Libuv采用的就是非阻塞异步IO的调用方式；

- node的事件循环阶段

  > 但是一次完整的事件循环Tick分成很多个阶段

  - 定时器（Timers）：
    - 本阶段执行已经被 setTimeout() 和 setInterval() 的调度回调函数。
  - 待定回调（Pending Callback）：
    - 对某些系统操作（如TCP错误类型）执行回调，比如TCP连接时接收到ECONNREFUSED。
  - idle, prepare：
    - 仅系统内部使用。
  - 轮询（Poll）：
    - 检索新的 I/O 事件；执行与 I/O 相关的回调；
  - 检测：
    - setImmediate() 回调函数在这里执行。 
  - 关闭的回调函数：
    - 一些关闭的回调函数，如：socket.on('close', ...)。

- 一次事件循环的Tick也分为微任务和宏任务：

  -  宏任务（macrotask）：setTimeout、setInterval、IO事件、setImmediate、close事件；
  - 微任务（microtask）：Promise的then回调、process.nextTick、queueMicrotask；

- 案例

  ```js
  async function async1() {
    console.log('async1 start')
    await async2()
    console.log('async1 end')
  }
  
  async function async2() {
    console.log('async2')
  }
  console.log('script start')
  setTimeout(() => {
    console.log("setTimeout0")
  }, 0)
  
  setTimeout(() => {
    console.log("setTimeout2")
  }, 300)
  
  setImmediate(() => console.log('setImmediate'))
  
  process.nextTick(() => console.log('nextTick1'))
  
  async1()
  
  process.nextTick(() => console.log('nextTick2'))
  
  new Promise((resolve, _) => {
    console.log('promise1')
    resolve()
    console.log(promise2)
  }).then(() => {
    console.log('promise3')
  })
  
  console.log('script end')
  ```

  - 执行过程

    - 打印`script start`

      ```text
      main script:
      nextTicks:
      other microtask:
      timers:
      immediate:
      打印:script start
      ```

    - `settimeout0`加入time `setImmediate`加入immediate,`nextTick1`加入nexttick

      ```
      main script:
      nextTicks:nextTick1
      other microtask:
      timers: settimeout0
      immediate:setImmediate
      打印:script start
      ```

    - async1执行 打印`async1 start`  立即执行`async2`  `async1 end`加入微任务队列 

      ```text
      main script:
      nextTicks:nextTick1
      other microtask: async1_end
      timers: settimeout0
      immediate:setImmediate
      打印:script_start async1_start async2 
      ```

    - `nexttick2`加入nexttick

      ```text
      main script:
      nextTicks:nextTick1 nextTick2
      other microtask: async1_end
      timers: settimeout0
      immediate:setImmediate
      打印:script_start async1_start async2 
      ```

    - promise1直接执行 打印`promise1` `promise3`放入微任务队列 打印`promise2`,`script end` main script结束

      ```text
      main script:
      nextTicks:nextTick1 nextTick2
      other microtask: async1_end promise3
      timers: settimeout0
      immediate:setImmediate
      打印:script_start async1_start async2 promise1 promise2 script_end
      ```

    - 执行nexttick中的代码 打印 `nextTick1`,`nextTick2`

      ```text
      main script:
      nextTicks:
      other microtask: async1_end promise3
      timers: settimeout0
      immediate:setImmediate
      打印:script_start async1_start async2 promise1 promise2 script_end nextTick1 nextTick2
      ```

    - 执行微任务中的代码 打印 `async1_end`,`promise3`

      ```text
      main script:
      nextTicks:
      other microtask: 
      timers: settimeout0
      immediate:setImmediate
      打印:script_start async1_start async2 promise1 promise2 script_end nextTick1 nextTick2 async1_end promise3
      ```

    - 执行timer和immediate中的代码 打印`settimeout0` ,`setImmediate`

      ```text
      main script:
      nextTicks:
      other microtask: 
      timers: 
      immediate:
      打印:script_start async1_start async2 promise1 promise2 script_end nextTick1 nextTick2 async1_end promise3 settimeout0 setImmediate
      ```

    - 事件队列等到300ms时 `settimeout2`加入time 打印`settimeout2`

      ```text
      script_start
      async1_start
      async2
      promise1
      promise2
      script_end
      nextTick1
      nextTick2
      async1_end
      promise3
      settimeout0
      setImmediate
      settimeout2
      ```

  - 案例

    ```js
    setTimeout(() => {
      console.log("setTimeout0")
    }, 0)
    
    setImmediate(() => console.log('setImmediate'))
    // 有时timeout先打印 有时Immediate先打印
    ```

    - 原因

      > 在一个tick中 io阶段会停留 io的回调尽可能早的响应
      >
      > 第一次tick的时候 settimeout不一定早于初始化事件循环
      >
      > ​	check阶段 setImmediate一定会放在这个tick中

### 9 Stream

> 所有流都是`eventEmitter`的实例
>
> 四种类型
>
> - writable
> - readable
> - duplex
> - transform

#### 9.1 readable

```js
const fs = require("fs")

const path = './hello.txt'
const reader = fs.createReadStream(path, {
  start: 3,
  end: 5,
  highWaterMark: 2,
})
reader.on("open", () => console.log("文件打开"))
reader.on("data", (data) => { 
  console.log(data)
  reader.pause()
  setTimeout(()=>{
    reader.resume()
  },2000)
})
reader.on("close", () => console.log("文件关闭"))

```

#### 9.2 writable
```js
const fs = require('fs')

// fs.writeFile("./test.txt","hello stream",{flag:"a"},console.log)

const writer  = fs.createWriteStream('./bar.txt',{
  flags:"a",
  start:4
})

writer.write("你好",err=>{
  if(err) return console.log(err)
  console.log("success")
})

writer.write("liz",err=>{
  if(err) return console.log(err)
  console.log("success")
})

// writer.close()
//  end = write + close
writer.end("结束的时候写入的东西")

writer.on("close",()=>{console.log("文件关闭")})
```

#### 9.3 pipe

```js
const fs = require('fs')

const reader = fs.createReadStream("./foo.txt")
const writer = fs.createWriteStream("./bar.txt")

// 直接写入writer流
reader.pipe(writer)
```

### 10 Http模块

- 安装nodemon

```shell
npm i nodemon -g
```

#### 10.1 基本操作

- 创建服务器

  ```js
  const http = require("http")
  
  const serve = http.createServer((req,res)=>{
      // request请求对象 response响应对象
      res.end("hello")
  })
  
  // 方式二  
  const serve =  new http.Server()
  // 方式一的本质是方式二
  
  serve.listen(8000,()=>{
    console.log("服务启动成功")
  })
  ```

- 监听主机和端口号

  > listen函数有三个参数
  >
  > - 端口port: 可以不传, 系统会默认分配端
  > - 主机host: 可以不传 通常可以传入localhost、ip地址127.0.0.1、或者ip地址0.0.0.0，默认是0.0.0.0；
  > - 回调函数：服务器启动成功时的回调函数；

#### 10.2 request对象

> request对象封装了客户端给服务器传递的所有信息
>
> - url
> - method
> - headers

- url的处理

  ```js
  const url = require("url")
  // 解析url
  url.parse(req.url)
  ```

- query的处理

  ```js
  const http = require("http")
  const url = require("url")
  const qs = require("querystring")
  
  const serve = http.createServer((req,res)=>{
    const {query} = url.parse(req.url)
    const {name} = qs.parse(query)
    console.log(name)
    res.end("hello")
  })
  
  ```

- post方式

  ```js
  const http = require("http")
  const url = require("url")
  
  const serve = http.createServer((req, res) => {
    const { pathname } = url.parse(req.url)
    console.log(pathname)
    if (pathname === "/login") {
      if (req.method === "POST") {
        req.on('data', data => console.log(data.toString()))
         
        res.end("hello")
      }
    }
  })
  ```

  - 也可以提前设置字符集

    ```js
     req.setEncoding("utf-8")
     req.on('data', console.log)
    ```

  - 使用`JSON.parse`解析body

    ```js
    req.on('data', data => {
    	const {name} = JSON.parse(data)
    })
    ```

- headers
  - application/json表示是一个json类型；
  - text/plain表示是文本类型；
  - application/xml表示是xml类型；
  - multipart/form-data表示是上传文件；
  - content-length：文件的大小和长度
  - keep-alive
  -  accept-encoding：告知服务器，客户端支持的文件压缩格式，比如js文件可以使用gzip编码，对应 .gz文件；
  - accept：告知服务器，客户端可接受文件的格式类型；
  - user-agent：客户端相关的信息；

#### 10.3 response对象

- 设置状态码

  ```js
  const http = require("http")
  
  const serve = http.createServer((req, res) => {
    // 设置状态码
    // 方式一 属性赋值
    res.statusCode = 401
    // 方式二 和head一起设置
    res.writeHead(503)
  })
  ```

- 响应header

  ```js
  // 方式一
  res.setHeader("Content-Type","text/plain")
  
  // 方式二
  res.writeHead(200,{
      "Content-Type","text/plain"
  })
  ```

#### 10.4 http请求

- 原生

  ```js
  // get方式
  http.get("http://localhost:8000", res => {
    res.setEncoding("utf-8")
    res.on("data", console.log)
  })
  
  // post方式
  http.request({
    method:"POST",
    hostname:"localhost",
    port:8000
  },res=>{
    res.setEncoding("utf-8")
    res.on("data", console.log)
  }).end()
  ```

- 使用axios

  >  axios库可以在浏览器中使用，也可以在Node中使用：
  >
  > - 在浏览器中，axios使用的是封装xhr； 
  > - 在Node中，使用的是http内置模块；

#### 10.5 ~~文件上传(原生)~~

```js
const http = require('http');
const fs = require('fs');
const qs = require('querystring');

const server = http.createServer((req, res) => {
  if (req.url === '/upload') {
    if (req.method === 'POST') {
      req.setEncoding('binary');

      let body = '';
      const totalBoundary = req.headers['content-type'].split(';')[1];
      const boundary = totalBoundary.split('=')[1];

      req.on('data', (data) => {
        body += data;
      });

      req.on('end', () => {
        console.log(body);
        // 处理body
        // 1.获取image/png的位置
        const payload = qs.parse(body, "\r\n", ": ");
        const type = payload["Content-Type"];

        // 2.开始在image/png的位置进行截取
        const typeIndex = body.indexOf(type);
        const typeLength = type.length;
        let imageData = body.substring(typeIndex + typeLength);

        // 3.将中间的两个空格去掉
        imageData = imageData.replace(/^\s\s*/, '');

        // 4.将最后的boundary去掉
        imageData = imageData.substring(0, imageData.indexOf(`--${boundary}--`));

        fs.writeFile('./foo.png', imageData, 'binary', (err) => {
          res.end("文件上传成功~");
        })
      })
    }
  }
});

server.listen(8000, () => {
  console.log("文件上传服务器开启成功~");
})

```

### 11 express框架

#### 11.1 基本使用

- 搭建环境

  ```shell
  npm init 
  npm i express
  ```

- 基本使用

  ```js
  const express = require("express")
  
  // express 本质是一个函数createApplication
  // 返回app
  const app = express()
  
  // 监听默认路径
  
  app.get('/',(req,res,next)=>{
    res.end("hello ")
  })
  
  app.listen(8000,()=>{
    console.log('启动成功')
  })
  ```

#### 11.2 脚手架方式

- 安装

  ```shell
  npm i express-generator -g
  ```

- 创建项目

  ```shell
  express express-demo
  ```

- 安装依赖

  ```shell
  npm i
  ```

- 启动

  ```shell
  node bin/www
  ```

  

#### 11.3 中间件

> 本质是传递给express的回调函数
>
> - 请求对象（request对象）； 
> - 响应对象（response对象）；
> - next函数（在express中定义的用于执行下一个中间件的函数）；

- 普通的中间件

  ```js
  // 普通的中间件
  // use方式注册一个中间件 
  app.use((req,res,next)=>{
    console.log('注册了一个普通的中间件')
      next()
  })
  
  app.use((req,res,next)=>{
    console.log('注册了一个普通的中间件2')
    res.end("hello midware")
  })
  ```

  - 不写路径方式 响应所有请求方式 所有请求路径
  - 不写next() 只响应第一个
  - res.end()一般写最后一个

- 路径匹配中间件

  ```js
  app.use("/login",(req,res,next)=>{
    console.log('注册了一个匹配路径的中间件')
    res.end("hello world")
  })
  ```

- 路径和方法中间件

  ```js
  app.get("/login",(req,res,next)=>{
    console.log('路径和方法中间件')
    res.end("hello world")
  })
  ```
  
- 连续注册中间件

  ```js
  app.get("/login",
    (req,res,next)=>{
    console.log('注册了一个匹配路径的中间件')
    next()
  },(req,res,next)=>{
    console.log('注册了一个匹配路径的中间件2')
    res.end("hello world")
  })
  ```

- 中间件应用:`JSON`解析

  ```js
  app.use((req, res, next) => {
    if (req.headers["content-type"] === "application/json") {
      req.on('data', data => {
        const info = JSON.parse(data.toString())
        req.body = info
        next()
      })
    } else {
      next()
    }
  })
  ```

  - 使用express

    ```js
    const express = require("express")
    const app = express()
    
    app.use(express.json())
    // true 使用qs库
    // false 使用node内部的querystring模块
    app.use(express.urlencoded({extended:true}))
    ```

- 中间件应用:解析form-data

  ```shell
   npm install multer
  ```

  - 解析文本

  ```js
  const multer = require("multer")
  
  const upload = multer()
  // 可以解析其他任意类型(text) 
  app.use(upload.any())
  
  ```

app.post("/login")
  ```

  - 解析文件
  
  ```js
  const upload = multer({
      dest:"./uploads/"
  })
  
  app.use(upload.any())
  // 获取上传文件 并保存
  // single 单个文件 array 多个文件
  app.post("upload",upload.single('key'),(req,res,next)=>{
      res.end("文件上传成功")
})
  ```

  - 自定义文件信息

  ```js
  const path = require("path")
  const storage = multer.diskStorage({
      destination:(req,file,callback)=>{
          callback(null,'./uploads/')
      },
      filename:(req,file,callback)=>{
          callback(null,"foo.png",Date.now() + path.extname(file.originalname))
      } 
  })
  const upload = multer({
      storage
  })
  
  // 获取上传文件 并保存
  // single 单个文件 array 多个文件
  app.post("upload",upload.fields([{name:'file',maxCount:2}]),(req,res,next)=>{
      res.end("文件上传成功")
  })
  ```

  - <span style="color:red">注意:  </span>有文件上传需求时`app.use(upload.any())`不要写成全局的,防止恶意上传,旨在需要处理文件上传的路由中使用
  
- 中间件应用:日志

  ```shell
  npm i morgan
  ```

  ```js
  const morgan = require("morgan")
  const writerStream = fs.createWriteStream("./logs/access.log",{
      flag:"a+"
  })
  
  app.use(morgan("combined",{stream:writerStream}))
  ```

#### 11.4 其他

- params

  - 请求地址

    ```text
    http://localhost:8000/login/abc/why
    ```

  - 获取参数

    ```js
    app.use('/login/:id/:name',(req,res)=>{
        console.log("请求成功,参数为" , req.params)
        res.json("请求成功")
    })
    ```

- query

  - 请求地址

    ```text
    http://localhost:8000/login?username=why&password=123
    ```

  - 获取参数

    ```js
    app.use('/login',(req,res)=>{
        console.log("请求成功,参数为" , req.query)
        res.json("请求成功")
    })
    ```

- 响应数据

  -  end方法
  - json方法
  - status方法

  ```js
  res.end("success")
  res.json({name:"jack"})
  res.status(204)
  ```

- 路由的使用

  ```shell
  mkdir routers
  cd .\routers\
  New-item user.js
  ```

  - 导出

  ```js
  const express = require("express")
  
  const router = express.Router()
  
  router.get("/",(_,res,_)=>{
    res.json(["why","coder"])
  })
  router.get("/:id",(req,res,_)=>{
    res.json(`${req.params.id}用户的信息`)
  })
  router.post("/",(_,res,_)=>{
    res.json("success")
  })
  module.exports = router
  ```

  - 导入(index.js)

  ```js
  const express = require("express")
  
  const userRouter = require("./routers/user")
  const app = express()
  
  app.use("user",userRouter)
  
  app.listen(8000, () => {
    console.log('启动成功')
  })
  ```

- 静态资源服务器

  ```js
  const express = require("express")
  const app = express()
  app.use(express.static('./build'))
  
  app.listen(8000,()=>{
      logs("静态服务器启动成功")
  })
  
  ```

- 服务端的错误处理

  ```js
  app.use((err,_,res,_)=>{
      const msg = err.msg
      
      switch(message){
          case "USER DOES NOT EXISTS":
              res.status(400).json({msg})
      }
      
      res.status(500)
  })
  
  app.post("/register",(req,res,next)=>{
      if(!isExists){
          
      }else{
          next(new Error("username is already exists"))
      }
  })
  ```

### 12 koa框架

#### 12.1 基本使用

```shell
npm i koa
```

```js
const Koa = require("koa")
const app = new Koa()
app.use((ctx,next)=>{
    // ctx.request ctx.response
    ctx.response.body = "hello koa"
})

app.listen(8000,()=>{
    console.log("running in 8000 port...")
})
```

#### 12.2 注册中间件

- 注意
  - 没有`app.get()`
  - 没有`app.use(path,callback)`
  - 没有`app.use(callback1,callback2)`

```js
app.use((ctx,next)=>{
  if(ctx.request.url === "/login"){
    if(ctx.request.method === "GET"){
      ctx.response.body = "success"
    }
  }else{
    ctx.response.body = "other"
  }
})
```

#### 12.3 路由的使用

- 安装

```shell
npm i koa-router
```

```shell
md routers
cd routers
New-item user.js
```

- 导出

```js
const Koa = require("koa")
const app = new Koa()
const userRouter = require("./routers/user") 

app.use(userRouter.routes())

app.listen(8000,()=>{
    console.log("running in 8000 port...")
})
```

- 导入

```js
const Koa = require("koa")
const app = new Koa()
const userRouter = require("./routers/user") 

app.use(userRouter.routes())
// 405提示 Method Not Allowed 默认404 NOT FOUND
app.use(userRouter.allowedMethods())

app.listen(8000,()=>{
    console.log("running in 8000 port...")
})
```

#### 12.4 参数解析

- params和query

  > app.use可以拿到query 拿不到query 使用router.get可以获取到params

  ```js
  app.use((ctx,next)=>{
    console.log(ctx.request.url)
    console.log(ctx.request.query)
    console.log(ctx.request.params) // undefined
  })
  
  router.get('/:id',(ctx,next)=>{
    console.log(ctx.request.query)
    console.log(ctx.request.params)
  })
  ```

- json和urlencoded

  ```shell
  npm i koa-bodyparser
  npm i koa-multer
  ```

  ```js
  const bodyParser = require("koa-bodyparser")
  
  
  app.use(bodyParser())
  
  ```

  ```js
  const multer = require("koa-multer")
  
  const upload = multer()
  
  router.post("/",(ctx,next)=>{
    ctx.response.body = "post request"
    console.log(ctx.request.body)
      next()
  })
  
  router.post("/avatar",upload.any(),(ctx,next)=>{
    ctx.response.body = "头像上传成功"
      // **req**
    console.log(ctx.req.body)
  })
  ```

- 上传文件

  ```js
  const Router = require("koa-router")
  const multer = require("koa-multer")
  
  const router = new Router({prefix:"/user"})
  const upload = multer({
    dest:'../uploads/'
  })
  
  router.post("/avatar",upload.single("avatar"),(ctx,next)=>{
    ctx.response.body = "头像上传成功"
    console.log(ctx.req.file)
  })
  ```

- 响应数据

  - 输出结果`ctx.response.body` || `ctx.body`
  - string
    - Buffer
  - Stream
    - Object | Array
    - null
  - 请求状态`ctx.status` || `ctx.response.status`



#### 12.5 静态服务器和错误处理

- 静态服务器

  ```shell
  npm i koa-static
  ```

  ```js
  const Koa = require('koa')
  const static = require("koa-static")
  
  const app = new Koa()
  
  app.use(static('./build'))
  
  app.listen(8000,()=>{
      console.log("启动成功")
  })
  ```

- 错误处理

  ```js
  const Koa = require('koa')
  
  const app = new Koa()
  
  app.use((ctx,next)=>{
      ctx.app.emit('error',new Error("info"),ctx)
  })
  
  app.on("error",(err,ctx)=>{
      console.log(err.message)
      ctx.response.body = "error"
  })
  app.listen(8000,()=>{
      console.log("启动成功")
  })
  ```

### 13 node中使用mysql

#### 13.1 Mysql2

- 安装依赖

  ```shell
  npm install mysql2 
  ```

- 基本使用

  ```js
  const mysql = require("mysql2")
  
  // 创建数据库连接
  const connection = mysql.createConnection({
      host:"localhost",
      database:"tc_demo",
      user:"root",
      password:"123"
  })
  
  // 执行sql语句
  const statement = `
  	SELECT * FROM tc_book;
  `
  connection.query(statement,(err,result,fileds)=>{
      console.log(result)
      connection.end() // 查询终止 可以通过.on监听err信息
      // connection.destory() // 强制停止 监听不到错误
  })
  ```

- 预处理语句

  - 特点

    - 提高性能：将创建的语句模块发送给MySQL，然后MySQL编译（解析、优化、转换）语句模块，并且存储它但是不执行，之后我们在真正执行时会给?提供实际的参数才会执行；就算多次执行，也只会编译一次，所以性能是更高的；
    - 防止SQL注入：之后传入的值不会像模块引擎那样就编译，那么一些SQL注入的内容不会被执行；or 1 = 1不会被执行；
    - 如果再次执行该语句，它将会从LRU（Least Recently Used） Cache中获取，省略了编译statement的时间来提高性能。
    - 内部先调用`prepare` 再调用`query`

  - 语法

    ```js
    const statement = "select * from pro where price > ? and brand like ?;"
    connection.execute(statement,["1000","sam"],(err,res)=>{
        console.log(res)
    })
    ```

- 连接池

  > 连接池可以在需要的时候自动创建连接，并且创建的连接不会被销毁，会放到连接池中，后续可以继续使用；
  >  在创建连接池的时候设置LIMIT，也就是最大创建个数；

  ```js
  const pool = mysql.cteatePool({
      host:"localhost",
      database:"",
      user:"root",
      password:"124",
      connectionLimit:5
  })
  
  pool.execute(statement,["1000","sam"],(err,res)=>{
      console.log(res)
  })
  
  //Promise方式
  pool.promise().execute(statement,["1000","sam"]).then(([res,fileds])=>{
      console.log(res)
  })).catch(console.log)
  ```

#### 13.2 ORM - Sequelize

>  对象关系映射（英语：Object Relational Mapping，简称ORM，或O/RM，或O/R mapping），是一种程序
> 设计的方案：
>
> - 从效果上来讲，它提供了一个可在编程语言中，使用 虚拟对象数据库 的效果；
> - 比如在Java开发中经常使用的ORM包括：Hibernate、MyBatis；



### 14 项目实战

#### 14.1 初始化

```shell
npm init -y
npm install nodemon -D
npm i dotenv
```

- 目录结构
  - 按照功能模块
  - 按照业务模块











