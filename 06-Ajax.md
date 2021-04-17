# php

## 软件架构

- Browser&Server架构（浏览器-服务器）

> B&S架构是一种特殊的C&S架构(client)
>
> - 请求报文request
>   - 请求行
>     - 请求方式
>     - 请求地址
>     - 协议
>   - 请求头（键值对）
>     - 浏览器的信息
>     - 接受的语言格式
>     - 想要发送给服务器的信息
>     - ...
>   - 请求主体
>     - 发送给服务器的数据内容
> - 响应报文response
>   - 状态行
>     - 请求是否成功
>     - 状态码 200- 成功 404-页面不存在...
>   - 响应头
>     - 服务器的一些信息
>     - 服务器想要告诉浏览器的一些信息
>   - 响应主体
>     - 正常用户能够看到的内容
>
> http协议：使用请求报文响应报文这种方式进行数据交互

![image-20200711191500107](C:\Users\何镇武\AppData\Roaming\Typora\typora-user-images\image-20200711191500107.png)

- C&S架构（客户端-服务器）

## php语法

#### 基础语法

```php
<style>
	span {
		font-size: 14px;
		color: red;
	}
</style>
<?php
// 单行注释
    
/*
多行注释
**php语言的分号不能省略**
*/
 echo 'hello world';   
?>
<p>php标签之外的代码不会执行，原封不动的返回给浏览器</p> 

    
-----------外部引入------------
<?php
include './data.php';  
?>
```

#### 定义，调用变量

```php
// 设置编码格式
header('content-type:text/html;charset=utf-8');

// 定义变量
$name = '张三';
$num = 123;
$isRight = true;
// 输出
echo 'hello world'.'你好世界';//PHP用.拼接字符串
echo '<br>'// 换行
echo $name;
```

#### 流程控制

```php
- if
if ($num === 1) {
    echo 'true';
} else {
    echo 'false';
}

- switch
switch ($num) {
	case '1':
	case '3':
	echo 'even num';
	break;
	case '2':
	case '4':
	echo 'odd num';
	break;
    default:
    echo 'NAN'
} 

-for
for ($i=0; $i < 2; $i++) { 
	echo $i;
    echo'<br>'
}

- while
$num = 1
while ($num <= 10) {
	echo 'str'.$i;
}    
```

#### 数据类型--数组

```PHP
- 数组
$arr1 = array('zs','ls','ww');
- 索引数组
$arr2 = array('a' => '张三','b' => '李四','c' => '王五');
- 二维数组
$arr3 = array(
    array('name' => '刘德华','gender' => '男','film' => '无间道'),
    array('name' => '陈宝国','gender' => '男','film' => '大宅门'),
    array('name' => '高圆圆','gender' => '女','film' => '倚天屠龙记')
);
- 数组的方法

// 打印某一项
echo $arr1[0];
echo $arr2['a'];
echo $arr3[0]['name'];
// 打印整个数组
print_r($arr2);
// 获取数组长度

count($arr3) // =>3
count($arr3,1) // =>9
    //第二个参数可选 1-所有元素 0-只看一层
//遍历数组
for ($i=0; $i < count($arr1); $i++) { 
	echo $arr1[$i].'<br>';
}
foreach ($arr2 as $key => $value) {
	echo $key.'---'.$value.'<br>';
}
for ($i=0; $i < count($arr3); $i++) { 
		echo '<span>'.$arr3[$i]['name'].'</span>,性别：'.$arr3[$i]['gender'].',主要作品：《'.$arr3[$i]['film'].'》<br>';	
}
```

## form表单提交（GET）

> 特点
>
> ​	数据存在url里面，数据安全性不高
>
> ​	数据的长度问题 
>
> ​	测试方便

```html
<form action="./getData.php" method="get">
    <!-- action：提交的目标地址-->
    <!--method: 提交的方式-->
		<input type="text" name="lolHero" id="" placeholder="请输入文本">
     		<!--name属性用来标记需要提交的值，必须有-->
		<input type="submit" value="提交">
	</form>
```

```php
// 超全局变量
$_GET 是一个数组
<?php 	
	header('content-type:text/html;charset=utf-8');
	$heroName = $_GET['lolHero'];
	include './data_lol_detail.php';	
	$hero = $heroArr[$heroName];
	echo '<h2>'.$hero["champion_title"].'---<span>'.$hero["champion_name"].'</span> </h2>';
	echo '<img src="'.$hero["champion_icon"].'"><br>';
	echo '英雄介绍：'.$hero['champion_info'].'<br>';
	echo '英雄定位：'.$hero['champion_tags'];
 ?>
     
===================================
可以用多个php标签解决长字符串的拼接
  echo '<h2>'.$hero["champion_title"].'---<span>'.$hero["champion_name"].'</span> </h2>';
	引入多个标签
  echo '<h2>' 
      <?php echo $hero["champion_title"] ?>
      '---<span>' 
      <?php echo $hero["champion_name"] ?>
      '</span> </h2>';

```

## form表单提交（POST）

> 特点：
>
> ​	必须写method="post"
>
> ​	提交的数据不在url中，安全性相对较好
>
> ​	没有长度限制
>
> ​	上传文件必须使用post

```php
// 超全局变量
$_POST
$_POST是一个数组，操作与get基本一致
```

## form文件上传（POST）

```html	
 **HTML上传文件必须添加enctype="multipart/form-data"**
<form action="./postData.php" method="post"  enctype="multipart/form-data">
    // 打印$_FILES 类型：数组
```

```php
// 上传文件
$_FILES
打印$_FILES
    Array ( [icon] => Array ( 
        [name] => gaben.jpg 
        [type] => image/jpeg 
        [tmp_name] => C:\...\AppData\Local\Temp\php68D.tmp 
        /* tmp 生成一个临时文件
         php代码执行完毕之后，临时文件就被销毁了
         sleep(5) 
         文件存在5秒，之后被销毁
         move_uploaded_file(file,newloc)
         参数1，移动的文件 参数2，新地址
         */
        [error] => 0 
        [size] => 81253 
    ) )
将上传的文件保存到某个位置：
move_uploaded_file($_FILES['icon']['tmp_name'],./files/$_FILES['icon']['name'])
```

# XML


### 格式语法
```xml
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tom</to>
  <from>Jenny</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
//1. 标签名字可以自定义
//2. 只能使用双标签
//3. 只能有一个根标签
```

###  php获取XML数据
```php
<?php
// 告诉浏览器返回编码格式
header('content-type:text/xml;charset=utf-8');
// 读取XML 返回字符串
$xmlStr = file_get_contents('data/data.xml');
echo $xmlStr
?>

```

### 获取返回数据
```js
- HTML获取返回数据
var ret =  xhr.responseXML
- 分割方法类似DOM操作
var from = ret.querySelector('from').innerHTML
// 'Jenny'
document.body.innerHTML = from
```

### 缺点
```
1. 传输的数据量大(标签成对增加,不是最主要缺点)
2. 解析稍微有点复杂
```



# JSON

> JSON是一种数据格式
>
> JSON跟编程语言没有关系
>
> JSON的载体是字符串 
> 
> 转换完之后是数组或者对象
>
> 基本上所有的编程语言都支持JSON
>
> 语法简洁

```json
// 如果是在.json文件里面写 可以不加最外面的''
// 属性名必须用""包裹(不可以用'')
// 属性值必须用""包裹，数值可以不用
// 键值对之间用,分割
- JSON的写法--对象表示
var JSONObj = '{
    "name":"jack",
    "skill": {
        "swim":"good",
        "bike":"just so so"
    }'
    "age":18
}
// 转化为对象
var ret = JSON.parse(JSONObj)
ret.name

- JSON的写法--数组表示
var JSONArr = '["jack","rose","shell","galDos"]'
// 转化为数组
var ret = JSON.parse(JSONArr)
ret[2]

- JSON的写法--对象数组表示
var JSONObjArr = '{"name":"jack","friends":["rose","freeman","lora"]}'
//转化为对象
var ret = JSON.parse(JSONObjArr)
ret.friends[1]
```

### 获取JSON格式的数据

```php
// 不设置也可以
header('content-type:application/json;charset=utf-8');
$jsonStr = file_get_contents('data/data.json');
echo $jsonStr
```

# Ajax

> 一般步骤
>
> ​	var xhr =  new XMLHttpRequest 	创建异步对象
>
> ​	xhr.open 												请求行
>
> ​	xhr.setRequestHeader 						请求头
>
> ​	xhr.responseText	   						响应事件+回调函数(响应报文返回到回调函数里）
>
> ​	xhr.send 												发送（请求主体 ）

## Ajax（get请求）
```html
	<h2>不刷新页面发送报文</h2>
	<input type="button" value="发送报文" id="input">
  <span id="span"></span>
```
```js
let btn = document.getElementById('input')

btn.onclick = function () {
	var xhr = new XMLHttpRequest();
	xhr.open('get','./xxx.php?name=zhangsan',true);
	xhr.onload = function () {
		console.log(xhr.responseText);
		document.getElementById('span').innerHTML = xhr.responseText;
	}
	xhr.send(null);
}

btn.onclick = function () {
	// 1.创建对象
	var xhr = new XMLHttpRequest();

	// 2.请求行 请求方法及地址（get请求数据写在这里）
	xhr.open('get','./xxx.php?name=zhangsan',true);
    /*第三个参数 是否使用异步操作
    false 同步操作
    默认是异步操作*/

	// 3.请求头（get请求中可以省略）
	// xhr.setRequestHeader('key1','value1');
    
	// 3.1设置回调函数
	xhr.onload = function () {
		// 获取数据
		console.log(xhr.responseText);
		document.getElementById('span').innerHTML = xhr.responseText;
	}
	// 4请求主题发送（get请求为空，post请求数据写在这里）
	xhr.send(null);
}

=============================================================
响应事件
 xhr.onreadystatechange 状态变化(兼容性好一些)

响应阶段
xhr.readyState
	 0 (未打开) 1 2 3 4-响应结束
状态码
 xhr.status 
    200 响应成功
    404 响应失败
xhr.onreadystatechange = function () {
    if(xhr.readState == 4&& xhr.status == 200) {
        console.log(xhr.responseText)
    }
}
```

### 例 判断用户名是否存在

```
思路：
1. 浏览器端
	input type=text
	失去焦点事件
	不刷新页面发送请求--ajax
	xxx.php?inputDom.value
	.onload
	.send()
2. 服务器端
	接受提交的数据 $_GET
	假数据模拟
	判断是否存在
	返回内容
3. 浏览器渲染（回调函数）
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>检查用户名</title>
</head>
<body>
	<h2></h2>
	<input type="text" placeholder="input name here">
	<script>
		document.querySelector('input').onblur = function() {
			// 创建对象
			var xhr = new XMLHttpRequest();
			// 设置请求行
			xhr.open('get','./checkName.php?name='+this.value);
			//回调函数
			xhr.onload = function(){
				console.log(xhr.responseText);
				document.querySelector('h2').innerHTML = xhr.responseText;
			}
			xhr.send(null);
		}
	</script>
</body>
</html>
```

```php
<?php 
// 接受提交的数据 $_GET
	$name = $_GET['name'];
// 	假数据模拟
	$nameArr = array('jack','rose');
// 	判断是否存在
	$ret = in_array($name, $nameArr);
	if($ret == true) {
		echo 'already been register';
	} else {
		echo 'ok';
	}
// 	返回内容
 ?>
```

## Ajax（post请求）

```js
	<h2>不刷新页面发送报文</h2>
	<input type="button" value="发送报文" id="input">
   	<div id="span"></span>
let btn = document.getElementById('input')
btn.onclick = function () {
	// 1.创建对象
	var xhr = new XMLHttpRequest();

	// 2.请求行 请求方法及地址
	xhr.open('post','./postData.php');

	// 3.请求头(post不发送数据也可以省略)
	// ajax	post请求必须写下段代码
	xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    
	// 3.1设置回调函数
	xhr.onload = function () {
		// 获取数据
		console.log(xhr.responseText);
		document.getElementById('span').innerHTML = xhr.responseText;
	}
	// 4请求主题发送（post请求数据写在这里）
	xhr.send('name=zhangsan&gender=male');
}
```

### 例 聊天机器人

```
思路：
1. 浏览器端
	输入框 
	发送按钮 
	创建自己的聊天框
	点击发送按钮 -ajax
2. 服务器端
	返回信息
3. 数据回来之后
	渲染
	创建一个聊天框
	服务器返回的内容
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		li {
			width: 200px;
		}
	</style>
</head>
<body>
	<ul></ul>
	<input type="text" placeholder="input msg here">
	<input type="button" value="发送">
<script>
	document.querySelector('input[type=button]').onclick = function () {
		var li = document.createElement('li');
		li.innerHTML = document.querySelector('input[type=text]').value;
		li.style.backgroundColor = '#666';
		document.querySelector('ul').appendChild(li);

		var xhr = new XMLHttpRequest();
		xhr.open('get','./chat.php');
		xhr.onload = function () {
			console.log(xhr.responseText);
			var herLi = document.createElement('li');
			herLi.innerHTML = xhr.responseText;
			herLi.style.backgroundColor = '#fff';
			document.querySelector('ul').appendChild(herLi);
		}
		xhr.send(null)

	}
</script>
</body>
</html>
```

```php
<?php 
	
	// 休息一会
	sleep(1);

	// 获取 用户发送的 消息  (可选)
	// 后端 对于 用户发过来的 时候 是否 使用 (可选)
	
	// 根据 发送 过来的 消息 返回 不同的内容
	$messageList =array(
		'你好啊^_^',
		'我没有吃饭哦',
		'坚持就是胜利',
		'唱、跳、rap、篮球'
	 );
	// 从 数组中 每次 随机 取出
	// array_rand 返回的 是一个 随机的 下标
	$randomKey = array_rand($messageList,1);
	// 使用 随机 下标 返回 随机的 值
	echo $messageList[$randomKey];
 ?>
```

### 例 csgo武器查询

```html
<h2 class="">csgo枪支介绍</h2>
<h1>ak47</h1>
价格：<span id="span1"></span><br>
备弹：<span id="span2"></span><br>
<img src="#" alt="...">
<p>info here</p>
```

```js
$(function () {
      // 绑定点击事件
      $('input').click(function(){
        $('h1').html($(this).val());
        //1.创建异步对象
        var xhr = new XMLHttpRequest();
        //2.设置请求行
        xhr.open('get','getWeapon.php?name='+$(this).val());
        //3.设置请求头(get请求可以省略)
        //4.注册状态改变事件
        xhr.onreadystatechange = function(){
          //4.1判断状态&请求是否成功并使用数据
          if(xhr.readyState==4&&xhr.status==200){
            console.log(xhr.responseText);
            // 修改对应的 dom元素的 内容
            // $('img').attr('src',xhr.responseText);
            // 切割数据
            var result =  xhr.responseText.split('||||');
            console.log(result); 
            // 分别设置给 img  p
            $('#span1').html(result[0]);
            $('#span2').html(result[1]);
            $('img').attr('src',result[2]);
            $('p').html(result[3]);
          }
        }
        //5.发送请求
        xhr.send(null);
      })
  })
```

```php
<?php
  include './weapon.php';

/* $weaponArr = array(
	'ak47' =>  array('cost' => '$2700', 'ammo' => '30/90','bonus' => '$300','info' => '强大又可靠，AK-47是世上最流行的突击步枪之一，近距离内控制好的短点射击极为致命。','link' => 'https://www.csgo.com.cn/csgo2018/images/pic_AK-47_4d7499c0a9dff01c8962fd55447a29d8.png'),*/
  $weaponName = $_GET['name'];
  $someWeapon = $weaponArr[$weaponName];
  echo $someWeapon['cost'];
  echo '||||';
  echo $someWeapon['ammo'];
  echo '||||';
  echo $someWeapon['link'];
  echo '||||';
  echo $someWeapon['info'];
?>
```

## 封装ajax

```js
-封装
/*

option:{
	url,
	type,
	data,(key1=value&key2=va;ue)
	success
}
 */
function ajax(option) {
    var xhr = new XMLHttpRequest()
    if(option.type == 'get' && option.data) {
        option.url += '?'
        option.url += option.data
        option.data = null
    }
    xhr.open(option.type,option.url)
    if (option.type == 'post'&&option.data){
         xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded")
    }
    xhr.onreadystatechange = function() {
        if(xhr.readyState == 4&& xhr.status == 200){
           	 // getResponseHeader方法可以获取响应头
              var type = xhr.getResponseHeader('Content-Type');
              // 是否为json
              if (type.indexOf('json') != -1) {
                option.success(JSON.parse(xhr.responseText));
              }
              // 是否为xml
              else if (type.indexOf('xml') != -1) {
                option.success(xhr.responseXML);
              }
              // 普通字符串
              else {
                option.success(xhr.responseText);
              }
        }
    }
    xhr.send(option.data)
}
=======================================================================
- 调用
	document.getElementById('btn').onclick = function () {
	ajax({
            url:'./test.php',
            type:'get',
            data:'name=jack&friend=rose',
            success:function(data){
                document.body.innerHTML = data
            }
        })
	}
```
## jQuery中的ajax封装调用
```js
$ajax({
    url:'./test.php',
    type:'get',
    // 默认get
    data:'name=jack&friend=rose',
    // jQuery的data支持多格式
    // data:{name='jack',friend='rose''}在jQuery里也是可以使用的
    success:function(data){
        document.body.innerHTML = data
    },
    ...
    error:function(XMLHttpRequest,textStatus,errorThrown){
        // 参数XMLHttpRequest 异步对象
        // 参数textStatus 错误信息
        // 参数errorThrown 错误位置
    },
    complete:function(){
        // 完成时触发 ，不管有没有err
    }
})
```

## 例 登录验证

```js
- 验证用户名 判断数据库是否有该数据 若有 tips标签显示已被注册
$('.name').blur(function () {
    // 为了在代码中 使用this 不出现问题
    var $this = $(this);
    // 发送ajax 请求
    $.ajax({
        url: '_api/checkName.php',
        data: {
            name: $this.val()
        },
        success: function (data) {
            console.log(data);
            $('.tips').find('p').html(data).fadeIn(1000).delay(1000).fadeOut(1000);
        }
    })
})

- 验证手机号
$('.mobile').blur(function () {
    $.ajax({
        url: '_api/checkMobile.php',
        type: 'post',
        data: {
            mobile: $('.mobile').val()
        },
        success: function (data) {
            console.log(data);
            $('.tips p').html(data).fadeIn(1000).delay(1000).fadeOut(1000);
        }
    })
})


- 点击获取验证码
$.ajax({
    url: "_api/code.php",
    // php 使用 echo rand(1000,9999);来生成随机数
    data: {
        mobile: $('.mobile').val()
    },
    success: function (data) {
        console.log(data);
        $('.code').val(data);
    }
})

- 按钮倒计时
if ($(this).hasClass('disabled')) {
    alert('哥们,别点了');
    return;
}
// 添加类名
$(this).addClass('disabled');
// 定义倒计时的时间
var totalTime = 4;
// 直接修改内容即可
$(this).val(totalTime + 'S后重新发送');
// 定时器 实现倒计时功能
var interId = setInterval(function () {
    totalTime--;
    if (totalTime == 0) {
        // 清除定时器
        clearInterval(interId);
        // 移除 类名
        $('.verify').removeClass('disabled');
        // 更改内容
        $('.verify').val('获取验证码');
        // 阻断后续代码执行
        return;
    }
    $('.verify').val('还有' + totalTime + 'S');
}, 1000)
```

## 模板引擎 art-template

```html
<script src="./template-web.js"></script>
// 引入art-template引擎
<script type="text/template" id="tpl">
	大家好我叫：{{ name }}
	我今年 {{ age }}
	我来自{{ address }}
	我喜欢：
	<ul>
		 {{each hobbies}} <li>{{ $value }}</li> {{ /each }}
	</ul>
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
```

## 瀑布流

```js
/*点击按钮加载数据，不刷新页面---ajax
	- 定时器实现加载动画
回调函数中
	- 渲染获取到的数据
	- 使用瀑布流插件布局页面
	- 渲染页面
	- 设置页码，最后一页禁止点击*/

// 导入jquery和瀑布流插件masonry
$(function () {
	// 定义页码
	var my_currentPage = 1;
	// 点击变....
	$('.tips').click(function () {
		// 添加判断类名的代码
		if($(this).hasClass('disabled')==true){
			alert('哥们,别点了,已经是最后一页了哦');
			return;
		}
		// .
		$(this).html('.');
		var $this = $(this);
		// 定时器
		var interId = setInterval(function () {
			var oldStr = $this.html();
			// 判断长度
			if (oldStr.length > 16) {
				oldStr = '';
			}
			// 累加 .
			oldStr += '.';
			// 赋值给元素的内容
			$this.html(oldStr);
		}, 100)
		// ajax获取数据

		$.ajax({
			url: 'api/waterFall.php',
			type: 'post',
			data: {
				currentPage: my_currentPage,
				pageSize: 40
			},
			success: function (data) {
				console.log(data); //测试代码
				// 清除定时器
				clearInterval(interId);
				// 修改内容为 1/xx
				$('.tips').html(data.currentPage + '/' + data.totalPage);
				// 渲染页面 -- 模板引擎
				var result = template('template', data);
				// console.log(result); //测试代码
				var $dom = $(result);
				// $('.items').append(result);

				// 瀑布流
				/*
					通过例子查看 缺少的代码
					通过文档 查看 方法的说明
					复制过来测试
					不对 检查 是不是 参数不对
				*/
				$('.items').masonry({
					transitionDuration: 0
				}).append($dom).masonry('appended', $dom).masonry();

				// 页码 累加
				my_currentPage++;

				// 判断是否是最后一页
				if(data.currentPage==data.totalPage){
					// 添加类名
					$('.tips').addClass('disabled');
				}
			}
		})
	})
})
```

## 跨域和同源

```js
- 同源
Request URL: http://127.0.0.1/pubuliu/waterfall.php
# 请求url
http://127.0.0.1/pubuliu/
# 浏览器的url
- 不同源
传输协议，ip地址，端口
三个都一样 称为同源
有一个不一样称为不同源
- 跨域
不同源的网站之间互相发送请求 称为跨域

# 浏览器默认限制跨域访问
# 解决方案
	# cors： cross origin resourse sharing(html5才支持)
		服务器端设置在响应头里面
		header('Access-Control-Allow-Origin: *');
	# JSONP: 
		DOM标签中的src属性支持跨域，如img script
		JSONP就是利用了这个属性，和Ajax没有关系
		属性在链接里面 所以不能发post请求
<script>
    function doSomething(data){
        console.log(data)
    }
</script>
<script src="http://192.168.0.107/jsonp/data.php?callback=doSomething"></script>

<?php 
$methodName = $_GET['callback'];
echo $methodName.'({"name":"jack","gender":"male"})'
 ?>
 
jQuery中的JSONP
$.ajax({
    url: 'api/waterFall.php',
    type: 'post',
    data: {
    currentPage: my_currentPage,
    pageSize: 40
    },
    success:function(data){
    	console.log(data)
    },
    dataType:'jsonp'
}
```









