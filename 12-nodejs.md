

# Node.js

- node.js既不是语言，也不是框架，是一个平台

- Node中，没有全局作用域，只有模块作用域

## Node中的模块系统

- 核心模块
  - fs
  - http
  - path等等
- 第三方模块
  - art-template
  - 必须通过npm下载
- 自己写的
  - 自己创建的文件

### commonJs模块规范

- 模块作用域
- export导出模块中的成员
- require加载模块

#### 加载`require`

- 执行被加载模块中的代码
- 得到被加载模块中exports导出接口对象

#### 导出`exports`

```js
exports.a = 123
exports.b = 'hello'
exports.c = function (a, b) {
    return a + b
}
exports.d = {
    foo: 'bar'
}

module.exports = add
// 导出单个常用这种
module.exports = 'hello'(后者覆盖前者)
// 直接导出某个成员，而不是通过挂载的方法
module.exports = {
    a: 123,
    b: 'hello',
    c: function (a, b) {
        return a + b
    }
} 
// 导出多个

或者
module.exports.a = 123
module.exports.b = 'hello'
module.exports.c = function (a, b) {
    return a + b
}
module.exports.d = {
    foo: 'bar'
}
//module.exports == exports
```

- cnpm淘宝镜像的安装

  `cmd` npm i -g cnpm --registry=https://registry.npm.taobao.org

#### `__dirname`和`__filename`

```shell
F:\codefc\node\blog
# __dirname获取当前文件模块所属目录的绝对路径 **动态获取**
F:\codefc\node\blog\app.js
#__filename获取当前文件的绝对路径**动态获取**
```

文件操作中，相对路径是不可靠的。

Node中文件操作的路径被设计为执行node命令所处的路径

```shell
path.join(__dirname,'/app.js')
```



#### 修改完代码自动重启

- nodemon安装和使用

  ```shell
  npm install -g nodemon
  
  # 使用方法
  nodemon app.js
  ```


## 终端基本使用

- md 创建目录
- rd(rmdir) 删除目录，目录内没有文档
- echo on a.txt 创建空文件
- del 删除文件
- rm 文件名 删除文件
- cat 文件名 查看文件内容
- cat > 文件名 向文件中写上内容
- cls 清屏

## Buffer基本操作

> Buffer对象是Node处理二进制数据的一个接口。它是Node原生提供的全局对象，可以直接使用，不需要require(‘buffer’)。

- 实例化
  + Buffer.from(array)
  + Buffer.from(string)
  + Buffer.alloc(size)
- 功能方法
  + Buffer.isEncoding() 判断是否支持该编码
  + Buffer.isBuffer() 判断是否为Buffer
  + Buffer.byteLength() 返回指定编码的字节长度，默认utf8
  + Buffer.concat() 将一组Buffer对象合并为一个Buffer对象
- 实例方法
  + write() 向buffer对象中写入内容
  + slice() 截取新的buffer对象
  + toString() 把buf对象转成字符串
  + toJson() 把buf对象转成json形式的字符串

# 核心模块API

> 模块文件的后缀：.js   .json  .node

## 路径操作Path

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

## 文件操作fs

> 异步I/O input/output
>   1、文件操作
>   2、网络操作
>
> 在浏览器中也存在异步操作：
> 1、定时任务
> 2、事件处理
> 3、Ajax回调处理
>
> js的运行是单线程的
> 引入事件队列机制
>
> Node.js中的事件模型与浏览器中的事件模型类似
> 单线程+事件队列
>
> Node.js中异步执行的任务：
> 1、文件I/O
> 2、网络I/O
>
> 基于回调函数的编码风格
> 带Sync的是同步的方法


### 文件信息获取

  ```js
  const fs = require('fs')
  
  异步操作
  fs.stst('./data.txt',(err,stat) =>{
      if (err) return
     	
      stats.isFile() 
      // 文件
      stats.isDirectory()
      // 目录
      stats.isBlockDevice()
      stats.isCharcterDevice()
      stats.isSymbolicLink()
      stats.isFIFO()
      stats.isSocket()
      
      属性
      atime // 访问时间
      ctime // 修改时间（文件状态，如权限）
      mtime // 修改时间（文件数据）
      birthtime // 文件创建的事件
  })
  
  同步操作
  let ret = fs.statSync('./data.txt')
  console.log(ret)
  ```

### 文件读写

```js
1. 使用require方法加载fs核心模块
var fs = require('fs') //file-system的缩写
=================================================================================
2.a 读取文件
	// 第一个参数 路径
	// 第二个参数可选 编码方式
	// 第二个参数 回调函数
		//读取成功  data  数据  error  null
		//读取失败  data null   error 错误对象
fs.readFile('./data/hello.txt','utf8', function (error, data) {
    if (error) {
        console.log('文件读取失败')
    } else {
        console.log(data.toString()) 
        // 不写输出二进制（自动转化为16进制）
        // 有第二个参数的时候，可以直接输出
    }    
})

同步操作
let ret = fs.readFileSync('./data/hello.txt','utf8')
// 返回异步操作中的data
========================================================================================
2.b 写文件(覆盖)
	// 第一个参数 路径
	// 第二个参数 文件内容
		//这里也可以加一个参数'utf8'
	// 第三个参数 回调函数
		//成功 error null
		//失败 error 错误对象
fs.writeFlie('./node/你好.md', 'hello world', function(error) {
    if(!err){
    console.log('写入成功')
    }
})

同步操作
fs.writeFlieSync('./node/你好.md', 'hello 世界','utf8')
========================================================================================
/*
	流式操作，针对大文件
    fs.createReadStream(path[, options])
    fs.createWriteStream(path[, options])
*/

const path = require('path');
const fs = require('fs');

let spath = path.join(__dirname,'../03-source','file.zip');
// 源路径
let dpath = path.join('C:\\Users\\www\\Desktop','file.zip');
// 目标路径

let readStream = fs.createReadStream(spath);
let writeStream = fs.createWriteStream(dpath);

let num = 1;
readStream.on('data',(chunk)=>{
     num++;
     writeStream.write(chunk);
});

readStream.on('end',()=>{
    console.log('文件处理完成'+num);
});
// -----------------------------------

// pipe的作用直接把输入流和输出流
readStream.pipe(writeStream);

// ----------------------------------
最终可以用下面一行代码完成
fs.createReadStream(spath).pipe(fs.createWriteStream(dpath));

```

### 目录操作

  ```js
1、创建目录
fs.mkdir(path[, mode], callback)
fs.mkdirSync(path[, mode])

fs.mkdir(path.join(__dirname,'abc'),(err)=>{
    console.log(err);
});

fs.mkdirSync(path.join(__dirname,'hello'));

2、读取目录
fs.readdir(path[, options], callback)
fs.readdirSync(path[, options])

let files = fs.readdirSync(__dirname);
files.forEach((item,index)=>{
    fs.stat(path.join(__dirname,item),(err,stat)=>{
        if(stat.isFile()){
            console.log(item,'文件');
        }else if(stat.isDirectory()){
            console.log(item,'目录');
        }
    });
});
3、删除目录
fs.rmdir(path, callback)
fs.rmdirSync(path)

fs.rmdir(path.join(__dirname,'abc'),(err)=>{
    console.log(err);
});

fs.rmdirSync(path.join(__dirname,'qqq'));

const path = require('path');
const fs = require('fs');

1、创建目录
fs.mkdir(path[, mode], callback)
fs.mkdirSync(path[, mode])
// 异步
fs.mkdir(path.join(__dirname,'abc'),(err)=>{
  console.log(err);
});
// 同步
fs.mkdirSync(path.join(__dirname,'hello'));
========================================================
2、读取目录
fs.readdir(path[, options], callback)
fs.readdirSync(path[, options])

//异步
fs.readdir(__dirname,(err,files)=>{
  // files是目录和文件的数组
  files.forEach((item,index)=>{
      fs.stat(path.join(__dirname,item),(err,stat)=>{
          if(stat.isFile()){
              console.log(item,'文件');
          }else if(stat.isDirectory()){
              console.log(item,'目录');
          }
      });
  });
});

//同步
let files = fs.readdirSync(__dirname);
files.forEach((item,index)=>{
  fs.stat(path.join(__dirname,item),(err,stat)=>{
      if(stat.isFile()){
          console.log(item,'文件');
      }else if(stat.isDirectory()){
          console.log(item,'目录');
      }
  });
});
========================================================
3、删除目录
fs.rmdir(path, callback)
fs.rmdirSync(path)

fs.rmdir(path.join(__dirname,'abc'),(err)=>{
  console.log(err);
});

fs.rmdirSync(path.join(__dirname,'qqq'));
  ```
### 初始化文件目录结构

  ```js
  /*
      文件操作案例（初始化目录结构）
  */
  const path = require('path');
  const fs = require('fs');
  let root = 'C:\\Users\\www\\Desktop';
  let fileContent = `
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Document</title>
  </head>
  <body>
      <div>welcome to this</div>
  </body>
  </html>
  `;
  // 初始化数据
  let initData = {
      projectName : 'mydemo',
      data : [{
          name : 'img',
          type : 'dir'
      },{
          name : 'css',
          type : 'dir'
      },{
          name : 'js',
          type : 'dir'
      },{
          name : 'index.html',
          type : 'file'
      }]
  }
  // 创建项目跟路径
  fs.mkdir(path.join(root,initData.projectName),(err)=>{
      if(err) return;
      // 创建子目录和文件
      initData.data.forEach((item)=>{
          if(item.type == 'dir'){
              // 创建子目录
              fs.mkdirSync(path.join(root,initData.projectName,item.name));
          }else if(item.type == 'file'){
              // 创建文件并写入内容
              fs.writeFileSync(path.join(root,initData.projectName,item.name),fileContent);
          }
      });
  });
  ```

## 服务器操作http

### 静态资源

```js
// 1. 使用require方法加载http核心模块 
var http = require('http')
// 2. 使用http.createServer()方法创建一个web服务器
var server = http.createServer()
// 3. 注册 request请求事件
	// Request请求对象 获取客户端的一些请求信息，例如请求路径
	// Response响应对象 给客户端发送响应消息
server.on('request', function (req, res) {
    // 监听server 的request请求 可以简写成req, res
	var url = req.url
    // url统一资源定位符
	// 端口号之后 以/开头

	console.log('收到客户端请求了，请求路径是：' + url)
	// response对象有一个方法：write可以用来给客户端发送响应数据
	// write可以使用多次，但是最后一定要使用end来结束响应，否则客户端会一直等待
	if (url === '/') {
		res.end('index page')
	} else if (url === '/login'){
		res.end('login page')
	} else {
         // 服务端默认发送utf8编码 浏览器按照系统默认编码解析（gbk）
    	res.setHeader('Content-Type', 'text/plain; charset=utf-8')
		res.end('错误的url')
	}
	// 响应内容只能是二进制或者字符   
	// res.write('hello'); res.end() 可以简写成res.end('hello')
})
// 4. 绑定端口号，启动服务器
server.listen(3000, function () {
    // 第二个(可选)参数监听的ip地址
    console.log('服务器启动成功，可以通过http://127.0.0.1:3000/来进行访问')
    // 第三个参数回调函数可以不写
})
// cmd里 ctrl + c 关闭服务（close）

=======================简写=====================  
http.createServer((req,res)=>{
    res.end('hello')
}).listen(3000)
```

- 响应完整的页面 http+fs 

```js
var fs = require('fs')
var http = require('http')
var server = http.createServer()

server.on('request', function (req, res) {
	var url = req.url
	if (url === '/') {
		fs.readFile('./resourse/index.html', function(err, data) {
			if(err) {
				res.setHeader('Content-Type', 'text/plain; charset=utf-8')
				res.end('文件读取失败，请稍后再试')
			} else {
				res.setHeader('Content-Type', 'text/html; charset=utf-8')
				res.end(data)
			}		
		})
	} else if (url === '/gaben') {
		fs.readFile('./resourse/gaben.jpg', function (err, data) {
			if (err) {
				res.setHeader('Content-Type', 'text/plain; charset=utf-8')
             
				res.end('文件读取失败，请稍后再试')
			} else {
				res.setHeader('Content-Type', 'image/jpeg')
				res.end(data)
			}
		})
	} 
}).listen(3000, function () {
	console.log('Server is running...')
})
//Content-Type查询网站 https://tool.oschina.net/commons
```

- 像Apache一样获取文件

  ```js
  var http = require('http')
  var fs = require('fs')
  var server = http.createServer()
  var wwwDir = 'F:/codefc/node/resourse'
  /*
  浏览器收到HTML响应内容之后，在解析的过程中，如果发现：
  link script img iframe video audio 等带有src或者href(link)
  属性标签的时候，浏览器会自动对这些资源发起新的请求。
  
  约定把所有的静态资源都存放在public目录里
  */
  server.on('request', function(req, res){
  	var url = req.url
  	var filePath = '/index.html'
  	if (url !== '/'){
  		filePath = url
  	}
  	fs.readFile(wwwDir + filePath, function(err, data) {
  		if (err) {
  			return res.end('404 not found')
  		} else {
  			res.end(data);
  		}
  	})
  })
  
  server.listen(3000, function(){
  	console.log('running')
  })
  ```


### get 与 post请求参数处理

```js
- get方式（可以通过url获取）
const http = require('http')
const path = require('path')
const url = require('url')

http.createServer((req, res)=>{
	let obj = url.parse(req.url,true)
    // 写第二个参数结果是一个对象，不写结果是一个字符串
	res.end(obj.query.username + '==' + obj.query.password)
	//query:参数不带?
}).listen(3000,()=>{
	console.log('running...')
})

=========================================================
- post方式
const querystring = require('querystring')

let param = 'username=lisi&password=123&abc=xyz&abc=222'
querystring的两个方法：
//parse 把字符串转为对象（符合name=lisi&word=123这种格式）
//stringify 把对象转为字符串
console.log(obj.abc)
// 返回['xyz','222']

http.createServer((req,res)=>{
    let pdata = ''
    req.on('data',(chunk)=>{
        pdata += chunk
    })
    req.on('end',()=>{
        console.log(pdata)
        let obj = querystring.parse(pdata)
        res.end(obj.username + '==' + obj.password)
    })
}).listen(3000,()=>{
    console.log('running')
})
```

### 登陆验证

```html
	<form action="http://127.0.01:3000/login" method="post">
		用户名：<input type="text" name="username"><br>
		密  码：<input type="password" name="password">
		<input type="submit" value="提交">
	</form>
```

```js
const http = require('http')
const url = require('url')
const ss = require('./staticServer.js')
const querystring = require('querystring')

http.createServer((req,res)=>{
    // 静态
    if(req.url.startsWith('/www')){
        ss.staticServer(req,res,__dirname)
    }
    // 动态
    if(req.url.startsWith('/login')){
        if(req.method == 'GET'){
            let param = url.parse(req.url,true).query             
            if(param.username == 'admin' && param.password =='123'){
                res.end('get sucess')
            } else {
                res.end('get failure')
            }
        }
        if(req.method =='POST'){
            let pdata = ''
            req.on('data',(chunk)=>{
                pdata += chunk
            })
            req.on('end',()=>{
                let obj = querystring.parse(pdata)
                if (obj.username == 'admin' && obj.password == '123'){
                    res.end('post sucess')                    
                } else {
                    res.end('post fail')
                }
            })
        }
    }
}).listen(3000,()=>{
    console.log('running')
})
```

### 动态网站开发案例（成绩查询）

```js
const http = require('http');
const path = require('path');
const fs = require('fs');
const querystring = require('querystring');
const scoreData = require('./scores.json');

http.createServer((req,res)=>{
    // 路由分发（请求路径+请求方式）
    // 查询成绩的入口地址 /query
    if(req.url.startsWith('/query') && req.method == 'GET'){
        fs.readFile(path.join(__dirname,'view','index.tpl'),'utf8',(err,content)=>{
            if(err){
                res.writeHead(500,{
                    'Content-Type':'text/plain; charset=utf8'
                });
                res.end('服务器错误，请与管理员联系');
            }
            res.end(content);
        });
    }else if(req.url.startsWith('/score') && req.method == 'POST'){
        // 获取成绩的结果 /score
        let pdata = '';
        req.on('data',(chunk)=>{
            pdata += chunk;
        });
        req.on('end',()=>{
            let obj = querystring.parse(pdata);
            let result = scoreData[obj.code];
            fs.readFile(path.join(__dirname,'view','result.tpl'),'utf8',(err,content)=>{
                if(err){
                    res.writeHead(500,{
                        'Content-Type':'text/plain; charset=utf8'
                    });
                    res.end('服务器错误，请与管理员联系');
                }
                // 返回内容之前要进行数据渲染
                content = content.replace('$$chinese$$',result.chinese);
                content = content.replace('$$math$$',result.math);
                content = content.replace('$$english$$',result.english);
                content = content.replace('$$summary$$',result.summary);

                res.end(content);
            });
        });
    }
    

}).listen(3000,()=>{
    console.log('running....');
});
```

## 模板引擎

### art-template

安装：

```shell
npm i art-template
npm i express-art-template
```



#### art-template中的mustache语法

```shell
1. 直接输出
{{ value }}
template('被替换的id',要替换的对象)
2. 原文输出
{{ @value }}
# 原文输出语句不会对HTML进行转义
3. 条件判断
{{ if value }} {{/if}}
{{ fi v1 }}...{{ else if v2 }}...{{ /if }}
4. 循环
{{each target}} {{ $index }}{{ $value }} {{ /each }}
	4.1 输出数组某一个
	{{ value[index] }}
	4.2 输出某一个属性
	{{ value.属性 }}
```

#### art-template的运用

```js
在浏览器中的运用
<script src="node_modules/art-template/lib/template-web.js"></script>
// 引入art-template引擎
<script type="text/template" id="tpl">
	大家好我叫：{{ name }}
	我今年 {{ age }}
	我来自{{ address }}
	我喜欢： {{each hobbies}} {{  $value }} {{ /each }}
</script>
<script>
	var ret = template('tpl',{
		name: 'jack',
		age: 18,
		address: 'beijing',
		hobbies: [
		'movie', 'ball', 'run'
		]
	})
		console.log(ret)
</script>
                                         
========================================================
            
在node中的应用
var template = require('art-template')

var tplStr = `
全民制作人大家好，
我是练习时常{{ time }}的个人练习生{{ name }}
喜欢： {{each hobbies}} {{ $value }} {{ /each }}
music...
`
// template.render('模板字符串', '替换对象')
template.render(tplStr, {
    time: '两年半'
    name: '蔡徐坤',
    age: 18,
    address: 'beijing',
    hobbies: [
        '唱', '跳', 'rap','篮球'
    ]
})
                                         
```

- include用法

  ```html
  <body>
  	{{ include './header.html'}}
  	<!-- 引用共同头部 -->
      
  	<h1>hello {{ name }}</h1>
      
      {{ include './footer.html'}}
  	<!-- 引用共同底部 -->
  </body>
  ```

- extend模板继承

  ```html
  layout.html(专门的排版html)
  <body>
  	{{ include './header.html'}}
      
  	<!-- 留一个坑，后面填 -->
      {{ block 'content' }}<h1>默认内容</h1>{{ /block }}
      
      {{ include './footer.html'}}
  </body>
  
  index.html
  {{ extend './layout.html' }}
  {{ block 'content'}}
  <div>index页面填坑内容 不写即为默认内容</div>
  {{ /block }}
  ```

  

## Express

- `helloworld`

  ```js
  var express = require('express')
  
  var app = express()
  
  app.get('/', (req,res)=>{
  	res.send('hello world')
  })
  
  app.get('/about',(req,res)=>{
      res.send('我是一个express文件')
      // send的时候不用写utf8
  })
  app.listen(3000,()=>{
  	console.log('running...')
  })
  ```

- static静态资源服务

  ```js
  app.use('/public/',express.static('../public/'))
  // 开放public文件夹下静态资源
  // http://127.0.0.1:3000/public/image/gaben.jpg
  app.use(express.static('../public/'))
  // 省略第一个参数'public'
  // http://127.0.0.1:3000/image/gaben.jpg 
  app.use('/a/',express.static('../public/'))
  // http://127.0.0.1:3000/a/image/gaben.jpg 
  app.use('/public/',express.static(path.join(__dirname, 'public')))
  
  http://127.0.0.1:3000/public/image/gaben.jpg？foo=bar
  console.log(req.query)
  // 返回{foo: 'bar'}
  // express中可以直接使用req.query获取查询字符串参数（post不行）
  ```

- 在Express中使用art-template

  ```js
  app.engine('art', require('express-art-template'))
  // 第一个参数是html的后缀，可以改成html
  res.render('html模板名',{模板数据})
  // 第一个参数不能写路径 默认回去views目录查找该模板文件 第二个参数可选
  // 约定：把所有的视图文件都放到views 目录中 后缀.art（默认）
  
  res.redirect('/')
  // 重定向
  
  app.set('views', '路径')
  // 修改默认路径
  ```

- Express获取表单get

  ```js
  app.get('/post',(req,res)=>{
      // 这里post是url的一部分 不是post方式
  var comment = req.query
  // get方法是query
  res.redirect('/')
  })
  ```

- Express获取表单post

  ```shell
  安装第三方包'body-parser'
  npm install --save body-parser
  
  var bodyParser = require('body-parser')
  # 引包
  
  app.use(bodyParser.urlencoded({ extended: false }))
  app.use(bodyParser.json())
  
  app.post('/post',(req,res)=>{
  var comment = req.body
  // get方法是query
  res.redirect('/')
  })
  ```

  

### CRUD方法的实现

- 初始化模板处理

  ```js
  var express = require('express')
  var fs = require('fs')
  var app = express()
  
  app.use('/public/', express.static('./public/'))
  // 开放资源
  app.engine('html', require('express-art-template'))
  //render必备
  app.get('/', (req,res)=>{
  	fs.readFile('./db.json','utf8',(err,data)=>{
  		if(err) return res.status(500).send('server error')	// 状态码500
  	res.render('index.html',{
  		fruits:[
  		'apple',
  		'peach',
  		'banana'],
  		students: JSON.parse(data).student
          // 可以直接传，也可以引入文件对象
  		// 从文件中读取到的数据是字符串，手动将字符串转化为对象
  	})
  	})	
  }).listen(3000,()=>{
  	console.log('running...')
  })
  
  =================================================================================
  json文件
  {
  	"students":[
  		{"id": 1,"name":"漩涡鸣人","gender": 1,"age": 18,"hobbies":"吃饭、睡觉、打豆豆"},
  		{"id": 2,"name":"宇智波佐助","gender": 1,"age": 15,"hobbies":"吃饭、睡觉、打豆豆"},
  		{"id": 3,"name":"春野樱","gender": 0,"age": 22,"hobbies":"吃饭、睡觉、打豆豆"},
  		{"id": 4,"name":"奈良鹿丸","gender": 1,"age": 33,"hobbies":"吃饭、睡觉、打豆豆"},
  		{"id": 5,"name":"山中井野","gender": 0,"age": 12,"hobbies":"吃饭、睡觉、打豆豆"},
  		{"id": 6,"name":"秋道丁次","gender": 1,"age": 24,"hobbies":"吃饭、睡觉、打豆豆"},
  		{"id": 7,"name":"zhangsan","gender": 0,"age": 22,"hobbies":"吃饭、睡觉、打豆豆"}
  	]
  }
  
  ====================================================================================
  index.html文件
   {{ each students }}
      <tr>
          <td>{{ $value.id }}</td>
          <td>{{ $value.name }}</td>
          <td>{{ $value.gender }}</td>
          <td>{{ $value.age }}</td>
          <td>{{ $value.hobbies }}</td>
          <td>
              <a href="/students/edit?id={{ $value.id }}">编辑</a>
          <a href="/students/delete?id={{ $value.id }}">删除</a>
          </td>
      </tr>
  {{ /each }}
  ```

- 路由设计

  请求方法|请求路径|get参数|post参数|备注
:-:|---|:-:|:-:|:-:
GET|/students| | |渲染首页
GET|/students/new| | |渲染添加学生
POST | /students/new | |name、age、gender、hobbies|处理添加学生请求
GET|/students/edit|id | |渲染编辑页面
POST|/students/edit||id、age、name、hobbies、gender|处理编辑请求
GET|/students/delete|id||处理删除请求

入口文件app.js
```js
var express = require('express')
var router = require('./router')
var app = express()

app.use('/public/', express.static('./public/'))
app.engine('html', require('express-art-template'))
// router(app)
// 通过这种方式调用router(方法1)

app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
// 配置模板引擎和body-parse一定要在挂载路由之前
app.use(router)
 // 通过这种方式调用router（方法2）   
app.listen(3000,()=>{
	console.log('running...')
})
======================================================================================
router.js
// 路由文件写到单独的一个文件
var fs = require('fs')
/*~~1.自己创建函数（不用）~~
module.exports = function (app) {
    app.get('/students', (req,res)=>{
		...
	})
}*/
2.使用Express的Router()方法
 var express = require('express')   
 // 创建一个路由容器
 var router = express.Router()
 //把路由都挂载在容器中
 router.get('/students', (req,res)=>{
		...
 })
 module.exports = router
 // 单独导出router
```

路由文件router.js
```js
var fs = require('fs')
var express = require('express')

var router = express.Router()
var Student = require('./student')

router.get('/students', (req,res)=>{
	Student.find((err,students)=>{
		if(err) return res.status(500).send('server error')
		res.render('index.html',{
		fruits:['apple','peach','banana'],
		students: students
		})
	})
})
router.get('/students/new',(req,res)=>{
	res.render('new.html')
})
router.post('/students/new',(req,res)=>{
	// 获取表单数据
	var student = req.body
	Student.add(student,(err)=>{
		if(err) {
			return res.status(500).send('server error')
		}
		res.redirect('/students')
	})
})
router.get('/students/edit',(req,res)=>{
	/*1.在客户端的列表页中处理链接问题（需要有id参数）
	2.获取要编辑的学生id
	3.渲染编辑页面
	*/
	Student.findById(parseInt(req.query.id),(err,student)=>{
		if(err) return res.status(500).send('server error')
		res.render('edit.html',{
			student: student
		})
	})
})
router.post('/students/edit',(req,res)=>{
	Student.updateById(req.body, (err)=>{
		if (err) return res.status(500).send('server error')
		res.redirect('/students')
	})
})
router.get('/students/delete',(req,res)=>{
	Student.deleteById(req.query.id,(err)=>{
		if (err) return res.status(500).send('server error')
		res.redirect('/students')
	})
})
 module.exports = router
```

crud方法文件students.js

```js
// 数据操作文件模块 只处理数据
var fs = require('fs')

var dbPath = './db.json'
// 获取学生列表
exports.find = (callback)=>{
	fs.readFile(dbPath,'utf8',(err,data)=>{
		if(err) return callback(err)
		callback(null,JSON.parse(data).students)
	})
}
// 查询某个学生
exports.findById = (id,callback)=>{
	fs.readFile(dbPath,'utf8',(err,data)=>{
		if(err) return callback(err)
		var students = JSON.parse(data).students
		var ret = students.find((item)=>{
			return item.id === id
		})
		callback(null,ret)
	})
}
// 添加学生
exports.add = (student,callback)=>{
	fs.readFile(dbPath,'utf8',(err,data)=>{
		if(err) return callback(err)
		var students = JSON.parse(data).students

		student.id = students[students.length - 1].id + 1
		students.push(student)	
		var fileData = JSON.stringify({
			students: students
		})
		fs.writeFile(dbPath,fileData,(err)=>{
			if(err) return callback(err)
			callback(null)
		})
	})
}
// 更新学生
exports.updateById = (student,callback)=>{
	fs.readFile(dbPath,'utf8',(err,data)=>{
		if(err) return callback(err)
		var students = JSON.parse(data).students

		student.id = parseInt(student.id)

		var stu = students.find((item)=>{
			return item.id === student.id
		})
		// 遍历拷贝对象
		for (var key in student) {
			stu[key] = student[key]
		}
		var fileData = JSON.stringify({
			students: students
		})
		fs.writeFile(dbPath,fileData,(err)=>{
			if(err) return callback(err)
			callback(null)
		})
	})
}
// // 删除学生
exports.deleteById = (id,callback)=>{
	fs.readFile(dbPath,'utf8',(err,data)=>{
		if(err) return callback(err)
		var students = JSON.parse(data).students
		var deleteId = students.findIndex((item)=>{
			return item.id === parseInt(id)
		})
		students.splice(deleteId,1)
		var fileData = JSON.stringify({
			students: students
		})
		fs.writeFile(dbPath,fileData,(err)=>{
			if(err) return callback(err)
			callback(null)
		})
})
}
```

## MongoDB数据库

- 启动/关闭

  ```shell
  # mongod  默认使用执行命令盘符根目录下/data/db 作为数据目录
  # 第一次执行前手动创建/data/db
  mongod/ctrl + c
  # 修改默认存储路径 
  mogod --dbpath=路径
  ```

- 连接数据库/退出

  ```shell
  # 默认连接本机的MongoDB服务 mongod必须启动
  mongo/exit
  ```

- 基本命令

  - show dbs
    - 查看显示所有数据库
  - db 
    - 查看目前操作的数据库
  - use 数据库名称
    - 切换到指定的数据库（没有会新建）
  - 插入数据
    - db.students.insertOne(["name": "Jack"])

### 在Node中操作MongoDB

- 基本概念

  - - MongoDB非常灵活，不需要像MySQL一样先创建数据库、表、设计表结构。
      - 在这里只需要：当你需要插入数据的时候，只需要指定往哪个数据库的哪个集合操作就可以了
    - 可以有多个数据库
    - 一个数据库可以有多个集合（表）
    - 一个集合可以有多个文档（表记录）

    ```shell
    {
    	qq:{ # 数据库
    		users:[ # 集合
    			{# 文档},
    			{name: '张三',age: 15}，
    			{name: '李四',age: 18}，
    			...
    		],
    		products:[
    		
    		],
    		...
    	},
    	taobao:{
    	
    	},
    	baidu:{
    	
    	},
    	...
    }
    ```

- mongodb(官方)

- mongoose（第三方）

  - 安装

    ```
    npm install mongoose
    ```

  - 使用
  
    ```js
    var mongoose = require('mongoose')
    
    var Schema = mongoose.Schema
    // 1. 连接数据库
    // 指定连接的数据库不需要存在，当插入第一条数据之后就会自动被创建出来
    mongoose.connect('mongodb://localhost/itcast')
    
    // 2. 设计文档结构（表结构）
    // 字段名称就是表结构中的属性名称
    // 约束的目的是为了保证数据的完整性，不要有脏数据
    var userSchema = new Schema({
      username: {
        type: String,
        required: true // 必须有
      },
      password: {
        type: String,
        required: true
      },
      email: {
        type: String
      }
    })
    
    // 3. 将文档结构发布为模型
    //    mongoose.model 方法就是用来将一个架构发布为 model
    //    第一个参数：传入一个大写名词单数字符串用来表示你的数据库名称
    //                 mongoose 会自动将大写名词的字符串生成 小写复数 的集合名称
    //                 例如这里的 User 最终会变为 users 集合名称
    //    第二个参数：架构 Schema
    //   
    //    返回值：模型构造函数
    var User = mongoose.model('User', userSchema)
    
    
    // 4. 当我们有了模型构造函数之后，就可以使用这个构造函数对 users 集合中的数据为所欲为了（增删改查）
    // **********************
    // #region /新增数据
    // **********************
    var admin = new User({
      username: 'zs',
      password: '123456',
    email: 'admin@admin.com'
    })
    
    admin.save(function (err, ret) {
      if (err) {
        console.log('保存失败')
      } else {
        console.log('保存成功')
        console.log(ret)
      }
    })
    
    // **********************
    // #region /查询数据
    // **********************
    User.find(function (err, ret) {
      if (err) {
        console.log('查询失败')
      } else {
        console.log(ret)
      }
    })
    
    User.find({
      username: 'zs'
    }, function (err, ret) {
      if (err) {
        console.log('查询失败')
      } else {
        console.log(ret)
      }
    })
    
    User.findOne({
      username: 'zs'
    }, function (err, ret) {
      if (err) {
        console.log('查询失败')
      } else {
        console.log(ret)
      }
    })
    // **********************
    // #region /删除数据 //已更新为deleteOne
    // **********************
    User.remove({
      username: 'zs'
    }, function (err, ret) {
      if (err) {
        console.log('删除失败')
      } else {
        console.log('删除成功')
        console.log(ret)
      }
    })
    // **********************
    // #region /更新数据
    // **********************
    User.findByIdAndUpdate('5a001b23d219eb00c8581184', {
      password: '123'
    }, function (err, ret) {
      if (err) {
        console.log('更新失败')
      } else {
        console.log('更新成功')
      }
    })
    ```
  
- md5数据库加密
  
    ```shell
    安装 
    npm iblueimp-md5
    配置
    var md5 = require('../node_modules/blueimp-md5/js/md5.js')
    使用
    body.password = md5(md5(body.password))
    # 对密码进行md5重复加密
    ```
    
    

## 异步编程

### callback hell



### node中的Promise

>  为了解决回调地狱，ES6新增Promise

异步任务三种状态

- Pending（正在执行 待解决）
- Resolved（已解决）
- Rejected（被拒绝）

```js
var fs = require('fs')
// 创建promise容器
var p1 = new Promise((resolve, reject) => {
    // Promise容器一旦创建，代码立即执行
    // 承诺本身不是异步，里面任务是异步的
    fs.readFile('./data/a.txt', 'utf8', (err, data) => {
        if (err) {
            //承诺容器中的任务成功了
            // 把容器的pending状态改为rejected状态
            // 调用reject相当于调用then的第二个参数
            reject(err)
        } else {
            //承诺容器中的任务成功了
            // 把容器的pending状态改为resolved状态
            // 调用resolve相当于调用then的第一个参数
            resolve(data)
        }
    })
})
var p2 = new Promise((resolve, reject) => {
    fs.readFile('./data/b.txt', 'utf8', (err, data) => {
        if (err) { reject(err) } else { resolve(data) }
    })
})
var p3 = new Promise((resolve, reject) => {
    fs.readFile('./data/c.txt', 'utf8', (err, data) => {
        if (err) { reject(err) } else { resolve(data) }
    })
})
// then方法接受的function就是容器中的resolve函数
// 异步操作 链式编程
p1.then((data)=>{
    console.log(data)
    return p2
    // **这里return的p2可以被后面一个.then接收到**
    // 当return一个Promise对象的时候 后面一个.then的第一个参数就是p2的resolved
}, (err) => {
    console.log('fail', err)
}).then((data)=>{
   	// 该方法作为p2的resolve
    console.log(data)
    return p3
}).then((data)=>{
    // 该方法作为p3的resolve
    console.log(data)
})

==  封装promise的readFile方法=================================
  function pReadFile(filePath) {
	return new Promise((resolve, reject) => {
        fs.readFile(filePath, 'utf8', (err, data) => {
            if (err) {reject(err)} else {resolve(data)}
        })
    })
}
```

## 项目案例

### 初始化目录结构

```shell
|-app.js 入口
|-controllers
|-mode1s				模型，用来存放数据
|-node_modules			第三方包
|-package.json			包描述文件
|-package-1ock.json		第三方包版本锁定文件（npm5以后才有）
|-public				公共的静态资源
|-README.md				项目说明文档
|-routes
|-views					存储视图目录
```

### 安装环境

```shell
npm init -y

npm i express 
		jquery 
		art-template 
		express-art-template 
		bootstrap@3.3.7 
		body-parser

#例子中的bootstrap版本3.3.7
```



### 初始化入口文件

```js
var express = require('express')
var path = require('path')

var app = express()

app.use('/public/', express.static(path.join(__dirname, './public/')))
app.use('/node_modules/', express.static(path.join(__dirname, './node_modules/')))

app.engine('html', require('express-art-template'))
app.set('views', path.join(__dirname, './views/'))

app.get('/', (req, res) => {
    res.render('index.html', {
        name: '张三'
    })
})

app.listen(3000, () => {
    console.log('running...')
})
```



### 初始化布局模板

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<link rel="stylesheet" href="../node_modules/bootstrap/dist/css/bootstrap.css">
	{{ block 'head' }}{{ /block }}
</head>
<body>
	{{ include './header.html'}}
	<!-- 引用共同头部 -->
	{{ block 'content' }}
		<div>默认内容</div>	
	{{ /block }}
	<h1>hello {{ name }}</h1>
	<script src="../node_modules/jquery/dist/jquery.js"></script>
	<script src="../node_modules/bootstrap/dist/js/bootstrap.js"></script>
	{{ block 'script'}}{{ /block }}
</body>
</html>
```

### 路由设计

| 路径 | 方法 | get参数 | post参数 | 备注 | 是否需要登录|
| ---- | ---- | ------- | -------- |---|---|
| / | GET |  |  |渲染首页||
| /register | GET |  |                           |渲染注册页面||
| /register | POST |  | email，password，nickname |处理注册请求||
| /login | GET |  |  |渲染登录页面||
| /login | POST |  | email，password |处理登录请求||
| /logout | GET |  |  |处理注销请求||

```js
var express = require('express')
var router = express.Router()

router.get('/', (req, res) => {
    res.render('index.html')
})

router.get('/login', (req, res) => {
    res.render('login.html')
})

router.post('/login', (req, res) => {

})

router.get('/register', (req, res) => {
    res.render('register.html')
})

router.post('/register', (req, res) => {
	var comment = req.body
	res.redirect('/')
})

module.exports = router
```

### 设计数据模型

```js
var mongoose = require('mongoose')
var Schema = mongoose.Schema

var userSchema = new Schema({
	email:{
		type: String,
		require: true
	},
	nickname:{
		type: String,
		require: true
	},
	password:{
		type: String,
		require: true
	},
	created_time:{
		type: Date,
		default: Date.now
        //这里不能写()
	},
    last_modifined_time: {
        type: Date,
		default: Date.now
    },
    avatar: {
        type:String,
        default: '/public/img/avatar-'
    },
    status: {
    	type: Number,
    	enum: [0, 1, 2],
    	// 0 无权限设置 1 不可以评论 2 不可以登录
    	default: 0
    }
})

moudles.exports = mongoose.model('User',userSchema)
```

