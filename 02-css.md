# CSS样式规则

~~~css
h1 {
    color:red;
    font-size:25px;
}
~~~
## 引入CSS样式表

### 外部样式

```html
<head>
<link href="style.css" rel="stylesheet"/> <!--路径可相对，可绝对-->
</head>
```

### 三种样式表总结

| 样式表     | 优点                     | 缺点                     | 使用情况       | 控制范围           |
| ---------- | ------------------------ | ------------------------ | -------------- | ------------------ |
| 行内样式表 | 书写方便，权重高         | 没有实现样式和结构相分离 | 较少           | 控制一个标签（少） |
| 内嵌样式表 | 部分结构和样式相分离     | 没有彻底分离             | 较多           | 控制一个页面（中） |
| 外部样式表 | 完全实现结构和样式相分离 | 需要引入                 | 最多，强烈推荐 | 控制整个站点（多） |

# CSS字体样式属性

## font-size:字号大小

- 相对长度：px，

- 绝对长度：in， cm， mm， pt

## font-family:字体

font-family属性用于设置字体。网页中常用的字体有宋体、微软雅黑、黑体等。

可以同时指定多个字体，中间以逗号隔开，表示如果浏览器不支持第一个字体，则会尝试下一个，直到找到合适的字体。

~~~css
p{ font-family:"微软雅黑";}
1. 现在网页中普遍使用14px+。
2. 尽量使用偶数的数字字号。ie6等老式浏览器支持奇数会有bug。
3. 各种字体之间必须使用英文状态下的逗号隔开。
4. 中文字体需要加英文状态下的引号，英文字体一般不需要加引号。当需要设置英文字体时，英文字体名必须位于中文字体名之前。
5. 如果字体名中包含空格、#、$等符号，则该字体必须加英文状态下的单引号或双引号
	font-family: "Times New Roman";。
6. 尽量使用系统默认字体，保证在任何用户的浏览器中都能正确显示。
~~~

### CSS Unicode字体

```css
1. 英文 比如 font-family:"Microsoft Yahei"。
2. Unicode 
font-family: "\5FAE\8F6F\96C5\9ED1"，表示设置字体为“微软雅黑”。
可以通过escape()  来测试属于什么字体。

为了照顾不同电脑的字体安装问题，我们尽量只使用宋体和微软雅黑中文字体
```

## font-weight:字体粗细

~~~css
1. b  和 strong 标签
2. normal、bold、bolder、lighter
3. 100~900（100的整数倍）。
	数字 400 等价于 normal，而 700 等价于 bold。  
~~~

## font-style:字体风格

 ~~~css
1. i 和 em 标签 
2. normal：默认值 italic：斜体 oblique：倾斜
2.2 em {
	font-style: normal;
    /*去倾斜*/
}
 ~~~



## font:综合设置字体样式 (重点)

font属性用于对字体样式进行综合设置，其基本语法格式如下：

```css
选择器{font: font-style  font-weight  font-size/line-height  font-family;}
例如 h2 {font: italic bold 14px "微软雅黑";}
/*记忆： 斜粗12黑*/

不能更换顺序，各个属性以空格隔开。除font-size和font-family外其余属性可省略不写
```

# CSS选择器

```css
- 标签选择器（元素选择器）
    标签名{属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }  
	或者
    元素名{属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }

- 类选择器 .class
    .class名{属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }
     标签调用的时候用 class=“类名”  即可。
	/*使用-,不要用_(区分js)，不要纯数字、中文等命名， 尽量使用英文字母来表示。*/

- id选择器
	#id名{属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }

- 通配符选择器
	* {
      margin: 0;                    /* 定义外边距*/
      padding: 0;                   /* 定义内边距*/
    }

- （链接）伪类选择器
	 标签:link{}

	 :link      /* 未访问的链接 */
     :visited   /* 已访问的链接 */
     :hover     /* 鼠标移动到链接上 */ 一般只用这个
     :active    /* 选定的链接 点击不松开的时候 */

      /* 顺序尽量不要颠倒  按照  lvha（绿哈） 的顺序。*/
    a:hover {   
                color: red; 
    }

- 结构(位置)伪类选择器（CSS3)
    li:first-child  /*  选择第一个子元素 */     	
    li:last-child   /* 最后一个子元素 */        	
    li:nth-child(odd) /* 选择所有奇数子元素*/
    li:nth-last-child(n) /*倒数第n个子元素*/

    子元素编号从1开始不是0
    n 可以是数字、关键词或公式 

- 目标伪类选择器(CSS3)
	:target目标伪类选择器 :选择器可用于选取当前活动的目标元素
	:target {
            color: red;
            font-size: 30px;
    }

- 伪元素选择器（CSS3)

    1. E::first-letter（如中文、日文、韩文等）
    2. E::first-line 文本第一行；
    3. E::selection 可改变选中文本的样式；
	
	p::first-letter {
        /*文本的第一个单词或字*/
      font-size: 20px;
      color: hotpink;
    }

    p::first-line {
        /* 首行特殊样式 */
      color: skyblue;
    }

    p::selection {
        /*选中的文本*/
     font-size: 50px;
      color: orange;
    }
	4、E::before和E::after
	在E元素内部的开始位置和结束位创建一个元素，该元素为行内元素
    div::befor {
      content:"开始";
        /*插入到div内部*/
        /*必须要结合content属性使用。*/
    }
    div::after {
      content:"结束";
    }
    /*E:after、E:before 在旧版本里是伪元素，CSS3的规范里“:”用来表示伪类，“::”用来表示伪元素，但是在高版本浏览器下E:after、E:before会被自动识别为E::after、E::before，这样做的目的是用来做兼容处理。*/
```



# CSS复合选择器

复合选择器是由两个或多个基础选择器，通过不同的方式组合而成的,目的是为了可以选择更准确更精细的目标元素标签。

## 交集选择器和并集选择器

~~~css
 div.one {
     /*类名为 .one的div标签。 直接连写 
	用的相对来说比较少，不太建议使用。*/
}    

.one,p,#test {
	color: #F00;
	/*通常用于集体声明 逗号连接
	.one 和 p 和 #test 这三个选择器都会执行颜色为红色。*/
} 
~~~

## 后代选择器和子元素选择器

~~~css
.class h3 {
	/*后代都可以*/
}
.demo > h3 {
	/*只能'亲儿子'*/
} 
~~~

## 属性选择器(语法类似正则表达式)
~~~css

li[type] {
  border: 1px solid gray;
    /* 获取到 拥有 该属性的元素 */
}

li[type="vegetable"] {
  background-color: green;
    /* 获取 属性完全等于某个值的 元素 属性值 可以使用 引号进行包裹 */
}

li[type~="hot"] {
  font-size: 40px;
    /* 使用空格分隔的 多个属性 其中有某个属性即可获取 */
}

li[color^='green'] {
  background-color: orange;
    /* 获取以某个属性开头的语法  */
}

li[type$='t']{
  color: hotpink;
  font-weight: 900;
    /* 获取以某个值 结尾的属性 */
}


li[type*=ea] {
  font-size: 100px;
    /* 获取 属性中 拥有某个值的 元素 */
}

li[price|='very'] {
  background-color: darkred;
    /*  如果属性的值 只有very 也能够获取  用来获取 多个属性 并且 使用-连接 */
}
~~~

~~~html
<ul>
  <li type='fruit' color='green'>西瓜</li>
  <li type='vegetable' color='greenyellow'>西兰花</li>
  <li type='meat'>牛肉</li>
  <li type='meat hot'>猪肉</li>
  <li type='drink hot'>可乐</li>
  <li type='drink hot'>雪碧</li>
  <li price='very-cheap'>红酒</li>
  <li price='very'>白酒</li>
</ul>
~~~

# CSS外观属性

### 示例

```css
p {
	color: #666;/*文本颜色*/
     /*  可选属性值：
     预定义的颜色值，如red，green，blue等。
	十六进制，如#FF0000(缩写#F00)，#FF6600，#29D794等。最常用
	RGB代码，如rgb(255,0,0)或rgb(100%,0%（不能省略%）,0%)。
    颜色半透明：color: rgba(r,g,b,a) 取值范围 0~1之间    color: rgba(0,0,0,0.3) 
    */
	line-height: 26px; /*行间距*/
    /* 可选属性值：
	像素px，相对值em，百分比%
	一般情况下，行距比字号大7.8像素左右就可以了。
    **设置行距等于盒子高可以使文字垂直居中(单行文字)**
    */
    text-align: center; /*水平对齐方式*/
    /* 可选属性值：
	left：左对齐（默认值） center：居中对齐	 right：右对齐
    */
    text-indent: 2em;/*首行缩进两个汉字的宽度*/
    /* 可选属性值：
    不同单位的数值、em字符宽度的倍数、百分比%，允许使用负值, 建议使用em作为设置单位。
    center： 文字水平居中
    */
    letter-spacing: normal; /*字间距*/
    /*可选属性值：
    不同单位的数值，允许使用负值，默认为normal。
    */
    word-spacing: 26px; /*单词间距*/
    /*可选属性值：
    不同单位的数值，允许使用负值，默认为normal。 **中文无效**
    */
	text-shadow:水平位置 垂直位置 模糊距离 阴影颜色; /*文字阴影*/
    /*
    水平/垂直位置：必需 允许负值
    模糊距离/阴影颜色（可rgba）：非必需
    可以有多组,用逗号隔开
    */
    text-decoration: none; /*文字下划线*/
    /*可选属性
    none(默认) underline(下划线) overline(上划线) line-through(删除线)
    */
}

```

- 文字两头对齐

  ```css
      .div {
        width: 100px;
        text-align: justify;
      }
  
      .div::after {
        display: inline-block;
        content: "";
        padding-left: 100%;
      }
  ```

  

# 标签显示模式（display）

## 块级元素(block-level)

每个块元素通常都会独自占据一整行或多整行，可以对其设置宽度、高度、对齐等属性，常用于网页布局和网页结构的搭建。

```css
常见的块元素有<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>等，其中<div>标签是最典型的块元素。
/*块级元素的特点：
（1）总是从新行开始
（2）高度，行高、外边距以及内边距都可以控制。
（3）宽度默认是容器的100%
（4）可以容纳内联元素和其他块元素。*/
```

## 行内元素(inline-level)

行内元素（内联元素）不占有独立的区域，仅仅靠自身的字体大小和图像尺寸来支撑结构，一般不可以设置宽度、高度、对齐等属性，常用于控制页面中文本的样式。

```css
常见的行内元素有<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>等，其中<span>标签最典型的行内元素。
/*行内元素的特点：
（1）和相邻行内元素在一行上。
（2）高、宽无效，但水平方向的padding和margin可以设置，垂直方向的无效。
（3）默认宽度就是它本身内容的宽度。
（4）行内元素只能容纳文本或则其他行内元素。（a特殊）   

注意：
1. 只有 文字才 能组成段落  因此 p  里面不能放块级元素，同理还有这些标签h1,h2,h3,h4,h5,h6,dt，他们都是文字类块级标签，里面不能放其他块级元素。
2. 链接里面不能再放链接。*/
```

## 行内块元素（inline-block）
```css
在行内元素中有几个特殊的标签——<img />、<input />、<td>，可以对它们设置宽高和对齐属性

/*行内块元素的特点：
（1）和相邻行内元素（行内块）在一行上,但是之间会有空白缝隙。
（2）默认宽度就是它本身内容的宽度。
（3）高度，行高、外边距以及内边距都可以控制。*/
```
## 三种模式相互转换
```css
div {
    display: inline;/*块转行内*/
    display: block;/*行内转块*/
    display: inline-block;/*块，行内转行内块*/
}
```

# CSS 背景(background)

## 背景图片(image)

~~~css
background-image : none | url (url)/*建议用后面这种*/ 

参数： 
none : 　无背景图（默认的）
url : 　使用绝对或相对地址指定背景图像 
~~~

## 背景平铺（repeat）

~~~css
background-repeat : repeat | no-repeat | repeat-x | repeat-y 

参数： 
repeat : 　背景图像在纵向和横向上平铺（默认的）
no-repeat : 　背景图像不平铺
repeat-x : 　背景图像在横向上平铺
repeat-y : 　背景图像在纵向平铺 
~~~

## 背景位置(position)

~~~css
background-position : length || length 
/*百分数或者数值10px 20px    默认0% 0% 
如果只指定了一个值，该值将用于横坐标。纵坐标将默认为50%。*/
background-position : position || position 
/*top | center | bottom | left | center | right (默认左上角left top)*/

注意：
1. position 后面是x坐标和y坐标。 可以使用方位名词或者 精确单位。
2. 如果和精确单位和方位名字混合使用，则必须是x坐标在前，y坐标后面。比如 background-position: 15px top;   则 15px 一定是  x坐标   top是 y坐标。
3 插入图片更改位置用padding或者margin
~~~

- 精灵图技术

## 背景附着

~~~css
background-attachment : scroll | fixed 

参数： 
scroll : 　背景图像是随对象内容滚动
fixed : 　背景图像固定 
~~~

## **背景简写*****

~~~css
body {
    background: #fff url(img/image.jpg) repeat-y  scroll 50% 20px;
    /*等价于
     background-color: #fff;
	background-image: url(img/image.jpg);
	background-repeat: repeat-y;
	background-attachment: scroll;
	background-position: 50% 20px;*/
}
推荐顺序:
background:背景颜色 背景图片地址 背景平铺 背景滚动 背景位置（研制普滚喂）
~~~

## 背景透明(CSS3)

~~~css
background: rgba(0,0,0,0.3);
/*文字颜色和边框也可以半透明*/
color:rgba(0,0,0,0.3);
border: 1px solid rgba(0,0,0,0.3);
~~~

## 背景缩放(CSS3)

~~~css
background-size: 300px 100px;
/*一般只写一个,写两个会拉伸
可以设置长度单位(px)或百分比*/
background-size: cover;
/*自动调整缩放比例，有溢出部分则会被隐藏*/
background-size: contain; 
/*自动调整缩放比例，不会溢出盒子*/
~~~

## 多背景(CSS3)

~~~css
background-image: url('images/gyt.jpg') no-repeat 10px 20px,red url('images/robot.png');
/*逗号分隔
如果发生重叠 前面的盖住后面的 
背景颜色一般设置在最后一组上*/
~~~

-------

# CSS 三大特性

层叠 继承 优先级 是我们学习CSS 必须掌握的三个特性。

>权重是优先级的算法，层叠是优先级的表现

## CSS特殊性（Specificity）

| 继承的权重               | 0,0,0,0  |
| ------------------------ | -------- |
| 每个元素（标签）贡献值为 | 0,0,0,1  |
| 每个类，伪类贡献值为     | 0,0,1,0  |
| 每个ID贡献值为           | 0,1,0,0  |
| 每个行内样式贡献值       | 1,0,0,0  |
| 每个!important贡献值     | ∞ 无穷大 |

 比如的例子：

 ~~~
div ul  li   ------>      0,0,0,3
.nav ul li   ------>      0,0,1,2
a:hover      -----—>      0,0,1,1
.nav a       ------>      0,0,1,1
#nav p       ----->       0,1,0,1

数位之间没有进制

1. 使用了 !important声明的规则。
2. 内嵌在 HTML 元素的 style属性里面的声明。
3. 使用了 ID 选择器的规则。
4. 使用了类选择器、属性选择器、伪元素和伪类选择器的规则。
5. 使用了元素选择器的规则。
6. 只包含一个通用选择器的规则。
 ~~~

# 盒子模型（重点）  

>  所谓盒子模型就是把HTML页面中的元素看作是一个矩形的盒子，也就是一个盛装内容的容器。每个矩形都由元素的内容、内边距（padding）、边框（border）和外边距（margin）组成。

## 盒子模型（Box Model）

![T%}$LA06KCITM86UDGXYMMM](https://images2015.cnblogs.com/blog/869623/201611/869623-20161130211301224-716676819.png)

## 盒子边框（border）

> 边框会撑开盒子

语法:

~~~css
border : border-width || border-style || border-color 

div {
    border-top: 1px solid #666；/*-top -bottom -left -right*/
}

border-style可选属性值：
none：无边框（默认值）solid：单实线(最为常用的) dashed：虚线 dotted：点线 double：双实线
~~~

### 表格的细线边框

```css
table{ 
    border-collapse:collapse; 
}
/*表示边框合并在一起*/
```

### 圆角边框(CSS3)

语法格式：

~~~css
border-radius: 左上角  右上角  右下角  左下角;
/*像素值或者百分比
一个值表示四个角
两个值表示 左上右下 和 右上左下
三个值表示 左上 ，右上左下 和 右下
*/
~~~

### 三角形

利用border可以绘制三角形

```css
div {
	width: 0;
    height: 0;
    border-right: 100px solid transparent;
    border-bottom: 100px solid #666;
}
```



## 内边距（padding）

> padding会撑开盒子

```css
div span {
	padding: 20px 30px;/*-top -bottom -left -right*/
}
/*
一个值表示 上下左右
两个值表示 上下，左右
三个值表示 上， 左右，下
四个值表示 上右下左   **顺时针**
*/
```

## 外边距（margin）

```css
div {
	margin-top: 20px;/*-top -bottom -left -right*/
}
/*
取值顺序跟padding相同
*/
```

### 外边距实现盒子居中

~~~css
.header{ width:960px; margin:0 auto;}
/*
1. 必须是**块级元素**。     
2. 盒子必须指定了宽度（width）*/
~~~

### 清除元素的默认内外边距 

~~~css
* {
   padding:0;         /* 清除内边距 */
   margin:0;          /* 清除外边距 */
}
注意：  行内元素是只有左右内外边距的，是没有上下内外边距的。
~~~

## 外边距合并

使用margin定义块元素的垂直外边距时，可能会出现外边距的合并。

### 相邻块元素垂直外边距的合并

当上下相邻的两个块元素相遇时，如果上面的元素有下外边距margin-bottom，下面的元素有上外边距margin-top，

则他们之间的垂直间距是两者中的较大者。

解决方案：  避免就好了。

### 嵌套块元素垂直外边距的合并

对于两个嵌套关系的块元素，如果父元素没有上内边距及边框，则父元素的上外边距会与子元素的上外边距发生合并，合并后的外边距为两者中的较大者，即使父元素的上外边距为0，也会发生合并。

<img src="./media/n.png" />

解决方案：

1. 可以为父元素定义1像素的上边框或上内边距。
2. 可以为父元素添加overflow:hidden。

待续。。。。

## content宽度和高度

~~~css
外盒尺寸（空间尺寸）= width + padding + border + margin
内盒尺寸（元素大小）= width + padding + border
注意：
1、宽度属性width和高度属性height仅适用于块级元素，对行内元素无效（img 标签和 input除外）。
2、计算盒子模型的总高度时，还应考虑上下两个盒子垂直外边距合并的情况。
3、如果没有给盒子的height和width（或**继承**），padding不会影响盒子大小
~~~

## 盒子模型布局稳定性

按照 优先使用  宽度 （width）  其次 使用内边距（padding）    再次  外边距（margin）。   

```
  width >  padding  >   margin   
```

原因：

1. margin 会有外边距合并 还有 ie6下面margin 加倍的bug（讨厌）所以最后使用。

2. padding  会影响盒子大小， 需要进行加减计算（麻烦） 其次使用。

3. width   没有问题（嗨皮）我们经常使用**宽度剩余法 高度剩余法**来做。



## CSS3盒模型

~~~css
div {
    box-sizing：border-box；
    /*让 盒子 优先保证自己所占区域的大小,对内容进行压缩;
    盒子大小为 width*/
	box-sizing: content-box;(默认)
    /*优先保证内容的大小 对盒子进行缩放;
     盒子大小为 width + padding + border*/
}
~~~

## 盒子阴影

语法格式：

~~~css
/*文字阴影*/
text-shadow:水平阴影 垂直阴影 模糊距离 阴影颜色
/*盒子阴影*/
box-shadow:水平阴影 垂直阴影(前两个必需) 模糊距离 阴影尺寸 阴影颜色  内/外阴影；

img {
  border:10px solid orange;
  box-shadow:3px 3px 5px 4px rgba(0,0,0,1);
}

外阴影 outset（默认 不能写 写了报错） 内阴影  inset 
~~~

# 浮动（重点）

CSS的定位机制有3种：普通流（标准流、文档流）、浮动和定位。

## 浮动

~~~
选择器{float:属性值;}
~~~

| 属性值 | 描述                 |
| ------ | -------------------- |
| left   | 元素向左浮动         |
| right  | 元素向右浮动         |
| none   | 元素不浮动（默认值） |

## 浮动详细内幕特性

```
浮动首先创建包含块的概念（包裹）。
浮动的元素总是找理它最近的父级元素对齐。
但是不会超出内边距的范围。 
```


   <img src="./media/one.jpg" width="500" /> 


```
浮动的元素排列位置，跟上一个元素（块级）有关系。
如果上一个元素有浮动，则A元素顶部会和上一个元素的顶部对齐；
如果上一个元素是标准流，则A元素的顶部会和上一个元素的底部对齐。
```


  <img src="./media/two.jpg" width="400" />


```
一个父盒子里面的子盒子，如果其中一个子级有浮动的，则其他子级都需要浮动。这样才能一行对齐显示。
```

~~~
浮动脱离标准流，不占位置，会影响标准流。浮动只有左右浮动。
~~~

```
元素添加浮动后，元素会具有行内块元素的特性。元素的大小完全取决于定义的大小或者默认的内容多少
```

~~~
浮动根据元素书写的位置来显示相应的浮动。
~~~

总结：  浮动 --->  浮漏特       

浮：    加了浮动的元素盒子是浮起来的，漂浮在其他的标准流盒子上面。
漏：    加了浮动的盒子，不占位置的，它浮起来了，它原来的位置漏 给了标准流的盒子。
特：    特别注意，这是特殊的使用，有很多的不好处，使用要谨慎。

## 清除浮动

### 清除浮动本质

清除浮动主要为了解决父级元素因为子级浮动引起内部高度为0 的问题。

<img src="./media/n.jpg" />

<img src="./media/no.jpg" />

### 清除浮动的方法

> 把浮动的盒子圈到里面，让父盒子闭合出口和入口不让他们出来影响其他元素。

1. clear属性

```css
选择器{clear:属性值;}

/* 属性值：left right both*/
```

2. 额外标签法

```html
是W3C推荐的做法是通过在浮动元素末尾添加一个空的标签例如 <div style=”clear:both”></div>，或则其他标签br等亦可。
```

优点： 通俗易懂，书写方便

缺点： 添加许多无意义的标签，结构化较差。

3. 父级添加overflow属性方法

可以通过触发BFC的方式，可以实现清除浮动效果。（BFC后面讲解）

~~~css
可以给父级添加： overflow为 hidden|auto|scroll  都可以实现。
~~~

优点：  代码简洁

缺点：  内容增多时候容易造成不会自动换行导致内容被隐藏掉，无法显示需要溢出的元素。

4. 使用after伪元素清除浮动

使用方法：

```css
 .clearfix:after {  
     content: "."; 
     display: block; 
     height: 0; 
     clear: both; 
     visibility: hidden;  
}   

 .clearfix {
     *zoom: 1;
}   /* IE6、7 专有 */
```

优点： 符合闭合浮动思想  结构语义化正确

缺点： 由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

代表网站： 百度、淘宝网、网易等

注意： content:"."  里面尽量跟一个小点，或者其他，尽量不要为空，否则在firefox 7.0前的版本会有生成空格。


5. 使用before和after双伪元素清除浮动

使用方法：

```css
.clearfix:before,.clearfix:after { 
  content:".";
  display:table;
}
.clearfix:after {
 clear:both;
}
.clearfix {
  *zoom:1;
}
```

优点：  代码更简洁

缺点：  由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

代表网站： 小米、腾讯等

# 定位（重点）

## 元素的定位属性

元素的定位属性主要包括**定位模式和边偏移**两部分。

1、边偏移
```css
`top` `bottom` `left` `right`
/*  top: 100px;  left: 30px; 等等 */
```



2、定位模式

在CSS中，position属性用于定义元素的定位模式，其基本语法格式如下：

```css
选择器{
	position:属性值;
}
/*属性值
static自动定位（默认定位方式）
relative相对定位，相对于其原文档流的位置进行定位
absolute绝对定位，相对于其上一个已经定位的父元素进行定位
fixed固定定位，相对于浏览器窗口进行定位*/
```

## 静态定位(static)

静态定位是所有元素的默认定位方式，可以将元素定位于静态位置。一般用来清除定位。

在静态定位状态下，**无法通过边偏移属性**（top、bottom、left或right）来改变元素的位置。

## 相对定位relative(自恋型)

对元素设置相对定位后，可以通过边偏移属性改变元素的位置，但是它在文档流中的位置仍然保留。如下图所示，即是一个相对定位的效果展示：

<img src="./media/r.png"  />

注意：    原来的所占的位置，继续占有，**不脱标**。

就是说，相对定位的盒子仍在标准流中，它后面的盒子仍以标准流方式对待它。

## 绝对定位absolute (拼爹型)

[注意] 如果文档可滚动，绝对定位元素会随着它滚动，因为元素最终会相对于正常流的某一部分定位。

注意： 绝对定位最重要的一点是，它可以通过边偏移移动位置，但是它**完全脱标，完全不占位置**。

### 父级没有定位

若所有父元素都没有定位，以浏览器为准对齐(document文档)。

<img src="./media/ab.png" />

### 父级有定位

绝对定位是将元素依据最近的已经定位（绝对、固定或相对定位）的父元素（祖先）进行定位。 

<img src="./media/ab1.png" />

### 绝对定位的盒子没有边偏移

如果只是给盒子指定了 定位，但是没有给与边偏移，则改盒子以标准流来显示排序，和上一个盒子的底边对齐，但是不占有位置。

<img src="./media/ab2.png" />

### 子绝父相

### 绝对定位盒子水平垂直居中

```css
.father .son {
	/*绝对定位的盒子margin： auto没用*/
    left: 50%；
    top: 50%;
    margin-left: -width;
    margin-top: -height;
}
```

## 固定定位fixed(认死理型)

固定定位是绝对定位的一种特殊形式，它以浏览器窗口作为参照物来定义网页元素。

当对元素设置固定定位后，它将脱离标准文档流的控制，始终依据浏览器窗口来定义自己的显示位置。不管浏览器滚动条如何滚动也不管浏览器窗口的大小如何变化，该元素都会始终显示在浏览器窗口的固定位置。

固定定位有几点：

1. 固定定位的元素跟父亲没有任何关系，只认浏览器。
2. 固定定位完全脱标，不占有位置，不随着滚动条滚动。
3. 固定定位要写宽和高。

ie6等低版本浏览器不支持固定定位。

## 叠放次序（z-index）

```css
z-index: 2;

注意：
1. z-index的默认属性值是0，取值越大，定位元素在层叠元素中越居上。
2. 如果取值相同，则根据书写顺序，后来居上。
3. 后面数字不能加单位。
4. 只有相对定位，绝对定位，固定定位有此属性，其余标准流，浮动，静态定位都无此属性，亦不可指定此属性。
```

## 四种定位总结

| 定位模式         | 是否脱标占有位置     | 边偏移 | 移动位置基准           |
| ---------------- | -------------------- | ------ | ---------------------- |
| 静态static       | 不脱标，正常模式     | 不可以 | 正常模式               |
| 相对定位relative | 不脱标，占有位置     | 可以   | 相对自身位置移动       |
| 绝对定位absolute | 完全脱标，不占有位置 | 可以   | 相对于定位父级移动位置 |
| 固定定位fixed    | 完全脱标，不占有位置 | 可以   | 相对于浏览器移动位置   |

## 定位模式转换

跟 浮动一样， 元素添加了 绝对定位和固定定位之后， 元素模式也会发生转换， 都转换为 行内块模式；

行内元素 如果添加了 绝对定位或者 固定定位后，可以不用转换模式，直接给高度和宽度就可以了。

## 元素的显示与隐藏

在CSS中有三个显示和隐藏的单词比较常见，我们要区分开，他们分别是 display visibility 和 overflow。

### display 显示

display 设置或检索对象是否及如何显示。

```css
div {
	display : none;
	/*隐藏之后，不再保留位置*/
	display:block;
	/*除了转换为块级元素之外，同时还有显示元素的意思。*/
}
```

### visibility 可见性

```css
div {
	visibility: hidden;
	/*隐藏之后，保留位置*/
	visibility: visible;
	/*除了转换为块级元素之外，同时还有显示元素的意思。*/
}
```

### overflow 溢出

检索或设置当对象的内容超过其指定高度及宽度时如何管理内容。

```css
div {
	overflow: hidden;
}
/*可选属性
visible : 　不剪切内容也不添加滚动条。
auto : 　 超出自动显示滚动条，不超出不显示滚动条
hidden : 　不显示超过对象尺寸的内容，超出的部分隐藏掉
scroll : 　不管超出内容否，总是显示滚动条*/
```

# CSS高级技巧

## CSS用户界面样式

 所谓的界面样式， 就是更改一些用户操作样式， 比如 更改用户的鼠标样式， 表单轮廓等。但是比如滚动条的样式改动受到了很多浏览器的抵制，因此我们就放弃了。 防止表单域拖拽

### 鼠标样式cursor

 设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。 

```html
cursor :  default  小白 | pointer  小手  | move  移动  |  text  文本
```

 鼠标放我身上查看效果哦：

```html
<ul>
  <li style="cursor:default">我是小白</li>
  <li style="cursor:pointer">我是小手</li>
  <li style="cursor:move">我是移动</li>
  <li style="cursor:text">我是文本</li>
</ul>
```

 尽量不要用hand  因为 火狐不支持     pointer ie6以上都支持的尽量用

### 轮廓 outline

 是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

~~~css
 outline : outline-color ||outline-style || outline-width 
~~~

 但是我们都不关心可以设置多少，我们平时都是去掉的。

最直接的写法是 ：  outline: 0; 

```html
 <input  type="text"  style="outline: 0;"/>
```

### 防止拖拽文本域resize

resize：none    这个单词可以防止 火狐 谷歌等浏览器随意的拖动 文本域。

右下角可以拖拽： 

<textarea></textarea>
右下角不可以拖拽： 

```html
<textarea  style="resize: none;"></textarea>
```

## vertical-align 垂直对齐

以前我们讲过让带有宽度的块级元素居中对齐，是margin: 0 auto;

以前我们还讲过让文字居中对齐，是 text-align: center;

但是我们从来没有讲过有垂直居中的属性， 我们的妈妈一直很担心我们的垂直居中怎么做。

vertical-align 垂直对齐， 这个看上去很美好的一个属性， 实际有着不可捉摸的脾气，否则我们也不会这么晚来讲解。

~~~css
vertical-align : baseline |top |middle |bottom 
~~~

设置或检索对象内容的垂直对其方式。 

![1498467742995](.\media\1498467742995.png)

vertical-align 不影响块级元素中的内容对齐，它只针对于 行内元素或者行内块元素，特别是行内块元素， 通常用来控制图片和表单等。

### 图片和文字对齐

所以我们知道，我们可以通过vertical-align 控制图片和文字的垂直关系了。 默认的图片会和文字基线对齐。

### 去除图片底侧空白缝隙

有个很重要特性你要记住： 如果一个元素没有基线，比如图片或者表单等行内块元素，则他的底线会和父级盒子的基线对齐。</strong> 这样会造成一个问题，就是图片底侧会有一个空白缝隙。

<img src="./media/3.jpg" />

解决的方法就是：  

1. 给img vertical-align:middle | top等等。  让图片不要和基线对齐。<img src="./media/1633.png"  width="500"  style="border: 1px dashed #ccc;" />


1. 给img 添加 display：block; 转换为块级元素就不会存在问题了。<img src="./media/sina1.png" width="500" style="border: 1px dashed #ccc;"/>

## CSS精灵技术（sprite）



### 精灵技术的使用

CSS 精灵其实是将网页中的一些背景图像整合到一张大图中（精灵图），然而，各个网页元素通常只需要精灵图中不同位置的某个小图，要想精确定位到精灵图中的某个小图，就需要使用CSS的background-image、background-repeat和background-position属性进行背景定位，其中最关键的是使用background-position属性精确地定位。

## 字体图标

图片是有诸多优点的，但是缺点很明显，比如图片不但增加了总文件的大小，还增加了很多额外的"http请求"，这都会大大降低网页的性能的。更重要的是图片不能很好的进行“缩放”，因为图片放大和缩小会失真。 我们后面会学习移动端响应式，很多情况下希望我们的图标是可以缩放的。此时，一个非常重要的技术出现了，额不是出现了，是以前就有，是被从新"宠幸"啦。。 这就是字体图标（iconfont).

### 字体图标优点

```
可以做出跟图片一样可以做的事情,改变透明度、旋转度，等..
但是本质其实是文字，可以很随意的改变颜色、产生阴影、透明效果等等...
本身体积更小，但携带的信息并没有削减。
几乎支持所有的浏览器
移动端设备必备良药...
```

### 字体图标使用流程

总体来说，字体图标按照如下流程：

<img src="./media/fontt.png" />

#### 设计字体图标

假如图标是我们公司单独设计，那就需要第一步了，这个属于UI设计人员的工作， 他们在 illustrator 或 Sketch 这类矢量图形软件里创建 icon图标， 比如下图：

<img src="./media/03.jpg" />

  之后保存为svg格式，然后给我们前端人员就好了。 

  其实第一步，我们不需要关心，只需要给我们这些图标就可以了，如果图标是大众的，网上本来就有的，可以直接跳过第一步，进入第三步。

#### 上传生成字体包

   当UI设计人员给我们svg文件的时候，我们需要转换成我们页面能使用的字体文件， 而且需要生成的是兼容性的适合各个浏览器的。

​    推荐网站： http://icomoon.io

**icomoon字库**

IcoMoon成立于2011年，推出的第一个自定义图标字体生成器，它允许用户选择他们所需要的图标，使它们成一字型。 内容种类繁多，非常全面，唯一的遗憾是国外服务器，打开网速较慢。

   推荐网站： http://www.iconfont.cn/

**阿里icon font字库**

http://www.iconfont.cn/

这个是阿里妈妈M2UX的一个icon font字体图标字库，包含了淘宝图标库和阿里妈妈图标库。可以使用AI制作图标上传生成。 一个字，免费，免费！！

**fontello**

[http://fontello.com/](http://fontello.com/)

在线定制你自己的icon font字体图标字库，也可以直接从GitHub下载整个图标集，该项目也是开源的。

**Font-Awesome**

[http://fortawesome.github.io/Font-Awesome/](http://fortawesome.github.io/Font-Awesome/)

这是我最喜欢的字库之一了，更新比较快。目前已经有369个图标了。

**Glyphicon Halflings**

[http://glyphicons.com/](http://glyphicons.com/)

这个字体图标可以在Bootstrap下免费使用。自带了200多个图标。

**Icons8**

[https://icons8.com/](https://icons8.com/)

提供PNG免费下载，像素大能到500PX

#### 下载兼容字体包

刚才上传完毕， 网站会给我们把UI做的svg图片转换为我们的字体格式， 然后下载下来就好了

当然，我们不需要自己专门的图标，是想网上找几个图标使用，以上2步可以直接省略了， 直接到刚才的网站上找喜欢的下载使用吧。

<img src="./media/fontt1.png" />

<img src="./media/fontt2.png" />

#### 字体引入到HTML

最后一步，是最重要的一步了， 就是字体文件已经有了，我们需要引入到我们页面中。

1. 首先把 以下4个文件放入到 fonts文件夹里面。 通俗的做法

   ![1498032122244](.\media\1498032122244.png)

   ### 第一步：引入项目下面生成的fontclass代码：

   ```html
   <link rel="stylesheet" type="text/CSS" href="./iconfont.CSS">
   ```

   ### 第二步：挑选相应图标并获取类名，应用于页面：

   ```html
   <i class="iconfont icon-xxx"></i>
   ```

## 滑动门

### 滑动门出现的背景

制作网页时，为了美观，常常需要为网页元素设置特殊形状的背景，比如微信导航栏，有凸起和凹下去的感觉，最大的问题是里面的字数不一样多，咋办？

<img src="./media/wxx.jpg" />

为了使各种特殊形状的背景能够自适应元素中文本内容的多少，出现了CSS滑动门技术。它从新的角度构建页面，使各种特殊形状的背景能够自由拉伸滑动，以适应元素内部的文本内容，可用性更强。 最常见于各种导航栏的滑动门。

### 核心技术

核心技术就是利用CSS精灵（主要是背景位置）和盒子padding撑开宽度, 以便能适应不同字数的导航栏。

一般的经典布局都是这样的：

```html
<li>
  <a href="#">
    <span>导航栏内容</span>
  </a>
</li>
```

总结： 

1. a 设置 背景左侧，padding撑开合适宽度。    
2. span 设置背景右侧， padding撑开合适宽度 剩下由文字继续撑开宽度。
3. 之所以a包含span就是因为 整个导航都是可以点击的。


## 伸缩布局(CSS3)

CSS3在布局方面做了非常大的改进，使得我们对块级元素的布局排列变得十分灵活，适应性非常强，其强大的伸缩性，在响应式开中可以发挥极大的作用。

主轴：Flex容器的主轴主要用来配置Flex项目，默认是水平方向

侧轴：与主轴垂直的轴称作侧轴，默认是垂直方向的

方向：默认主轴从左向右，侧轴默认从上到下

主轴和侧轴并不是固定不变的，通过flex-direction可以互换。

![1498441839910](.\media\1498441839910.png)



Flex布局的语法规范经过几年发生了很大的变化，也给Flexbox的使用带来一定的局限性，因为语法规范版本众多，浏览器支持不一致，致使Flexbox布局使用不多

**2、各属性详解******

a、flex-direction调整主轴方向（默认为水平方向）

b、justify-content调整主轴对齐

c、align-items调整侧轴对齐

d、flex-wrap控制是否换行

e、align-content堆栈（由flex-wrap产生的独立行）对齐

f、flex-flow是flex-direction、flex-wrap的简写形式

g、flex子项目在主轴的缩放比例，不指定flex属性，则不参与伸缩分配

h、order控制子项目的排列顺序，正序方式排序，从小到大

此知识点重在理解，要明确找出主轴、侧轴、方向，各属性对应的属性值

## 过渡(CSS3)

过渡（transition)是CSS3中具有颠覆性的特征之一，我们可以在不使用 Flash 动画或 JavaScript 的情况下，当元素从一种样式变换为另一种样式时为元素添加效果。


语法格式:

~~~
transition: 要过渡的属性  花费时间  运动曲线  何时开始;
~~~

| 属性                       | 描述
| -------------------------- | -------------------------------------------- 
| transition                 | 简写属性，用于在一个属性中设置四个过渡属性。
| transition-property        | 规定应用过渡的 CSS 属性的名称。
| transition-duration        | 定义过渡效果花费的时间。默认是 0。
| transition-timing-function | 规定过渡效果的时间曲线。默认是 "ease"。
| transition-delay           | 规定过渡效果何时开始。默认是 0。

运动曲线示意图：

![1498445454760](.\media\1498445454760.png)

~~~css
img {
  width:80px; height: 80px; border:8px solid #ccc; border-radius: 50%;
  transition:transform 0.5s ease-in 0s;
}
img:hover {
  transform:rotate(180deg);
}
~~~

## 2D变形(CSS3)

转换是CSS3中具有颠覆性的特征之一，可以实现元素的位移、旋转、变形、缩放，甚至支持矩阵方式，配合过渡和即将学习的动画知识，可以取代大量之前只能靠Flash才可以实现的效果。

变形转换 transform  

- 移动 translate(x, y) 

![1498443715586](.\media\1498443715586.png)

```css
translate(50px,50px);
```

使用translate方法来将文字或图像在水平方向和垂直方向上分别垂直移动50像素。

可以改变元素的位置，x、y可为负值；

~~~
 translate(x,y)水平方向和垂直方向同时移动（也就是X轴和Y轴同时移动）
 translateX(x)仅水平方向移动（X轴移动）
 translateY(Y)仅垂直方向移动（Y轴移动）
~~~

~~~css
.box {
  width: 499.9999px;
  height: 400px;
  background: pink;
  position: absolute;
  left:50%;
  top:50%;
  transform:translate(-50%,-50%);  /* 走的自己的一半 */
}
~~~

 让定位的盒子水平居中

- 缩放 scale(x, y) 

![1498444645795](.\media\1498444645795.png)

```css
transform:scale(0.8,1);
```

可以对元素进行水平和垂直方向的缩放。该语句使用scale方法使该元素在水平方向上缩小了20%，垂直方向上不缩放。

~~~
scale(X,Y)使元素水平方向和垂直方向同时缩放（也就是X轴和Y轴同时缩放）
scaleX(x)元素仅水平方向缩放（X轴缩放）
scaleY(y)元素仅垂直方向缩放（Y轴缩放）
~~~

 scale()的取值默认的值为1，当值设置为0.01到0.99之间的任何值，作用使一个元素缩小；而任何大于或等于1.01的值，作用是让元素放大

- 旋转 rotate(deg) 

可以对元素进行旋转，正值为顺时针，负值为逆时针；

![1498443651293](.\media\1498443651293.png)

~~~css
transform:rotate(45deg);
~~~

1. 当元素旋转以后，坐标轴也跟着发生的转变
2. 调整顺序可以解决，把旋转放到最后
3. 注意单位是 deg 度数

案例旋转扑克牌

~~~css
body {
  background-color: skyblue;
}
.container {
  width: 100px;
  height: 150px;
  border: 1px solid gray;
  margin: 300px auto;
  position: relative;
}
.container > img {
  display: block;
  width: 100%;
  height: 100%;
  position: absolute;
  transform-origin: top right;
  /* 添加过渡 */
  transition: all 1s;
}
.container:hover img:nth-child(1) {
  transform: rotate(60deg);
}
.container:hover img:nth-child(2) {
  transform: rotate(120deg);
}
.container:hover img:nth-child(3) {
  transform: rotate(180deg);
}
.container:hover img:nth-child(4) {
  transform: rotate(240deg);
}
.container:hover img:nth-child(5) {
  transform: rotate(300deg);
}
.container:hover img:nth-child(6) {
  transform: rotate(360deg);
}
~~~

- 倾斜 skew(deg, deg) 

![1498443827389](.\media\1498443827389.png)

```css
transform:skew(30deg,0deg);
```

该实例通过skew方法把元素水平方向上倾斜30度，处置方向保持不变。

可以使元素按一定的角度进行倾斜，可为负值，第二个参数不写默认为0。

5.transform-origin可以调整元素转换的原点

![1498443912530](.\media\1498443912530.png)

```css
 div{transform-origin: left top;transform: rotate(45deg); }  /* 改变元素原点到左上角，然后进行顺时旋转45度 */    
```

案例：  菱形照片        三角盒子  

## 3D变形

左手坐标系

伸出左手，让拇指和食指成“L”形，大拇指向右，食指向上，中指指向前方。这样我们就建立了一个左手坐标系，拇指、食指和中指分别代表X、Y、Z轴的正方向。如下图

![1498445587576](.\media\1498445587576.png)



CSS3中的3D坐标系与上述的3D坐标系是有一定区别的，相当于其绕着X轴旋转了180度，如下图

![1498459001951](.\media\1498459001951.png)

###  rotateX() 

 就是沿着 x 立体旋转.

~~~css
img {
  transition:all 0.5s ease 0s;
}
img:hove {

  transform:rotateX(180deg);
}
~~~

### rotateY()

沿着y轴进行旋转

~~~css
img {
  transition:all 0.5s ease 0s;
}
img:hove {

  transform:rotateX(180deg);
}
~~~

### rotateZ()

沿着z轴进行旋转

~~~css
img {
  transition:all .25s ease-in 0s;
}
img:hover {
  /* transform:rotateX(180deg); */
  /* transform:rotateY(180deg); */
  /* transform:rotateZ(180deg); */
  /* transform:rotateX(45deg) rotateY(180deg) rotateZ(90deg) skew(0,10deg); */
}
~~~

### 透视(perspective)

电脑显示屏是一个2D平面，图像之所以具有立体感（3D效果），其实只是一种视觉呈现，通过透视可以实现此目的。

透视可以将一个2D平面，在转换的过程当中，呈现3D效果。

注：并非任何情况下需要透视效果，根据开发需要进行设置。

perspective有两种写法

1. 作为一个属性，设置给父元素，作用于所有3D转换的子元素
2. 作为transform属性的一个值，做用于元素自身

理解透视距离原理：

![1498446715314](.\media\1498446715314.png)

###  开门案例

~~~css
body {
}
.door {
  width: 300px;
  height: 300px;
  margin: 100px auto;
  border: 1px solid gray;
  perspective: 1000px;
  background: url('images/dog.gif') no-repeat center/cover;
  position: relative;
}
.door > div {
  box-sizing: border-box;
  border: 1px solid black;
}
.left {
  float: left;
  width: 50%;
  height: 100%;
  background-color: brown;
  transform-origin: left center;
  transition: 1s;
  position: relative;
}
.left::before {
  content: '';
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  top: 50%;
  right: 0px;
  transform: translateY(-10px);
  border: 1px solid whitesmoke;
}
.right {
  width: 50%;
  height: 100%;
  float: left;
  background-color: brown;
  transform-origin: right center;
  transition: 1s;
  position: relative;
}
.right::before {
  content: '';
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  top: 50%;
  left: 0px;
  transform: translateY(-10px);
  border: 1px solid whitesmoke;
}
.door:hover .left {
  transform: rotateY(-130deg);
}
.door:hover .right {
  transform: rotateY(130deg);
}
~~~

### translateX(x)

仅水平方向移动**（X轴移动）

![1498459697576](.\media\1498459697576.png)

主要目的实现移动效果

### translateY(y)

仅垂直方向移动（Y轴移动）

![1498459770252](.\media\1498459770252.png)

### translateZ(z)

transformZ的直观表现形式就是大小变化，实质是XY平面相对于视点的远近变化（说远近就一定会说到离什么参照物远或近，在这里参照物就是perspective属性）。比如设置了perspective为200px;那么transformZ的值越接近200，就是离的越近，看上去也就越大，超过200就看不到了，因为相当于跑到后脑勺去了，我相信你正常情况下，是看不到自己的后脑勺的。

###  3D呈现（transform-style）

设置内嵌的元素在 3D 空间如何呈现，这些子元素必须为转换原素。

flat：所有子元素在 2D 平面呈现

preserve-3d：保留3D空间

3D元素构建是指某个图形是由多个元素构成的，可以给这些元素的父元素设置transform-style: preserve-3d来使其变成一个真正的3D图形。

一般而言，该声明应用在3D变换的兄弟元素们的父元素上。

### 翻转盒子案例(百度钱包)

~~~css
body {
  margin: 0;
  padding: 0;
  background-color: #B3C04C;

}

.wallet {
  width: 300px;
  height: 300px;
  margin: 50px auto;
  position: relative;
  transform-style: preserve-3d;
  transition: all 0.5s;
}

.wallet::before, .wallet::after {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  display: block;
  width: 100%;
  height: 100%;
  background-image: url(./images/bg.png);
  background-repeat: no-repeat;
}

.wallet::before {
  background-position: right top;
  transform: rotateY(180deg);
}

.wallet::after {
  background-position: left top;
  transform: translateZ(2px);
}

.wallet:hover {
  transform: rotateY(180deg);
}
~~~



## 动画(CSS3)

动画是CSS3中具有颠覆性的特征之一，可通过设置多个节点来精确控制一个或一组动画，常用来实现复杂的动画效果。

语法格式：

~~~css
animation:动画名称 动画时间 运动曲线  何时开始  播放次数  是否反方向;
~~~

![1498461096243](.\media\1498461096243.png)

关于几个值，除了名字，动画时间，延时有严格顺序要求其它随意r

~~~css
@keyframes 动画名称 {
  from{ 开始位置 }  0%
  to{  结束  }  100%
}
~~~

~~~
animation-iteration-count:infinite;  无限循环播放
animation-play-state:paused;   暂停动画"
~~~

### 小汽车案例

~~~css
body {
  background: white;
}
img {
  width: 200px;
}
.animation {
  animation-name: goback;
  animation-duration: 5s;
  animation-timing-function: ease;
  animation-iteration-count: infinite;
}
@keyframes goback {
  0%{}
  49%{
    transform: translateX(1000px);
  }
  55%{
    transform: translateX(1000px) rotateY(180deg);
  }
  95%{
    transform: translateX(0) rotateY(180deg);
  }
  100%{
    transform: translateX(0) rotateY(0deg);
  }
}
~~~

## BFC(块级格式化上下文)

BFC(Block formatting context)

直译为"块级格式化上下文"。

### 元素的显示模式

我们前面讲过 元素的显示模式 display。 

分为 块级元素   行内元素  行内块元素 ，其实，它还有很多其他显示模式。

<img src="./media/dis.png"  style="border: 1px dashed #ccc; padding: 5px;" />

### 那些元素会具有BFC的条件

不是所有的元素模式都能产生BFC，w3c 规范： 

display 属性为 block, list-item, table 的元素，会产生BFC.

大家有么有发现这个三个都是用来布局最为合理的元素，因为他们就是用来可视化布局。

注意其他的，display属性，比如 line 等等，他们创建的是 IFC ，我们暂且不研究。

这个BFC 有着具体的布局特性： 

<img src="./media/box.gif" />

有宽度和高度 ， 有 外边距margin  有内边距padding 有边框 border。


### 什么情况下可以让元素产生BFC

-float属性不为none

-position为absolute或fixed

-display为inline-block, table-cell, table-caption, flex, inline-flex

-overflow不为visible。

### BFC元素所具有的特性

BFC布局规则特性：

1.在BFC中，盒子从顶端开始垂直地一个接一个地排列.

2.盒子垂直方向的距离由margin决定。属于同一个BFC的两个相邻盒子的margin会发生重叠

3.在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）。

1. BFC的区域不会与浮动盒子产生交集，而是紧贴浮动边缘。
2. 计算BFC的高度时，自然也会检测浮动或者定位的盒子高度。

它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

白话文： 孩子在家里愿意怎么折腾都行，但是出了家门口，你就的乖乖的，不能影响外面的任何人。

### BFC的主要用途

BFC能用来做什么？

(1) 清除元素内部浮动

只要把父元素设为BFC就可以清理子元素的浮动了，最常见的用法就是在父元素上设置overflow: hidden样式，对于IE6加上zoom:1就可以了。

主要用到 

```
计算BFC的高度时，自然也会检测浮动或者定位的盒子高度。
```

<img src="./media/fu.jpg" />

(2) 解决外边距合并问题

外边距合并的问题。

主要用到 

```
盒子垂直方向的距离由margin决定。属于同一个BFC的两个相邻盒子的margin会发生重叠
```

属于同一个BFC的两个相邻盒子的margin会发生重叠，那么我们创建不属于同一个BFC，就不会发生margin重叠了。

<img src="./media/ma.png" />

(3) 制作右侧自适应的盒子问题

主要用到 

```
普通流体元素BFC后，为了和浮动元素不产生任何交集，顺着浮动边缘形成自己的封闭上下文
```

<img src="./media/you.png" />

### BFC 总结

BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。包括浮动，和外边距合并等等，因此，有了这个特性，我们布局的时候就不会出现意外情况了。

## 优雅降级和渐进增强

什么是渐进增强（progressive enhancement）、优雅降级（graceful degradation）呢？

渐进增强 progressive enhancement：

针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。

 类似 爬山，由低出往高处爬

  <b>优雅降级 graceful degradation：</b>

一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

类似蹦极，由高处往低处下落

　　区别：渐进增强是向上兼容，优雅降级是向下兼容。

个人建议： 现在互联网发展很快， 连微软公司都抛弃了ie浏览器，转而支持 edge这样的高版本浏览器，我们很多情况下没有必要再时刻想着低版本浏览器了，而是一开始就构建完整的效果，根据实际情况，修补低版本浏览器问题。

## 浏览器前缀

| 浏览器前缀 | 浏览器                                 |
| ---------- | -------------------------------------- |
| -webkit-   | Google Chrome, Safari, Android Browser |
| -moz-      | Firefox                                |
| -o-        | Opera                                  |
| -ms-       | Internet Explorer, Edge                |
| -khtml-    | Konqueror                              |

后面我们会有 常用的解决H5和C3 的兼容解决文件， 我们这里暂且不涉及。

## 背景渐变

在线性渐变过程中，颜色沿着一条直线过渡：从左侧到右侧、从右侧到左侧、从顶部到底部、从底部到顶部或着沿任何任意轴。如果你曾使用过制作图件，比如说Photoshop，你对线性渐变并不会陌生。

兼容性问题很严重，我们这里之讲解线性渐变

语法格式： 

~~~css
background:-webkit-linear-gradient(渐变的起始位置， 起始颜色， 结束颜色)；
~~~

~~~css
background:-webkit-linear-gradient(渐变的起始位置， 颜色 位置， 颜色位置....)；
~~~

## CSS W3C 统一验证工具

CssStats 是一个在线的 CSS 代码分析工具

```
网址是：  http://www.cssstats.com/
```

如果你想要更全面的，这个神奇，你值得拥有：

W3C 统一验证工具：    http://validator.w3.org/unicorn/  ☆☆☆☆☆

因为它可以检测本地文件哦！！

## CSS 压缩

通过上面的检测没有错误，为了提高加载速度和节约空间（相对来说，css量很少的情况下，几乎没啥区别），可以通过css压缩工具把css进行压缩。

 w3c css压缩   http://tool.chinaz.com/Tools/CssFormat.aspx   网速比较慢

 还可以去站长之家进行快速压缩。

 http://tool.chinaz.com/Tools/CssFormat.aspx  