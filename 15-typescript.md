## typescript
- `npm install -g typescript`
- tsc -v 查看安装
- cd到目录
- tsc --init 生成tsconfig.json 
- 修改outDir 
- 终端=>运行任务=>typescript=>tsc监视


### ts语法

#### 数据类型
> 类型校验
```js
  - boolean 
  var flag:boolean=false
  flag = true

  - number 
  var a:number=123
  a = '1243' // 报错

  - string 
  var str:string='hello'

  - array 
  // 第一种方法
  var arr:number[]=[1,2,3] //只能保存数字
  var arr:string[]=['php','js','python']
  // 第二种方法 泛型
  var arr:Array<number>=[1,2,3]
  // 第三种
  var arr:any[] = [123,'hello',true]

  - tuple 元组
  // 属于数组的一种
  let arr:[number,string,boolean]=[123,'hello',true]

  - enum 枚举
  // 标识状态或者固定值
  enum 枚举名{
    标识符[=整型常数],
    标识符[=整型常数],
    ...
    标识符[=整型常数]
  }

  enum Flag {success=1,error=-1}
  var f:Flag=Flag.success // 1


  // 如果没有赋值打印索引 有部分赋值 下一个没有赋值的以上一个赋值的为基准
  enum Color {red,blue=5,orange}
  var a:Color=Color.red //索引0
  var b:Color=Color.blue // 定义的5
  var c:Color=Color.orange // 5+1 =6




  any 任意
  // 任意类型的作用
   var oBox = document.getElementById('box')
   oBox.style.color = 'red'
  //报warning 修改oBox:any解决

  - null undefined
  // 其他(never)类型的子类型
  let num:number
  console.log(num)
  //定义未赋值 报warning 
  let num:number | undefined | null


  - void
  // 没有任何类型,一般用于定义方法的时候方法没有返回值
  function run():void{
    console.log('123')
  }
  ():number => {return 123}

  - never
  // 其他类型 包括null和undefined 从来不会出现的值
  var a:never

  a = (() => {
    throw new Error('error')
  })()
```

#### 函数

```ts
// 函数声明
function run():string{
  return "hello typescript"
}
//匿名函数
var fun  = function():number {
  return 123
}

//带参数
function getInfo(name:string,age:number):string{
  return `${name} --- ${age}`
}

- 可选参数
//可选参数必须配置在参数的最后面
function getInfo(name:string,age?:number):string{
  if(age) {
    return `${name} --- ${age}`
  } else {
    return `${name} --- 年龄保密`
  }
}

- 默认参数
function getInfo(name:string,age:number=20):string{
  if(age) {
    return `${name} --- ${age}`
  } else {
    return `${name} --- 年龄保密`
  }
}

- 剩余参数
function sum(a:number,...result:number[]):number{
  let sum = a
  for(let i = 0;i< result.length;i++) {
    sum += result[i]
  }
  return sum
}

- 函数的重载
// 通过为同一个函数提供多个函数类型定义来实现多种功能
// ts为了兼容ES5 ES6 和java中有区别
// ES5中出现同名方法 下面的替换上面的

function getInfo(name:string):string

function getInfo(age:number):string

function getInfo(desc:any):any{
  if(typeof desc === 'string'){
    return '我叫:' + desc
  } else {
    return '我的年龄:' + desc
  }
}
```

#### 类

```ts
class Person {
  public name:string
  constructor(name:string){
    this.name = name
  }
  getName():string {
    return this.name
  }
  setName(name:string):void{
    this.name = name
  }
}

- 继承
super()

- 类里面的修饰符
public(默认)
// 公有 在类里面 子类 类外部都可以访问
protected
// 保护 在类里面和子类 可以访问  类外部不能访问
private
// 私有 类里面可以访问 子类类外面都不能访问

- 静态属性
static
// 静态方法里面没发调用类里面的属性
// 解决方案: 定义静态属性

- 多态
// 多态：父类定义一个方法不去实现，让继承它的子类去实现 每一个子类有不同的表现
// 多态属于继承
class Animal {
  name:string
  constructor(name:string) {
    this.name=name
  }
  eat(){
    console.log('吃的方法')
  }
}

class Dog extends Animal{
  constructor(name:string){
    super(name)
  }
  eat() {
    return this.name + '吃肉'
  }
}

class Cat extends Animal{
  constructor(name:string){
    super(name)
  }
  eat() {
    return this.name + '吃鱼'
  }
}


- 抽象类
// ts中的抽象类 他是提供其他类的基类 不能直接被实例化
// 用 abstract 关键字定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。
// abstract抽象方法只能放在抽象类里面 
// 抽象类的子类必须实现抽象类的抽象方法
// 抽象类和抽象方法用来定义标准
abstract class Animal {
  public name:string
  constructor(name:string) {
    this.name = name
  }
  abstract eat():any
}

class Dog extends Animal{
  constructor(){
    super(name)
  }
  eat() {
    //必须有 没有报错
  }
}


```
#### 接口
> 定义标准 
- 属性接口 
> 对json的约束 对对象的约束

```ts
interface Fullname {
  firstName: string; // 注意;结束
  secondName?: string;
  // 加?表示可选参数
}

function printName(name:Fullname) {
  // 必须传入 firstname 和  secondname
  console.log(name.firstName + '---' + name.secondName)
}
//不报错
let obj = {age:20,firstName:'rock',secondName:'lee'}
printName(obj)

// 报错
printName({age:20,firstName:'rock',secondName:'lee'})
```
- 函数类型接口 
> 对参数 返回值进行约束
```ts
// 加密的函数类型接口
interface encrypt{
  (key:string,value:string):string
}

var md5:encrypt(key:string,value:string):string{
  return key.split('').reverse().join('') + value
}
console.log(md5('name','zhangsan'))
```
- ~~可索引接口~~
> 对数组 对象的约束 (不常用)
```ts
// 对数组的约束
interface UserArray{
  [index:number]:string
  // 数组的每一项必须是string
}

var arr:UserArray=['aaa','bbb']

// 对对象的约束
interface UserObj {
  [index:string]:string
  // 对象的每一项必须是string
}
var obj:UserObj={name:'jack',age:'20'}
```

- **类类型接口**
> 对类的约束 和 抽象类有点相似
```ts
interface Animal {
  name:string
  eat(str:string):void
}
// implement (实施 实现接口)
class Dog implements Animal{
  name:string
  constructor(name:string) {
    this.name = name
  }
  eat() {
      console.log(this.name + 'eating')
  }
}
```

- 接口的扩展
> 接口可以继承接口
```ts
interface Animal{
    eat():void
}
interface Person extend Animal {
    work():void
}
class Programmer{
    public name:string
    constructor(name:string){
        this.name = name
    }
    coding(code:string){
        console.log(this.name + code)
    }
}
// 继承结合接口
class Worker extends Programmer implements Person {
    constructor(name:string){
        super(name)
    }
    eat() {
        alert(this.name +  'eating')
    }
    work() {
        alert(this.name +  'working')
    }
}

```

#### 泛型

> 泛型：软件工程中，我们不仅要创建一致的定义 良好的API，同时也要考虑可重用性。组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。
> 在像C#和Java这样的语言中，可以使用泛型来创建可重用的组件，**一个组件可以支持多种类型的数据**，这样用户就可以以自己的数据类型来使用组件。
> 通俗理解:泛型就是解决 类 接口 方法的复用性 以及对**不特定数据类型的支持**
```ts
// 需求: 传入类型 返回同样类型

//T表示泛型 具体什么类型时调用这个方法的时候决定的 一般用T
function getData<T>(value:T):T{
    return value
}

getData<number>(123)   //正确写法
getData<number>('123') //错误写法
```
- 泛型类

```ts
//需求: 最小堆算法: 需要同时支持数字和字符串

class MinClass<T>{
  public list:T[]=[]
  add(value:T):void{
      this.list.push(value)
  }
  min():T{
      var minNum = this.list[0]
      for(let i in this.list){
          if(minNum>this.list[i]){
              minNum = this.list[i]
          }
      }
      return minNum
  }
}

var m = new MinClass<number>() //实例化类 指定类的T的类型是number

m.add(1)
m.add(3)
m.add(2)
m.add(1)
m.add(5)

var m2 = new MinClass<string>()

m2.add('f')
m2.add('c')
m2.add('d')
m2.add('b')
m2.add('a')

console.log(m.min(),m2.min())

```

- 泛型的接口

```ts

```