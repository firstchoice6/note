## 浏览器内核    

```
最开始渲染引擎和 JS 引擎并没有区分的很明确，后来 JS 引擎越来越独立，内核就倾向于只指渲染引擎。有一个网页标准计划小组制作了一个 ACID 来测试引擎的兼容性和性能。内核的种类很多，如加上没什么人使用的非商业的免费内核，可能会有10多种，但是常见的浏览器内核可以分这四种：Trident、Gecko、Blink、Webkit。
```

（1）Trident(IE内核) 

国内很多的双核浏览器的其中一核便是 Trident，美其名曰 "兼容模式"。

代表： IE、傲游、世界之窗浏览器、Avant、腾讯TT、猎豹安全浏览器、360极速浏览器、百度浏览器等。

Window10 发布后，IE 将其内置浏览器命名为 Edge，Edge 最显著的特点就是新内核 EdgeHTML。

（2）Gecko(firefox) 

Gecko(Firefox 内核)： Mozilla FireFox(火狐浏览器) 采用该内核，Gecko 的特点是代码完全公开，因此，其可开发程度很高，全世界的程序员都可以为其编写代码，增加功能。 可惜这几年已经没落了， 比如 打开速度慢、升级频繁、猪一样的队友flash、神一样的对手chrome。

（3） webkit(Safari)  

 Safari 是苹果公司开发的浏览器，所用浏览器内核的名称是大名鼎鼎的 WebKit。

 现在很多人错误地把 webkit 叫做 chrome内核（即使 chrome内核已经是 blink 了），苹果感觉像被别人抢了媳妇，都哭晕再厕所里面了。

 代表浏览器：傲游浏览器3、 Apple Safari (Win/Mac/iPhone/iPad)、Symbian手机浏览器、Android 默认浏览器，

（4） Chromium/Bink(chrome) 

   在 Chromium 项目中研发 Blink 渲染引擎（即浏览器核心），内置于 Chrome 浏览器之中。Blink 其实是 WebKit 的分支。 

​     大部分国产浏览器最新版都采用Blink内核。

（5） Presto(Opera) 

  Presto 是挪威产浏览器 opera 的 "前任" 内核，为何说是 "前任"，因为最新的 opera 浏览器早已将之抛弃从而投入到了谷歌怀抱了。

# Web标准

##  Web 标准构成

 Web标准不是某一个标准，而是由W3C和其他标准化组织制定的一系列标准的集合。主要包括结构（Structure）、表现（Presentation）和行为（Behavior）三个方面。

~~~
结构标准：结构用于对网页元素进行整理和分类，主要包括XML和XHTML两个部分。
样式标准：表现用于设置网页元素的版式、颜色、大小等外观样式，主要指的是CSS。
行为标准：行为是指网页模型的定义及交互的编写，主要包括DOM和ECMAScript两个部分
~~~

## HTML骨架格式

```html
<HTML>   
    <head>     
        <title></title>
    </head>
    <body>
    </body>
</HTML>
```

~~~html
1 HTML标签：

作用所有HTML中标签的一个根节点。

2 head标签：

作用：用于存放：

title,meta,base,style,script,link

注意在head标签中我们必须要设置的标签是title

3.title标签：

作用：让页面拥有一个属于自己的标题。

4.body标签：

作用：页面在的主体部分，用于存放所有的HTML标签：

<p>,<h>,<a>,<b>,<u>,<i>,<s>,<em>,<del>,<ins>,<strong>,<img>

~~~

## HTML标签分类

  在HTML页面中，带有“< >”符号的元素被称为HTML标签，如上面提到的 &lt;HTML&gt;、&lt;head&gt;、&lt;body&gt;都是HTML标签。所谓标签就是放在“< >” 标签符中表示某个功能的编码命令，也称为HTML标签或 HTML元素

1.双标签

~~~html
<标签名> 内容 </标签名>
~~~

该语法中“<标签名>”表示该标签的作用开始，一般称为“开始标签（start tag）”，“</标签名>” 表示该标签的作用结束，一般称为“结束标签（end tag）”。和开始标签相比，结束标签只是在前面加了一个关闭符“/”。

> ~~~html
> 比如 <body>我是文字  </body>
> ~~~

2.单标签

~~~html
<标签名 />
~~~

  单标签也称空标签，是指用一个标签符号即可完整地描述某个功能的标签。

> ~~~html
> 比如  <br />
> ~~~

## HTML标签关系

标签的相互关系就分为两种：

1.嵌套关系

```html
<head>  <title> </title>  </head>
```

2.并列关系

```html
<head></head>
<body></body>
```

# 字符集

<meta charset="UTF-8">
utf-8是目前最常用的字符集编码方式，包含全世界所有国家需要用到的字符

# HTML标签

## 排版标签

### 标题标签 (熟记)

```html
<hn>   标题文本   </hn>
```

### 段落标签( 熟记)

~~~html
<p>  文本内容  </p>
~~~

### 水平线标签(认识)

```html
<hr />是单标签
```

### 换行标签(熟记)

```html
<br />
```

### div span标签(重点)

div  span    是没有语义的     是我们网页布局主要的2个盒子

div 就是  division  的缩写   分割， 分区的意思  其实有很多div 来组合网页。

span, 跨度，跨距；范围    

语法格式：

~~~html
<div> 这是头部 </div>    <span>今日价格</span>
~~~



## 文本格式化标签(熟记)

在网页中，有时需要为文字设置粗体、斜体或下划线效果，这时就需要用到HTML中的文本格式化标签，使文字以特殊的方式显示。

<img src="./media/tab.png" />

  b  i  s  u   只有使用 没有 强调的意思       strong   em  del   ins  语义更强烈



## 标签属性

```html
<标签名 属性1="属性值1" 属性2="属性值2" …> 内容 </标签名>
```

1.标签可以拥有多个属性，必须写在开始标签中，位于标签名后面。

2.属性之间不分先后顺序，标签名与属性、属性与属性之间均以空格分开。

3.任何标签的属性都有默认值，省略该属性则取默认值。

## 图像标签img (重点)

```html
<img src="图像URL" />
```

| 属性         | 属性值                    | 描述                     |
| ------------ | ------------------------- | ------------------------ |
| src          | URL                       | 图像路径                 |
| alt          | 文本                      | 图片不能显示时的替换文本 |
| title        | 文本                      | 鼠标悬停显示文本         |
| width/height | 像素（XHTML不支持百分比） | 设置图像宽度/高度        |
| **border**   | 数字                      | 边框宽度                 |

## 链接标签(重点)

在HTML中创建超链接非常简单，只需用标签环绕需要被链接的对象即可，其基本语法格式如下：

```html
<a href="跳转目标" target="目标窗口的弹出方式">文本或图像</a>
```

href：用于指定链接目标的url地址，当为标签应用href属性时，它就具有了超链接的功能。  Hypertext Reference的缩写。意思是超文本引用

target：用于指定链接页面的打开方式，其取值有_self和_blank两种，其中_self为默认值，_blank为在新窗口中打开方式。

注意：

1.外部链接 需要添加 http:// www.baidu.com

2.内部链接 直接链接内部页面名称即可 比如 < a href="index.html"> 首页 </a >

3.如果当时没有确定链接目标时，通常将链接标签的href属性值定义为“#”(即href="#")，表示该链接暂时为一个空链接。

4.不仅可以创建文本超链接，在网页中各种网页元素，如图像、表格、音频、视频等都可以添加超链接。

### 锚点定位 （难点）

~~~html
1.使用“a href=”#id名>“链接文本"</a>创建链接文本。
2.使用相应的id名标注跳转目标的位置。
~~~

### base 标签

```html
<base target="_blank" />
<base target="_self" />
    <!--base 可以设置整体链接的打开状态   
	base 写到  <head>  </head>  之间-->
```



##  特殊字符标签 （理解）

 https://tool.lu/htmlentity/

## 注释标签

```html
 <!-- 注释语句 -->
```

# 路径

## 相对路径

1. 同一文件夹：只需输入图像文件的名称即可，如&lt;img src="logo.gif" /&gt;。
2. 下一级文件夹：输入文件夹名和文件名，之间用“/”隔开，如&lt;img src="img/img01/logo.gif" /&gt;。
3. 上一级文件夹：在文件名之前加入“../” ，如果是上两级，则需要使用 “../ ../”，以此类推，如&lt;img src="../logo.gif" /&gt;。

## 绝对路径

绝对路径

“D:\web\img\logo.gif”，或完整的网络地址，例如“http://www.itcast.cn/images/logo.gif”。

# 列表

## 无序列表 ul （重点）

无序列表的各个列表项之间没有顺序级别之分，是并列的。其基本语法格式如下：

```html
<ul>
  <li>列表项1</li>
  <li>列表项2</li>
  <li>列表项3</li>
  ......
</ul>
```

脚下留心：

```
 1. <ul></ul>中只能嵌套<li></li>，直接在<ul></ul>标签中输入其他标签或者文字的做法是不被允许的。
 2. <li>与</li>之间相当于一个容器，可以容纳所有元素。
```

## 有序列表 ol （了解）

有序列表即为有排列顺序的列表，其各个列表项按照一定的顺序排列定义，有序列表的基本语法格式如下：

```html
<ol>
  <li>列表项1</li>
  <li>列表项2</li>
  <li>列表项3</li>
  ......
</ol>
```

## 自定义列表（理解）

定义列表常用于对术语或名词进行解释和描述，定义列表的列表项前没有任何项目符号。其基本语法如下：

```html
<dl>
  <dt>名词1</dt>
  <dd>名词1解释1</dd>
  <dd>名词1解释2</dd>
  ...
  <dt>名词2</dt>
  <dd>名词2解释1</dd>
  <dd>名词2解释2</dd>
  ...
</dl>
```

# 表格

## 创建表格

```html
<table>
  <tr>
    <td>单元格内的文字</td>
    ...
  </tr>
  ...
</table>
```

~~~
1.table用于定义一个表格。

2.tr 用于定义表格中的一行，必须嵌套在 table标签中，在 table中包含几对 tr，就有几行表格。

3.td /td：用于定义表格中的单元格，必须嵌套在<tr></tr>标签中，一对 <tr> </tr>中包含几对<td></td>，就表示该行中有多少列（或多少个单元格）。
~~~

注意：

```
1. <tr></tr>中只能嵌套<td></td>
```

```
2. <td></td>标签，他就像一个容器，可以容纳所有的元素
```



## 表格属性

```css
table{
 	border-collapse:collapse;
    /*合并边框*/
 }
```

| 属性名      | 含义                                     | 常用属性值        |
| ----------- | ---------------------------------------- | ----------------- |
| border      | 设置表格的边框（默认border="0"无边框）   | px                |
| cellspacing | 设置单元格与单元格边框之间的空白间距     | px(默认2px)       |
| cellpadding | 设置单元格内容与单元格边框之间的空白间距 | px(默认1px)       |
| width       | 设置表格的宽度                           | px                |
| height      | 设置表格的高度                           | px                |
| align       | 设置表格在网页中的水平对齐方式           | left center right |



## 表头标签

表头一般位于表格的第一行或第一列，其文本加粗居中，如下图所示，即为设置了表头的表格。设置表头非常简单，只需用表头标签&lt;th&gt;</th&gt;替代相应的单元格标签&lt;td&gt;</td&gt;即可。

## 表格结构（了解）

```
在使用表格进行布局时，可以将表格划分为头部、主体和页脚（页脚因为有兼容性问题，我们不在赘述），具体 如下所示：

<thead></thead>：用于定义表格的头部。

必须位于<table></table> 标签中，一般包含网页的logo和导航等头部信息。


<tbody></tbody>：用于定义表格的主体。

位于<table></table>标签中，一般包含网页中除头部和底部之外的其他内容。
```

## 表格标题

**表格的标题： caption**

**定义和用法**

caption 元素定义表格标题。

```html
<table>
   <caption>我是表格标题</caption>
</table>
```

caption 标签必须紧随 table 标签之后。您只能对每个表格定义一个标题。通常这个标题会被居中于表格之上。

## 合并单元格(难点)

跨行合并：rowspan    跨列合并：colspan

合并单元格的思想：

​     将多个内容合并的时候，就会有多余的东西，把它删除。    例如 把 3个 td 合并成一个， 那就多余了2个，需要删除。

​     公式：  删除的个数  =  合并的个数  - 1   

   合并的顺序 先上   先左 

# 表单

## input 控件(重点)

|   属性    |   属性值   |         描述         |
| :-------: | :--------: | :------------------: |
|           |    text    |       单行文本       |
|           |  password  |         密码         |
|           |   radio    |       单选按钮       |
|           |  checkbox  |        复选框        |
|   type    |   button   |         按钮         |
|           |   submit   |         提交         |
|           |   reset    |         重置         |
|           |   image    |   图片形式**提交**   |
|           |    file    |        文件域        |
|   name    |            |       控件名称       |
|   value   |            |       默认文本       |
|   size    |   正整数   |    input控件宽度     |
|  checked  |  checked   |   复选框默认选中项   |
| selected  |  selected  |  下拉选单默认选中项  |
| maxlength |   正整数   | 允许输入的最多字符数 |
| disabled  | true/false |   文本框是否可编辑   |

##  label标签

用于绑定一个表单元素, 当点击label标签的时候, 被绑定的表单元素就会获得输入焦点

for 属性规定 label 与哪个表单元素绑定。

```html
1. 直接用<label>包裹<input>
    <label>输入用户名：<input type="text"></label>
2. for属性
	<label for="male">Male</label>
	<input type="radio" name="sex" id="male" value="male">
```

## textarea控件(文本域)

如果需要输入大量的信息，就需要用到&lt;textarea&gt;&lt;/textarea&gt;标签。通过textarea控件可以轻松地创建多行文本输入框，其基本语法格式如下：

```html
<textarea cols="每行中的字符数" rows="显示的行数">
  文本内容
</textarea>
```

## 下拉菜单

使用select控件定义下拉菜单的基本语法格式如下

```html
<select>
  <option>选项1</option>
  <option>选项2</option>
  <option>选项3</option>
  ...
</select>
```

注意：

1. &lt;select&gt;</select&gt;中至少应包含一对&lt;option></option&gt;。
2. 在option 中定义selected =" selected "时，当前项即为默认选中项。

## 表单域

在HTML中，form标签被用于定义表单域，即创建一个表单，以实现用户信息的收集和传递，form中的所有内容都会被提交给服务器。创建表单的基本语法格式如下：

```html
<form action="url地址" method="提交方式" name="表单名称">
  各种表单控件
</form>
```

常用属性：

1. Action
   在表单收集到信息后，需要将信息传递给服务器进行处理，action属性用于指定接收并处理表单数据的服务器程序的url地址。
2. method
   用于设置表单数据的提交方式，其取值为get或post。
3. name
   用于指定表单的名称，以区分同一个页面中的多个表单。

注意：  每个表单都应该有自己表单域。

# HTML5新标签与特性

## 文档类型设定

- document
  - HTML:        sublime 输入  html:4s
  - XHTML:      sublime 输入  html:xt
  - HTML5        sublime 输入  html:5       <!DOCTYPE html>

## 字符设定

- <meta http-equiv="charset" content="utf-8">：HTML与XHTML中建议这样去写

- <meta charset="utf-8">：HTML5的标签中建议这样去写

## 常用新标签

 w3c  [手册中文官网]()     :   http://w3school.com.cn/

- header：定义文档的页眉 头部

- nav：定义导航链接的部分

- footer：定义文档或节的页脚 底部

- article：定义文章。

- section：定义文档中的节（section、区段）

- aside：定义其所处内容之外的内容 侧边

  ~~~html
  <header> 语义 :定义页面的头部  页眉</header>
  <nav>  语义 :定义导航栏 </nav> 
  <footer> 语义: 定义 页面底部 页脚</footer>
  <article> 语义:  定义文章</article>
  <section> 语义： 定义区域</section>
  <aside> 语义： 定义其所处内容之外的内容 侧边</aside>
  ~~~

  

- datalist   标签定义选项列表。请与 input 元素配合使用该元素

  ~~~html
  <input type="text" value="输入明星" list="star"/> <!--  input里面用 list -->
  <datalist id="star">   <!-- datalist 里面用 id  来实现和 input 链接 -->  
      		<option>刘德华</option>
      		<option>刘若英</option>
      		<option>刘晓庆</option>
      		<option>郭富城</option>
      		<option>张学友</option>
      		<option>郭郭</option>
  </datalist>
  ~~~

  

- fieldset 元素可将表单内的相关元素分组，打包      legend 搭配使用

  ~~~HTML
  <fieldset>
      		<legend>用户登录</legend>  标题
      		用户名: <input type="text"><br /><br />
      		密　码: <input type="password">
  </fieldset>
  ~~~

  

## 新增的input type属性值：

| **类型**     | **使用示例**            | **含义**             |
| ------------ | ----------------------- | -------------------- |
| **email**    | <input type="email">    | 输入邮箱格式         |
| **tel**      | <input type="tel">      | 输入手机号码格式     |
| **url**      | <input type="url">      | 输入url格式          |
| **number**   | <input type="number">   | 输入数字格式         |
| **search**   | <input type="search">   | 搜索框（体现语义化） |
| **range**    | <input type="range">    | 自由拖动滑块         |
| **time**     | <input type="time">     | 小时分钟             |
| **date**     | <input type="date">     | 年月日               |
| **datetime** | <input type="datetime"> | 时间                 |
| **month**    | <input type="month">    | 月年                 |
| **week**     | <input type="week">     | 星期 年              |

## 

## 常用新属性

| **属性**         | **用法**                                   | **含义**                                                 |
| -------------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| **placeholder**  | <input type="text" placeholder="请输入用户名"> | 占位符  当用户输入的时候 里面的文字消失  删除所有文字，自动返回 |
| **autofocus**    | <input type="text" autofocus>                  | 规定当页面加载时 input 元素应该自动获得焦点                  |
| **multiple**    | <input type="file" multiple>                   | 多文件上传                                                   |
| **autocomplete** | <input type="text" autocomplete="off">         | 规定表单是否应该启用自动完成功能  有2个值(on/off)<br />on 代表记录已经输入的值  1.autocomplete 首先需要提交按钮 <br/>2.这个表单必须有名字 |
| **required**    | <input type="text" required>                   | 必填项  内容不能为空                                         |
| **accesskey**   | <input type="text" accesskey="s">              | 规定激活（使元素获得焦点）元素的快捷键   采用 alt + s的形式  |



## 综合案例

~~~html
<form action="">
  <fieldset>
    <legend>学生档案</legend>
    <label for="userName">姓名:</label>
    <input type="text" name="userName" id="userName" placeholder="请输入用户名"> <br>
    <label for="userPhone">手机号码:</label>
    <input type="tel" name="userPhone" id="userPhone" pattern="^1\d{10}$"><br>
    <label for="email">邮箱地址:</label>
    <input type="email" required name="email" id="email"><br>
    <label for="collage">所属学院:</label>
    <input type="text" name="collage" id="collage" list="cList" placeholder="请选择"><br>
    <datalist id="cList">
      <option value="前端与移动开发学院"></option>
      <option value="java学院"></option>
      <option value="c++学院"></option>
    </datalist><br>
    <label for="score">入学成绩:</label>
    <input type="number" max="100" min="0" value="0" id="score"><br>
   <form action="">
    <fieldset>
    	<legend>学生档案思密达</legend>
    	<label>姓名: <input type="text" placeholder="请输入学生名字"/></label> <br /><br />
    	<label>手机号: <input type="tel" /></label> <br /><br />
    	<label>邮箱: <input type="email" /></label> <br /><br />
    	<label>所属学院:  <input type="text" placeholder="请选择学院" list="xueyuan"/>
    	<datalist id="xueyuan">
    		<option>java学院</option>
    		<option>前端学院</option>
    		<option>php学院</option>
    		<option>设计学院</option>
    	</datalist>

    	<br /><br />

    	<label>出生日期:   <input type="date" /></label> <br /><br />
    	<label>成绩:  <input type="number" /></label> <br /><br />
    	<label>毕业时间:  <input type="date" /></label> <br /><br />
    	<input type="submit" />  <input type="reset" />
    </fieldset>
    </form>
    <label for="inTime">入学日期:</label>
    <input type="date" id="inTime" name="inTime"><br>
    <label for="leaveTime">毕业日期:</label>
    <input type="date" id="leaveTime" name="leaveTime"><br>
    <input type="submit">
  </fieldset>
</form>
~~~



## 多媒体标签

- embed：标签定义嵌入的内容

- audio：播放音频

- video：播放视频


### 多媒体 embed（会使用）

embed可以用来插入各种多媒体，格式可以是 Midi、Wav、AIFF、AU、MP3等等。url为音频或视频文件及其路径，可以是相对路径或绝对路径。

因为兼容性问题，我们这里只讲解 插入网络视频， 后面H5会讲解 audio 和video 视频多媒体。 

```html
<embed src="http://player.youku.com/player.php/sid/XMTI4MzM2MDIwOA==/v.swf" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>
```

### 多媒体 audio

HTML5通过<audio>标签来解决音频播放的问题。

使用相当简单，如下图所示

![1498468026526](C:/Users/何镇武/Documents/前端/md/html/media/1498468026526.png) 

并且可以通过附加属性可以更友好控制音频的播放，如：

autoplay 自动播放

controls 是否显不默认播放控件

loop 循环播放   loop = 2 就是循环2次   loop  或者  loop = "-1"   无限循环

由于版权等原因，不同的浏览器可支持播放的格式是不一样的，如下图供参考

![1498468041058](C:/Users/何镇武/Documents/前端/md/html/media/1498468041058.png) 

多浏览器支持的方案，如下图

![1498468052965](C:/Users/何镇武/Documents/前端/md/html/media/1498468052965.png) 



### 多媒体 video

HTML5通过<audio>标签来解决音频播放的问题。

同音频播放一样，<video>使用也相当简单，如下图

![1498468072194](C:/Users/何镇武/Documents/前端/md/html/media/1498468072194.png) 

同样，通过附加属性可以更友好的控制视频的播放

autoplay 自动播放

controls 是否显示默认播放控件

loop 循环播放

width 设置播放窗口宽度

height 设置播放窗口的高度

由于版权等原因，不同的浏览器可支持播放的格式是不一样的，如下图供参考

![1498468086199](C:/Users/何镇武/Documents/前端/md/html/media/1498468086199.png) 

**多浏览器支持的方案，如下图**

![1498468097509](C:/Users/何镇武/Documents/前端/md/html/media/1498468097509.png)



