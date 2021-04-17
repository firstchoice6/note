# Web-Api

## 元素操作

### 获取元素

```js
		1 document.getElementById('id名字')；
		2 document.getElementByTagName('标签名')；
		//返回一个集合 可以继续获取下一级
		var divs = document.getElementById('id名字')；
		divs.getElementByTagName('标签名')；
```
```js
		document.getElementByClassName('类名')；
		document.querySelector(选择器);//只能获取一个
		document.querySelectorAll(选择器);
		exp: document.querySelector("div.class input[name=login]");
// 不常用
```
### 创建元素	

```js
	1. document.write()  //完全改写，覆盖之前 基本不用
		exp:
			document.write('hello<p>world</p>')
```


```js
	2. element.innerHTML  //创建简单元素
```


```js
	3. document.createElement()//创建元素 注册事件
		exp:
			var p = document.createElement('p');
			p.innerText = 'hello';
			var box = document.getElementById('box');
			box.appendChild(p);
```


### 元素操作方法：
```js
		createElement()
		appendChild() 追加到最后 参数是已有元素时，会“剪切”
			node.cloneNode()复制节点
		removeChild()
		insertBefore(新元素,参考元素) 插入到指定位置之前
		replaceChild(新元素,旧元素) 元素标签替换
```


## 事件操作
### 注册事件
```js
	1) btn.onclick = function() {
		alert('warning');
	}
	//如果同时有多个事件，前面的事件会覆盖后面的事件

	2) btn.addEventListener(type,listener,options)<!--  兼容问题 -->
		EXP:
			btn.addEventListener('click',function () {
				alert('warning');
			})
			btn.addEventListener('click',function () {
				alert('warning2');
			}，true)

		/*
		type : 'click' 事件类型
		listener ： 事件处理函数
		options ： listener的可选参数对象
							false： 事件冒泡 先执行最内层
							true： 不冒泡 先执行最外层
							e.stopPropagation(); 取消冒泡
		*/

	3) btn.attachEvent('onclick',function () {
			alert('warning');
		}) <!--  兼容问题 ie9-10支持 -->
```
-----------------------------------------------------
	onclick 点击
	onfocus 获得焦点
	onchange 文本改变并且失去焦点
	onblur 失去焦点
	onmouseover 鼠标经过
	onmouseout 鼠标离开

	onkeydown 键盘按下
	onkeyup 键盘弹起
-----------------------------------------------------

### 事件委托
```js
	ul.onclick = function (e) {
		e = e || window.event
        //兼容问题
		e.target.style.backgroundColor = '#000';
		//e.target是真正触发事件的对象
	}
```

### 移除事件
```js
1. btn.onclick = function () {
				alert('warning');
				btn.onclick = null;
			}
2. 	function btnClick() {
				alert('warning');
				btn.removeEventListener('click',btnClick);
				})
			}
	btn.addEventListener('click',btnClick);
	//不能用匿名函数
3. function btnClick() {
			alert('warning');
			btn.detachEvent('click',btnClick);
			})
			}
	btn.attachEvent('click',btnClick);
	//不能用匿名函数
```
### 距离三大家client，offset，scroll
		e.clientX,e.clientY     鼠标在可视区域的坐标
		e.pageX,e.pageY         鼠标在整个页面的坐标 <!-- 兼容问题 ie9以后支持 -->
		offsetLeft,offsetTop    盒子距离边界的距离
		scrollLeft,scrollTop    滚动条距离


## 属性操作
- 非表单类属性:
		href、title、id、src、className（class是关键字不可用）

- 表单元素属性:
		value type(文本) disable checked selected(布尔类型的值)

	innerHTML(含标签) innerText(仅标签内部) textContent

	 <!-- 获取开始标签和结束标签之间的内容,可以加标签,特殊符号要加转义符"\" -->
- 自定义属性的获取/修改方法
	getAttribute('属性名')
	setAttribute('属性名','属性值')
- 删除属性
	moveAttribute('属性名')


### 节点相关属性
节点名称|节点类型|节点值
:-:|:-:|:-:
"DIV"|1元素节点|null
""|2属性节点|属性值
#text|3文本节点|文本值
|8注释节点|
### 节点操作
- 父节点		parentNode
- 子节点      childNodes(子节点)    <!-- 空格 换行 注释都算子节点 -->
					判定是否有子节点  hasChildNodes() <!-- 是一个方法 -->
					firstchild		 获取第一个子节点
					lastchild 		获取最后一个子节点
  				children(子元素)		 <!-- 注释不算子元素 -->
  				 	ul.children[0] ul的第一个子元素
  					firstElementChild	第一个子元素
- 兄弟节点     nextSibling  		下一个兄弟节点
  				previousSibling		上一个兄弟节点
  				nextElementSibling  		下一个兄弟元素
  				previousElementSibling		上一个兄弟元素


## BOM
```js
BOM的最高对象是window window的属性可以省略window

alert('内容');提示
prompt('提示信息','默认值');提示输入   <!-- 基本不用这三种 -->
confirm('提示'); 确认，返回布尔

onload 页面加载
onunload 页面卸载

setInterval() <!-- 定时器重复执行 -->
setTimeout(function () {
		alert('boom'); <!-- 定时器只执行一次 -->
	},3000)
		//第一个参数 函数 可匿名，可命名 命名时不加()
		//第二个参数 间隔时间 单位ms
	取消定时器执行
	clearTimeout('定时器标识')

location.href = http://cn.bing.com;记录历史
location.assign(http://cn.bing.com);委派	 记录历史
lacation.replace(http://cn.bing.com);替换 不记录历史
location.reload(true/false); 服务器刷新/缓存刷新，对应ctrl+F5/F5

history.back();页面后退
history.forward();前进
history.go(-1/2); 后退1/前进2
```


## tips

- 取消a链接跳转
	link.onclick = function (e) {
		//return false;       <!-- 推荐这种 -->
		//e.preventDefault();		<!-- DOM标准方法 -->
			<!-- 取消默认行为 不止限于链接跳转 -->
		//e.returnValue = false;  <!--  IE老版本非标准方法 -->
	}
	或者 <a href="javascript:void(0)">
	或者 <a href="javascript:undefined">
- 取消冒泡
		e.stopPropagation();
		e.cancelBubble = true; <!-- ie -->
- 页面加载完成后执行
		window.onload = function () {
		}
- URL格式：
		scheme：//host:port/path?query#fragment
		协议：//主机：端口/路径？查询#信息片段 锚点
