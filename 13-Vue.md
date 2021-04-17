## 0. vue的安装

- 直接CDN引入

  ```html
  <!--开发版本-->
  <script src="http://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <!--生产版本-->
  <script src="http://cdn.jsdelivr.net/npm/vue"></script>
  ```

- 下载引入

  ```SHELL
  http://vuejs.org
  ```

- **NPM**

  ```shell
  npm install vue
  ```

  

## 1. vue的基本使用

### 1.1 hello world

1. 提供标签用域填充数据
2. 引入vue.js
3. 使用vue的语法做功能
4. 把vue提供的数据填充到标签里

```html
<body>
	<div id="app">
		<div>{{msg + '---' +123}}</div>
        <!-->插值表达式，将数据填充到HTML标签中，支持简单的计算<-->
	</div>
	<script src="vue.js"></script>
	<script>
		var vm = new Vue({
			el:'#app',
            // 元素的挂载位置（值可以是css选择器或者DOM元素）
			data:{
                // 模型数据 是一个对象
				msg:'hello vue'
			}
		})
	</script>
</body>
```

### 1.2 模板语法

#### 1. 差值表达式



#### 2. 指令（自定义属性 v-）

```js
var vm = new Vue({
    el:'#app',
    data:{
        msg:'hello vue',
        msg1:'<h1>hello vue</h1>',
    }
})
======================================
v-cloak
// 解决页面闪动
[v-cloak]{
	display: none;
}
<div v-cloak>{{msg}}</div>
-------------------------------------
v-text
// 填充纯文本,不灵活，一般不用
<div v-text="msg"></div>
-------------------------------------
v-html
// 填充HTML片段
	// 存在安全问题
	// 内部数据可用，跨域数据不可用
<div v-html="msg1"></div>
-------------------------------------
v-pre
// 填充原始信息,跳过编译过程
<div v-pre>{{msg}}</div>
// 显示{{msg}}
--------------------------------------
v-once
// 显示的信息后期不需要修改,可以添加v-once,调高性能
// 数据响应式:数据改变会导致页面内容跟着改变
<div v-once>{{msg}}</div>
---------------------------------------
v-model
// 双向数据绑定(数据绑定,DOM监听)
<div>{{ msg }}</div>
<input type="text" v-model='msg'>

```
```html
/*  v-model相当于两个方法的综合
<input type="text" :value="msg" :input="valueChange">
<h2>{{msg}}</h2>
<script>
    data: {msg: 'hello'},
    methods:{
        valueChange(event){
            this.msg = event.target.value
        }
    }
</script>
*/
```



#### 3. 事件绑定v-on

```shell
var vm = new Vue({
    el:'#app',
    data:{
        num : 1
    },
    methods:{
    	handle1: function(event) {
    		console.log(event.target.innerHTML)
    		this.num++
    	},
    	handle2: function(p,event) {
    		console.log(event.target.innerHTML)
    		console.log(p)
    		this.num++
    	}
    }
})

v-on
<button id="app" v-on:click='num++'>{{ num }}</button>
# 简写
<button id="app" @click='num++'>{{ num }}</button>

<button id="app" @click='handle'>{{ num }}</button>
# 如果事件直接绑定函数名称，那么默认会传递事件对象作为事件函数的第一个参数
<button id="app" @click='handle2(p, $event)'>{{ num }}</button>
# 如果事件绑定函数调用，那么事件对象必须作为最后一个参数显示传递，并且事件对象的名称必须是$event
======================================================================
事件修饰符
<a @click.stop='handle'>跳转</button>
# 阻止冒泡
<a @click.prevent='handle'>跳转</button>
# 阻止默认行为
<a @click.self='handle'>跳转</button>
# 只当event.target是当前元素自身的时候触发处理函数(解决冒泡)
<a @click.self.prevent='handle'>阻止所有点击</button>
<a @click.prevent.self='handle'>阻止对元素自身的点击</button>
# 事件修饰符可以串联，有顺序之分
<son @click.native="comClick"></son>
# 监听组件根元素的原生事件
	原生js阻止冒泡和跳转的方式
	handle1:function(event){
         event.stopPropagation()；
	}
	handle2:function(event){
        event.preventDefault()；
	}
========================================================================
按键修饰符
var vm = new Vue({
    el: '#app',
    data: {
        uname: ''
        pwd: ''
    },
    methods: {
        handleSubmit: function() {
            console.log(this.pwd)
        },
        clearContent:function() {
        	this.pwd = ''
        }
    }
})
<div id="app">
用户名:<input type="text" @keyup.delete="clearContent" v-model="uname">
# delete清空
密码:<input type="text" @keyup.enter="handleSubmit" v-model="pwd">
# enter打印密码

# 自定义按键修饰符
	Vue.config.keyCOdes.f1= 112
	# 全局config.keyCodes对象(写在创建Vue实例前面,f1是自定义的,后面的值必须是keyCode)
	@keyup.65="fn"
	# 自定义按键 65 对应 a
</div>
```

#### 4. 属性绑定

```vue
var vm = new Vue({
el: '#app',
data: {
	url:'http://cn.bing.com'
}
)}
v-bind指令
<a v-bind:href='url'>必应</a>
// 简写
<a :href='url'>必应</a>
-----------------------------------
<h2 :class:"{类1: Boolean, 类2: Boolean}">{{msg}}</h2>
<h2 :class:"['类1', '类2']">{{msg}}</h2>
<h2 :class:"[变量1, 变量2]">{{msg}}</h2>
// 可以通过修改布尔值来修改元素的类
<h2 :style:"{属性名: 属性值, fontSize: '40px'}">{{msg}}</h2>
// 要加''，不然会解析为变量
// 驼峰方式不用加''，'font-size'
------------------------------------
v-model的底层实现,双向绑定
<input v-model='msg'>
<input v-bind:value='msg' v-on:input='msg=$event.target.value'>
-----------------------------------------
```



#### 5. 样式绑定

1. class样式处理

```html
- 对象语法
var vm = new Vue({
    el: '#app',
    data: {
        isActive: true,
		objClasses:{'active1': true,'active2': true}
    },
    methods: {
        handle: function(){
            this.isActive = !this.isActive;
            this.isError = !this.isError;
			this.objClasses.active1 = false
        }
    }
})

<div class="base" v-bind:class="{ active: isActive }">必应</div>
默认的class会保留
{active: isActive,error: isError} # 多个类用,隔开

<button @click="handle">切换</button>
==================================================================
- 数组语法
var vm = new Vue({
    el: '#app',
    data: {
        activeClass: 'active',
        errorClass: 'error',
		arrClasses:['active1','active2','active3']
    },
    methods: {
        handle: function(){
            this.activeClass = '';
        }
    }
})

<div v-bind:class="[activeClass, errorClass]">必应</div>
<button @click="handle">切换</button>
<div v-bind:class="[arrClasses]">必应</div>
```

2. style样式处理

```js
var vm = new Vue({
    el: '#app',
    data: {
        borderStyle: '1px solid blue',
        widthStyle: '100px',
        heightStyle: '200px',
        objStyle: {
            border: '1px solid blue',
            width: '100px',
            height: '200px',
        },
        overridingStyles:{
            border: '2px solid blue'
        }
    },
    methods: {
        handle: {
            this.heightStyle: '100px',
            this.objStyle.width = '200px'
        }
    }

})
```
```html
- 对象语法
<div v-bind:style:'{border: borderStyle, width: widthStyle,height: heightStyle}'>test</div>
// 可以写在对象里面
<button @click="handle">切换</button>

<div v-bind:style:'objStyle'>test</div>
// 也可以直接引用对象
- 数组语法
<div v-bind:style:'[objStyle, overridingStyles]'>test</div>
// 引用overridingStyles会覆盖之前同类,没有的添加
```



#### 6. 分支循环结构

1. 分支结构

- v-if
- v-else
- v-else-if
- v-show
```js
var vm = new Vue({
    el: '#app',
    data: {
        score: 99,
        flag: false
    },
    methods: {
        handle: {
            this.flag = !this.flag
        }
    }
})

<div v-if='score>=90'>优秀</div>
<div v-else-if='score<90&&score>=80'>良好</div>
<div v-else-if='score<80&&score>60'>一般</div>
<div v-else>不及格</div>
// 只会出现一个div

<div v-show='flag'>测试v-show</div>
<button @click="handle">切换</button>
 // v-show的原理: display: none

/*
v-if和v-show的区别:
	v-if控制元素是否渲染到元素
	v-show控制元素是否显示
*/
---------------------------------------
- diff带来的复用问题解决方案
 添加key属性作为一个标识，key不同不复用
```

2. 循环结构

```js
var vm = new Vue({
    el: '#app',
    data: {
        fruits: ['apple','pair','peach'],
        
    }
})
- V-for遍历数组
<ul>
	<1i v-for='item in fruits">{{item}}</1i>  // in 关键字
</ul>
<1i v-for='(item,index) in list'>{{item}}+'---'+{{index}}</1i>
// item,index都是可以自定义名称的
// 如果数组内元素是对象,可以通过.属性的方式

<1i :key='item.id' v-for='(item,index)in list'>{{item}}+'---'{{index}}</1i>

- V-for遍历对象
<div v-for='(value, key, index) in object"></div>

- v-if 和 v-for结合使用
<div v-if='value==12' v-for='(value,key,index) in object'>{{value + '--' + key + '--' + index }}</div>
// value,key,index都是可以自定义名称的
// 只会渲染满足value==12的那个数据


```

- key的作用：

  ​	帮助Vue区分不同的元素，从而提高性能

  ​	绑定的value要唯一

  ​	**key不要无脑绑index**

![](2020-07-14-14-35-14.png)



#### 7.数组中哪些方法是响应式的

```js
push()
// 后面添加，多个参数用逗号隔开
pop()
// 后面删除
shift()
// 前面删除
unshift()
// 前面添加，多个参数用逗号隔开
splice()
// 增删改，第一个参数start index 第二个参数 to 第三个参数 
/* 
删除 splice(1,1) 从1位置开始删除1个
	splice(1) 从1开始删除到最后一个
替换 splice(1,2,'m','n') 从1位置开始后面2个元素替换为'm'和'n'
插入 splice(1,0,'x','y') 从1位置开始后面插入2个元素'x'和'y'
	*/
sort()
// 排序
reverse()
// 反转


***Vue.set方法***
set(要修改的对象，索引，修改后的值)
Vue.set(this.arr,0,'xyz')
======================================
通过索引改变数组元素不是响应式的
```



### 1.3 切换水果案例

> 声明式编程
>
> - 模板的结构和最终显示的效果基本一致

```css
.active {
    background-color: orange;
}

.tab .current {
    display: block;
}

.tab div {
    display: none;
    /*默认图片全部隐藏,显示谁改谁的class名为current*/
}
```

```html
<div id="app">
    <div class="tab">
        <ul>
            <li @click='change(index)' :class='currentIndex==index?"active":""' :key='item.id' v-for='(item, index) in list'>{{item.id}}:{{item.title}}</li>
        </ul>
        <div :class='currentIndex==index?"current":""' :key='item.id' v-for='(item, index) in list'>
            <img :src='item.path'><!-- 属性绑定要加: -->
        </div>
    </div>
</div>
```

```js
var vm = new Vue({
    el: '#app',
    data: {
        currentIndex: 0,
        // 选项卡当前的索引
        list: [{
            id: 1,
            title: 'apple',
            path: './img/apple.jpg'
        }, {
            id: 2,
            title: 'banana',
            path: './img/banana.jpg'
        }, {
            id: 3,
            title: 'peach',
          	path: './img/peach.jpg'
        }]
    },
    methods: {
        change(index) {
            this.currentIndex = index;
        }
    }
})
```

## 2. Vue常用特性

### 2.1 表单操作

#### 基于Vue的表单操作

- input 单行文本

  ```html
  <input type="text" v-model='uname'>
  data: {
  	uname: 'jack'
  }
  双向绑定
  ```

- submit

  ```html
  <input type="sublime" @click.prevent='handle'>
  methods: {
  	handle: function() {
  		// ajax
      }
  }
  双向绑定
  ```

  

- textarea 多行文本

  ```html
  <textarea v-model="text"></textarea>
  // Vue里面 内容不能写在标签里面
  data: {
  	text: "hello world"
  }
  ```

  

- select 下拉多选

  ```html
  - 单选
  <select  v-model="hobby">
      <option value="1">唱</option>
      <option value="2">跳</option>
      <option value="3">rap</option>
  </select>
  
  data: {
  	hobby: 1
  }
  =============================================
  - 多选(shift)
  <select  v-model="hobby" multiple="true">
    data: {
  	hobby: ['2','4']
  }  
  ```

  

- radio 单选

  ```html
  <input type="radio" id='male' value="1" v-model='gender'>
  <label for='male'>男</label>
  <input type="radio" id='female' value="2" v-model='gender'>
  <label for='female'>女</label>
  data: {
  	gender: 1
  // 默认选中男
  }
  ```

  

- checkbox 多选

  ```html
  <input type="checkbox" id="sing" value="1" v-model="hobby">
  <label for="sing">唱</label>
  <input type="checkbox" id="dance" value="2" v-model="hobby">
  <label for="dance">跳</label>
  <input type="checkbox" id="rap" value="3" v-model="hobby">
  <label for="rap">rap</label>
  <input type="checkbox" id="ball" value="4" v-model="hobby">
  <label for="ball">篮球</label>
  data: {
  	hobby: ['2','4']
  }
  ```

- 表单域修饰符

  - number:转化为数值
  - trim:去掉开始和结尾的空格
  - lazy: 将input事件切换为change事件

  ```html
  <input type="text" v-model.number='age'>
  // 转换为数值
  <input type="text" v-model.trim='info'>
  // 中间的空格去不掉
  <input type="text" v-model.lazy='msg'>
  // 不加msg只要改变div内容就变 加了lazy失去焦点才变
  <div>{{msg}}</div>
  data: {
  	age: '',
  	info: '',
  	msg:''
  }
  ```

  

### 2.2 自定义指令

```js
不带参数自定义指令(获取元素焦点)
- 声明
Vue.directive('focus', {
              // 第一个参数,指令的名称
	inserted: function(el) {
        // el表示指令绑定的元素
        el.focus();
	}
})
- 调用
<input type="text" v-focus>
-------------------------------
带参数的自定义指令
Vue.directive('color', {
	inserted: function(el, binding) {
        el.style.backgroundColor = binding.value.color;
        /* binding是一个对象 
        binding.name => 'color'
        binding.value => msg对象
        binding.value.color => 'red'
        */
	}
})

data:{
    msg: {
        color: 'red'
    }
}

<input type="text" v-color='msg'>
<input type="text" v-color='{color:"orange"}'>
---------------------------------
局部指令
var vm = new Vue({
    el: '#app',
    data: {},
    methods: {},
    directives: {
        focus: {
            inserted: function(el) {
                el.focus()
            }
        }
    }
})
====================================
钩子函数
- inserted

- bind

```



### 2.3 计算属性

> 表达式的逻辑可能比较复杂，使用计算属性可以使模板内容更加简洁

```js
data： {
	msg： 'hello'
},
computed: {
	reverseStr: function() {
		return this.msg.split('').reverse().join('')
	}
}
- 反转字符串
<div>{{msg.split('').reverse().join('')}}</div>
// 结构复杂
<div>{{reverseStr}}</div>
// 不需要加()
========================================================
computed和methods的区别：
- 计算属性是基于他们的依赖（data）进行缓存的
	// 比较耗时的计算可节省时间
	// 多次调用性能更高
- 方法不存在缓存
- 一般起名methods带有动词（get）

=======================================================
计算属性的getter和setter（computed的完全格式）
data： {
	msg： 'hello'
},
computed: {
	reverseStr: {
        // 本质是一个属性，所以调用不需要（）
		set: function() {
			//计算属性一般不需要设置set方法，只读属性，可以直接删除简写为上面的形式
		},
		get: function() {
			return this.msg.split('').reverse().join('')
        }
	}
}
```



### 2.4 过滤器

> 过滤器的作用: 格式化数据

- 不带参数的过滤器

```html
- 定义
Vue.filter('过滤器名称', function() {
    // 过滤器业务逻辑
}
           
-使用
<div> {{msg | upper}</div>
<div> {{msg | upper | lower}</div>
<div v-bind:id='id | formatId'></div> 
----------------------------------------------
例子: 首字母大写
<input type="text" v-model='msg'><span>{{msg | upper}}</span>
<div :abc=' msg | upper'></div>
Vue.filter('upper', function(val) {
	return val.charAt(0).toUpperCase() + val.slice(1);
})
// 写在创建Vue实例前面

```

- 带参数的过滤器

```js
Vue.filter('过滤器名称', function(value, arg1,arg2) {
    // value就是过滤器传递过来的参数
}
------------------------------------------
例子: 格式化日期
Vue.filter('format',function(value, arg) {
    // 接收参数从第二个开始接收
    if(arg == 'yyyy-MM-dd'){
        var ret = ''
        ret += value.getFullYear() + '-' + 'value.getMonth() + 1' + '' + value.getDate();
    }
	return ret
    // 第一个参数:创建的日期实例date
    // 第二个参数:参数字符串'yyyy-MM-dd'
})
data: {
	date: new Date()
}

<div>
    {{date | format('yyyy-MM-dd')}}
</div>

```

- 局部过滤器

  ```js
  // 只能在本组件内使用
  var vm = new Vue({
      el: '#app',
  	filters: {
          capitalize: function() {}
      }
  })
  ```

  

### 2.5 侦听器

> 应用场景: 数据变化执行异步操作或开销大的操作
>
> 一旦数据变化就会触发侦听器绑定的方法

```js
data:{
    firstName: 'jim',
    lastName: 'green',
    fullName: 'jim green'
},
watch:{
    firstName: function(val) {
        // firstName名字要与要监听的对象一致
        this.fullName = val + '' +lastName;
    },
    lastName: function(val) {
        this.fullName = firstName + '' +val;
    }
}
===============================================
判断用户名是否可用
<input type="text" v-model.lazy='uname'><span>{{tip}}</span>
data: {
    uname:'jack',
    tip:''
},
methods: {
    checkName : function(uname) {
        var that = this
        // 调用接口,使用定时器模拟
        setTimeout(function() {
            if(uname == 'admin') {
                that.tip = 'uname has already exist'
            }else {
                that.tip = 'uname can be use'
            }
        },1000)
    }
},
watch: {
    uname: function(val) {
        // 调用后台接口
        this.checkName(val)
        this.tip = 'checking'
    }
}
```



### 2.6 生命周期

#### 主要阶段

- 挂载
  - beforeCreate
  - created
- 挂载
  - beforeMount
  - **mounted**
- 更新
  - beforeUpdate
  - updated
- 销毁
  - beforeDestroy
  - destroyed

####  Vue实例的产生过程

> beforeCreate在实例初始化之后，数据观测和事件配置之前被调用。
>
> created在实例创建完成后被立即调用。
>
> beforeMount在挂载开始之前被调用。
>
> mounted el被新的建的vm.$el替换，并挂载到实例上去之后调用该钩子。
>
> beforeUpdate数据更新时调用，发生在虚拟DOM打补丁之前。
>
> updated由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。
>
> beforeDestroy实例销毁之前调用。
>
> destroyed实例销毁后调用。**

## 3. 综合案例 图书管理

### 数组相关API

- 变异方法(修改原有数据)
  - push()  pop()  shift()  unshift()
  - splice()
  - sort()
  - reverse()
- 替换数组(不会改变原数组,生成新的数组)
  - filter()
  - concat()
  - slice()
- 修改响应式数据(数组或者对象)
  - Vue.set(vm.items,indexOfItem,newValue)
  - vm.$set(vm.items,indexOfItem,newValue)
    - 参数1: 要处理的数组名称(对象名称)
    - 参数2: 要处理的数组的索引(对象的键名)
    - 参数3: 要处理的数组的值(属性的值)

### 3.1 基础CRUD

```html
    <div id="app">
        <!--操作框部分 start-->
        <div id="add">
            No.:<input type="text" v-model.number='id' :disabled="flag">
            <!--添加disabled属性,让修改的时候不能修改id-->
            Name:<input type="text" v-model='name'>
            <input type="submit" value="add" @click.prevent='handle'>
        </div>
        <!--表格部分 start-->
        <table>
            <thead>
                ...
            </thead>
            <tbody>
                <tr :key='item.id' v-for='item in books'>
                    <td>{{item.id}}</td>
                    <td>{{item.name}}</td>
                    <td>{{item.date | format('yyyy-MM-dd hh:mm:ss')}}</td>
                    <td>
                        <a href="" @click.prevent='change(item.id)'>修改</a> 
                        <span>|</span> 
                        <a href="" @click.prevent='del(item.id)'>删除</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
```

```js
var vm = new Vue({
    el: '#app',
    data: {
        submitFlag: false,
        flag: false,
        id: '',
        name: '',
        books: []
    },
    methods: {
        handle: function() {
            // 根据flag判断 选择不同的处理方式
            if (this.flag) {
                // 编辑
                this.books.some((item) => {
                    if (item.id == this.id) {
                        item.name = this.name
                        return true
                    }
                })
                this.flag = false
            } else {
                // 添加
                var book = {}
                book.id = this.id
                book.name = this.name
                book.date = ''
                this.books.push(book)
            }
            // 清空表单
            this.id = ''
            this.name = ''
        },
        change: function(id) {
            this.flag = true
            var book = this.books.filter(function(item) {
                return item.id == id
            })
            // book是一个数组,第一项是一个对象,包含id name等
            this.id = book[0].id
            this.name = book[0].name
        },
        del: function(id) {
            // 方法1
            // var index = this.books.findIndex(function(item){
            // 	return item.id == id
            // })
            // this.books.splice(index,1)
            // 方法2
            this.books = this.books.filter(function(item) {
                return item.id != id
            })
        }
    }
})
```



### 3.2 常用特性应用场景

- 3.2.1 过滤器(格式化日期)

  ```html
  <td>{{item.date | format('yyyy-MM-dd hh:mm:ss')}}</td>
  ```

  ```js
  Vue.filter('format', function(value, arg) {
      ...
  })
  ```

- 3.2.2 自定义指令(获取表单焦点)

  ```html
  No.:<input type="text"  v-focus>
  ```

  ```js
  Vue.directive('focus', {
      inserted: function(el) {
          el.focus();
      }
  })
  ```

  

- 3.2.3 计算属性(统计图书数量)

  ```html
  <div class="total">
      <span>total:</span><span>{{total}}</span>
  </div>
  ```

  ```js
  computed: {
      total: function() {
      	return this.books.length
      }
  }
  ```

  

- 3.2.4 侦听器(验证图书存在性)

  ```html
  <input type="submit" value="add" :disabled='submitFlag'>
  ```

  ```js
  data: {
      submitFlag: false
  },
  watch: {
      name: function(val) {
          var ret = this.books.some(function(item) {
              return item.name == val
          })
          if (ret) {
              this.submitFlag = true
          } else {
              this.submitFlag = false
          }
      }
  }
  ```

  

- 3.2.5 生命周期(图书数据处理)

  ```js
  data:{
      books:[]
  },
  mounted: function() {
  	// 一般此时用于获取后台数据,把数据填充到模板
  	var data = [{
          id: 1,
          name: 'aaa',
          date: new Date()
      }, {
          id: 2,
          name: 'bbb',
          date: new Date()
      }, {
          id: 3,
          name: 'ccc',
          date: new Date()
      }]
      this.books = data
  }
  ```
  
  

## 4. 组件化开发

> 将一个复杂的问题转换为多个简单的问题

### 4.1 组件注册

#### 全局组件（语法糖）

```js
Vue.component(组件名称,{
    /*4. 组件命名:
    驼峰式: 用在字符串模板里
        	<HelloWorld></HelloWorld>
    短横线式: 普通的标签模板
    	<button-counter></button-counter>
    	
    如果使用驼峰式命名组件，在使用组件的时候，只能在字符串模板中用驼峰的方式使用组件，但是在普通的标签模板中，必须使用短横线的方式使用组件
    */ 
    data: 组件数据,
    // 1. data必须是一个函数
    template: 组件模板内容
})
/*相当于
const cpn = Vue.extend({...})
Vue.component('cpn1',cpn)
*/
----------------------
例: 按钮显示点击了多少次
Vue.component('button-counter',{
    data: function(){
        return {
            count: 0
        }
    },
    template: '<button @click="handle">点击了{{count}}次</button>',
    // 2. 组件模板内容必须是单个根元素,里面可以包含多个子元素
    /* 3. 组件模板内容可以是模板字符串
     template:`
     <div>
     	<button @click="handle">点击了{{count}}次</button>
     	<button>test<button>
     </div>
     `,
    */
    methods: {
        handle: function(){this.count++}
    }
})

调用
<div id="app">
	<button-counter></button-counter>
	<button-counter></button-counter>
// 如果调用多个,他们之间的数据互不影响(闭包)
</div>


```

#### 局部组件

> 只能在注册他的父组件中使用

```
var ComponentA = {
	data:function(){return{msg:'hello world'}}
}
var ComponentB = {...}
var ComponentC = {...}

var vm = new Vue({
    el: '#app',
    components:{
    	'component-a':ComponentA,
    	'component-b':ComponentB,
    	'component-c':ComponentC
    }
})
```

#### * 模板的分离写法

```html
<script type="text/x-template" id="cpn">
	<div>
		<h2>我是标题</h2>
		<p>我是内容</p>
	</div>
</script>
----------------------
或者
<template id="cpn">
	<div>
		<h2>我是标题</h2>
		<p>我是内容</p>
	</div>
</template>
=====================================
<script type="text/javascript">
	Vue.component('cpn', {
		template: '#cpn'
	})
	const app = new Vue({
		el: '#cpn',
		data:{
			msg: 'hello'
		}
	})
</script>
```



### 4.2 组件间数据交互

> 组件内部不能直接访问实例数据

#### 4.2.0 data属性为什么必须是函数

> 组件内部的data属性必须是一个函数
> 函数在多次调用的时候返回的对象不是同一个对象，数据分离
> 如果data仅仅是一个对象，那么会造成数据调用混乱问题

#### 4.2.1 父组件向子组件传值（props）


```html
<body>
<!-- 父组件模板 -->
	<div id="app">
		<cpn :c-msg='msg' />
        <!-- 父组件通过属性将值传递给子组件 （驼峰命名）-->
	</div>
    <!-- 子组件模板 -->
	<template id="cpn">
    <!-- 组件模板必须包含在一个根标签里面 -->
		<div>
			<h2>{{cMsg}}</h2>
            <!-- props传递数据原则: 单向数据流 -->
            <!-- 如果要在子组件修改展示的父组件的数据 要通过这种方式 -->
            <h2>{{dataChildMsg}}</h2>
            <input type='text' v-model='dataChildMsg' />
		</div>
	</template>

	<script type="text/javascript">
		const app = new Vue({
			el: '#app',
			data: {
				msg: '父组件中的信息'
			},
			components:{
                // 定义并引入子组件
				cpn:{
					template: '#cpn',
					props:{
						cMsg: String
					},
                    // 子组件内部通过props接收传递过来的值
                    data() {
                        return {
                            dataChildMsg: this.cMsg
                        }
                    }
				}
			}
		})
	</script>
</body>
```
- props属性名规则
	+ 在props中使用驼峰形式，模板中需要使用短横线的式(HTML不区分大小写)
	+ 字符串形式的模板中没有这个限制


- props里面可以写数组和对象 
  ```js
    props： {
        // 同时指定类型（字符串、数字、布尔值、数组、对象）
        movie: Array,
        //也可以是自定义类型
        person: Person,
        // 可以同时指定默认值
        
        msg：{
            type: String,
            default: 'aaa',
            // 使用required属性设置是否必须传值
            required: true
        },

        // 类型是属性或者数组的时候，默认值必须是一个函数
        info: {
            type: Array,
            // 直接写default: []可能会在某些版本报错
            default() {
                return []
            }
        }
        
    }
  ```


#### 4.2.2 子组件向父组件传值（$emit）

```html
<!-- 子组件 -->

//1.子组件通过自定义事件向父组件传递信息（不带参数）
<body>
    <!-- 父组件模板 -->
	<div id="app">
        <!-- 父组件监听自定义事件 并执行父组件的handle方法 -->
		<cpn @item-click='handle'></cpn>
	</div>
    <!-- 子组件模板 -->
	<template id="cpn">
		<div>
			<button v-for="item in sort" @click='btnClick(item)'>
				{{item.name}}
			</button>
		</div>
	</template>
	<script type="text/javascript" src="./vue.js"></script>
	<script type="text/javascript">
        // 父组件
		const app = new Vue({
			el: '#app',
			methods:{
				handle(item) {
					console.log(item.id)
				}
			},
			components:{
                // 子组件
				cpn:{
					template: '#cpn',
					data() {
						return {
							sort:[{id:0,name:'电脑'},{id:1,name:'手机'}]
						}
					},
					methods: {
						btnClick(item) {
                            // 子组件通过$emit发出事件
							this.$emit('item-click',item)
						}
					}
				}
			}
		})
	</script>
</body>
```



#### 4.2.3 非父子组件间传值(兄弟)

```js
1.单独的事件中心管理组件间的通信
组件A触发事件中心

var eventHub=new Vue()
// 创建事件中心

2.监听事件与销毁事件

eventHub.$on('add-todo',addTodo)
// 监听事件 第一个参数事件名称  第二个参数自定义函数

eventHub.$off('add-todo')
// 事件销毁

3.触发事件

eventHub.$emit('add-todo',id)
// 事件名称要和监听的保持一致，可以传参


```

- 例

```html
<div id="app">
    <div>父组件</div>
    <div>
		<button @click='handle'>销毁事件</button>
	</div>
    <test-first></test-first>
    <test-second></test-second>    
</div>
```

```js
//定义事件中心
var hub = new Vue()

Vue.component('test-first', {
    data: function() {return {num:0}},
    template: `
        <div>
            <div>1st:{{num}}</div>
            <div>
                <button @click='handle'>点击</button>
            </div>
        </div>
     `,
    methods: {
    	handle: function() {
    		//触发兄弟组件的事件
            hub.$emit('2nd-event', 1)
		}
	},
    mounted: function() {
        // 监听事件 
        hub.$on('1st-event',(val)=>{ 
            this.num += val
        })
    }
})
Vue.component('test-second', {
    data: function() {return {num:0}},
    template: `
        <div>
            <div>2nd:{{num}}</div>
            <div>
                <button @click='handle'>点击</button>
            </div>
        </div>
     `,
    methods: {
    	handle: function() {
    		hub.$emit('1st-event', 2)
		}
	},
    mounted: function() {
        // 监听事件 
        hub.$on('2nd-event',(val)=>{ 
            this.num += val
        })
    }
})
var vm = new Vue({
    el:'#app',
    methods: {
        handle:function() {
            hub.$off('1st-event')
            hub.$off('2nd-event')
        }
    }
})
```


#### 4.2.4 父子组件的访问方式:
 > 父组件访问子组件: $children 或者 $refs(引用)
 > 子组件访问父组件: $parent
 > 访问根组件: $root

 ```html
    //$children是一个数组 包含这个组件的所有子组件 
    //$parent 是一个对象 
    // 一般不用$parent和$children方式 
    - $refs
    <div id="app">
    <!-- 父组件调用的时候定义ref属性 -->
        <cpn ref='aaa'></cpn>
    </div>


    ref 绑定在组件中，通过`this.$refs.refname`获取到的是一个组件对象

    ref 绑定在普通元素上，通过`this.$refs.refname`获取到的是一个元素对象
    // $ref默认是一空对象
    就可以通过this.$refs.aaa获取到这个对象

     
 ```

### 4.3 组件插槽

> 父组件向子组件传递内容（模板）

#### 4.3.1 插槽的基本用法

```js
- 1.插槽位置<slot></slot>
vue.component('alert-box',{
template:`
<div class="demo-alert-box">
	<strong>Error!</strong>
	<slot>默认内容</slot>
</div>
`
}）
```

```html
- 2.插槽内容
<alert-box>Something bad happened.</alert-box>
```

#### 4.3.2 具名插槽

```js
1.插槽定义
Vue.component('base-layout',{
template: `
    <div class="container">
        <header>
            <slot name="header"></slot>
        </header>
        <main>
            <slot></slot>
        </main>
        <footer>
            <slot name=" footer"></slot>
        </footer>
    </div>
`
})
```

```html
2. 插槽内容
<base-layout>
	<h1 slot="header">标题内容</h1>

	<p>主要内容1,填充到默认的插槽中</p>
	<p>主要内容2,填充到默认的插槽中</p>
	<template>
        <!--<template>标签不会渲染到页面上,多个标签的时候起包裹作用-->
		<p slot="footer">底部内容1</p>
        <p slot="footer">底部内容2</p>
    </template>
</base-layout>
```




#### 4.3.3 作用域插槽

> 父组件替换插槽的标签，但是内容由子组件来提供

```js
1. 插槽定义
template:`
<ul>
	<1i v-for="item in list" v-bind:key="item.id">
        <slot v-bind:item="item">
            {{item.name}}
        </slot>
	</1i>
</ul>
`
```



```html
2. 插槽内容
<fruit-list v-bind:list="list">
	<template slot-scope="slotProps">
		<strong v-if="slotProps.item.current">
			{{slotProps.item.text}}
        </strong>
    </template>
</fruit-list>

```

### 4.4 组件化开发案例(购物车)

- 实现整体布局和样式效果

- 划分独立的功能组件心

- 组合所有的子组件形成整体结构

- 逐个实现各个组件功能
  - 标题组件

  - 列表组件
  - 结算组件

```html
<div id="app">
		<div class="container">
			<my-cart></my-cart>
		</div>
</div>	
<script src="./vue.js"></script>
<script src="./main.js"></script>
```

```js
var CartTitle = {
    props: ['uname'],
    template: `
		<div class="title">{{uname}}的商品</div>
	`
}
var CartList = {
	props:['list'],
    template: `
		<div>
          <div :key='item.id' v-for='item in list' class="item">
            <img :src="item.img"/>
            <div class="name">{{item.name}}</div>
            <div class="change">
              <a href="" @click.prevent='reduceNum(item.id)'>－</a>
              <input type="text" class="num" :value='item.num' @blur='changeNum(item.id, $event)'/>
              <a href="" @click.prevent='addNum(item.id)'>＋</a>
            </div>
            <div class="del" @click='del(item.id)'>×</div>
          </div>
        </div>
	`,
	methods: {
		del: function(id) {
			// 把id传递给父组件
			this.$emit('cart-del', id)
		},
		changeNum: function(id, event) {
			this.$emit('change-num', {
				id: id,
				type: 'change',
				num: event.target.value
			})
		},
		reduceNum: function (id, val) {
			this.$emit('change-num', {
				id: id,
				type: 'sub'
			})
		},
		addNum: function (id, val) {
			this.$emit('change-num', {
				id: id,
				type: 'add'
			})
		}
	}
}
var CartTotal = {
	props:['list'],
    template: `
		<div class="total">
          <span>总价：{{total}}</span>
          <button>结算</button>
        </div>
    `,
    computed: {
    	total: function() {
    		//计算总价
    		var t = 0
    		this.list.forEach(item => {
    			t += item.price * item.num
    		})
    		return t
    	}
    }
}

Vue.component('my-cart', {
    data: function() {
        return {
            uname: 'fc',
            list: [{
                id: 1,
                name: 'TCL彩电',
                price: 1999,
                num: 1,
                img: 'img/a.jpg'
            }, {
                id: 2,
                name: '机顶盒',
                price: 258,
                num: 1,
                img: 'img/b.jpg'
            }, {
                id: 3,
                name: '海尔冰箱',
                price: 3338,
                num: 1,
                img: 'img/c.jpg'
            }, {
                id: 4,
                name: '小米手机',
                price: 3490,
                num: 1,
                img: 'img/d.jpg'
            }, {
                id: 5,
                name: '电视',
                price: 2080,
                num: 1,
                img: 'img/e.jpg'
            }]
        }
    },
    template: `
		<div class="cart">
			<cart-title :uname='uname'></cart-title>
			<cart-list :list='list' @change-num='changeNum($event)' @cart-del='delCart($event)'></cart-list>
			<cart-total :list='list'></cart-total>
		</div>
	`,
    components: {
        'cart-title': CartTitle,
        'cart-list': CartList,
        'cart-total': CartTotal
    },
    methods: {
    	delCart: function (id) {
    		//根据id，删除list中的数据
    		// 1，找到索引
    		var index = this.list.findIndex(item => {
    			return item.id == id
    		})

    		// 2， 删除对应
    		this.list.splice(index,1)
    	},
    	changeNum: function (val) {
    		// 三种情况：
    		// 1. 输入域变更
    		// 2. --
    		// 3. ++
    		// 根据子组件传递的数据，更新list
    		if(val.type == 'change') {
    			this.list.some(item => {
	    			if(item.id == val.id) {
	    				item.num = val.num
	    				return true
	    			}
	    		})
    		} else if (val.type == 'sub') {
    				this.list.some(item => {
		    			if(item.id == val.id) {
		    				item.num -= 1
		    				return true
		    			}
		    		})    			
    		} else if(val.type == 'add'){
    			this.list.some(item => {
	    			if(item.id == val.id) {
	    				item.num += 1
	    				return true
	    			}
	    		})
    		}
    		
    		
    	},
    	reduceNum: function(val) {
    		console.log(val)
    	}
    }
})
var vm = new Vue({
    el: '#app',
    data: {

    }
})
```

## 5 模块化开发

- 常见的模块化规范
  - CommonJS(了解)
  
  ```js
  - 导出
  module.exports = {
      flag: true,
      test (a, b) {
          return a + b
      }
  }
  - 导入
  let {flag, test} = require('./test.js')
  ```
  
  
  
  - AMD
  - CMD
  - **ES6的Modules**
  
  ```js
  - export导出
  // index.html
  <script src="a.js" type="module"></script>
  // a.js
  // - 先定义再导出
  export {flag, fn}
  // - 定义同时导出
  export var num = 112
  export class Person {
      talk() {
          console.log('hi')
      }
  }
  - import导入
  // b.js
  import {flag, fn} from "./a.js"
  
  ---------------------------------------------
  - export default
  // 默认的只能有一个
  
  // 先定义再导出
  const address = "beijing"
  export default address
  
  // - 定义同时导出
  export const address = "beijing"
  
  // 导入的时候自定义名称
  import addr from "./a.js"
  ----------------------------------------------
  - 统一全部导入
  import * as test from './a.js'
  // test是一个对象
  console.log(test.flag)
  ```


## 7 Vue CLI(脚手架)

### 7.1 CLI2的使用

- 安装

```shell
npm install -g @vue/cli

#拉取2.x模板(旧版本)
npm install @vue/cli-init -g
```

- 初始化项目

```shell
- Vue CLI2初始化项目
vue init webpack my-project(项目名称)

? Project name (my-project)
# 项目名（一般和项目文件夹名字一样）
? Project description (A Vue.js project)
# 项目描述信息(可修改)
? Author (...)
# 作者
? Vue build (Use arrow keys)
# 箭头选择第二个 Runtime-only
? Install vue-router? (Y/N)
# 是否安装路由
? Use ESlint to lint your code? (Y/N)
# 是否对JS代码进行限制(代码规范化,代码不规范会报错)
# 后续可以在config/index.js中修改useES-lint: false
? Pick an ESlint preset (Use arrow keys)
# 选择代码规范[标准,爱彼迎,自定义]
? Set up unit tests (Y/N)
# 单元测试
? Setup e2e() tests with Nightwatch? (Y/N) #端到端测试
# 安装Nightwatch,是一个利用selenium或者webdriver或phantomjs等进行自动化测试的框架
? should we run `npm install` for you after the project has been created? (commended) (Use arrow keys)
# 使用npm还是yarn

```

- 生成的目录结构

```shell
├── build/                      # webpack 编译任务配置文件: 开发环境与生产环境
│   └── ...
├── config/                     
│   ├── index.js                # 项目核心配置
│   └── ...
├ ── node_module/               #项目中安装的依赖模块
── src/
│   ├── main.js                 # 程序入口文件
│   ├── App.vue                 # 程序入口vue组件
│   ├── components/             # 组件
│   │   └── ...
│   └── assets/                 # 资源文件夹，一般放一些静态资源文件
│       └── ...
├── static/                     # 纯静态资源 (直接拷贝到dist/static/里面)
├── test/
│   └── unit/                   # 单元测试
│   │   ├── specs/              # 测试规范
│   │   ├── index.js            # 测试入口文件
│   │   └── karma.conf.js       # 测试运行配置文件
│   └── e2e/                    # 端到端测试
│   │   ├── specs/              # 测试规范
│   │   ├── custom-assertions/  # 端到端测试自定义断言
│   │   ├── runner.js           # 运行测试的脚本
│   │   └── nightwatch.conf.js  # 运行测试的配置文件
├── .babelrc                    # babel 配置文件
├── .editorconfig               # 编辑配置文件
├── .gitignore                  # 用来过滤一些版本控制的文件，比如node_modules文件夹 
├── index.html                  # index.html 入口模板文件
└── package.json                # 项目文件，记载着一些命令和依赖还有简要的项目描述信息 
└── README.md                   #介绍自己这个项目的，可参照github上star多的项目。
build/

```

- runtime-only 和 runtime-compiler的区别

```js
- runtime-only 的main.js
/* template保存 => 解析成ast(抽象语法树) => 编译成render函数 => 虚拟DOM => UI
*/
new Vue({
    el: '#app',
    template: '</App>',
    components: {App}
})
- runtime-compiler 的main.js
// render函数 => 虚拟DOM => UI
// 性能更高，代码更少（小6kb）
new Vue ({
    el: '#app',
    render: h => h(App)
    /* 
    1. 普通用法
    render: function(createElement) {
    	return createElement('标签名',{属性}，[内容(可套娃)])
    }
    2. 传入组件对象，不再需要template属性
    */
})
```

- npm run build和npm run dev的运行过程

![](./media/npmRunBuild.jpg)

![npmRunDev](./media/npmRunDev.jpg)

### 7.2 CLI3的使用

> 更少的配置

- vue-cli3与2的区别
  - vue-cli3是基于webpack4打造，vue-cli2还是webpack3
  - vue-cli3的设计原则是“0配置”，移除的配置文件根目录下的，build和config等目录
  - vue-cli3提供了vue ui命令，提供了可视化配置，更加人性化
  - 移除了static文件夹，新增了public文件夹，并且index.html移动到public中

```shell
- Vue CLI3初始化项目
vue create my-project(项目名称）

? Please pick a preset? 
# 选择配置[默认(babel,eslint),手动(空格选择/取消),预设]
? Where do you prefer config for Babel,ESlint,etc.?
# 配置文件放package.json中还是单独一个文件
? Save this as a preset for future project?
# 将当前设置保存为自定义预设吗（起名）
# 配置文件存在.vuerc里面 可以手动修改


```

```js
- main.js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false //提示信息

new Vue ({
    render: h => h(App)
}).$mount('#app')

```

- 图形界面

```shell
任意界面vue ui
```



## 8 Vue-router

### 8.1 后端路由和前端路由

#### 8.1.1 后端路由阶段

- 早期的网站开发整个HTML页面是由服务器来渲染的.
  - 服务器直接生产渲染好对应的HTML页面，返回给客户端进行展示.

- 但是，一个网站，这么多页面服务器如何处理呢？
  - 一个页面有自己对应的网址，也就是URL.
  - URL会发送到服务器，服务器会通过正则对该URL进行匹配，并且最后交给一个Controller进行处理.
  - Controller进行各种处理，最终生成HTML或者数据，返回给前端。
  - 这就完成了一个IO操作。

- 上面的这种操作，就是后端路由.
  - 当我们页面中需要请求不同的路径内容时，交给服务器来进行处理，服务器渲染好整个页面，并且将页面返回给客户顿。
  - 这种情况下渲染好的页面，不需要单独加载任何的js和css，可以直接交给浏览器展示，这样也有利于SEO的优化。
- 后端路由的缺点：
  - 一种情况是整个页面的模块由后端人员来编写和维护的.
  - 另一种情况是前端开发人员如果要开发页面，需要通过PHP和Java等语言来编写页面代码。
  - 而且通常情况下HTML代码和数据以及对应的逻辑会混一起，编写和维护都是非常糟糕的事情.

#### 8.1.2 前后端分离阶段

- 前后端分离阶段：

   - 随着Ajax的出现，有了前后端分离的开发模式
- 后端只提供API来返回数据，前端通过Ajax获取数据，并且可以通过JavaScript将数据渲染到页面中
   - 这样做最大的优点就是前后端责任的清晰，后端专注于数据上，前端专注于交互和可视化上.
- 并且当移动端（iOS/Android)出现后，后端不需要进行任何处理，依然使用之前的一套API即可.
   - 目前很多的网站依然采用这种模式开发。

#### 8.1.3 单页面富用阶段

- SPA
  - SPA(single page web application)页面: 整个网页只有一个html页面
  - SPA最主要的特点就是在前后端分离的基础上加了一层前端路由。也就是前端来维护一套路由规则

- 前端路由
  - 映射url到页面
  - 改变URL，但是页面不进行整体的刷新

### 8.2 修改URL不跳转页面的两种模式

- hash(默认)

  ```js
  location.hash = 'foo'
  ```

- history

  ```js
  - push
  history.pushState({},'','home')
  // 参数 data，title，url
  // url: .../home
  history.pushState({},'','about')
  // url: .../about
  history.pushState({},'','me')
  // url: .../me
  history.back()
  // url: .../about
  history.back()
  // url: .../home
  栈结构，先进后出
  
  - replace
  history.replaceState({},'','home')
  替换不能返回
  
  - go
  history.back()等价于history.go(-1)
  history.forward()则等价于history.go(1)
  这三个接口等同于浏览器界面的前进后退。
  
  ```

### 8.3 vue-router安装和使用

#### 8.3.1 安装

```shell
npm install vue-router --save

或者脚手架
```

#### 8.3.2 使用

```js
创建..src/router/index.js

// 配置路由相关信息
import VueRouter from 'vue-router'
import Vue from 'vue'

// 5.2.2 引入路由vue
import Home from '../components/Home.vue'
import About from '../components/About.vue'

// 1. 通过Vue.use（插件），安装插件
Vue.use(VueRouter)

// 2. 创建路由对象
const routes = [
    // 5.2.2 配置路由映射关系
    {
        path: '',
        redirect: '/home'
        // 默认显示首页，重定向
    },
	{
        path: '/home',
        component: Home
    },
    {
        path: '/about',
        component: About
    }
]
const router = new VueRouter({
    routes,
    mode: 'history',
    // 修改默认模式为history模式
    linkActiveClass: 'active'
    // 和后面的router-link标签中的active-class = "active"一个效果
})
// 3. 将router对象传入到Vue实例
export default router
====================================
..src/main.js文件
// 4.导入router，自动寻找index.js文件，所以/index可以省略
import router from './router/index'

new Vue({
    el:'#app',
    router,
    render:h => h(App)
})
========================================
// 5.1 创建路由组件  src/components/xxx.vue

```

```vue
- 5.3 使用路由 App.vue
<template>
  <div id="app">
      <!--写法1-->
      <router-link to="/home" active-class = "active">首页</router-link> 
      <router-link to="/about" active-class = "active">关于</router-link>
      <router-view></router-view>
      <!--router-link和router-view是两个全局组件
      router-view起占位作用， 决定渲染页面的位置-->
      
      <!--写法2 start-->
      <button @click="homeClick">首页</button>
      <button @click="aboutClick">关于</button>
      <router-view></router-view>
  </div>
</template>

<script>
export default {
    name: 'App',
    methods:{
        homeClick() {
            //通过代码的方式修改路由 vue-router
            // this.$router.push('/home')
            this.$router.replace('/home')
            // console.log('homeClick')
        },
        aboutClick() {
            this.$router.replace('/about')
            // console.log('aboutClick')
        }
    }
}
    // 写法2 end
</script>

<style>
    .active {
        color: #f00
    }
</style>

```

- router-link属性：
  - to: 目标地址，渲染成a标签
  - tag="button" 将router-link渲染成button标签，点过之后的btn多了两个class：router-link-active和router-link-exact-active可以用来修改style
  - replace 没有值 将pushState改为replaceState（禁用返回前进按钮）
  - active-class = "active": 修改默认的router-link-active为active

### 8.4 动态路由

1. 创建src/components/User.vue

   ```vue
   <template>
     <div id="app">
        <!-- 可以通过计算属性获取$route中的userId -->
     	<h2>{{userId}}</h2>
         <!-- 也可以直接获取 -->
         <h2>{{$route.params.userId}}</h2>
     </div>
   </template>
   
   <script>
   	export default {
           name: "User",
           computed: {
               userId() {
                   return this.$route.params.userId
                   // 动态获取userId
               }
           }
       }
   </script>
   
   <style>
   
   </style>
   
   ================================
   $route和$route的区别
   $route 当前活跃路由
   $router new的VueRouter实例
   params（parameters、参数）
   ```
   
   


2. index.js

   ```js
   import User from '../components/User.vue'
   
   const routes = [
       // 5.2.2 配置路由映射关系
       {
           path: '',
           redirect: '/home'
           // 默认显示首页，重定向
       },
   	{
           path: '/home',
           component: Home
       },
       {
           path: '/about',
           component: About
       },
       {
           path: '/user/:userId',
           component: User
       }
   ]
   ```

3. App.vue

   ```vue
   <template>
     <div id="app">
         <router-link to="/home">首页</router-link> 
         <router-link to="/about">关于</router-link>
         <router-link :to="'/user/'+userId">用户</router-link>
         
         <router-view></router-view>
     </div>
   </template>
   
   <script>
   	export default {
           name: 'App',
           data() {
               return {
                   userId: 'jack'
               }
           },
           methods: {
               ...
           }
       }
   </script>
   ```


### 8.5 路由的懒加载

> 解决打包构建应用时，js包过大影响页面加载的问题
>
> 即用到的时候再加载(一个路由打包一个js文件，可以在dist文件夹下面查看)

```js
// import Home from '../components/Home.vue'
// import About from '../components/About.vue'
// import User from '../components/User.vue'
- ES6语法
const Home = () => import('../components/Home.vue')
const About = () => import('../components/About.vue')
const User = () => import('../components/User.vue')
```



### 8.6 路由嵌套

> 嵌套路由是一个很常见的功能
>
> ​     比如在home页面中，我们希望通过/home/news和/home/message访问一些内容.
>
>  	一个路径映射一个组件，访问这两个路径也会分别渲染两个组件.

#### 8.6.1 创建对应的子组件，并且在路由映射中配置对应

```vue
- 创建src/components/HomeNews.vue, HomeMsg.vue
<template>
	<div>
        <ul>
            <li>新闻1</li>
            <li>新闻2</li>
            <li>新闻3</li>
            <li>新闻4</li>
        </ul>
    </div>
</template>
...
```

```js
-index.js
// 懒加载
const Home = () => import('../components/Home.vue')
const HomeNews = () => import('../components/HomeNews.vue')
const HomeMsg = () => import('../components/HomeMsg.vue')

const About = () => import('../components/About.vue')
const User = () => import('../components/User.vue')

const routes = [
    {
        path: '',
        redirect: '/home'
    },
	{
        path: '/home',
        component: Home,
        // 路由嵌套
        children: [
            {
              // 默认子路径
              path: '',
                redirect: 'news'
            },
            {
                path: 'news',
                // 子路由不加"/"
                component: HomeNews
            },
            {
                path: 'msg',
                // 子路由不加"/"
                component: HomeMsg
            }
        ]
    },
    ...
]
```



#### 8.6.2 在组件内部使用`<router-view>`标签

```vue
- Home.vue里面加上router-view标签
<template>
    <div>
        <h2>首页</h2>
        <router-link to="/home/news">新闻</router-link>
        <router-link to="/home/msg">消息</router-link>   
        <router-view></router-view>
    </div>
</template>

```

### 8.9 传递参数

#### 8.9.1 params的类型

- 配置路由格式： /router/:id
- 传递的方式： 在path后面跟上相应的值
- 传递后形成的路径： /router/123， /router/abc

#### 8.9.2 query的类型

- 配置路由格式： /router 也就是普通配置
- 传递的方式： 对象中使用query的key作为传递方式
- 传递后形成的路径： /router?id=123， /router?id=abc

```js
- index.js
...
const Profile = () => import('../components/Profile.vue')
...
{
    path: '/profile',
    component: Profile
}

```

```vue
- App.vue
<template>
	...
	<!--方式1： router-link方式-->
	<router-link :to="{path: 'profile', query:{name: 'jack',age: 18}}">档案</router-link>

	<!--方式2：btn+点击事件-->
	<button @click='profileClick'>档案</button>
</template>
<script>
	export default {
        name: 'App',
        data(){
            return {
                userId: 'zhangsan'
                
            }
        }
        methods:{
       		...
            profileClick() {
                this.$route.replace({path:'profile', query:{name:'jack',age:18}})
            }
        }
        
    }
</script>
==========================================

- Profile.vue
<template>
	<h2>profile页面</h2>
	<h2>{{$route.query.name}}</h2>
	<h2>{{$route.query.age}}</h2>
</template>
<script>
	export default {
        name: 'Profile'
        
    }
</script>

此时的url：
localhost:8080/profile?name=jack&age=18
```

### 8.10 全局导航守卫

> 在spa应用中，改变网页的标题

```js
-index.js
...
const routes = [
    {
        path: '',
        redirect: '/home'
    },
	{
        path: '/home',
        component: Home,
        children:[...],
        meta:{
            //元数据
            title:'首页'
        }
    },
    {
        path: '/about',
        component: About,
        meta:{
            title:'关于'
        }
    },
    {
        path: '/user/:abc',
        component: User,
        meta:{
            title:'用户'
        }
    }
]
// 前置钩子（hook）跳转前回调
router.beforeEach((to,from,next)=> {
    // 从from跳转到to，next()必须执行
    document.title = to.matched[0].meta.title
    next()
})

/* 后置钩子
router.afterEach()跳转后回调
两个参数 没有next() */

router.afterEach((from,to)=> {
    // write here
})
======================================================
-~~路由独享的守卫~~
const router = new VueRouter({
	routes:[{
		path:'/foo',
		component: Foo,
		beforeEnter (to, from, next)=> {
		//...
		}
	}]
})
---------------------------------------------------
-~~组件内的守卫~~
const Foo ={
    template:``,
    beforeRouteEnter (to,from,next){
        //在渲染该组件的对应路由被confirm前调用
		//不！能！获取组件实例this
		//因为当守卫执行前，组件实例还没被创建
    },
    beforeRouteUpdate (to,from,next){
        //在当前路由改变，但是该组件被复用时调用
		//举味说，对于一个带有动态参数的路径/foo/：id，在/foo/1和/foo/2之间跳转的时候，
		//由于会渲染同样的Foo组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
		//可以访问组件实例this
    },
    beforeRouteLeave (to,from,next){
        // 导航离开该组件的对应路由的时候调用
        //可以访问组件实例`this`
    }
}
=======================================================

```

### 8.11 keep-alive和vue-router

> keep-alive是Vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。
>
> router-view 也是一个组件，如果直接被包在keep-alive里面，所有路径匹配到的视图组件都会被缓存

#### 8.11.1 keep-alive

- 两个属性
  - include ---字符串或正则，只有匹配的组件会被缓存
  - exclude---字符串或正则，任何匹配的组件都不会被缓存

```vue
- App.vue
<template>
	<button @click='userClick'>用户</button>
	<keep-alive exclude='Profile,User'>
    <!--多个之间不能加空格-->
    	<router-view/> 
    </keep-alive>
</template>

- Home.vue
<template>

</template>
<script>
    // 解决重新加载home/news的时候需要重新点一下
export default {
    ...
    // 这两个函数必须有<keep-alive>才能运行
    activated(){
        this.$router.push(this.path)
    },
        deactivated(){
            //...
        }
    beforeRouteLeave (to, from, next){
        this.path = this.$route.path
        // 使用path记录离开时的路径
        next()
    }
}
</script>
```

## 9 Promise

### 9.1 基础用法

异步任务三种状态

- Pending 
  - 等待状态，比如正在进行网络请求，或者定时器没有到达时间
- fulfilled
  - 满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()
- Rejected
  - 拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()

```js
- 表述方式1
new Promise((resolve,reject) => {
    // 第一次网络请求的代码
    setTimeout(()=> {
        //成功的时候调用resolve
        resolve()
        //失败的时候调用reject
        // reject('error msg')      
    },1000)
}).then(() => {
    // 第一次拿到结果的处理代码
    console.log('11111')
    // 第二次网络请求的代码
    return new Promise((resolve,reject)=>{
        setTimeout(()=> {
        	resolve()
    	},1000)
    })
}).then(() => {
    // 第二次网络请求的代码
    console.log('22222')
    // 第三次网络请求的代码
    return new Promise((resolve,reject)=>{
        setTimeout(()=> {
        	resolve()
    	},1000)
    })
}).then(() => {
    // 第三次网络请求的代码
    console.log('33333')
}).catch(err => {
    console.log(err)
})

- 表述方式2
new Promise((resolve,reject) => {
    // 第一次网络请求的代码
    setTimeout(()=> {
        resolve()
       	reject('error msg')    
    },1000)
}).then(data =>{
    console.log(data)
}, err =>{
    console.log(err)
})
```

### 9.2 链式调用

```js
- 原始形式
new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('aaa')
    },1000)
}).then(res => {
    console.log(res,'第一次的代码处理，这里直接打印代替')
    return new Promise(resolve => {
        resolve(res + '111')
    })
}).then(res => {
    console.log(res,'第二次的代码处理，这里直接打印代替')
    return new Promise(resolve => {
        resolve(res + '222')
    })
}).then(res => {
    console.log(res,'第三次的代码处理，这里直接打印代替')
})

- 简写1
new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('aaa')
    },1000)
}).then(res => {
    console.log(res,'第一次的代码处理，这里直接打印代替')
    return Promise.resolve(res + '111')
}).then(res => {
    console.log(res,'第二次的代码处理，这里直接打印代替')
    return Promise.resolve(res + '222')
}).then(res => {
    console.log(res,'第三次的代码处理，这里直接打印代替')
})

/*有错误的情况
new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('aaa')
    },1000)
}).then(res => {
    console.log(res,'第一次的代码处理，这里直接打印代替')
    return Promise.resolve(res + '111')
}).then(res => {
    console.log(res,'第二次的代码处理，这里直接打印代替')
    return Promise.reject('err happened')
}).then(res => {
    console.log(res,'第三次的代码处理，这里直接打印代替')
}).catch(err =>{
	console.log(err)
})

打印结果：
aaa '第一次的代码处理，这里直接打印代替'
aaa111 '第二次的代码处理，这里直接打印代替'
err happened


*/

- 简写2
new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('aaa')
    },1000)
}).then(res => {
    console.log(res,'第一次的代码处理，这里直接打印代替')
    return res + '111'
}).then(res => {
    console.log(res,'第二次的代码处理，这里直接打印代替')
    return res + '222'
}).then(res => {
    console.log(res,'第三次的代码处理，这里直接打印代替')
})

/*抛出异常
new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('aaa')
    },1000)
}).then(res => {
    console.log(res,'第一次的代码处理，这里直接打印代替')
    return res + '111'
}).then(res => {
   	console.log(res,'第二次的代码处理，这里直接打印代替')
    throw 'err happened'
}).then(res => {
    console.log(res,'第三次的代码处理，这里直接打印代替')
}).catch(err =>{
	console.log(err)
})

打印结果：
aaa '第一次的代码处理，这里直接打印代替'
aaa111 '第二次的代码处理，这里直接打印代替'
err happened
*/
```

### 9.3 promise的all方法使用

> 某个需求需要发送多次请求时，使用all

```js
Promise.all([
    // 这里传入可迭代对象（数组）
    new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('ret1')
        },1000)
    }),
    new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('ret2')
        },2000)
    })
]).then(rets =>{
	console.log(rets)
})

// 返回结果
['ret1','ret2']
```



## 10 Vuex

> 状态管理工具：
>
> ​	多个组件的状态需要共享 ，将状态存储到Vuex
>
> // 父子组件之间传递可以使用props
>
> 非父子组件使用Vuex

### 10.0 单页面的状态管理

```html
<template>
  <div id="app">
    <h2>{{msg}}</h2>
    <h2>{{counter}}</h2>
    <button @click='counter++'>+</button>
    <button @click='counter--'>-</button>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return{
      msg: '我是App组件',
      counter: 0
    }
  }
}
</script>
```

 -  State：存储当前页面的状态
 -  View：状态在模板上展示
 -  Actions：界面上可以通过action控制state

![](2020-07-14-13-59-49.png)

### 10.1 多界面状态管理

```shell
-1. 安装webpack
vue init webpack learn-vuex
npm install vuex --save

```
![](2020-07-14-14-00-55.png)


```js
-2. 新建src/store/index.js
import Vue from 'vue'
import Vuex from 'vuex'
// 安装插件
Vue.use(Vuex)
// 创建对象
const store = new Vuex.Store({
	state:{
		// 保存状态
		counter: 0
	},
	mutations:{
		// 方法
		increment(state){
			state.counter++
		},
		decrement(state){
			state.counter--
		}
	},
	actions:{

	},
	getters:{

	},
	modules:{
		
	}

})

-3. main.js挂载
import Vue from 'vue'
import App from './App'
import store from './store'

Vue.config.productionTip = false

new Vue({
  el: '#app',
  store,
  render: h => h(App)
})

```

```html
<template>
  <div id="app">
    <h2>{{msg}}</h2>
    <h2>{{$store.state.counter}}</h2>
      <!--可以通过$store获取store中定义的变量-->
    <button @click='addition'>+</button>
    <button @click='subtraction'>-</button>
    <hello-vuex/>
  </div>
</template>

<script>
  import  HelloVuex from './components/HelloVuex.vue'
export default {
  name: 'App',
  components:{
    HelloVuex
  },
  data(){
    return{
      msg: '我是App组件',
      
    }
  },
  methods:{
      // 通过summit的方式调用store中定义的方式
      //好处：可跟踪
    addition(){
      this.$store.commit('increment')
    },
    subtraction(){
      this.$store.commit('decrement')
    }
  }
}
</script>
-------------------------------------------------
HelloVuex.vue
<template>
  <div>{{$store.state.counter}}</div>
</template>

<script>
export default {
  name: 'HelloVuex'
}
</script>

```

### 10.2 Vuex核心概念

#### 10.2.1 State单一状态树

> Single Source of Truth ，单一数据源，方便管理，维护，
>
> 一个项目用一个Store
>
> 目录结构：
>
> ​	![](2020-07-14-14-02-18.png)
>
> 除了state其余都抽离成一个文件，方便管理

#### 10.2.2 Getters

> 类似计算属性
>
> - 当属性需要经过fn之后再被调用，使用getters

```js
- 定义
const store = new Vuex.Store({
    state:{
		counter: 100,
        students:[
		{id: 1, name:'zhangsan', age: 18},
		{id: 2, name:'lisi', age: 23},
		{id: 3, name:'wangwu', age: 23},
		{id: 4, name:'zhaoliu', age: 19}
		]
    },
	getters:{
		powerCounter(state){
            return state.counter * state.counter
        },
        filterStudent(state){
            return state.students.filter(s => s.age> 20)
        },
        // 参数也可以传getters
        filterStudentLength(state,getters){
            return getters.filterStudent.length
        }，
        //getter默认是不能传递参数的 传输参数不能写state后面 是形参 传了age也是当作getters
        filterStudentByAge(state){
			return function(age){
    			// 通过return function的方式传递参数
				return state.students.filter(s => s.age> age)
            }
        }
	}
})

```

```html
- 调用
<template>
	<h2>{{$store.getters.powerCounter}}</h2>
	// 不用加()
	<h2>年龄大于20岁的学生：{{$store.getters.filterStudent}}</h2>
	<h2>年龄大于20岁的学生的个数：{{$store.getters.filterStudent}}</h2>
	<h2>年龄大于age的学生：{{$store.getters.filterStudentByAge(age)}}</h2>
</template>
```



#### 10.2.3 mutations

> Vuex的store状态的更新**唯一**方式：提交Mutation

```js
- mutation的定义方式
// store/index.js
mutation:{
    increment(state){
        state.count++
    }
}

- 通过mutation更新
// test.vue
increment: function(){
    this.$store.commit('increment')
}


```

- 传参

  > Mutation主要包括两部分
  >
  >  - 字符串的事件类型（type）
  >  - 一个回调函数（hander），该回调函数的第一个参数就是state

  ```js
  - store/index.js
    const store = new Vuex.Store({
        state:{
            // 保存状态
            counter: 0,
            students:[
                {id: 1, name:'zhangsan', age: 18},
                {id: 2, name:'lisi', age: 23},
                {id: 3, name:'wangwu', age: 23},
                {id: 4, name:'zhaoliu', age: 19}
            ]
        },
        mutations:{
            incrementCount(state, count){
                // 一个参数的情况
                state.counter += count
            },
            addStudent(state, stu){
                // 传入对象作为参数的情况
                state.students.push(stu)
            }
        }
    })
  
  ```

  ```html
  - 调用
  <template>
    <div id="app">
      <h2>{{$store.state.counter}}</h2>
      <button @click='addCount(5)'>+5</button>
      <button @click='addCount(10)'>+10</button>
      <button @click='addStudent'>添加学生</button>
      <hello-vuex/>
    </div>
  </template>
  
  <script>
  export default {
    name: 'App',
    methods:{
      addCount(count){
        this.$store.commit('incrementCount',count)
      },
      addStudent(){
          const stu = {id: 5, name:'zhuqi', age: 17}
          this.$store.commit('addStudent',stu)
      }
    }
  }
  </script>
  ```

- 两种提交风格

  ```html
  <script>
  export default {
    name: 'App',
    methods:{
      addCount(count){
          // 1.普通的提交封装
        this.$store.commit('incrementCount',count)
          
          // 2.特殊的提交封装
          /*这种方式下
              addCount(state,payload){
                 state.counter += payload.count
              }
              count为一个对象{ type: 'incrementCount',count： count}
          */ 
        this.$store.commit({
            type: 'incrementCount',
            count： count
        })
      },
      addStudent(stu){
          const stu = {if: 5, name:'zhuqi', age: 17}
          this.$store.commit('addStudent',stu)
      }
    }
  }
  </script>
  ```

- mutation的响应规则

  - Vuex的store中的state是响应式的，当state中的数据发生改变时，Vue组件会自动更新（观察者模式）
  - state中的属性都会被加入到响应式系统中，响应式系统监听属性的变化，当属性（已存在）变化的时候，会通知所有界面中用到该属性的地方，让界面发生刷新，**新增属性不会（不在监听范围）**

  ```js
  updateInfo(state){
      // 修改已有属性是响应式的
      state.info.name = 'jack'
      // 新增，删除属性不是响应式
      state.info['address'] = 'beijing'
      delete state.info.age
      // 使用Vue的方法使具有响应式
      Vue.set(state.info, 'address','beijing')
      Vue.delete(state.info, 'age')
      
      //*** 新版本自动处理好了***
  }
  ```

  

- 这就要求我们必须遵守一些Vuex对应的规则

  - 提前在store中初始化好所需的属性
  - 当给state中的对象添加新属性的时候，使用下面的方式
    - 1. 使用Vue.set(obj, 'newProp', 123)
    - 2. 用新对象给旧对象重新赋值

- Mutation同步函数

  - 通常情况下，Vuex押送求我们Mutation中的方法必须使同步方法
    - 异步操作devtools不能追踪

#### 10.2.4 Action

> 异步操作的时候添加一个Action操作 而不是绕过他

```js
mutations:{
    updateInfo(state){
        state.info.name = 'jack'
    }
},
action:{
    updatedInfoByAction(context){
        //默认属性context（上下文）理解为类似store
        setTimeout(() => {
            context.commit('updateInfo')
            // 修改store一定要通过mutation
            // 错误代码： state.info.name = 'jack'
        },1000)
    }
}

-- App.vue调用
methods:{
    updateInfo(){
        this.$store.dispatch('updatedInfoByAction')
    }
}


```

- action传参，回调(Promise)

```js
mutations:{
    updateInfo(state){
        state.info.name = 'jack'
    }
},
action:{
    updatedInfoByAction(context,payload){
       return new Promise((resolve,reject)=>{
            setTimeout(() => {
                context.commit('updateInfo')
                console.log(payload)
                resolve('回调的参数')
            },1000)
       })
    }
}

------------------------组件内调用----------------------------
methods：{
    updateInfo(){
        this.$store
            .dispatch('updatedInfoByAction','携带的信息')
            .then(res =>{
				console.log('里面完成了提交')
            	console.log(res)
        	})
    }
}
```



#### 10.2.5 modules

> Vue使用单一状态数，应用复杂的时候，store对象可能变的臃肿
>
> 为了解决这个问题。Vuex允许将store分割成模块，每个模块拥有自己的state...

```js
modules:{
    a: {
        state:{
            name:'zhangsan'
        },
        mutations:{
            // 子模块起名要避免重复
            // mutation和getter的调用，先在根模块里找，找不到去子模块找
        },
        actions:{
            aUpdateName(context){
                // 接收一个context参数对象
                setTimeout(()=>{
                    // 子模块只能用自己的mutation
                    context.commit('updateName','wangwu')
                },1000)
            }
        },
        getters:{
            fullname(state,getters,rootState){
                // 第一个参数：state
                // 第二个参数（可选）：其他getter
                // 第三个参数（可选）：根模块的State
                return getters.fn + rootState.bar
            }
        }
    },
    b:{
        ...
    },
    ...
}
    
----调用----
<h2>{{$store.state.a.name}}</h2>

```

## 11 网络请求封装（axios）

### 11.1 网络模块选择
```js
- Ajax
    配置使用比较混乱 很少使用

- jQuery-Ajax
    使用较少
    没有必要只为了使用jquery的一点功能引入全部框架

- Vue-resource
    停止更新维护 不用

- axios
    - 在浏览器中发送xhr请求
    - 在node.js中发送http请求
    - 支持Promise
    - 拦截请求和响应
    - 转换请求和响应数据
    ...
```

#### 11.1.1 安装

```shell
- 安装
npm install axios --save

```

```js
-导入
import axios from 'axios'

```

#### 11.1.2 使用

```js
-使用
1.直接使用(支持promise)
    // http请求模拟测试网站 https://httpbin.org
    axios({
        url: 'http://123.207.32.32:8000/home/multidata',
        method: 'get'
        // 默认是get请求
    }).then(res =>{
        console.log(res)
    })

2. 所有方式
axios(config)

axios.request(config)

axios.get(url[,config)

axios.delete(url[,config])

axios.head(url[,config])

axios.post(url[,data[,config]])

axios.put(url[,data[,config]])

axios.patch(url[,data[,config]])



3. 传递参数
axios({
    // 方式一：直接拼接在链接里(get方式)
	// url: 'http://123.207.32.32:8000/home/multidata?type=pop&page=1'
    
    // 方式二：params
    url: 'http://123.207.32.32:8000/home/multidata',
    params: {
        type: 'pop',
        page: 1
    }
}).then(res =>{
    console.log(res)
})
```

### 11.2 axios发送并发请求--all()
- 使用`axios.all`，可以放入多个请求的数组
- `axios([])`返回的结果是一个数组，使用`axios.spread`可将数组[res1,res2]展开为res,res2
```js
axios.all([axios({
    url:'http://123.207.32.32:8000/home/multidata'
}),axios({
    url:'http://123.207.32.32:8000/home/data',
    params:{
        type: 'sell',
        page: 5
    }
})]).then(axios.spread((res1, res2) =>{
    console.log(res1)
    console.log(res2)
}))
```

### ~~11.3 全局配置~~

> 对重复的属性进行抽离,抽离为全局属性
>
> 当业务复杂后，不太方便使用创建全局配置

```js
axios.defaults.baseURL = 'http://123.207.32.32:8000'
axios.defaults.timeout = 5000

axios.all([axios({
    url:'/home/multidata'
}),axios({
    url:'/home/data',
    params:{
        type: 'sell',
        page: 5
    }
})]).then(axios.spread((res1, res2) =>{
    console.log(res1)
    console.log(res2)
}))
```

#### 11.3.1 常见的配置选项

- 请求地址

  - `url:'/user'`

- 请求类型

  - `method: 'get'`

- 根路径

  - `baseURL:'http://www.mt.com/api`

- 请求前的数据处理
  - `transformRequest:[function(data){}]`

- 请求后的数据处理
  - `transformResponse:[function(data){}]`

- 自定义的请求头
  - `headers:{'x-Requested-With':'XMLHttpRequest'}`

- URL查询对象
  - `params:{id:12}`

- 查询对象序列化函数

  - `paramsSerializer:function(params){}
    `
- request body

  - `data:{ key: value}`

- 超时设置s

  - `timeout:1000`

- 跨域是否带Token

  - `withCredentials:false`

- 自定义请求处理

  - `adapter:function(resolve,reject,config){}`,

- 身份验证信息

  - `auth:{ uname:",pwd:'123'}`,

- 响应的数据格式 json/blob/document /arraybuffer/text/stream
  - `responseType:json'`

### 11.4 Axios的实例和模块封装

#### 11.4.1 axios的实例

```js
const instance1 = axios.create({
    baseURL:'http://222.111.33.33:8000',
    timeout: 5000
})

instance1({
    url:'/home/multidata'
}).then(res =>{
    console.log(res)
})

instance1({
    url:'/home/multidata',
    params:{
        type: 'sell',
        page: 1
    }
}).then(res =>{
    console.log(res)
})

const instance2 = axios.create({
    baseURL:'http://333.222.11.11:8000',
    timeout: 6000
})
```

#### 11.4.2 axios的模块封装

> 第三方的组件不要在每一个vue文件中依赖
>
> 单独封装

```js
新建src/network/request.js
/*
======================~~方法一：自己传入回调函数~~============================
- request.js封装
import axios from 'axios'

export function request(config,success,failure) {
	// 创建axios的实例
    const instance = axios.create({
        baseURL:'http://222.221.22.22:2000',
        timeout: 5000
    })
    // 发送真正的网络请求
    instance(config)
    .then(res =>{
        // 回调
        success(res)
    })
    .catch(err =>{
        failure(err)
    })
}
export function instance2() {
	...
}
    
    
----------main.js调用-------------------
import {request} from './network/request'
    
request({
    url:'/home/multidata'
},res =>{
    console.log(res)
}, err =>{
    console.log(err)
})
=============================================================
========================方法二：对象config包含回调=================================
- request.js封装
import axios from 'axios'

export function request(config) {
	// 创建axios的实例
    const instance = axios.create({
        baseURL:'http://222.221.22.22:2000',
        timeout: 5000
    })
    // 发送真正的网络请求
    instance(config.baseConfig)
    .then(res =>{
        // 回调
        config.success(res)
    })
    .catch(err =>{
        config.failure(err)
    })
}
    
    
----------main.js调用-------------------
import {request} from './network/request'
    
request({
    baseConfig:{
        url:'/home/multidata',
        timeout: 5000
    },
    success: function (res) {
        
    },
    failure: function(err) {
        
    }
})
====================================================
===================方法三=======================
- request.js封装
import axios from 'axios'

export function request(config,success,failure) {
    return new Promise((resolve,reject) =>{
         const instance = axios.create({
            baseURL:'http://222.221.22.22:2000',
            timeout: 5000
        })
        // 发送真正的网络请求
        instance(config)
        .then(res =>{
            resolve(res)
        })
        .catch(err =>{
            reject(err)
        })
        })

}
    
----------main.js调用-------------------
import {request} from './network/request'
    
request({
    url:'/home/multidata'
}).then(res =>{
    console.log(res)
}.catch(err =>{
    console.log(err)
})

==========================================================

*/
==================******方法四**********=====================
- request.js封装
import axios from 'axios'

export function request(config) {
    // 创建axios实例
    const instance = axios.create({
        baseURL:'http://222.221.22.22:2000',
        timeout: 5000
    })
    // 发送真正的网络请求
	return instance(config)
}
    
----------main.js调用-------------------
import {request} from './network/request'

request({
    url:'/home/multidata'
}).then(res =>{
    console.log(res)
}.catch(err =>{
    console.log(err)
})
=======================================================
```

### 11.5 拦截器

> axios提供了拦截器,用域在发送每次请求或者得到相应后,进行响应的处理
>
> - 请求成功
> - 请求失败
> - 响应成功
> - 响应失败


-   请求拦截的作用   
    -  比如config的一些信息不符合服务器的要求
    -  比如每次发送网络请求的时候,都希望在界面中显示一个请求的图标
    - 某些网络请求(登录)必须携带一些特殊的信息
```js
// axios.interceptors 全局拦截
import axios from 'axios'

export function request(config,success,failure) {
    const instance = axios.create({
        baseURL:'http://222.221.22.22:2000',
        timeout: 5000
    })
    // 请求拦截
	instance.interceptors.request.use(config =>{
        console.log(config)
        // 拦截之后要继续return出去
        return config
    },err =>{
        console.log(err)
    })
    
    // 响应拦截
    instance.interceptors.response.use(res =>{
        console.log(res)
        return res.data
    },err =>{
        console.log(err)
    })

    return instance(config)
}
```













