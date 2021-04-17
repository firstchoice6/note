# React

##  1 介绍


- 依赖


  - reactDOM

    ```jsx
    // reactDOM.render(渲染的内容，挂载对象)
    let msg = "hello react"
    reactDOM.render(<h2>{msg}</h2>,document.getElementById("adpp"))
    ```

  - babel


    - 将jsx转化成react代码
    - `<script type="text/babel">`

  - react 


    - 核心代码

- 特点


  - 声明式编程
  - 组件化开发
  - 多平台适配

## 2. jsx语法

### 2.1 认识jsx

- JSX是一种JavaScript的语法扩展（eXtension），也在很多地方称之为JavaScript XML，因为看起就是一段XML语法；
- 用于描述UI界面，并且其完成可以和JavaScript融合在一起使用；
- 不同于Vue中的模块语法，你不需要专门学习模块语法中的一些指令（比如v-for、v-if、v-else、v-bind）；

### 2.2 规范

- JSX的顶层只能有一个根元素，所以我们很多时候会在外层包裹一个div原生（或者使用后面我们学习的Fragment）；
- 为了方便阅读，我们通常在jsx的外层包裹一个小括号()，这样可以方便阅读，并且jsx可以进行换行书写；
- JSX中的标签可以是单标签，也可以是双标签；
  - 注意：如果是单标签，必须以/>结尾；

### 2.3 使用

- 注释

  ```jsx
  {/*这是注释*/}
  ```

- JSX嵌入变量

  - 情况一：当变量是Number、String、Array类型时，可以直接显示
  - 情况二：当变量是null、undefined、Boolean类型时，内容为空；
    - 如果希望可以显示null、undefined、Boolean，那么需要转成字符串；
    - 转换的方式有很多，比如toString方法、和空字符串拼接，String(变量)等方式；
  - 情况三：对象类型不能作为子元素（not valid as a React child）

- JSX嵌入表达式

  - 运算表达式
  -  三元运算符
  -  执行一个函数

### 2.4 JSX的本质

- 实际上，jsx 仅仅只是 React.createElement(component, props, ...children) 函数的语法糖。
  - 所有的jsx最终都会被转换成React.createElement的函数调用。
- createElement需要传递三个参数：
  - 参数一：type
    - 当前ReactElement的类型；
    - 如果是标签元素，那么就使用字符串表示 “div”；

    - 如果是组件元素，那么就直接使用组件的名称；
  - 参数二：config
    - 所有jsx中的属性都在config中以对象的属性和值的形式存储
  -  参数三：children
    - 存放在标签中的内容，以children数组的方式进行存储；

### 2.5 阶段案例 列表的增删改查

```jsx
import React,{Component} from "react"
class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      list: [
        { id: 1, name: "老人与海", time: '2020', price: 20, count: 1 },
        { id: 2, name: "农夫与蛇", time: '2020', price: 20, count: 1 },
        { id: 3, name: "东郭先生与狼", time: '2020', price: 20, count: 1 },
        { id: 4, name: "吕洞宾与狗", time: '2020', price: 20, count: 1 },
      ]
    }
  }
  renderBook(){
    return((
      <div>
        <table>
          <thead>
            <tr>
              <th> </th>
              <th>书籍名称</th>
              <th>出版时间</th>
              <th>价格</th>
              <th>数量</th>
              <th>操作</th>
            </tr>
          </thead>
          <tbody>
            {this.state.list.map((item, index) => {
              return (
                <tr>
                  <td>{index + 1}</td>
                  <td>{item.name}</td>
                  <td>{item.time}</td>
                  <td>{this.formatPrice(item.price)}</td>
                  <td>
                    <button disabled={item.count === 0} onClick={()=>this.handleChange(index,-1)}>-</button>
                    &nbsp; {item.count} &nbsp;
                    <button onClick={()=>this.handleChange(index,1)}>+</button>
                  </td>
                  <td><span style={{ color: 'blue', cursor: 'pointer' }} onClick={e => this.remove(index)}>删除</span></td>
                </tr>
              )
            })}
          </tbody>
        </table>

        <h2>总价格：{this.formatPrice(this.getTotalPrice())}</h2>
      </div>
    ))
  }
  renderEmpty(){
    return(
      <h2>购物车为空</h2>
    )
  }
  render() {
    return this.state.list.length? this.renderBook():this.renderEmpty()
  }
  handleChange(index,count){
    const newList = [...this.state.list]
    newList[index].count += count
    this.setState({
      list: newList
    })
  }
  remove(index) {
    this.setState({
      list: this.state.list.filter((i, indey) => index !== indey)
    })
  }
  formatPrice(data) {
    if (typeof data != "number") {
      data = Number(data) || 0
    }
    return "$" + data.toFixed(2)
  }
  getTotalPrice() {
    return this.state.list.reduce((pre, i) => pre += i.count * i.price, 0)
  }
}

export default App;

```



## 3 脚手架

- 安装
  
- `npm install -g create-react-app`
  
- 创建项目
  
  - `create-react-app 项目名称`
  
- 项目运行
  
  - `yarn start`
  
- 目录结构

  

## 4 组件

### 4.1 类组件

-  类组件的定义有如下要求： 
  - 组件的名称是大写字符开头（无论类组件还是函数组件）
  - 类组件需要继承自 React.Component
  - **类组件必须实现render函数**
- 在ES6之前，可以通过create-react-class 模块来定义类组件，但是目前官网建议我们使用ES6的class类定义。
- 使用class定义一个组件：
  - constructor是可选的，我们通常在constructor中初始化一些数据；
  
    ```jsx
  constructor(props){
            super(props)
            this.state = {
                msg:"hello"
            }
        }
    ```
  
    
  
  - this.state中维护的就是我们组件内部的数据；
  
  - render() 方法是 class 组件中唯一必须实现的方法；
  
- render函数的返回类型：
  - React 元素：
    - 通常通过 JSX 创建。
    - 例如，<div /> 会被 React 渲染为 DOM 节点，<MyComponent /> 会被 React 渲染为自定义组件； 
    - 无论是 <div /> 还是 <MyComponent /> 均为 React 元素。 
  - 数组或 fragments：使得 render 方法可以返回多个元素。
  - Portals：可以渲染子节点到不同的 DOM 子树中。
  - 字符串或数值类型：它们在 DOM 中会被渲染为文本节点
  - 布尔类型或 null：什么都不渲染。

```jsx
class App extends React.component{
    constructor(props){
        super(props)
        // 父组件的值
        this.state = {
            // 子组件本身的值
            msg:"hello"
        }
    }
    render(){
        return(
            <>
            <h2>{msg}</h2>
            </>
        )
    }
}
ReactDOM.render(<App/>,document.getElementById("app"))
```

> 小技巧 安装react插件
>
>  输入rcc可快速生成类组件模板
> 输入

### 4.2 函数式组件

```jsx
function App(){
    return(
        <div>这是一个函数式组件</div>
    )
}
```

- 区别
  - 函数式组件没有`this`
  - 没有内部状态
  - 没有生命周期

### 4.3  生命周期

-  Constructor
  -  如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。
  - constructor中通常只做两件事情：
  - 通过给 this.state 赋值对象来初始化内部的state；
  - 为事件绑定实例（this）；
-  componentDidMount
  - componentDidMount() 会在组件挂载后（插入 DOM 树中）立即调用。
  - componentDidMount中通常进行哪里操作呢？
    - 依赖于DOM的操作可以在这里进行；
    -  在此处发送网络请求就最好的地方；（官方建议） 
    - 可以在此处添加一些订阅（会在componentWillUnmount取消订阅）；
- componentDidUpdate
  - componentDidUpdate() 会在更新后会被立即调用，首次渲染不会执
    行此方法。
  - 当组件更新后，可以在此处对 DOM 进行操作；
  - 如果你对更新前后的 props 进行了比较，也可以选择在此处进行网
    络请求；（例如，当 props 未发生变化时，则不会执行网络请求）。
  - 三个参数：
    - preProps
    - prevState
    - snapshot
- componentWillUnmount
- componentWillUnmount() 会在组件卸载及销毁之前直接调用。
  -  在此方法中执行必要的清理操作；
  -  例如，清除 timer，取消网络请求或清除 在 componentDidMount() 中创建的订阅等；

```jsx
import React, { Component } from 'react'

export default class App extends Component {
  constructor(){
    super()
    console.log("constructor执行")
  }
  render() {
    console.log('render方法执行')
    return (
      <div>
        
      </div>
    )
  }
  componentDidMount(){
    console.log('组件挂载')
  }
  componentDidUpdate(){
    console.log('组件更新')
  }
  componentWillUnmount(){
    console.log('组件卸载 ')
  }

}

```

### 4.4 组件的通信

#### 4.4.1 基本通信

- 父传子 `props`

  - 类组件

  ```jsx
  import React, { Component } from 'react'
  class ChildCpn extends Component{
    /*
      constructor(props){
         // super()
         //  this.props = props
          super(props)
    	}
      constructor不实现也可以传递
      原理：
      	底层父类Component已经处理过了 
      */
    render(){
        const {anme,age} = this.props
      return(
      <h2>父组件传过来的数据：{"姓名" + name +"，年龄"+ age}</h2>
      )
    }
  }
  
  
  export default class App extends Component {
    constructor(){
      super()
    }
    render() {
      return (
        <div>
          <ChildCpn name="why" age="18"/>
        </div>
      )
    }
  
  }
  ```

  >  props补充
  >
  > ```jsx
  >  constructor(props){
  >      // super()
  >      //  this.props = props
  >      // 等价于
  >      super(props)
  >  }
  > 
  > 
  > // =============
  >  constructor(props){
  >      super()
  >      console.log(this.props) // undefined
  >  }
  > 
  > render(){
  >     console.log(this.props) // render()里面可以获取到
  > }
  > 
  > componentDidMount(){
  >     console.log(this.props) // componentDidMount()里面可以获取到
  > }
  > 
  > ```
  >
  > 

  

  - 函数式组件

  ```jsx
  import React, { Component } from 'react'
  function ChildCpn (props){
    render(){
        const {anme,age} = props
      return(
      <h2>父组件传过来的数据：{"姓名" + name +"，年龄"+ age}</h2>
      )
    }
  }
  
  
  export default class App extends Component {
    constructor(){
      super()
    }
    render() {
      return (
        <div>
          <ChildCpn name="why" age="18"/>
        </div>
      )
    }
  
  }
  ```

  - 属性验证 依赖`prop-types` 或者使用`ts`

  ```jsx
  import React, { Component } from 'react'
  import PropType from "prop-types"
  class ChildCpn extends Component{
    render(){
      return(
      <h2>父组件传过来的数据：{"姓名" + this.props.name +"，年龄"+ this.props.age}</h2>
      )
    }
  }
  
  ChildCpn.propType = {
    name:PropType.string.isRequired,// 必传属性
    age:PropType.number,
    list:PropType.array
  }
  // 默认值
  ChildCpn.defaultProps = {
      name:"why",
      age: 18,
      list:["a","b"]
  }
  export default class App extends Component {
    constructor(){
      super()
    }
    render() {
      return (
        <div>
          <ChildCpn name="why" age={18} list={["hello","react"]}/>
        </div>
      )
    }
  
  }
  ```

  - 写法二 `static`

  ```jsx
  class ChildCpn extends Component{
    // 默认属性写法二
     static propTypes = {
          name:PropType.string.isRequired,// 必传属性
    		age:PropType.number,
    		list:PropType.array
     }
      static defaultProps = {
          name:"why",
          age: 18,
          list:["a","b"]
      }
    render(){
      return(
      <h2>父组件传过来的数据：{"姓名" + this.props.name +"，年龄"+ this.props.age}</h2>
      )
    }
  }
  ```

  

- 子传父 `props传入回调函数 子组件调用函数`

  ```jsx
  class ChildCpn extends PureComponent{
      const {btnClick} = this.props
      render(<button onClick={btnClick}> + 1</button>)
  }
  
  
  class App extends PureComponent{
       constructor(props){
           super(props)
           this.state = {
               count: 0
           }
       }
      render(){
          return(
          	<div> 
              	<span>{this.state.count}</span>
                  <ChildCpn btnClick={this.increment}/>
                  {/*<ChildCpn btnClick={e => this.increment() 也可以}/> */}
                  {/*<ChildCpn btnClick={this.increment.bind(this) 也可以}/> */}
              </div>
          )
      }
      // 写成箭头函数是为了解决this的绑定问题
      increment = () =>{
          this.setState({
              count: this.state.count + 1
          })
      }
  }
  ```

#### 4.4.2 react 实现类似 v-slot

```jsx
class App extends PureComponent{
    render(){
        return(
        	<div>
                {/*只有一个插槽的时候 不用考虑顺序问题的时候可以用这种*/}
                <NavBar>
                	<span>传递过去的元素</span>
                </NavBar>
                <NavBar2 leftSlot={<span>左边插槽内容</span>}
                    		midSlot={<span>中间插槽内容</span>}
                    />
            </div>
        )
    }
}
```

> 方式一：子组件通过`this.props.children` 获取

```jsx
class NavBar extends PureComponent{
    
    render(){
        
        return(
        	<div className="nav-bar">
            	<div className="nav-left">
                	{this.props.children[0]}
                </div>
            	<div className="nav-mid">
                	{this.props.children[1]}
                </div>
            	<div className="nav-right">
                	{this.props.children[2]}
                </div>
            </div>
        )
    }
}
```

> 方式二 直接把jsx作为属性传递

```jsx
class NavBar2 extends PureComponent{
    
    render(){
        const {leftSlot,midSlot} = this.props
        return(
        	<div className="nav-bar">
            	<div className="nav-left">
                	{leftSlot}
                </div>
            	<div className="nav-mid">
                	{midSlot}
                </div>
            </div>
        )
    }
}
```



```scss
.nav-bar{
    display:flex;
    height:44px;
   .nav-left,.nav-right{
        width:70px;
    }
    .nav-mid{
        flex:1
    } 
}

```

#### 4.4.3 跨组件通信

```jsx
function Header(props){
    return(
    	<div>
        	<h2>name:{props.name}</h2>
        	<h2>level：{props.level}</h2>
        </div>
    )
}

function Profile(props){
    return(
    	<div>
        	<Header name={props.name} level={props.level} />
        </div>
    )
}

class App extends PureComponent{
    constructor(props){
        super(props)
        this.state = {
            name:"张三",
            level:999
        }
    }
    render(){
        const {name,level} = this.state
        return(
        	<div>
                <Profile name={name} level={level} />
            </div>
        )
    }
}
```

- 缺点：

  - 繁琐
  - 冗余

- 解决： 属性展开

  ```jsx
  function Profile(props){
      return(
      	<div>
          	<Header {...props} />
          </div>
      )
  }
  
  
  class App extends PureComponent{
      constructor(props){
          super(props)
          this.state = {
              name:"张三",
              level:999
          }
      }
      render(){
          return(
          	<div>
                  <Profile {...this.state} />
              </div>
          )
      }
  }
  ```

  

### 4.7 context

> 解决中间层不需要传递props

- API
  - `React.createContext`
  - `Context.Provider`
  - `Class.contextType`
  - `Context.Consumer`

```jsx
// 创建context对象 
const UserContext = React.createContext({
    // 默认值
    name:"张三",
    level:0
})
// ================类组件================
class Header extends PureComponent{
    render(){
         console.log(this.context)
        // {name:"张三",level:999}
        return(
            <div>
                <h2>name:{this.context.name}</h2>
                <h2>level：{this.context.level}</h2>
            </div>
        )
    }
}
Header.contextType = UserContext


//==================函数组件===================
function Header(){
    return(
    	<UserContext.Consumer>
            {
                value => {
                    return (
                        <div>
                            <h2>name:{value.name}</h2>
                            <h2>level：{value.level}</h2>
                        </div>
                    )
                }
            }
        </UserContext.Consumer>
    )
}
// ++++++++++++++++++++++++++++++++++++++++++++++++++++++++

function Profile(props){
    return( 
    	<div>
        	<Header />
        </div>
    )
}



class App extends PureComponent{
    constructor(props){
        super(props)
        this.state = {
            name:"张三",
            level:999
        }
    }
    render(){
        return(
        	<div>
                <UserContext.Provider value={this.state}>
                    {/*value这个地方也可以传对象{{name:"",level:}}*/}
					<Profile />	                
                </UserContext.Provider>
            </div>
        )
    }
}
```

- 多个context

```jsx
const UserContext = React.createContext({
    name:"张三",
    level:0
})
const ThemeContext = React.createContext({
    color:"red"
})
function Header(){
    return(
    	<UserContext.Consumer>
            {
                value => {
                    return (
                        <ThemeContext.Consumer>
                        	{
                                theme =>{
                                    return(
                                    	<div>
                                            <h2>name:{value.name}</h2>
                                            <h2>level：{value.level}</h2>
                                            <h2>color:{theme.color}</h2>
                                        </div>
                                    )
                                }
                            }
                        </ThemeContext.Consumer>
                    )
                }
            }
        </UserContext.Consumer>
    )
}
function Profile(props){
    return( 
    	<div>
        	<Header />
        </div>
    )
}
class App extends PureComponent{
    constructor(props){
        super(props)
        this.state = {
            name:"张三",
            level:999
        }
    }
    render(){
        return(
        	<div>
                <UserContext.Provider value={this.state}>
                    <ThemeContext.provider value={{color:"red"}}
                        <Profile />
                    </ThemeContext.provider>
                </UserContext.Provider>
            </div>
        )
    }
}
```



### 4.8 `setState`

- setState方法是从Component中继承过来的。

- setState是异步更新

  > setState设计为异步，可以显著的提升性能；
  >
  > - 如果每次调用 setState都进行一次更新，那么意味着render函数会被频繁调用，界面重新渲染 ，这样效率是很低的；
  > -  最好的办法应该是获取到多个更新，之后进行批量更新；
  >   
  > - 如果同步更新了state，但是还没有执行render函数，那么state和props不能保持同步；
  >    state和props不能保持一致性，会在开发中产生很多的问题；

  ```jsx
  this.setState({msg: 'hello word'})
  console.log(this.state.msg) // 还是原来的msg
  
  // 第二个参数 回调函数 类似于vue的nextTick
  this.setState({msg: 'hello word'},()=>{
      console.log(this.state.msg)
      // hello world
  })
  
   componentDidUpdate(){
         console.log(this.state.msg)
      // hello world 先执行的这个
  }
  ```

  

- `setState`同步更新

  ```jsx
  // 方式一 settimeout
  setTimeout(()=>{
      this.setState({msg: 'hello word'})
      console.log(this.state.msg) // hello world
  },0)
  
  
  //方式二 原始事件监听
  render(){
      return(
          <div>
              <h2>{this.state.msg}</h2>
              <button id="btn">改变文本</button>
          </div>
      )
  }
  componentDidMount(){
      document.getElementById("btn").addEventListener("click",()=>{
           this.setState({msg: 'hello word'})
              console.log(this.state.msg) // hello world
      })
  }
  
  
  ```

  > setState的两种情况：
  > Ø 在组件生命周期或React合成事件中，setState是异步；
  > Ø 在setTimeout或者原生dom事件中，setState是同步；

- 数据的合并

  ```jsx
  // setState底层实现了数据合并 
  Object.assign({},this.state,{msg:"hello world"})
  
  
  // 多次setState会在内部合并
  this.setState({count:this.state.count ++})
  this.setState({count:this.state.count ++})
  this.setState({count:this.state.count ++})
  // 最终结果 只会加1
  
  
  
  // setState合并时进行累加
  this.setState((preState,props)=>{
      return {
          count: preState.count++
      }
  })
  this.setState((preState,props)=>{
      return {
          count: preState.count++
      }
  })
  this.setState((preState,props)=>{
      return {
          count: preState.count++
      }
  })
  // 最终结果 + 3
  ```

- setState不可变的力量

  

### 4.9 性能优化

#### 4.9.1 React的更新机制

优化的diff算法 

-  同层节点之间相互比较，不会跨节点比较；
- 不同类型的节点，产生不同的树结构；
- 开发中，可以通过key来指定哪些节点在不同的渲染下保持稳定；

```jsx
//没有key  子节点一个一个比较 后面添加数据 影响不大
this.setState({
    list:[...this.state.list,"c"]
})

//没有key 列表前面添加数据 全部重新渲染
this.setState({
    list:["c",...this.state.list,]
})

// 有key 相同元素 平移操作 不会重新渲染

```

-  key应该是唯一的；
-  key不要使用随机数（随机数在下一次render时，会重新生成一个数字）； 
- 使用index作为key，对性能是没有优化的；

#### 4.9.2 `shouldComponentUpdate`

 React给我们提供了一个生命周期方法 shouldComponentUpdate（很多时候，我们简称为SCU），这个方法接受参数，并且需要有返回值：

- 两个参数：
  - 参数一：nextProps 修改之后，最新的props属性
  - 参数二：nextState 修改之后，最新的state属性
- 该方法返回值是一个boolean类型
  - 返回值为true，那么就需要调用render方法；
  - 返回值为false，那么久不需要调用render方法；
  - 默认返回的是true，也就是只要state发生改变，就会调用render方法；
- 比如我们在App中增加一个message属性：
  - jsx中并没有依赖这个message，那么它的改变不应该引起重新渲染；
  - 但是因为render监听到state的改变，就会重新render，所以最后render方法还是被重新调用了；

```jsx
shouldComponentUpdate(nextProps,nextState){
    return this.state.count !== nextState.count
}
```

#### 4.9.3  `pureComponent`

- class继承自PureComponent 自动实现`shouldComponentUpdate` return false

```jsx
class Main extends PureComponent{
    
}
```

- memo

  > `pureComponent`只能解决class组件

  ```jsx
  import React,{memo} from 'react'
  
  const MemoHeader = memo(function(){
      return <h2>header组件</h2>
  })
  ```

  

### 4.10 事件总线

- 依赖第三方库`events`

  ```shell
  # 安装
  yarn add events
  ```

- 发出事件

  - 封装导出

  ```js
  import { EventEmitter } from "events"
  
  export const eventBus = new EventEmmitter()
  ```

  - 导入使用

  ```jsx
  import {eventBus} from '../xxx'
  
  class App extends PureComponent{
      emmitEvent(){
          eventBus.emit(string | symbol,...arg)
      }
  }
  ```

  - 监听

  ```jsx
  componentDidMount(){
      eventBus.addListener("eventName",(args)=>{
          
      })
  }
  componentWillUnmount(){
      eventBus.removeListener("eventName",(args)=>{
          // 取消哪个写哪个的回调函数
      })
  }
  ```



### 4.11  受控和非受控组件

#### 4.11.1 ref

- 获取ref的三种方式

  - `ref=字符串`(不推荐)

    ```jsx
    <h2 ref="titleRef"></h2>
    // 使用
    this.refs.titleRef
    ```

  - `ref={}`(推荐)

    ```jsx
    import {createRef} from "react"
    
    class App extends PureComponent{
        constructor(props){
            super(props)
            this.titleRef = createRef()
        }
    }
    
    <h2 ref={this.titleRef}></h2>
    
    // 使用
    this.titleRef.current
    ```

  - 回调函数

    ```jsx
    import {createRef} from "react"
    
    class App extends PureComponent{
        constructor(props){
            super(props)
            this.titleEl = null
        }
    }
    
    <h2 ref={arg => this.titleEl = arg}></h2>
    
    // 使用
    this.titleEl
    ```

- ref的类型

  - 用在dom元素上面的时候,获取到的是dom对象

  - 用在组件上的时候,获取到的是组件对象 通过.current获取

  - 不能用在函数式组件 因为他们没有实例

    > 获取函数式组件中的dom元素可以使用`React.forwardRed`

#### 4.12.2 受控组件

> 在React中，HTML表单的处理方式和普通的DOM元素不太一样：表单元素通常会保存在一些内部的state。

- 可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 setState()来更新。
  - 我们将两者结合起来，使React的state成为“唯一数据源”； 
  - 渲染表单的 React 组件还控制着用户输入过程中表单发生的操作；
  - 被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”；
- 由于在表单元素上设置了 value 属性，因此显示的值将始终为 this.state.value，这使得 React 的 state 成为唯一数据源。 

```jsx
render(){
    return(
    	<div>
        	<form onSubmit={e => e.preventDefault()}>
                {/*jsx里面 原生事件依然可以使用*/}
                <input 
                    type="text" 
                    id="name" 
                    onChange={e=>handleOnchange(e)}
                    value={this.state.name}
				 />
                <input type="submit" onClick={()=>handleSubmit()}/>
            </form>
        </div>
    )
}
handleOnchange(e){
	this.setState({
        // name:e.target.value
        // 计算属性名 
        [e.target.name]: e.traget.value
    })
}
handleSubmit(){
    console.log(this.state.name)
}
```

#### 4.13.3 非受控组件(不推荐)

```jsx
render(){
    return(
    	<div>
        	<form onSubmit={e=>this.handleSubmit(e)}>
                <input 
                    type="text" 
                    id="name" 
                    ref={this.nameRef}
				 />
                <input type="submit"/>
            </form>
        </div>
    )
}
handleSubmit(e){
    e.preventDefault()
    console.log(this.nameRef.current.value)
}
```
### 4.12 高阶组件

> High-Ordered Component   简称HOC
>
> 参数为组件 返回值为组件的**函数**
>
> 不是react的Api
>
> 在 redux react-router中非常常见

- 定义

  ```jsx
  class App extends PureComponent{
      
  }
  App.displayName = "aaa"
  // 修改组件的名字 (调试工具)
  
  function enhanceCpn(WrappedCpn){
      //NewCpn 可以省略 省略后 结构显示父类名字 PureComponent
      class NewCpn extends PureComponent {
          render(){
              return <WrappedCpn />
          }
      }
      NewCpn.displayName = "bbb"
      return NewCpn 
  }
  
  const EnhanceCpn = enhanceCpn(App)
  
  export default EnhanceCpn
  
  // 此时的组件的结构 (通过调试工具控制台查看)
  // NewCpn > App
  // const aaa = class{
   	// 此时的类名可以省略
  // }
  ```

- 应用

  - 增强props

    > 场景:给`Home`和`About`组件增加地区`region`属性

    ```jsx
    // 定义一个高阶组件
    function enhanceRegionProps(wrappedCpn){
        return props => {
            return <wrappedCpn {...props} region="中国" />
        }
    }
    
    class Home extends PureComponent{
        render(){
            return (
                <h2>
                    home组件: {`昵称${this.props.name},
    							等级${this.props.level},
    							区域${this.props.region}`
                            }
                </h2>
            )
    }
        
    class About extends PureComponent{
        render(){
            return (
                <h2>
                   about组件: {`昵称${this.props.name},
    							等级${this.props.level},
    							区域${this.props.region}`
                            }
                </h2>
            )
    }
        
    
    const EnhanceHome = enhanceRegionProps(Home)
    const EnhanceAbout = enhanceRegionProps(About)
        
    class App extends PureComponent{
        render(){
            return(
                <div>
                	<h2>这是App组件</h2>
                    <EnhanceHome name="why" level={11}/>
                    <EnhanceAbout name="zhangsan" level={18} />
                </div>
            )
        }
    }
    ```

  - 增强props(二)（劫持props）

    ```jsx
    // 创建context
    const UserContext = createContext({
        name:"默认姓名",
        age:18,
        region:"中国"
    })
    
    // 定义一个高阶组件
    function withUser(wrappedCpn){
        return props => {
            return (
                <UserContext.Consumer>
                	{
                        user => {
                            return (
                            	<wrappedCpn {...props} {...user}/>
                            )
                        }
                    }
                </UserContext.Consumer>
           	)
        }
    }
    class Home extends PureComponent{
        render(){
            return (
                <h2>
                   home组件: {`昵称${this.props.name},
    							等级${this.props.level},
    							区域${this.props.region}`
                            }
                </h2>
            )
    }
        
    class About extends PureComponent{
        render(){
            return (
                <h2>
                   about组件: {`昵称${this.props.name},
    							等级${this.props.level},
    							区域${this.props.region}`
                            }
                </h2>
            )
    }
    
    const UserHome = withUser(Home)
    const UserAbout = withUser(About)
        
    class App extends PureComponent{
        render(){
            return(
                <div>
                	<h2>这是App组件</h2>
                    <UserContext.provider value={{name:"张三",age:18,level:99}}>
        	            <UserHome />
    	                <UserAbout />
                    </UserContext.provider>
                </div>
            )
        }
    }
    ```

  - 渲染判断鉴权(劫持jsx)
  
    ```jsx
    function withAuth(WrappedCpn){
        const NewCpn props => {
            const {isLogin} = props
            if(isLOgin){
            	return <WrappedCpn {...props}/>
            } else {
                return <LoginPage />
            }
        }
        return NewCpn
    }
    class LoginPage extends PureComponent{
        render(){
            return <h2>请先登录</h2>
        }
    } 
    class CartPage extends PureComponent{
        render(){
            return <h2>CartPage</h2>
        }
    }
    
    class App extends PureComponent{
        render(){
            return(
            	<div>
                	<CartPage />
                </div>
            )
        }
    }
    ```
  
  - 生命周期劫持
  
    ```jsx
    function withTimer(wrappedCpn){
        
        return class extends PureComponents {
            UNSAFE_componentWillMount(){
                console.time(`${wrappedCpn.name}渲染所用时间`)
            }
            componentdidMount(){
                console.timeEnd(`${wrappedCpn.name}渲染所用时间`)
            }
            return (
            	<wrappedCpn {...this.props} />
            )
        }
    }
    
    
    class Home extends PureComponent{
        
        render(){
            return (
                <h2>
                   home
                </h2>
            )
    }
        
    class About extends PureComponent{
        render(){
            return (
                <h2>
                   about
                </h2>
            )
    }
        
    const  HomeWithTime = withTimer(Home)
    const  AboutWithTime = withTimer(About)
    
    
    class App extends PureComponent{
        render(){
            return(
                <div>
                    <HomeWithTime />
                    <AboutWithTime />
                </div>
            )
        }
    }
    ```

### 4.13 组件补充

- ref的转发

  > 函数式组件没有ref  
>
  > ref不是属性 不能传到props

  ```jsx
  import {forwardRef} from "react"
  const Profile = forwardRef(
  	function(props,ref){
        return <h2 ref={ref}>函数式组件</h2>
      }
  )
  
  class App extends PureComponent{
      constructor(props){
          super(props)
          this.titleRef = createRef()
          this.profileRef = createRef()
      }
      render(){
          return(
              <div>
              	<h2 ref={this.titleRef}>helloRef</h2>
                  <Profile ref={this.profileRef} />
              </div>
          )
      }
  }
  ```

- `Portals`的使用

  > 在开发中希望组件独立于父组件 甚至独立于当前挂载的Dom元素（`#root`） 使用portals
  >
  > 例子（全屏弹窗）

  - 参数
    
    - 第一个参数（child）是任何可渲染的React子元素，例如字符串、元素、 fragment
    - 第二个参数（container） 是一个DOM元素
    
  - 语法
  
    ```jsx
    ReactDOM.createPortal(child,container)
    ```
  
  - 案例
  
    ```jsx
    import React,{PureComponent} from "react"
    import ReactDOM from "react-dom"
    
    class Home extends PureComponent{
        render(){
            return(
            	<div>
                	<h2>home</h2>
                    <Modal>
                    	<h2>title</h2>
                        <p>paragraph</p>
                    </Modal>
                </div>
            )
        }
    }
    class Modal extends PureComponent{
        render(){
            return (
            	ReactDOM.createPortal(
                	this.props.children,
                    document.getElementByIdd("modal")
                )
            )
        }
    }
    ```
  
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <div id="root"></div>
        <div id="modal">
            
        </div>
    </body>
    </html>
    ```
  
    ```css
    #modal{
        position:fixed;
        left:50%;
        top:50%;
        transform: translate(-50%,-50%);
    }
    ```
  
- `Fragment`的使用

  ```jsx
  import React,{PureComponent,Fragment} from "react"
  class Home extends PureComponent{
      render(){
          return(
          	<Fragment>
                  <h2>title</h2>
                  <button>+1</button>
              </Fragment>
          )
      }
  }
  ```

  - 短语法

    ```jsx
    import React,{PureComponent,Fragment} from "react"
    class Home extends PureComponent{
        render(){
            return(
            	<>
                    <h2>title</h2>
                    <button>+1</button>
                </>
            )
        }
    }
    ```

  - **短语法不能添加任何属性**

- 严格模式

  ```js
  ReactDOM.render(
  	<React.StrictMode>
      	<App />
      <React.StrictMode>,
      document.getElementById("root")
  )
  ```

  > 严格模式
  >
  > - StrictMode 是一个用来突出显示应用程序中潜在问题的工具。
  >   - 与 Fragment 一样，StrictMode 不会渲染任何可见的 UI；
  >   - 它为其后代元素触发额外的检查和警告；
  >   - 严格模式检 查仅在开发模式下运行；它们不会影响生产构建；
  >
  > ```jsx
  > ReactDOM.render(
  >     <Header />
  > 	<React.StrictMode>
  >     	<Cpn />
  >     <React.StrictMode>,
  >     <Footer />
  >     document.getElementById("root")
  > )
  > ```
  >
  > 
  >
  > - 可以为应用程序的任何部分启用严格模式：
  >   - 不会对 Header 和 Footer 组件运行严格模式检查；
  >   - 但是，ComponentOne 和 ComponentTwo 以及它们的所有后代元素都将进行检查；

  - 作用

    1. 识别不安全的生命周期

    2. 使用过时的ref API

    3. 使用废弃的findDOMNode方法

    4. 检查意外的副作用

       1. 这个组件的constructor会被调用两次；
       2. 这是严格模式下故意进行的操作，让你来查看在这里写的一些逻辑代码被调用多次时，是否会产生一些副作用；
       3. 在生产环境中，是不会被调用两次的；

    5. 检测过时的context API

       早期的Context是通过static属性声明Context对象属性，通过getChildContext返回Context对象等方式来使用Context的；

## 6 REACT 的CSS

### 6.1 内联样式

```jsx
class App extends Component{
    constructor(props){
        super(props)
        this.state = {
            color:"red"
        }
    }
    const pStyle = {
        color:this.state.color,
        fontSize:"20px"
    }
    render(){
        return(
        	<div>
            	<h2 style={{color:"red",fontSize:"32px"}}>test</h2>
                <p style={pStyle}>内联样式</p>
            </div>
        )
    }
} 
```

- 内联样式是官方推荐的一种css样式的写法：
  - style 接受一个采用小驼峰命名属性的 JavaScript 对象，，而不是 CSS 字符串；
  - 并且可以引用state中的状态来设置相关的样式；
- 优点
  - 内联样式, 样式之间不会有冲突
  - 可以动态获取当前state中的状态
- 缺点
  - 写法上都需要使用驼峰标识
  - 某些样式没有提示
  - 大量的样式, 代码混乱
  - 某些样式无法编写(比如伪类/伪元素)



### 6.2 普通css

```jsx
import "./style.css"

class App extends Component{
    render(){
        return(
        	<div>
            	<h2 className="title">test</h2>
            </div>
        )
    }
} 
```

- 缺点
  - 繁琐
  - 冲突

### 6.3 css modules

> - css modules并不是React特有的解决方案，而是所有使用了类似于webpack配置的环境下都可以使用的。
>   - 但是，如果在其他项目中使用个，那么我们需要自己来进行配置，比如配置webpack.config.js中的modules: true等。
> - React的脚手架已经内置了css modules的配置：
>   - **.css/.less/.scs s 等样式文件都修改成 .module.css/.module.less/.module.scss 等；**
>   - 之后就可以引用并且进行使用了；
>   - css modules确实解决了局部作用域的问题，也是很多人喜欢在React中使用的一种方案。

```jsx
import AppStyle from "./style.module.css"

class App extends Component{
    render(){
        return(
        	<div>
            	<h2 style={AppStyle.title}>test</h2>
            </div>
        )
    }
} 
```

- 缺点
  - 引用的类名，不能使用连接符(.home-title)，在JavaScript中是不识别的；
  -  所有的className都必须使用{style.className} 的形式来编写；
  - 不方便动态来修改某些样式，依然需要使用内联样式的方式；



### 6.4 CSS in JS

> “CSS-in-JS” 是指一种模式，其中 CSS 由 JavaScript 生成而不是在外部文件中定义；
> ***注意此功能并不是 React 的一部分，而是由第三方库提供。*** React 对样式如何定义并没有明确态度；
>
> - `styled-components`
> - `emotion`
> - `glamorous`

- 第三方库 `styled-components`

  - 安装

  ```shell
  # yarn
  yarn add styled-components 
  # npm
  npm install styled-components
  ```

- 标签模板字符串的使用

  ```js
  const name = "张三"
  const age = 18
  function foo(...arg){
      console.log(arg)
  }
  
  foo`my name is ${name},age is ${age}`
  // 输出二维数组[[["my name is ", ",age is ", "",],"张三",18]
  //["my name is ", ",age is ", "",] 是被变量分割的
  ```

  

- 使用

  - style.js
  
  ```js
  import styled from "styled-components"
  export const HomeWrapper = styled.div`
  	font-size: 50px;
  	color:red;
  	.banner{
  		background-color: blue;
  		span {
  			color: white;
  			&.active{
  				color:red;
  			}
  			&:hover{
  				color:#ccc;
  			}
  			&::after{
  				content:"aaa";
  			}
          }
      }
  `
  ```
  
  ```jsx
  import {HomeWrapper} from "./style"
  // 返回一个react组件
  
  class App extends Component{
      render(){
          return(
          	<HomeWrapper>
              	<h2>test</h2>
                  <div className="banner">
                	<span className="active">轮播图1</span>
                  	<span>轮播图1</span>
                  	<span>轮播图1</span>
                  </div>
              </HomeWrapper>
          )
      }
  } 
  ```
  
- 属性穿透

  - 写法一

    ```jsx
    const MyInput = styled.input`
    	bgc:blue;
    `
    
    class App extends Component{
        render(){
            return(
            	<div>
                	<MyInput type="password"/>
                </div>
            )
        }
    } 
    
    ```

    

  - 写法二 

    ```jsx
    const MyInput = styled.input.attrs({
        placeholder:"why",
        // 可以自定义属性  调用使用箭头函数
        bColor:"red"
    })`
    	bgc:blue;
    	border-color:${props=>props.bColor}
        font-size:${props=> props.fontSize}
    `
    
    class App extends Component{
        constructor(){
            super(props)
            this.state = {
                fontSize:"12px"
            }
        }
        render(){
            return(
            	<div>
                	<MyInput fontSize={this.state.fontSize}/>
                </div>
            )
        }
    } 
    
    ```

- 继承

  ```jsx
  import styled from "styled-components"
  const MyButton = styled.button`
  	padding: 10px 20px;
  	color:red;
  `
  // 继承
  const PrimaryButton = styled(MyButton)`
  	bgc:#ccc;
  `
  
  
  class App extends Component{
      render(){
          return(
          	<div>
                  <MyButton >我是按钮</MyButton>
                   <PrimaryButton >继承样式的button</PrimaryButton>	
              </div>
          )
      }
  } 
  
  ```

- 主题

  ```jsx
  import styled,{ ThemeProvider } from "styled-components"
  const MyButton = styled.button`
  	padding: 10px 20px;
  	color:red;
  `
  // 继承
  const PrimaryButton = styled(MyButton)`
  	bgc:#ccc;
  `
  
  
  class App extends Component{
      render(){
          return(
              {/*穿透所有*/}
          	<ThemeProvider theme={{themeColor:"yellow", fontSize:"30px"}}>
                  <MyButton >我是按钮</MyButton>
                   <PrimaryButton >继承样式的button</PrimaryButton>	
              </ThemeProvider>
          )
      }
  } 
  ```

### 6.5 `classnames`库

- 安装

  ```shell
  yarn add classnames
  ```

- 使用

  ```jsx
  import classNames from "classnames"
  const errClass = "err"
  class App extends Component{
      render(){
          return(
              <div>
                  {/*原生*/}
              	<h2 className={"foo bar"}></h2>
              	<h2 className={"foo " +  isActive?"bar":""}></h2>
              	<h2 className={["foo","bar"].join" "}></h2>
                  {/*使用classNames库*/}
              	<h2 className={classNames("foo","bar",errClass)}></h2>
              	<h2 className={classNames({"foo":isActive,"bar":isBar},"title")}></h2>
              	<h2 className={classNames(["foo","bar",{"foo2":isActive,}]}></h2>
              	<h2></h2>
              </div>
          )
      }
  }
  ```



### 6.6 `antDesign`库

- 安装

  ```shell
  yarn add antd
  # 
  npm install antd
  ```

- 解决主题配置 --- 依赖`craco`

  - 安装

    ```shell
    yarn add @croco/croco
    ```

  - 修改`package.json`

    ```json
    "script":{
        "start":"croco start",
        "build":"croco build",
        "test":"croco test",
        "eject":"react-scripts eject"
    }
    ```

  - 安装`croco-less`
  
  ```shell
    yarn add croco-less
  ```
  
  - 创建`croco.config.js`

    > 这里的配置会和`webpack`的配置合并

    ```js
    const CrocoLessPlugin = require("croco-less")
    const path = require("path")
    const resolve = dir => path.resolve(__dirname, dir)
    module.exports = {
         plugins:[
             {
                 plugin:CracoLessPlugin,
                 options:{
                     lessLoaderOptions:{
                         lessOptions:{
                             modifyVars:{'@primary-color':"#1DA57A"},
                             javascriptEnabled:true
                         }
                     }
                 }
             }
         ],
        //**配置别名**
        webpack:{
        alias:{
                '@':resolve("src"),
                "components":resolve("src/components"),
                ...
            }
        }
    }
    ```
    
  - 修改导入.css文件为导入less
  
    

- [新版本解决方案]: https://ant.design/docs/react/customize-theme-cn

## 7 网络请求

### 7.1 axios的使用

- fetch

  > - Fetch是AJAX的替换方案，基于Promise设计，很好的进行了关注分离，有很大一批人喜欢使用fetch进行项目开发；
  > - 但是Fetch的缺点也很明显，首先需要明确的是Fetch是一个 low-level（底层）的API，没有帮助你封装好各种各样的功能和实现；
  > - 比如发送网络请求需要自己来配置Header的Content-Type，不会默认携带cookie等；
  > - 比如错误处理相对麻烦（只有网络错误才会reject，HTTP状态码404或者500不会被标记为reject）；
  > - 比如不支持取消一个请求，不能查看一个请求的进度等等；

- 安装

  ```shell
  yarn add axios
  ```

- 调用

  ```jsx
  import axios from "axios"
  
  class App extends Component{
      componentDidMount(){
          // 一般在本生命周期里面调用
          axios({
              url:String,
              params:{
                  name: "get"
              },
              // 默认get方式
          }).then(console.log)
          .catch(err => {
              console.log(err)
          })
      }
      render(){
          return (
              <div>
              
              </div>
          )
      }
  }
  ```

- axios.all

  ```js
  axios.all([req1,req2]).then(res => {
      console.log(res)
      // 结果是一个数组
  }).catch(err => {
      
  })
  ```

- `axios`的配置

  ```js
  axios.default.baseURL = ""
  axios.default.timeout = 5000
  axios.default.headers.common["token"] = ""
  axios.default.headers.common["Content-Type"] = "application/text"
  ```

- 创建新的实例

  ```js
  const instance = axios.create({
      // property
      // ...
  })
  ```

- 拦截器

  ```js
  // 请求拦截
  instance.interceptors.request.use(config =>{
      /**
          显示loading
          验证token
          序列化操作
      */
      return config
  },err =>{
      
  })
  // 响应拦截
  instance.interceptors.reponse.use(res => {
      return res.data
  },err =>{
      if(err && err.reponse){
          const {status} = err.reponse
          switch(status){
                  ...
          }
      }
      return err
  })
  ```

- 二次封装

  ```js
  const devBaseUrl = "开发时url"
  const proBaseUrl = "上线url"
  
  export const BASE_URL = process.env.NODE_ENV === "development"?devBaseUrl:proBaseUrl
  
  export const TIMEOUT = 5000
  ```

  

  ```js
  import axios from "axios"
  import {BASE_URL,TIMEOUT} from "./config"
  const instance = axios.create({
      baseURL: BASE_URL,
      timeout: TIMEOUT
  })
  // 拦截器...
  export default instance
  ```


### 7.2 react-router

- 安装

  ```shell
  yarn add react-router-dom
  # 安装react-router-dom自动安装react-router
  ```

- API

- 基本用法

  ```jsx
  import { BrowserRouter, Link, Route } from "react-router-dom"
  
  
  
  import Home from "./pages/home"
  import About from "./pages/about"
  import Profile from "./pages/profile"
  class App extends Components {
      render(){
          return(
          	<div>
                  <BrowserRouter>
                      
                  	<Link to="/">首页</Link>
                  	<Link to="/about">关于</Link>
                  	<Link to="/profile">我的</Link>
                      
                      <Route exact path="/" component={Home} />
                      <Route path="/about" component={About} />
                      <Route path="/profile" component={Profile} />
                  </BrowserRouter>
              </div>
          )
      }
  }
  // exact 属性表示精准匹配 不加 '/profile'也会渲染Home组件
  ```

  - 说明
    - `BrowserRouter`对应history模式 
    - `HashRouter`对应hash模式
    - `link`渲染为`a`标签
    - 渲染的位置和`Route`标签的位置有关

- `NavLink`

  ```jsx
  
  <NavLink exact to="/" activeStyle={{color:"red"}}>首页</NavLink>
  <NavLink to="/about" activeClass="link-active">关于</NavLink>
  <NavLink to="/profile" activeStyle={{color:"red"}}>我的</NavLink>
  
  
  //activeStyle匹配也是模糊匹配 NavLink也有exact属性 解决匹配穿透
  
  // 活跃的路由默认添加了active 的class 自定义以后就没有默认的active class
  ```

  

- `switch`

  ```jsx
  // 只会渲染一个
  import {Switch} from "react-router-dom"
  
  <NavLink exact to="/" activeStyle={{color:"red"}}>首页</NavLink>
  <NavLink to="/about" activeClass="link-active">关于</NavLink>
  <NavLink to="/profile" activeStyle={{color:"red"}}>我的</NavLink>
  <Switch>
  	<Route exact path="/" component={Home} />
  	<Route path="/about" component={About} />
  	<Route path="/profile" component={Profile} />
  	<Route path="/login" component={Login} />
  	<Route component={NoMatch} />
  </Switch>
  
  // ：表示动态路由
  // 不写path属性 所有路由都会显示
  ```

- `Redirect`

  ```jsx
  render(){
      return this.state.isLogin?(
          <h2>welcome</h2>
      ):<Redirect to="/login">
  }
  ```

- 路由嵌套

  ```jsx
  <NavLink exact to="/about/p1">关于1</NavLink>
  <NavLink to="/about/p2">关于2</NavLink>
  <NavLink to="/about/p3">关于3</NavLink>
  <Switch>
  	<Route exact path="/about/p1" component={P1} />
  	<Route path="/about/p1" component={P2} />
  	<Route path="/about/p1" component={P3} />
  </Switch>
  ```

- 手动路由跳转

  ```jsx
  <button onClick={e => this.handleJump()}></button>
  // 路由跳转过来的组件
  handleJump(){
      this.props.history.push("/about/join")
  }
  
  // 通过路由跳转 渲染的组件 会自动获得属性
  // match history location
  //  可以通过this.props.history...获得
  // history 不同于 window.history
  ```

- WithRouter

  ```jsx
  import {WithRouter} from "react-router-dom"
  
  class App extends Components{
      render(){
          return(
              <>
                  <NavLink exact to="/" >首页</NavLink>            
                  <button onClick={e => this.handleJump()}></button>
                  <Switch>
                      <Route exact path="/" component={Home} />
                      <Route path="/about/join" component={Join} />
                  </Switch>
              </>
          )
      }
      handleJump(){
          this.props.history.push("/about/join")
      }
  }
  
  
  export default WithRouter(App)
  ```

  index.js

  ```jsx
  ReactDOM.render(
      <BrowserRouter>
      	<App />
      </BrowserRouter>,
      #root
  )
  ```

- 动态路由

  ```jsx
  <BrowserRouter>
      ...
      <Link to={`/detail${id}`}>详情</Link>
      <Switch>
      	<Route path="/detail/:id/" component={Detail} />x
      </Switch>
  </BrowserRouter>
  ```

  ```jsx
  // Detail组件通过this.props.match获取id
  
  function Detail(props){
      const id = this.props.match.params.id
      return(
      	<h2>detail:{id}</h2>
      )
  }
  ```

- 传递参数

  > - 动态路由
  >
  > - search传递参数
  >
  >   ```jsx
  >   <Link to="/detail?name=why&age=18">详情</Link>
  >   <Switch>
  >       <Route path="/detail" component={Detail} />x
  >   </Switch>
  >   ```
  >
  >   ```jsx
  >   // 子组件通过this.props.location获取
  >   function Detail(props){
  >       const info = this.props.location.search
  >       return(
  >       	<h2>detail:{info}</h2>
  >       )
  >   }
  >   // 获取到的search是字符串 '?name=why&age=18'  不推荐这种方式
  >   ```
  >
  > - Link中to传入对象
  >
  >   > to 可以传 字符串 函数 对象
  >   >
  >   > 对象可以传
  >   >
  >   > - pathname: 跳转路径
  >   > - search
  >   > - hash
  >   > - state: 状态 
  >
  >   ```jsx
  >   <Link to={{
  >           pathname:"detail3",
  >           search:"?name=abc",
  >   		state:{
  >           	name:"zhangsan",
  >   			age: 18
  >           }
  >       }}>详情</Link>
  >   <Switch>
  >       <Route path="/detail" component={Detail} />x
  >   </Switch>
  >   
  >   // 子组件通过this.props.location获取
  >   ```
  
- react-router-config

  - 安装

    ```shell
    yarn add react-router-config 
    ```

  - 新建router/index.js

    ```js
    export const routes = [
        {
            path: "/",
            exact:true,
            component: Home
        },
        {
            path: "/about",
            component: About,
            routes:[
                {
                    path:"/about/p1",
                    exact:true,
                    component:P1
                }
            ]
        },
        {
            path: "/",
            component: Home
        },
    ]
    ```

  - 使用

    ```jsx
    import {renderRoutes} from "react-router-config"
    import {routes} from "./router"
    class App extends Component{
        render(){
            return(
            	<div>
                	{renderRoutes(routes)}	
                </div>
            )
        }
    }
    ```

    ```jsx
    // 子路由 使用
    class About extends Component{
        render(){
            // route只有 config方式 才有 
            let route = this.props.route
            return(
            	<div>
                	{/*renderRoutes(routes[1].routes)*/}
                    {renderRoutes(route.routes)}
                </div>
            )
        }
    }
    ```

  - matchRouter函数

    ```js
    let route = this.props.route
    const branch = matchRoutes(route.routes,"/about")
    
    // 返回一个数组 表示匹配到的路由
    ```

    

## 8 Redux

### 8.0 纯函数

> - 确定的输入，一定会产生确定的输出；
> -  函数在执行过程中，不能产生副作用；

### 8.1 redux 核心理念

- Store
- action
  - 所有数据的变化，必须通过派发（dispatch）action来更新；
  - action是一个普通的JavaScript对象，用来描述这次更新的type和content；

- reducer
  - reducer是一个纯函数；
  - reducer做的事情就是将传入的state和action结合起来生成一个新的state；

- Redux的三大原则
  - 单一数据源
    - 整个应用程序的state被存储在一颗object tree中，并且这个object tree只存储在一个 store 中：
    - Redux并没有强制让我们不能创建多个Store，但是那样做并不利于数据的维护；
    - 单一的数据源可以让整个应用程序的state变得方便维护、追踪、修改；
  - State是只读的
    - 唯一修改State的方法一定是触发action，不要试图在其他地方通过任何的方式来修改State：
    - 这样就确保了View或网络请求都不能直接修改state，它们只能通过action来描述自己想要如何修改state；
    - 这样可以保证所有的修改都被集中化处理，并且按照严格的顺序来执行，所以不需要担心race condition（竟态）的问题；
  - 使用纯函数来执行修改
    - 通过reducer将 旧state和 actions联系在一起，并且返回一个新的State：
    - 随着应用程序的复杂度增加，我们可以将reducer拆分成多个小的reducers，分别操作不同state tree的一部分；
    - 但是所有的reducer都应该是纯函数，不能产生任何的副作用；

### 8.2 安装使用(单独项目)

- 安装

  ```shell
  npm i redux --save
  # npm
  yarn add redux 
  # yarn
  ```

- 创建项目

  ```shell
  yarn init -y
  
  yarn add redux
  ```

  

- 创建`src/index.js`

  ```js
  // node里部分版本不认识ES6导入方式 使用commonJS导入方式
  
  const  redux = require("redux")
  
  // 初始化数据
  const initialState = {
      counter: 0
  }
  
  // reducer
  function reducer(state = initialState,action){
      switch(action.type){
          case "INCREMENT":
              return {...state, counter: state.counter + 1}
  		case "DECREMENT":
              return {...state, counter: state.counter - 1}
  		case "ADD_NUM":
              return {...state, counter: state.counter + action.num}
          case "SUB_NUM":
              return {...state, counter: state.counter - action.num}
          
      }
  }
  
  // store (创建的时候需要传入一个reducer)
  const store = redux.createStore(reducer)
  
  
  // 订阅store的修改
  store.subscribe(()=>{
      console.log("state changed",store.getState().counter)
  })
  
  // 打印 1 0 -1 4 -1 
  
  
  // actions
  const action1 = {type:"INCREMENT"}
  const action2 = {type:"DECREMENT"}
  const action3 = {type:"ADD_NUM",num:5}
  const action4 = {type:"SUB_NUM",num:5}
  
  // 派发action
  store.dispatch(action1)
  store.dispatch(action2)
  store.dispatch(action2)
  store.dispatch(action3)
  store.dispatch(action4)
  
  
  ```

  

  

### 8.3 优化

- 优化 代码执行方式

  `package.json`

  ```json
  "script":{
      "start":"node src/index.js"
  }
  ```

  ```shell
  yarn start
  ```

- node 对ES6模块化的支持

  - `13.2.0`版本之前
    - 在`package.json` 添加 `"type":"module"`
    - 执行命令 添加`node --experimental-module src/index.js`
  - `13.2.0`版本之后
    - 在`package.json` 添加 `"type":"module"`
  - **导入文件的时候 需要跟上 `.js`后缀**

- 文件分模块优化

  - `index.js`

  ```js
  import store from "./store/index.js"
  import {addAction} from "./src/actionCreators.js"
  // .js不能省略
  
  store.subcribe(()=>{
      console.log(store.getState())
  })
  
  
  store.dispatch(addAction(10))
  ```

  - `store/index.js`

  ```js
  import redux from "redux"
  import reducer from "./reducer.js"
  
  const store redux.createStore()
  
  
  ```

  - `store/reducer.js`

  ```js
  import{
      ADD_NUMBER
  }from "./const.js"
  
  const defaultState = {
      counter:0
  }
  
  function reducer(state = defaultState,action){
      switch(action.type){
          case ADD_NUMBER:
              return {...state, counter:state.counter + action.num}
              
          default:
              return state
      }
  }
  
  export default reducer
  ```

  - `store/actionCreators.js`

  ```js
  import{
      ADD_NUMBER
  }from "./const.js"
  
  // 把action封装成一个函数 
  export const addAction = num => ({
          type: ADD_NUMBER,
          num
      })
  
  ```

  - `store/const.js`

  ```js
  // 封装常量
  export const ADD_NUMBER = "ADD_NUMBER"
  ```

### 8.4 React-Redux

- 案例 按钮点击数字变化 数字在两个组件内共享

  home.jsx

  ```jsx
  import store from "../store"
  import {
      addAction
  } from "../store/actionCreators"
  class Home extends React.component{
      constructor(props){
          super(props)
          this.state = {
              counter:store.getState().counter
          }
      }
      componentDidMount(){
          // 订阅变化
          this.unsubscribe = store.subscribe(()=>{
              this.setState({
                  counter: store.getState().counter
              })
          })
      }
      
      componentWillUnmount(){
          // 取消订阅
          this.unsubscribe()
      }
      
      render(){
          return(
              <>
                  <h2>当前计数:{this.state.counter}</h2>
              	<button onCLick = {e=> this.increment(5)}>+5</button>
              </>
          )
      }
      increment(num){
          store.dispatch(addAction(num))
      }
  }
  ```

- 优化

  util/connet.js

  ```js
  import store from "../store"
  export function connent(mapStateToProps,mapDispatchToProp){
      return function enhanceHOC(wrappedComponent){
          return class extends PureComponent{
              constructor(props){
                  super(props)
                  this.state = {
                      storeState:mapStateToProps(store.getState())
                  }
              }
              componentDidMount(){
                  const unsubscribe = store.subscribe(()=>{
                      this.setState({
                          storeState: mapStateToProps(store.getState())
                      })
                  })
              }
              componentWillUnmount(){
                  this.unsubscribe()
              }
              render(){
                  <wrappedComponent 
                  	{...this.props}
                  	{...mapStateToProps(store.getState())}
                  	{...mapDispatchToProp(store.dispatch)}
                  />
              }
          }
      }
  }
  ```

  - 优化后的`home.jsx`

    ```jsx
    import {connect} from "../util/connect"
    import {addAction} from "../store/actionCreators"
    function About(props) {
        render(){
            return(
                <>
                    <h2>当前计数:{this.state.counter}</h2>
                	<button onCLick = {e=> props.addNum(5)}>+5</button>
                </>
            )
        }
    }
    const mapSatteToProps = state => ({counter: state.counter})
    const mapDispachToProp = dispatch => ({
        addNum: function(num){
            dispatch(addAction(num))
        },
    
    })
    export default connect(mapStateToProps,mapDispatchToProp)(About)
    ```

  - 再次优化  不依赖store

    创建util/context.js

    ```js
    import React from "react"
    
     const storeContext React.createContext()
    
    export {
    	storeContext
    }
    ```

    util/connect.js

    ```jsx
    import store from "../store"
    import {StoreContext} frpm "./context.js"
    
    export function connent(mapStateToProps,mapDispatchToProp){
        return function enhanceHOC(wrappedComponent){
            class EnhanceComponent extends PureComponent{
                constructor(props,context){
                    super(props,context)
                    this.state = {
                        storeState:mapStateToProps(context.getState())
                    }
                }
                componentDidMount(){
                    const unsubscribe = this.context.subscribe(()=>{
                        this.setState({
                            storeState: mapStateToProps(this.context.getState())
                        })
                    })
                }
                componentWillUnmount(){
                    this.unsubscribe()
                }
                render(){
                    <wrappedComponent 
                    	{...this.props}
                    	{...mapStateToProps(this.context.getState())}
                    	{...mapDispatchToProp(this.context.dispatch)}
                    />
                }
            }
            EnhanceComponent.contextType = storeContext
            return EnhanceComponent
        }
    }
    ```

    App.jsx

    ```jsx
    import React from "react"
    import ReactDom from "react-dom"
    
    import store from "./store"
    
    import {StoreContext} from "./utils/context"
    
    import App from "./App"
    
    ReactDOM.render(
        <StoreContext.provider value = {store}>
    		<App />
        <StoreContext.provider>,
        document.getElementById("root")
    )
    ```

- react-redux库

  ```shell
  yarn add react-redux
  ```

  App.js

  ```jsx
  import React from "react"
  import ReactDom from "react-dom"
  
  import store from "./store"
  
  import {Provider} from "./utils/context"
  
  import App from "./App"
  
  ReactDOM.render(
      <Provider store = {store}>
  		<App />
      <Provider>,
      document.getElementById("root")
  )
  ```

  home.jsx

  ```jsx
  import {connect} from "react-redux"
  ```

### 8.5 组件中的异步操作

- 中间件 Middleware -- `redux-thunk`

  > 中间件的目的时在dispach和action最终到达reducer之间,扩展自己的代码
  >
  > - 日志记录
  > - 调用异步接口
  > - 添加代码调试功能

  

  - 中间件实现过程

    ```jsx
    // 基本做法 
    console.log("before dispatch",addAction(10))
    
    store.dispatch(addAction(10))
    
    console.log("after dispatch",store.getState())
    
    // 封装函数做法
    function dispatchAndLogging(action){
       console.log("before dispatch",action)
    
        store.dispatch(action)
    
        console.log("after dispatch",store.getState()) 
    }
    
    dispatchAndLogging(addAction(5))
    
    // 函数优化做法 (monkeyingpatch)
    const next = store.dispatch
    function dispatchAndLogging(action){
       console.log("before dispatch",action)
    
        next(action)
    
        console.log("after dispatch",store.getState()) 
    }
    store.dispatch = dispatchAndLogging
    store.dispatch(addAction(5))
    
    // 4 封装3做法
    export fucntion patchLogging(store){
        const next = store.dispatch
        function dispatchAndLogging(action){
           console.log("before dispatch",action)
    
            next(action)
    
            console.log("after dispatch",store.getState()) 
        }
        store.dispatch = dispatchAndLogging
        store.dispatch(addAction(5))
    }
    
    patchLogging(store)
    store.dispatch(addAction(5))
    
    // 5 封装patchThunk功能
    function patchThunk(store){
        const next = store.dispatch
        function dispatchAndThink(action){
            if(typeof action === "function"){
                action(store.dispatch,store.getState)
            }else{
                next(action)
            }
        }
        store.dispatch = dispatchAndThunk
    }
    
    patchLogging(store)
    patchThunk(store)
    
    store.dispatch(addAction(10))
    function foo(dispatch,getState){
        dispatch(subAction(10))
    }
    store.dispatch(foo)
    
    
    // 6 封装 applyMIddleware
    patchLogging(store){
        const next = store.dispatch
        function dispatchAndLogging(action){
           console.log("before dispatch",action)
    
            next(action)
    
            console.log("after dispatch",store.getState()) 
        }
        return dispatchAndLogging
        store.dispatch(addAction(5))
    }
    function patchThunk(store){
        const next = store.dispatch
        function dispatchAndThink(action){
            if(typeof action === "function"){
                action(store.dispatch,store.getState)
            }else{
                next(action)
            }
        }
        return dispatchAndThunk
    }
    function applyMIddleware(...middlewares){
        // const newMiddleware = [...middlewares]
        middlewares.forEach(middleware => {
            store.dispatch = middleware(store)
        })
    }
    applyMiddleware(patchLogging,patchThunk)
    ```

    

  - 安装 `redux-thunk`

  ```shell
  yarn add redux-thunk
  ```

  - 使用

    > - 我们知道，默认情况下的dispatch(action)，action需要是一个JavaScript的对象；
    > - redux-thunk可以让dispatch(action函数)，action可以是一个函数；
    > - 该函数会被调用，并且会传给这个函数一个dispatch函数和getState函数；
    >   - dispatch函数用于我们之后再次派发action；
    >   - getState函数考虑到我们之后的一些操作需要依赖原来的状态，用于让我们可以获取之前的一些状态；

    store/index.js	

    ```js
    import {createStore, applyMiddleware} from "redux"
    import thunkMiddleware from "redux-thunk"
    
    import reducer from "./reducer.js"
    
    // 应用中间件
    const storeenhancer = applyMiddleware(thunkMiddleware)
    
    const store createStore(reducer,storeenhancer)
    
    export default store
    ```

    `home.jsx`

    ```jsx
    import {connect} from "../util/connect"
    import {addAction,getHomeDataAction} from "../store/actionCreators"
    function About(props) {
        render(){
            return(
                <>
                    <h2>当前计数:{this.state.counter}</h2>
                	<button onCLick = {e=> props.addNum(5)}>+5</button>
                </>
            )
        }
    }
    const mapSatteToProps = state => ({counter: state.counter})
    const mapDispachToProp = dispatch => ({
        addNum: function(num){
            dispatch(addAction(num))
        },
        getHomeData: function(){
            // 异步 直接传入函数 不要调用
            dispatch(getHomeDataAction)
        }
    
    })
    export default connect(mapStateToProps,mapDispatchToProp)(About)
    ```

    actionCreators.js

    ```js
    import axios from "axios"
    export const changeHomeData = homeData=>({
        type:"CHANGE_HOME_DATA",
        homeData
    })
    export const getHomeDataAction = (dispatch,[,getState]) =>{
        // getState可选 如需依赖state中的数据 则传
        axios({...}).then((res)=>{
            dispatch(changeHomeData(res.data))
        })
    }
    ```

    store/reducer.js

    ```js
    function reducer(state = defaultState,action){
        switch(action.type){
            case CHANGE_HOME_DATA:
                return{...state,action.homeData}
        }
    }
    ```

### 8.6 devtools

> 谷歌浏览器安装redux后 store/index.js还需集成中间件

store/index.js

```jsx
import {compose} from "redux"

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose
// 应用中间件
// 创建thunkMiddleWare中间件
const storeEnhancer = applyMiddleware(thunkMiddleWare)
const store = createStore(reducer, composeEnhancers(storeEnhancer))
```

### 8.7 generator(生成器)

- 定义

  ```js
  function foo(){
      
  }
  console.log(foo()) // undefined
  // 生成器
  function* foo(){
      yield "hello"
      yield "world"
  }
  
  const it = foo()
  console.log(it) // 返回foo迭代器对象
  
  // 调用一次迭代器就会消耗一次结果
  it.next() 
  // {value:"hello",done:false}
  it.next() 
  // {value:"world",done:false}
  it.next()
  // {value:undefined,done:true}
  
  
  
  ```

- 执行顺序

  ```js
  function* foo(){
      console.log(111)
      yield "hello"
      console.log(222)
      yield "world"
      console.log(333)
  }
  
  const it = foo()
  console.log(it) // 打印迭代器 不会有代码执行
  
  const res1 = it.next()
  console.log(res1)
  // 打印111 {value:"hello",done:false}
  
  const res2 = it.next()
  console.log(res2)
  // 打印222 {value:"world",done:true} 
  
  it.next()
  // 打印333
  ```

- 结合`Promise`使用

  ```js
  function* bar (){
      console.log(111)
      const res = yield new Promise((resolve,reject) => {
          // 异步请求 
          setTimeout(()=>{
              resolve("hello generator")
          },100)
      })
  }
  
  const it = bar()
  
  it.next().value.then(res=>{
      it.next(res)
  })
  // 111 之后等3秒 打印 hello generator
      
  ```

### 8.8 redux-saga

> redux-saga是另一个比较常用在redux发送异步请求的中间件，它的使用更加的灵活。
>
> 优点 单独将代码卸载文件里 降低和actionCreators的耦合度

- 安装

  ```shell
  yarn add saga
  ```

- 使用

  store/index

  ```js
  import {compose} from "redux"
  import createSagaMiddleware from "redux-saga"
  // 1. 引入
  import saga from "./saga.js"
  const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose
  // 引入thunkMiddleWare中间件
  // 2 创建 sagaMiddleware中间件
  const sagaMiddleware = createSagaMiddleware()
  
  const storeEnhancer = applyMiddleware(thunkMiddleWare,sagaMiddleware)
  
  const store = createStore(reducer, composeEnhancers(storeEnhancer))
  // 3 运行中间件 参数为一个生成器函数
  sagaMIddleware.run(saga)
  export default store
  ```

  创建saga.js

  ```js
  // export default 生成器函数
  import { takeEvery,put,all } from "redux-saga/effects"
  import axios from "axios"
  import { changeHomeData } from "./actionCreators"
  function* fetchHomeData(action){
      const res = yield axios.get(...)
  	console.log(res) 
      // 多个action写法1
      
      // put可以写多个 内部判断 done的状态 循环执行到done = true
      yield put(changeHomeData(res.data))
      yield put(changeAboutData(res.about))
      
      // 多个action写法2
      yield all([
           yield put(changeHomeData(res.data)),
      yield put(changeAboutData(res.about))
      ])
      
      
      /*saga内部做了类似
      it.next().value.then(res=>{
          it.next(res)
      })
      所以可以获取到res
      */
  }
  // 4 创建监听生成器函数
  export function* mySaga(){
      //  两个参数  1 拦截哪个 2 生成器函数
      yield takeEvery("FETCH_HOME_DATA",fetchHomeData)
      yield takeEvery("FETCH_ABOUT_DATA",fetchAboutData)
      // takeLatest 只能监听一个在对应的action(最后一次)
      // takeLatest 每一个都会监听
      
      // 写法2
      yield all([
         yield takeEvery("FETCH_HOME_DATA",fetchHomeData),
      yield takeEvery("FETCH_ABOUT_DATA",fetchAboutData) 
      ])
  }
  ```

  actionCreators.js

  ```js
  // redux-saga拦截的action
  export const fetchHomeDataAction = {
      type: "FETCH_HOME_DATA"
  }
  ```

  home.jsx

  ```jsx
  import {connect} from "../util/connect"
  import {addAction,fetchHomeDataAction} from "../store/actionCreators"
  function About(props) {
      render(){
          return(
              <>
                  <h2>当前计数:{this.state.counter}</h2>
              	<button onCLick = {e=> props.addNum(5)}>+5</button>
              </>
          )
      }
  }
  const mapSatteToProps = state => ({counter: state.counter})
  const mapDispachToProp = dispatch => ({
      addNum: function(num){
          dispatch(addAction(num))
      },
      getHomeData: function(){
          // 异步 直接传入函数 不要调用
          dispatch(fetchHomeDataAction)
      }
  
  })
  export default connect(mapStateToProps,mapDispatchToProp)(About)
  ```

### 8.9 reducer的拆分

reducer.js

```js
import { reducer as counterReducer } from './counter'
import { reducer as homeReducer } from './home'


// 拆分homeReducer
const initialHomeState = {
    home:[],
    about:[]
}
function 拆分homeReducer(state = initialHomeState,action){
    switch(action.type){
        case CHANGE_HOME_DATA:
            return{ ...state,homeData:state.homeData}
        case CHANGE_ABOUT_DATA:
            return{ ...state,aboutData:state.aboutData}
        default:
            return state
    }
}

export function reducer(state = {},action){
    return {
        counterInfo: counterReducer(state.counterInfo,action),
        homeInfo:homeReducer(state.homeReducer,action)
    }
}

// 调用的时候多套一层 .counterInfo
```

actionCreator.js, const.js , index.js reducer.js 同理可以拆分

store/counter/index.js

```js
// 拆分counterReducer
const initialCounterState = {counter: 0}
function counterReducer(state = initialCounterState,action){
    switch(action.type){
        case ADD_NUMBER:
            return{ ...state,counter:state.counter + action.num}
        case SUB_NUMBER:
            return{ ...state,counter:state.counter - action.num}
        case INCREMENT:
            return {...state,counter:state.counter + 1}
        case DECREMENT:
            return {...state,counter:state.counter - 1}
        default:
            return state
    }
}
```

### 8.10 combineReducers函数

> 目前我们合并的方式是通过每次调用reducer函数自己来返回一个新的对象
>
> redux给我们提供了一个combineReducers函数可以方便的让我们对多个reducer进行合并

- 实现原理
  - 传入的reducers合并到一个对象中，最终返回一个combination的函数
  - 在执行combination函数的过程中，它会通过判断前后返回的数据是否相同来决定返回之前的state还是新的state
  - 新的state会触发订阅者发生对应的刷新，而旧的state可以有效的组织订阅者发生刷新；

- 使用

  ```jsx
  import { combineReducers } from "redux"
  
  const reducer = combineReducers({
      counterInfo: counterReducer,
      homeInfo: homeReducer 	
  })
  
  export default reducer
  ```

- 原理

  > ```js
  > export function reducer(state = {},action){
  >     return {
  >         counterInfo: counterReducer(state.counterInfo,action),
  >         homeInfo:homeReducer(state.homeReducer,action)
  >     }
  > }
  > ```
  >
  > 自己封装的reducer 每次都会返回一个新的对象
  >
  > combineReducers函数判断有无state变化 有的话返回newState 无state变化 返回state 最终的返回值是一个纯函数

## 9 React-Hooks

> 不编写class的情况下使用state及其他特性(生命周期等)
>
> class组件相对于函数式组件的优点
>
> - state
> - 生命周期
> - 状态改变时 render只调用一次componentDidMount
>
> 存在的问题
>
> - 复杂的组件难以理解,逻辑难以拆分
> - this指向
> - 组件复用状态很难

### 9.1 体验hooks



```jsx
import React,{useState} from "react"

export default function  CounterHook(){
    const [count,setCount] = useState(0)
    return (
    	<div>
        	<h2>当前计数: {count}</h2>
            <button onClick={e => setCount(count + 1)}>+1</button>
            <button onClick={e => setCount((preCount) => preCount + 1)}>+10</button>
            <button onClick={e => setCount(count - 1)}>-1</button>
        </div>
    )
}
```

- 说明
  - 本身是一个函数 来自react包
  - 返回一个数组 数组两个元素
    -  第一个：当前state(第一次调用为初始化值)
       -  初始化state的时候也可以传入一个函数 `()=>0`
    -  第二个：设置新的值的函数
       -  可以拿到前一次的值 再进行操作
       -  区别: 类似于setState的两种写法 直接setState多次操作会合并
  - 参数 
    - 作用：给创建的状态一个默认值
  - 规则
    - 只能在函数最外层调用Hook 不要再循环 条件判断中调用
    - 只能在react的函数组件中调用Hook 不要再其他js函数中调用

### 9.2 useState的使用

- 多个状态

  ```jsx
  import React,{useState} from "react"
  
  export default function  CounterHook(){
      const [count,setCount] = useState(0)
      const [friends,setFriends] = useState(["kebe","lilei"])
      return (
      	<div>
          	<h2>当前计数: {count}</h2>
              <ul>
              	{
                      friends.map((item,index)=>{
                          return <li key={index}>{item}</li>
                      })
                  }
              </ul>
          </div>
      )
  }
  ```

- 复杂状态的修改

  ```jsx
  import React,{useState} from "react"
  
  export default function  CounterHook(){
      const [friends,setFriends] = useState(["kebe","lilei"])
      const [stus,setStus] = useState([
          {id:1,age:18,name:"zhangsan"},
          {id:2,age:19,name:"lisi"},
          {id:3,age:20,name:"wangwu"},
      ])
      function changeAge(index){
          const newStus = [...stus]
          newStus[idnex].age += 1
          setStus(newStus)
      }
      
      return (
      	<div>
              <ul>
              	{
                      friends.map((item,index)=>{
                          return <li key={index}>{item}</li>
                      })
                  }
              </ul>
              <button onClick={e => setFriends([...friends,"tom"])}>添加好友</button>
              <ul>
              	{
                      stus.map((item,index) => {
                          return(
                          	<li>
                                  <span>name:{item.name};age:{item.age}</span>
                                  <button onClick={e => changeAge(index)}>
                                      age + 1
                                  </button>
                              </li>
                          )
                      })
                  }
              </ul>
          </div>
      )
  }
  ```

  > 不能直接push (数据更新 但视图不渲染)
  >
  > - 点击事件传参 写**箭头函数** 
  >
  > - 不传参 两种都可以

### 9.3 useEffect的使用

- 初体验

  ```jsx
  import React,{useEffect} from "react"
  
  export default function  CounterHook(){
      const [count,setCount] = useState(0)
      useEffect(()=>{
      	// didMount + didUpdate 
          document.title = counter
      )
      return (
      	<div>
          	<h2>当前计数: {count}</h2>
              <button onClick={e => setCount(count + 1)}>+1</button>
          </div>
      )
  }
  ```

- 模拟订阅和取消

  ```jsx
  import React,{useEffect} from "react"
  
  export default function  CounterHook(){
      const [count,setCount] = useState(0)
      useEffect(()=>{
      	console.log("订阅事件")
          
          return ()=>{
              console.log("取消订阅")
          }
          // 第二个参数传入一个空数组 取消count变化时的useEffect执行
      },[])
      return (
      	<div>
          	<h2>当前计数: {count}</h2>
              <button onClick={e => setCount(count + 1)}>+1</button>
          </div>
      )
  }
  ```

  - Effect Hook 可以让你来完成一些类似于class中生命周期的功能；
  - useEffect传入的回调函数A本身可以有一个返回值，这个返回值是另外一个回调函数B
    - 这是 effect 可选的清除机制。每个 effect 都可以返回一个清除函数；
    - 如此可以将添加和移除订阅的逻辑放在一起；
    - 它们都属于 effect 的一部分；
  - 决定useEffect在什么时候应该执行和什么时候不应该执行
    - useEffect实际上有两个参数：
    -  参数一：执行的回调函数；
    - 参数二：该useEffect在哪些state发生变化时，才重新执行；（受谁的影响）

- 多个useEffect

  ```jsx
  import React,{useEffect} from "react"
  
  export default function  CounterHook(){
      // 按照定义顺序执行
      useEffect(()=>{
      	console.log("修改dom")
      })
      useEffect(()=>{
      	console.log("订阅事件")
      },[])
      useEffect(()=>{
      	console.log("网络请求")
      },[])
      return (
      	<div>
          	<h2>多个useEffect</h2>
          </div>
      )
  }
  ```

### 9.4 useContext的使用

```jsx
import React from "react"
export const userContext = createContext()
export const themeContext = createContext()
export default function  App(){
    return (
    	<div>
        	<UserContext.provider value={name:"why",age:18}>
            	<ThemeContext.provider value={{fontSize:"30px"}}>
                	<ContextCpn />
                </ThemeContext.provider>
            </UserContext.provider>
        </div>
    )
}
```

```jsx
import React,{useContext} from "react"
import {userContext,themeContext} from "../App"

export default function ContextHook(props){
    const user = useContext(userContext)
    const theme = useContext(userTheme)
    
    console.log({user,theme})
    return(
    	<div>
            
		</div>
    )
}
```



### 9.5 其他hooks

#### 9.5.1 useReducer

> useState的一种替代方案
>
> 不是redux的替代

```js
export default function reducer(state,action){
  switch (action.type) {
    case "increment":
      return {...state,count:state.count + 1}
    case "decrement":
      return {...state,count:state.count - 1}
    default:
      return state
  }
}
```



 ```jsx
import React,{useReducer} from 'react'
import reducer from './reducer'
export default function Untitled() {
  const [state, dispatch] = useReducer(reducer, {count:0})
  return (
    <div>
      <h2>Home当前计数:{state.count}</h2>
      <button onClick={e => dispatch({type:"increment"})}>+1</button>
      <button onClick={e => dispatch({type:"decrement"})}>+1</button>
      <button>-1</button>
    </div>
  )
}

 ```



#### 9.5.2 useCallback

> - 目的:性能优化
>   - useCallback函数会返回一个函数的memoized(记忆的)值
>   - 在依赖不变的情况下 多次定义的时候 返回的值是相同的

- 不能进行的性能优化

  ```jsx
  import React from 'react'
  
  export default function Untitled() {
    const [count, setCount] = useState(0)
    const increment = () => {
      console.log("执行increment")
      setCount(count + 1)
    }
    const increment2 = useCallback(() => {
        console.log("执行increment")
        setCount(count + 1)
        // 没有数组依赖的时候调用的都是 最开始的state = 0
      },[count])
    return (
      <div>
        <h2>callback demo</h2>
        <h2>当前计数：{count}</h2>
        <button onClick={increment}>+1</button>
        <button onClick={increment2}>+1</button>
      </div>
    )
  }
  ```

- 进行性能优化

  ```jsx
  import React from 'react'
  
  const MyButton = memo((props) =>{
      // 点击切换show按钮的时候 这种写法不会重新渲染
      // increment 依赖count变化 show变化不会影响
      return (
          <button onClick={props.increment}>+1</button>
      )
  })
  export default function Untitled() {
    const [count, setCount] = useState(0)
    const [show, setShow] = useState(true)
    const increment = useCallback(
      () => {
        console.log("执行increment")
        setCount(count + 1)
      },[])
    return (
      <div>
        <h2>当前计数：{count}</h2>
        <MyButton increment={increment}/>
        <button onClick={setShow(!show)}>change show</button>
      </div>
    )
  }
  ```

  - 使用场景
    - 将一个组件中的函数,传递给子元素进行回调 使用`useCallback`进行处理

#### 9.5.3 useMemo

> 目的:性能优化
>
> - useMemo返回的也是一个记忆的值
> - 在依赖不变的情况下 多次定义 返回值相同
> - `const memoizedValue = useMemo(()=>computeExpensiveValue(a,b),[a,b])`

- 场景 - 复杂计算

  ```jsx
  import React,{useMemo} from 'react'
  function calc(count){
    alert("重新计算")
    let sum = 0
    for (let i = 0; i < count.length; i++) {
      sum += i
    }
    return sum
  }
  export default function Untitled() {
    const [count, setCount] = useState(10)
    const [show, setShow] = useState(true) 
  
    const sum = useMemo(() => calc, [count])
    return (
      <div>
        <h2>计算数字的和:{sum}</h2>
        <button onClick={e=>setCount(count + 1)}>+1</button>
        <button onClick={e=> setShow(!show)}>无关值改变</button>
      </div>
    )
  }
  
  
  ```

- useMemo传入子组件应用类型

  ```jsx
  import React,{useMemo} from 'react'
  const MyInfo = memo((props) => {
    alert("重新渲染")
    return(
      <h2>name:{props.info.name} age:{props.info.age}</h2>
    )
  })
  export default function MemoHookDemo() {
    // 使用useMemo 父组件渲染 子组件不重新渲染
    alert("重新渲染")
    // const info = {name:"kobe",age:18}
    const info = useMemo(() => ({name:"kobe",age:18}), [])
    const [show, setShow] = useState(true)
    return (
      <div>
        <MyInfo info={info}/>
        <button onClick={setShow(!show)}>无关值改变</button>
      </div>
    )
  }
  
  
  ```

- 还可以返回一个函数 当作useCallback使用

  ```jsx
  import React from 'react'
  
  const MyButton = memo((props) =>{
      return (
          <button onClick={props.increment}>+1</button>
      )
  })
  export default function Untitled() {
    const [count, setCount] = useState(0)
    const [show, setShow] = useState(true)
    const increment = useMemo(()=>{
      return () => {
        console.log("执行increment")
        setCount(count + 1)
      }  
    },[count])
    return (
      <div>
        <h2>当前计数：{count}</h2>
        <MyButton increment={increment}/>
        <button onClick={setShow(!show)}>change show</button>
      </div>
    )
  }
  ```

#### 9.5.4 useRef

> 使用场景
>
> - 引用dom/class组件
> - 保存一个数据 这个对象在整个生命周期中保持不变

- 引用DOM

  ```jsx
  import React,{useRef} from 'react'
  
  export default function UseRef() {
    const titleRef = useRef()
    const inputRef = useRef()
  
    function changeDom(){
      titleRef.current.innerHTML = "hello world"
      inputRef.current.focus()
    }
    return (
    <div>
        <h2 ref={titleRef}>RefHookDemo</h2>
        <input type="text" ref={inputRef}/>
        <button onClick={e => changeDom()}>修改DOM</button>
      </div>
    )
  }
  
  // 类组件拿到的是react 组件
  // 子组件是function组件的时候不能用
  
  ```
  
- 引用其他数据

  ```jsx
  import React,{useState,useRef} from 'react'
  
  export default function UseRef() {
    const [count,setCount] = useState(0)
    const numRef = useRef(count)
    return (
      <div>
        <h2>{numRef.current}</h2>
        <h2>{count}</h2>
        <button onClick={e => setCount(count + 1)}>+1</button>
      </div>
    )
  }
  // 点击按钮state值改变 但是ref值不会改变
  // useRf返回一个对象 
  ```


#### 9.5.5 useImperativeHandle

> 通过useImperativeHandle的Hook，将传入的ref和useImperativeHandle第二个参数返回的对象绑定到了一起；
>
> - 所以在父组件中，使用 inputRef.current时，实际上使用的是返回的对象； 
> - 比如我调用了 focus函数，甚至可以调用 printHello函数；

```jsx
import React, { forwardRef,useRef } from 'react'

const MyInput = forwardRef((props, ref) => {
  const inputRef = useRef()
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus()
      // console.log("useImperativeHandle中的回调函数返回对象的focus")
    }
  }),[inputRef.current])
  return <input ref={inputRef} type="text"/>
})
export default function UseImperativeHandleHookDemo() {
  const inputRef = useRef()
  return (
    <div>
      <MyInput ref={inputRef}/>
      <button onClick={e=>inputRef.current.focus()}>聚焦</button>
    </div>
  )
}

```

#### 9.5.6  useLayoutEffect

>  useLayoutEffect看起来和useEffect非常的相似，事实上他们也只有一点区别而已：
>
> - useEffect会在渲染的内容更新到DOM上后执行，不会阻塞DOM的更新；
> - useLayoutEffect会在渲染的内容更新到DOM上之前执行，会阻塞DOM的更新； 
> - 如果我们希望在某些操作发生之后再更新DOM，那么应该将这个操作放到useLayoutEffect。

```jsx
import React, { useLayoutEffect } from 'react'
export default function UseLayoutEffectHookDemo() {
  const [count, setCount] = useState(10)
  useLayoutEffect(() => {
    if(count === 0){
      setCount(Math.random())
    }
  }, [count])
  return (
    <div>
      <h2>{count}</h2>
      <button onClick={e=>setCount(0)}>修改数字</button>
    </div>
  )
}

```

### 9.6 自定义hook

> 自定义Hook本质上只是一种函数代码逻辑的抽取，严格意义上来说，它本身并不算React的特性。
>
> - 函数前面加`use` 不然会报错

```jsx
export default function MyHookDemo(){
    useMyHook("demo")
    return (
        <h2>自定义钩子</h2>
    )
}

function useMyHook(name){
    useEffect(()=>{
        console.log(`${name}创建`)
        return () => {
            console.log(`${name}销毁`)
        }
    },[])
}
```

- context的共享

  ```jsx
  export default function MyHookDemo(){
      const [user,token] = useUserToken()
      return (
          <h2>自定义钩子</h2>
      )
  }
  
  function useUserToken(){
      const user  = useContext(UserContext)
      const token  = useContext(TokenContext)
      return [user,token]
  }
  ```

- 获取滚动位置

  ```jsx
  function useScrollPosition(){
      const [scrollPosition,setScrollPosition] = useState(0)
      useEffect(()=>{
          const handleScroll = ()=>{
              setScrollPosition(window.scrollY)
          }
          document.addEventListener("scroll",handleScroll)
          return ()=>{
          	document.removeEventListener("scroll",handleScroll)
          }
      },[])
      return scrollPosition
  }
  ```

- localStorage数据存储

  ```jsx
  function useLocalStorage(key){
      const [data,setData] = useState(()=>{
          return JSON.parse(window.localStorage.getItem(key))
      })
      
      useEffect(()=>{
           window.localStorage.setItem(key,JSON.stringify(data))
      },[data])
      
      retunr [data,setData]
  }
  ```

  

