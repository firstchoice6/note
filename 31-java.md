# java

## 1 javase语法

### 1.1 变量

#### 基本数据类型

```java
- 数值
    整型：byte,short,int,long
    浮点：float,double
- 字符型
    char
- 布尔型
    boolean
```

#### 引用数据类型

```text
- 类
	class
- 接口
	interface
- 数组
	[]
```

| 数据类型 | 说明         | 范围                      | 补充                         |
| -------- | ------------ | ------------------------- | ---------------------------- |
| byte     | 1字节=8bit位 | -128~127                  |                              |
| short    | 2字节        | -2^15~2^15-1              |                              |
| int      | 4字节        | -2^31~2^31-1              | 默认整型为int                |
| long     | 8字节        | -2^63~2^63 -1             | `long l = 1000L`后面要加L或l |
| float    | 4字节        | -3.403E38 ~ 3.403E38      | 后面要加f 或者 F             |
| double   | 8字节        | -1.798E308 ~ 1.798E308    | 默认浮点为double             |
| char     | 2字节        | '中','\n','\u0056', 65(A) | \n换行 \t制表                |
| true     |              | true,false                |                              |

### 1.2 变量间运算

#### 自动类型提升

> byte -> short,char -> int -> long -> float -> double
>
> 小容量和大容量数据做运算	用大容量变量接收

#### 强制类型转换

> byte short char三者做运算 先将自身提升位int再运算
>
> ​	**char转换伪assll码**

```java
public class Demo{
    public static void main(String[] args){
        int a = 100;
        long l = 200L;
        a = a + l;
		// 报错 应该用long接收
        byte c = 10;
        short d = 0.1;
		int c = c + d;
        // 正确 应该用int 接收
    }
}
```

#### 默认类型

- 整型默认为`int`
- 浮点数默认为`double`
- 常量可以不加L 只能在`int`类型范围内

#### 数值和字符串运算

- 字符串的声明

```java
String str = "aaa";
String s = new String("sss");

```

- 字符串和数值只能做连接运算(+)并且结果只能用字符串接收

```java
String s = "sss";
boolean boo = true;
int num = 123;
char c = '中';

String str1 = str + boo;
//'ssstrue'
String str2 = str + num;
//'sss123'
String str3 = str + c;
// 'sss中'
```
- 字符串比较

```java
String s1 = "sss";
String s2 = "ddd";
s1 == s2 // false
s1.equals(s2) // true

// 一个变量一个常量 变量写括号里面
```
### 1.3 关键字

- 数据类型类

  ```text
  class 
  interface 
  enum
  void
  byte 
  long 
  short 
  int 
  float 
  double 
  char
  ```

- 数据类型值类

  ```text
  true false null
  ```

  

- 流程控制类

  ```text
  if else switch case default while 
  do for break continue return
  ```

- 访问权限修饰类

  ```text
  private protected public
  ```

- 类\函数\变量修饰符

  ```text
  abstract final Static synchronized
  ```

- 类与继承

  ```text
  extends implements
  ```

- 实例引用

  ```text
  new this super instanceof
  ```

- 异常处理

  ```text
  try catch finally throw throws
  ```

- 包
  ```text
  package import
  ```
- 其他
  ```text
  native strictfp transient volatile assert
  ```

- 保留字
  ```text
  byValue, cast, false, future, generic, inner, operator, outer, rest, true, var ， goto ，const,null
  ```
### 1.3 标识符
 - 标识符规则
   - 字母 数字 _ $
   - 数字不能开头
   - 不可以使用关键字和保留字
   - 严格区分大小写
   - 不能有空格

- 名称命名规范
  - 包名: 多单词组成时所有字母都小写 `xxxyyyzzz`
  - 类名 接口名: 多单词组成时,所有单词的首字母大写 `XxxYyyZzz`
  - 变量名 方法名: 驼峰命名 `xxxYyyZzz`
  - 常量名: 所有字母都大写 每个单词用_连接 `XXX_YYY_ZZZ`
- 运算符
  - 算术
  - 赋值
  - 比较
  - 逻辑
    ```text
    & 与 
    && 短路与 
    | 或 
    || 短路或 
    ! 非 
    ^ 异或
    ```
    > &与&&  , |与||的区别
    > & 前面为false的时候 后面的依然会执行
    > && 前面false 后面不运行
    > | 前面为true的时候 后面的依然会执行
    > || 前面true 后面不运行
  - 位运算
  - 三元
    ```java
    条件?值1:值2
    ```
### 1.4 进制与位运算
- 进制
  - 二进制 0b或者0B开头
  - 八进制 0开头
  - 十六进制 0x开头

- 进制转换
  - 2进制8进制互转
    - 从后向前 每隔3位相互转 不够补0
  - 2进制16进制互转
    - 从后向前 每隔4位相互转 不够补0

- 补码和反码
  - 计算机存储的是补码
  - 正数的原码 补码 反码都相同
  - 原码: 直接转为2进制 最高位为1
  - 负数反码: 除最高位 按位取反 
  - 负数补码: 反码 + 1

- 位运算
  | 运算符 | 运算       | 范例           | 说明                                       |
  | ------ | ---------- | -------------- | ------------------------------------------ |
  | <<     | 左移       | 3 << 2 = 12    | 0011 -> 1100 每左移一位×2                  |
  | >>     | 右移       | 3 >> 1 = 1     | 0011 -> 0001 每右移一位/2                  |
  | >>>    | 无符号右移 | -3 >>> 1 = 126 | 1111 1100 -> 0111 1110                     |
  | &      | 与运算     | 6 & 2 = 2      | 0110 & 0010 -> 0010   |
  | `| `   | 或运算     | 6  `|` 2 =  6  | 0110 `|` 0010 -> 0110 |
  | ^      | 异或       | 6 ^ 3 = 5      | 0110 ^ 0010 -> 0101 |
  | ~      | 反码       | ~6 = -7        | ~0110 -> 1111 |

  -  `>>` 和`>>>`的区别
    `>>`正数高位用0补 负数高位用1补
    `>>` 无论正数 负数 高位都用0补
  - 反码 -n = ~n + 1   ===>   ~n = -n -1

- java 的scanner
  ```java
  /*
  - 导包
  import java.util.scanner
  - 创建对象
  Scanner s = new Scanner(System.in)
  - 调用方法
  */
  import java.util.Scanner;
  public class ScannerTest{
    public static void main(String[] args){
      Scanner s = new Scanner(System.in);
      String name = s.next();
      boolean boo = s.nextBoolean();
      String sex;
      if(sex) {
        sex = "男";
      } else {
        sex = "女";
      }
      System.out.println("你的名字: " + name + "你的性别: " + sex )
    }
  }
  ```
- 流程判断
  - if
    > 语句只有一行的时候可以省略大括号
    ```java
    if(条件){
      语句1
    } else {
      语句2
    }

    if(条件1){
      语句1
    } else if(条件2) {
      语句2
    } else {
      语句3
    }
    ```
  - switch
  ```java
  switch(条件){
    case 常量1:
    语句1;
    break;
    case 常量2:
    语句2;
    break;
    ...
    default: 
    语句n;
    break;
  }
  ```
  - for
    ```java
    for(初始化条件;循环条件;迭代条件){
      循环体
    }
    ```
  - while
    ```java
    int i  = 0;
    while(条件){
      语句;
      迭代条件;
    }
    ```
  - do...while
    ```java
    int i  = 0;
    do{
      语句;
      迭代条件;
    }while(条件)
    ```

### 1.5 数组

- 数组的声明和初始化

```java
- 声明
String[] names;
int scores[];

- 静态初始化
    // 声明和赋值同时进行
    names = new String[]{"a","b","c"};
	int[] scores = {1,2,3,4,5};
	// 第二种写法声明和初始化要一起进行
- 动态初始化
    // 声明和赋值分开进行
    String[] names2 = new String[5];
	// 数组的长度为5 

// 数组的长度不可变
	
```

- 数组的操作

  - 赋值

    ```java
    names2[0] = "a";
    ```

  - 读取长度

    ```java
    int len = names.length;
    ```

  - 默认值

    - byte short int long  => `0`
    - float double => `0.0`
    - boolean => `false`
    - char => `\u0000`(不是空格)
    - 引用数据类型 => `null`

- 数组的常见算法

  - 最大值，最小值，数组求和，查找，二分查找
  - 复制，反转、排序

- 数组的异常

  - 下角标越界
  - 空指针异常

- 数组工具类

  

  ```java
  // 导入
  import java.util.Arrays;
  
  public class ArraysTest {
      public static void main(String[] args) {
          int[] array = {2,56,24,-2,33};
          // 排序 返回本身
          // sort(int[] array)
          Arrays.sort(array);
          // 转化为字符串
          // toString(int[] array)
          String str = Arrays.toString(array);
          System.out.println(str);
          // 二分查找 返回索引 
          // 二分查找要先排序 不排序结果会出错
          int index = Arrays.binarySearch(array,33);
          System.out.println(index);
      }
  }
  
  ```

## 2 面向对象的编程

### 2.0 相关概念

- 类 ： 抽象的概念上的定义

  - 成员： 属性(field)和方法(method)

    ```java
    class Person{
        // 属性
        int age;
        String name;
        // 方法
        public void say(){
            System.out.println("hello");
        }
    }
    ```

    

- 对象： 具体的实例 对象是由类派生出来的

  ```java
  Person p = new Person();
  // 成员变量
  p.age = 18;
  p.name = "jack";
  p.say();
  // 方法执行
  
  int sex = 0;
  // 局部变量
  
  System.out.println(p);
  // 直接输出对象 输出的是地址
  ```

- 匿名对象

  ```java
  new Person().age = 20;
  // 匿名对象不能多次调用
  ```

  > 应用: 将匿名对象作为哦实参进行传递

- 默认值

  - 局部变量名没有默认值
  - 成员变量默认值和数组一样

- 权限修饰符 (局部变量没有权限修饰符)

  |                      | public | protected | (default) | private |
  | -------------------- | ------ | --------- | --------- | ------- |
  | 同一个类(我自己)     | yes    | yes       | yes       | yes     |
  | 同一个包(我领居)     | yes    | yes       | yes       | no      |
  | 不同包子类(我儿子)   | yes    | yes       | no        | no      |
  | 不同包非子类(陌生人) | yes    | no        | no        | no      |

- 内存

  - 局部变量: 栈
  - 成员变量: 堆中的对象开辟的内存中





### 2.2 面向对象的三大特征

- 封装

  > 将属性保护起来,添加校验规则,信息隐藏

  ```java
  class Account{
      private String name;
      // 属性私用
      
      // 接口公开
      public void setName(String n){
          if(n.length() == 2 || n.length() == 3){
              name = n
          }
      }
  }
  ```

  

- 继承

- 多态

### 2.3 其他关键字

- this
- super
- interface

### 2.4 UML类图

> 项目的设计图

| 名称                                  | 描述                                                    |
| ------------------------------------- | ------------------------------------------------------- |
| Account                               | 类名                                                    |
| -balance:double                       | 属性名： 属性类型                                       |
|                                       | +表示public<br /> - 表示private<br /> # 表示protected   |
| <u>+Account(init_balance: double)</u> | 方法类型（+ - ）方法名（参数名：参数类型）： 返回值类型 |
|                                       | 下划线表示构造器                                        |
| +gteBalance() : double                | 无参有返回值                                            |
| +deposit(amt: double)                 | 有参无返回值                                            |
| +withdraw(amt:double)                 |                                                         |

- 对应的代码

  ```java
  public class Account {
      private double balance;
  
      public Account(double balance){
          this.balance = balance;
      }
      public double getBalance(){
          return balance;
      }
      public void deposit(double amt){
        // 存钱
          balance += amt;
      }
  
      public void withdraw(double amt){
          // 取钱
          if(amt > balance){
              System.out.println("余额不足");
          }else{
              balance -= amt;
          }
      }
  }
  ```

## 3 类

### 3.0 java类及类的成员

- 属性

- 方法

  - 方法的格式

  ```java
  权限修饰符 返回值类型 方法名(形参列表){
      方法体
  }
  ```

  - 方法的类型

   ```java
    public void say(){}
    // 无返回值 无参
    public void setAge(int age){}
    // 无返回值 有参
    public int getAge(){}
    // 有返回值 无参
    public String getDes(String name){}
    // 有返回值 有参
   ```

  - 方法的参数传递

    - 基础类型
    - 引用类型

  - 方法的重载

    > 两同一不同: 同一个类 同一个方法名 不同的形参列表
    >
    > 不同的形参列表:(顺序 类型 个数)
    >
    > 和权限修饰符 返回类型 形参名称无关
    >
    > 通过 方法名+形参列表 确定调用的是哪个方法

    ```java
    public class MethodsTest {
        public static void main(String[] args) {
    
            A a = new A();
            System.out.println(a.add(4,5));
            A b = new A();
            System.out.println(b.add("hello"," world"));
        }  
    }
    
    class A {
        public int add(int a,int b){
            return a + b;
        }
        public String add(String a,String b){
            return a + b;
        }
    }
    ```

  - 可变形参

    ```java
    格式: 方法名 (变量类型 ... 参数){
        
    }
    // 一个方法只能有一个可变形参 且只能放在最后面
    public int sum(int ... numbers){
        int sum = 0;
        for(int i = 0;i<numbers.length;i++){
            sum += numbers[i];
        }
        return sum
    }
    ```

  - package

    > - java为了对类进行统一管理 所以有了包的概念
    >
    > - 包的命名:   com.域名.项目名.模块名
    >
    > - 每`.`一层代表一层目录
    >
    > - 不同包类名可以相同 相同包类名不能相同
    > - 源文件首位置: 行

  - import

    - 1.在源文件中使用inport显式的导入指定包下的类或接口

    - 2.声明在包的声明和类的声明之间。

    - 3.如果需要导入多个类或接口，那么并列显式多个import语句那可

    - 4.举例：可以使用`java.util.*`的方式，一次性导入util包下所有的类或接口。

    - 5.如果导入的类或接口是`java.lang`包下的，或者是**当前包**下的，则可以省略此import语句。

    - 6.如果在代码中使用不同包下的同名的类。那么就需要使用类的**全类名**的方式指明调用的是哪个类。

      ```java
      new java.sql.Date(111L);
      //这种需要传入值
      ```

      

    - 7.import static组合的使用：调用指定类或接口下的静态的属性或方法

      ```java
      import static java.lang.System.out;
      
      out.print("hello");
      // 不常用
      ```

      

    - 8.如果已经导入java.a包下的类。那么如果需要使用a包的子包下的类的话，仍然需要导入。

- 构造器(构造方法)

  - 特征

    - 具有与类相同的名称
    - 不声明返回值类型(与声明为void不同)
    - 不能被`static` `final` `sync...` `abstract` `native`修饰 不能有`return`

  - 作用

    - 创建对象

      创建对象必调用构造器,且只能调用一次

    - 对对象进行初始化

  - 规则

    - 创建对象时才会调用构造器
    - 如果类中没有显示的声明构造器，那么系统会默认给该类创建一个空参的构造器
    - 如果类中已经显示的声明了构造器，那么系统将不再默认创建一个空参的构造器
    - 构造器可以有多个。但是彼此之间必须构成重载

  - 格式

    ```java
    权限修饰符 类名(形参列表){
        方法体
    }
    ```

    ```java
    Person p = new Person('张三');
    // 初始化的同时对对象进行初始化
    
    class Person {
        /*
        public Person(){
            System.out.println("show");
        }
    	public Person(String name){
            System.out.println(name);
        }
        */
        public void show(){
            System.out.println("show");
        }
    }
    ```

- 代码块

- 内部类

### 3.1 类的继承

- 格式

  ```java
  A extends B
      A: Subclass 子类
      B: SuperClass 父类 超类 基类
  ```

- 规则

  - 构造器不在继承范围内
  - 好处
    - 减少冗余 提高复用
    - 提高了代码的复用性
    - 为多态提供了前提
  - `private`属性不能直接访问 但是可以通过间接的方式访问`(get set)`
  - 子类可以有自己的属性
  - 父类的概念是相对的
    - 直接父类
    - 间接父类
  - 所有类都会继承`Object`父类
  - 一个父类可以有多个子类 子类只能有一个父类

  

- 方法的重写

  - 重写后 调用被重写的方法的时候实际上调用的是重写后的方法

  - 重写的条件

    - 方法名一致
    - 形参列表一致
    - 子类重写的方法的权限修饰符 ***不小于*** 父类被重写方法的权限修饰符
    - 返回值void - void Number - Number或者Number的子类
    - 子类方法抛出的异常不能大于父类被重写的方法的异常
    - 父类`private`方法,不能被重写
    - 父类和子类相同的方法 要么同时加`static` (不是重写) 要么同时不加`static`(重写)

  - 注解

    

  ```java
  class Person{
      String name;
      int age;
      private int sex;
      public setSex(int sex){
          this.sex = sex;
      }
      public void show(){
          System.out.print("test");
      }
      public void speak(){
          System.out.print("speak");
      }
  }
  class Student extends Person{
      int scores;
      public void exam(){
          System.out.print("exam");
      }
      // 重写
      @Override // 注解
      public void speak(){
          System.out.print("student speak");
      }
  }
  ```

- super

  - 作用: 调用父类的属性方法构造器

  - `super`类似`this` `this`代表本类对象的引用` super`代表父类的内存空间的标识

  - 当子父类出现同名成员的时候 可以用`super`进行区分

  - 规则

    - 无重写方法 可以省略`super`
    - 无相同属性 可以省略`super`
    - `super`必须放在构造器首行
    - `super(形参列表)`和`this(形参列表)`在一个构造器中只能同时存在一个
    - 一个构造器中如果没有显示的使用`this(形参列表)`和`super(形参列表)`那么**默认使用的是`super()`(父类空参构造器)**
    - 创建子类对象时,必调用父类构造器

  - 报错

    ```text
    Implicit super constructor Person() is undefined.
    Must explicitly invoke another constructor
    
    解决方案:
    1.父类中显示的再定义一个空参的构造器
    2.子类的构造器中显示的指定调用父类中的有参构造器
    ```

    

  ```java
  class Person{
      String name = "person";
      public Person(){
          System.out.print("person无参构造器");
      }
      public Person(String name){
          System.out.print("person有参构造器");
      }
      public void show(){
          System.out.print("person");
      }
      public void say(){
          System.out.print("hello");
      }
  }
  class Student{
      String name = "student";
      public Student(){
          super();
          // 调用父类空参构造器
          System.out.print("student无参构造器");
      }
      public Student(String name){
          super("zhangsan");
          // 调用父类有参构造器
          System.out.print("student有参构造器");
      }
      public void info(){
          System.out.print(name);
          System.out.print(super.name);
      }
      public void show(){
          super.show();
          // 调用父类的show方法 
          say(); // 相当于this.say() 父类 子类的say方法都可以调用 
          System.out.print("student");
      }
  }
  ```

### 3.2 多态

- 广义的多态： 方法的重写 重载 子类对象的多态性

- 侠义的多态：子类对象的多态性

  ```java
  class Person{
      String name = "person";
      public void show(){
          System.out.println("person show");
      }
  }
  class Man extends Person(){
      String name = "man";
       public void show(){
          System.out.println("man show");
      }
       public void smoking(){
          System.out.println("man smoking");
      }
  }
  class Woman extends Person(){
       public void show(){
          System.out.println("woman show");
      }
       public void buy(){
          System.out.println("woman buy");
      }
  }
  
  public class Test{
      public static void main(String[] args){
          // 父类的引用指向子类的对象
          // 这就是一种多态
          Person p = new Man();
          // 虚拟方法调用(动态绑定): 编译看左边,运行看右边
          p.show();
          p.smoking();// 报错
      }
  }
  ```

- 多态的缺点:不能调用子类独有的方法和属性

- 解决方案: 向下转型

  ```java
  Man m = (Man)p;
  m.smoking(); //正确用法
  
  // p本身是一个man对象 转化为woman报错
  Woman w = (Woman)P; // 错误用法 类型转换异常
  
  
  - instanceof方法(返回true/false)
      
  if(p instanceof Man){
      System.out.println("i am a man");
  }else {
      System.out.println("i am not a man");
  }
  
  p instanceof Person // true
  p instanceof Man // true    
  p instanceof Woman // false    
  ```

- 属性不存在多态性

  ```java
  Person pp =  new Man();
  pp.name; // "person"
  ```

- 多态的两个条件

  - 要有继承关系
  - 要有方法的重写

- 多态的应用

  ```java
  // 需求 创建不同的手机调用它的两个方法
  
  class Test{
      pubic static void main(String[] args){
          // 使用多态
          Test test = new Tset();
          test.phone(new Android());
          test.phone(new Android());
          test.phone(new Iphone());
          // 不使用多态
          Android ap1 = new Android();
          ap1.call();
          ap1.sendMsg();
          ap1.scanFile();
          Android ap2 = new Android();
          ap2.call();
          ap2.sendMsg();
          ap2.scanFile();
          Iphone iphone = new Iphone();
          iphone.call();
          iphone.sendMsg();
      }
      public void phone(Phone phone){
          phone.call();
          phone.sendMsg();
          if(phone instanceof Android){
              Android a = (Android)phone;
              a.scanFile();
          }
      }
  }
  class Phone{
       public call(){
          System.out.println("phone call");
      }
      public sendMsg(){
          System.out.println("phone send msg");
      }
  }
  class Android extends Phone{
      public call(){
          System.out.println("android call");
      }
      public sendMsg(){
          System.out.println("android send msg");
      }
      public scanFile(){
          System.out.println("android scan file");
      }
  }
  class Iphone extends Phone{
      public call(){
          System.out.println("iphone call");
      }
      public sendMsg(){
          System.out.println("iphone send msg");
      }
  }
  ```
  

### 3.3 Object类

- object类是所有java类的父类
- object有一个构造器且空参
- object有11个方法，其中三个是重载的（`wait`）



- `equals`方法

  ```java
  - 源码
      
  public boolean equals(Object obj){
      return (this == obj);
      // 比的是地址
  }
  String str1 = new String("str1");
  String str2 = new String("str2");
  
  String Date类对equals方法进行了重写
      
  所以 str1.equals(str2); // true
  
  
      
  ```

  - `==`和`equals`的区别
    - 如果比较的是基本类型 比较的是值
    - 比较引用类型 比较的是地址
  - 自定义类中一般都会重写`equals`方法,用来比较内容

- `toString`方法

  ```java
  - 源码
  public String toString(){
      return getClass().getName() + "@" + Integer.toHexString(hashCode());
  }
  
  Animal animal = new Animal();
  System.out.println(animal);
  
  // 输出引用 调用的就是.toString() 相当于
  System.out.println(animal.toString());
  ```

  - `String` `Date`等类对`toString`方法进行了重写
  - 自定义类中一般都会重写`toString`方法,用来输出对象

### 3.4 static关键字

- `static`可以修饰属性方法 代码块 内部类(类本身的属性 方法 )
- 成员变量(属性): 
  - 实例变量
  - 类变量(static修饰的变量)

```java
// 通过类
Person.nation = "china";
Person.demo();
// 通过实例    
Person p = new Person("jack",18);
p.nation = "russia";
p.demo();

class Person{
    String name;
    int age;
    // 类属性
    static String nation;
    // 静态方法
    public static void demo(){
        System.out.println("demo");
    } 
}

/*
类加载的过程

- 当我们创建对象时，首先会在方法区去查找该类的信息
- 如果方法区没有该类的信息，这时便会进行类加载。如果方法区有该类的信息则直接创建对象
- 类加载：将字节码文件加载到JVM中，同时在方法区特定的区域用来存放类变量。
- 类加载完成后，再创建对象
- 再次创建对象时仍在方法区查找该类的信息。如果存在将不会再进行类加载（类加载只加载一次）*/
```

- `static`修饰变量 ----- 类变量 规则
  - 同一个类的对象共同拥有一份类变量 每个对象各自拥有一份实例变量 当一个对象对类变量进行修改 其他对象看到的也是修改后的类变量
  - 类变量随着类的加载而加载, 实例对象随着对象的创建而创建
  - 类加载只加载一次
  - 调用类加载的方式:
    - 类名.类变量名
    - 实例对象.类变量名
- `static`修饰方法 ----- 静态方法 规则
  - 静态方法随着类的加载而加载
  - 调用静态方法 
    - 类名.静态方法名
    - 实例对象.静态方法名
  - 静态方法可以调用类变量  静态方法
  - 静态方法不能调用非静态方法  不能调用实例变量(加载的时机不同)
  - 非静态方法可以调用静态方法
  - 静态方法不能调用`this` `super`
- `static`的使用场景
  - `static`修饰属性
    - 当一个变量为常量的时候(Math.PI  Math.E )
    - 多个对象共同使用一个属性
  - `static`修饰方法
    - 工具类中的方法(array)
    - 有时为了调用类变量,也会使用

```java
 // - id自增
class Dog{
    int id;
    String name;
    static  int count;
    public Dog(String name){
        this.name = name;
        id = ++count;
    }
}
```



### 3.6 代码块

- 代码块只能被`static`修饰
- 分类
  - 静态代码块:
    - 对类中的信息进行初始化
    - 随着类的加载二加载 只加载一次
    - 优先于非静态代码块
    - 可以有多个 从上到下执行
    - 只能调用**类变量**和**静态方法**
  - 非静态代码块: 
    - 对对象进行初始化
    - 随着对象的创建而加载
    - 优先于构造器
    - 可以有多个 从上到下执行
    - 可以调用**类变量**和**静态方法**

```java
class Student{
    String name;
    {
        // 非静态代码块
        System.out.println("{}");
    }
    // 静态代码块
    static {
        System.out.println("static{}");
    }
}
```

- 变量的赋值方式

  - 默认值

  - 显示赋值

  - 代码块赋值

  - 构造器赋值

  - 对象名.方法名/属性名

    > 显示赋值 和 代码块赋值: 谁在上面谁先赋值

- 代码块实现单例

  ```java
  class Bank{
      private Bank(){}
      private static Bank bank = null;
      static{
          bank = new Bank();
      }
      public static Bank getInstance(){
          if(bank == null){  
          	bank = new Bank();
          }
          return bank;
      }
  }
  ```

### 3.7 final关键字

- 规则

  - `final`修饰的类

    - 不能被继承(比如`String` `StringBuffer`)

  - `final`修饰的方法

    - 不能被重写

  - `final`修饰的变量

    - 只能被赋值一次 并且必须初始化
    - 修饰的变量往往作为**常量**
    - 赋值方式: 显示赋值 代码块 构造器赋值

    ```java
    class C{
        final int age;
        public C(){
            age = 20;
        }
        public C(String name){
            // 不对age赋值报错
        }
    }
    ```

  - final修饰的对象不能重新new 但可以对属性进行赋值 因为地址没有改变

### 3.8 抽象类

- 用`abstract`关键字来修饰一个类时，这个类叫做抽象类

- 用`abstract`来修饰一个方法时，该方法叫做抽象方法。

  - 只有方法的声明，没有方法的实现。以分号结束

    ```java
    abstract class Bank{
        abstract int abstractMethod( int a );
    }
    ```

- 含有抽象方法的类必须被声明为抽象类。
  - 抽象类不能被实例化。
  - 抽象类是用来被继承的，抽象类的**非抽象子类** **必须重写父类的抽象方法，并提供方法体。**
  - 如果非抽象子类的父类(抽象或非抽象)重写了抽象方法 那么抽象子类将并**不用**再重写抽象方法
  - 若没有重写全部的抽象方法，仍为抽象类。

- **不可以和`abstract` `final` `static`关键字一起使用**






### 3.10 接口

>  接口是一种特殊的抽象类，这种抽象类中只包含常量和方法的定义，而没有变量和方法的实现。

- 格式

  ```java
  interface 接口名{
      
  }
  ```

- 说明

  - 接口和类是并列关系
  - 接口不能被实例化
  - 接口只能有常量和抽象方法(**jdk1.8**)
  - 接口和接口的关系: 继承而且是多继承
  - 类和接口通过`implements`实现连接
    - 一个类实现接口后必须重写接口中的**所有**抽象方法
    - 如果不想重写接口中的方法 该类可以声明成抽象类
    - 可以实现多个接口(java单继承 多实现)

  ```java
  interface MyEnglish{
      // 常量
      public static final String NAME = "interface";
      // 默认会添加public static final
      int age = 10;
      
      // 抽象方法
      public abstract void say();
      // public abstract 可以省略 默认添加
      void show()
  }
  interface B{
      void test();
  }
  interface A extends MyEnglish,B{
      void testA();
  }
  
  class Student implements B,A{
      @Override
      public void test(){
          // 类需要实现接口的方法
      }
      @Override
      public void testA(){
          // 必须重写接口中的 所有 抽象方法
      }
  }
  
  abstract class HighStudent implements B{
  	// 如果不想重写接口中的方法 该类可以声明成抽象类
  }
  ```

- 接口和类之间的多态性

  - 格式

    ```java
    接口的类型 变量名 = new 接口的实现类的对象() - 接口的类型指向接口实现类的对象
    ```

    ```java
    public static void main(String[] args){
        Computer pc = new Computer();
        Mouse mouse = new Mouse();
        pc.runUSB(mouse);
        
        pc.runBlueTooth(mouse);
    }
    
    
    interface USB{
        void start();
        void close();
    }
    interface BlueTooth{
        void b_start();
        void b_close();
    }
    
    class Mouse implements USB,BlueTooth{
        @Override
        public woid start(){
            System.out.println("鼠标插入");
        }
        @Override
        public woid close(){
            System.out.println("鼠标拔出");
        }
        @Override
        public woid b_start(){
            System.out.println("鼠标已连接蓝牙");
        }
        @Override
        public woid b_close(){
            System.out.println("鼠标断开蓝牙连接");
        }
    }
    
    class Computer{
        public void runUSB(USB usb){
            usb.start();
            System.out.println("usb设备运行中");
            usb.close();
            System.out.println("usb设备停止运行");  
        }
        public void runBlueTooth(BlueTooth bt){
            bt.start();
            System.out.println("蓝牙设备运行中");
            usbtb.close();
            System.out.println("蓝牙设备停止运行");  
        }
    }
    ```


 ### 3.11 内部类

>  一个类A中声明另一个类B B就是内部类 A是外部类

- 分类

  - 成员内部类

    ```java
    class A{
        class B{
            // 非静态成员内部类
        }
        static class D{
            // 静态成员内部类
        }
    }
    ```

    

  - 局部内部类

    ```java
    public void demo(){
        class C{
            
        }
    }
    ```

    

- 规则(作为内部类) 同时有类的所有功能

  - 可以使用四种权限修饰符
  - 可以使用`static`修饰符
  - 可以调用外部类的结构(属性 方法)

- 语法

  - 创建

    ```java
    // 创建非静态内部类的对象
    // 写法1
    A.B b = new A().new B();
    // ~~写法2~~
    // 导包
    import com.path.java
    B b = new A().new B();
    
    // 创建静态内部类的对象
    A.C c = new A.C()
    ```

  - 调用外部类的结构
  
    ```java
    外部类名.this.属性名/方法名
    // 静态成员内部类 不能调用 **外部类的实例变量和非静态方法**    
    ```
  
    
  
    ```java
    class A{
        String name = "外部";
        int age = 18;
        String msg = "外部类的数据";
        Class B{
            int age  =19;
            String msg = "内部类的数据"
            public void info(){
            	System.out.println(name); // 外部
            	System.out.println(age); // 19
            	System.out.println(A.this.age); // 18
                
                String msg = "当前的数据"
                System.out.println(msg); // 当前的数据
                System.out.println(this.msg); // 内部类的数据
                System.out.println(A.this.msg); // 外部类的数据
                
                
                // *** 方法同理 ***
            }
        }
    }
    
    ```
  
  - 局部内部类
  
    ```java
    public class InnerClass{
    	public static void main(String[] args){
            InnerClass ic = new InnerClass();
            // 调用直接调用
            ic.say();
            // 获取 -- 通过接口实现获取  不能向下转型
            SayInterface say2 = ic.demo();
            say2.demo();
            // 获取 -- 多态 接受的是父类的引用 但实际返回的是子类的对象
            SuperSay say3 = ic.say3();
            say3.demo();
                
        }
        public void say(){
            class D{
                public void demo(){
                    System.out.println("demo");
                }
            }
            D d = new D();
            d.demo();
        }    
        // 通过接口
         public SayInterface say2(){
            class F implements SayInterface{
                public void demo(){
                    System.out.println("demo");
                }
            }
            F f = new F();
             return f;
        }
        // 通过继承
        public SuperSay say3(){
            class H extends SuperSay{
                public void demo(){
                    System.out.println("demo");
                }
            }
            H h = new H();
             return h;
        }    
    }
    
    interface SayInterface{
        public abstract void demo();
    }  
    
    class SuperSay{
        public void demo(){
            
        }
    }
    
    ```
  
    - 如何获取局部内部类的对象
      1. 在类的外部创建一个类或者一个接口
      2. 使局部内部类实现接口或继承类
      3. 局部内部类所在的方法的返回值类型为"父类"或"接口"的类型
      4. 在方法中返回局部内部类的对象(多态)

### 3.12 枚举类

>  一个类的对象是可数多个的。这样的类叫做枚举类

- ~~自定义枚举类~~

  ```java
  class Season{
  	
  	//需求 ：不能修改属性的值
  	private final String seasonName;
  	private final String seasonDes;
  	
  	//1.私有化构造器 - 在类的外部不能再去创建该类的对象
  	private Season(String seasonName,String seasonDes){
  		this.seasonDes = seasonDes;
  		this.seasonName = seasonName;
  	}
  	
  	//2.创建本类的对象
  	public static final Season SPRING = new Season("春天","春眠不觉晓");
  	public static final Season SUMMER = new Season("夏天","处处蚊子咬");
  	public static final Season AUTUNM = new Season("秋天","秋风扫落叶");
  	public static final Season WINTER = new Season("冬天","冬天不洗澡");
  
  	//提供获取属性的方法
  	public String getSeasonName() {
  		return seasonName;
  	}
  	public String getSeasonDes() {
  		return seasonDes;
  	}
  		
  }
  
  ```

- 使用enum定义枚举类

  - 格式

    ```java
    enum 类名{}
    ```

  - 方法

    ```java
     values()方法：
         返回枚举类型的对象数组。该方法可以很方便地遍历所有的枚举值。
         
    valueOf(String str)：
        可以把一个字符串转为对应的枚举类对象。
    	要求字符串必须是枚举类对象的“名字”。// 区分大小写
        如不是，会有运行时异常:IllegalArgumentException。
    
    ```

  - 语法

    ```java
    enum Season3{
    
    	//创建枚举类的对象，必须放在枚举类的首行，多个对象之间用","隔开。以";"结尾
    	SPRING("spring"), //调用的有参构造器
    	SUMMER("summer"),
    	AUTUNM("autumn"),
    	WINTER("winter");
    	
    	final String seasonName;
    	
    	//构造器只能使用private修饰 - 如果是空参的构造器可以省略不写默认也是private修饰的
    	private Season3(String seasonName){
    		this.seasonName = seasonName;
    	}
    
    	public String getSeasonName() {
    		return seasonName;
    	}
    }
    
    ```

    

  - 实现接口的枚举类

    ```java
    enum Season2 implements Des{
    
    	//创建枚举类的对象，必须放在枚举类的首行，多个对象之间用","隔开。以";"结尾
    	SPRING{
    		@Override
    		public void info() {
    			System.out.println("spring info");
    		}
    	},SUMMER{
    		@Override
    		public void info() {
    			System.out.println("summer info");
    		}
    	},AUTUNM{
    		@Override
    		public void info() {
    			System.out.println("autumn info");
    		}
    	},WINTER{
    		@Override
    		public void info() {
    			System.out.println("winter info");
    		}
    	};
    	//构造器只能使用private修饰 - 如果是空参的构造器可以省略不写默认也是private修饰的
    	private Season2(){
    		
    	}
    }
    
    ```

### 3.13注解

> 注解可以对类中的结构（属性，方法构，构造器...）进行补充说明，并不改变原来的结构。

- 常用的三个注解：

  - `@Override` : 限定重写父类方法, 该注解只能用于方法
  - `@Deprecated`: 用于表示某个程序元素(类, 方法 等)已过时
  - `@SuppressWarnings`: 抑制编译器警告 

- 自定义注解

  - 格式

    ```java
    @interface MyAnn{
        // 可以认为声明了一个属性
        String name();
        // 给age赋值默认值20
        int age() default 20;
    }
    
    class Demo{
        @MyAnn(name="张三")
        public void show(){
            //...
        }
    }
    ```

- 元注解

  - 注解上的注解 （元注解是用来对注解的一个补充说明）
  - 常用元注解
    - Retention:保留策略
      - SOURCE : （被丢弃了）。当进行编译时便不能使用了。
      - CLASS ：编译时 - 运行时

      -  RUNNTIME（记住 ） ： 在整个运行期间 - 如果想要通过反射拿到该注解那么必须是RUNNTIME
    - Target：目标，可以用来修饰什么数据
    - Documented：能否在生成的帮助文档中显示
    - Inherited：注解是否具备继承性

## 4 异常处理

### 4.1 异常体系结构

- `error`
  - `error`没有针对的代码处理
- `exception`
  - `exception`可以进行处理
  - 分类
    - 编译时异常 
    - 运行时异常 



### 4.2 异常处理



- `try-catch-finally`

  ```java
  try{
      可能发生异常的代码
  }catch(异常类型 e){
  	发生异常时的代码
  }
  ...
  finally{
      一定会执行的代码
  }
   
  // 先写针对某个异常的 后面写Exception
  // 可以有多个catch 子类写父类上面
  // 可以不写finally 可以不加catch(注意关闭资源)
  
  // 运行时异常一般都不处理
  try{
      
  }catch(ArithmeticException e){
      String errorInfo = e.getMessage();
      // 通过 e.getMessage 获取错误信息
      e.printStackTrace();
      // 打印详细错误信息
  }
  ```

- `finally`

  ```java
  // catch中再次发生异常 (或者让没有捕捉到异常) finally也一定会执行
  
  try{
      System.out.println(1/0);
  }catch(Exception e){
      System.out.println(1/0);
      System.out.println("不会打印这句话");
  }finally{
      System.out.println("会打印这句话");
  }
  // catch中 return finally也一定会执行 且finally里的代码优先于return执行
  
  public int getNumber(){
     try{
         System.out.println(1/0);
     }catch(Exception e){
         return 10;
     }finally{
         return 20;
         // 最终return 20
     }
  }
  
  
  public int getNumber(){
     try{
         System.out.println(1/0);
     }catch(Exception e){
         return 10;
         // 后 return 10
     }finally{
        System.out.println("先打印这句话");
     }
  }
  ```

- `throws`

  - 格式

    ```java
    方法名(形参) throws 异常类型,异常类型2...{
        方法体
    }
    ```

  - 说明

    - `throws`没有处理异常 只是向上抛 谁调用谁处理异常

    - 最终还是`try-catch`解决

      ```java
      public class ThrowsTest{
      
          public static void main(String[] args){
      		ThrowsTest tt = new ThrowsTest();
              // 调用的时候解决异常
              try{
                  tt.test3();
              }catch(FileNotFoundException){
                  e.printStackTrace();
              }
          }
          // 向上抛出异常
          public void test3() throws FileNotFoundException{
              test2();
          }
          
          // 向上抛出异常
          public void test2() throws FileNotFoundException{
              test1();
          }
          
          // 向上抛出异常
          public void test1() throws FileNotFoundException{
              new FileInputStream("123.txt")
          }
      }
      
      ```

  - 使用`throws`的时机

    > - 在main中如果需要连续调用多个方法 那么发生异常,该异常只能向上抛,给方法的调用者进行处理
    >
    > - 如果父类被重写的方法没有抛出异常 那么子类重写的方法也不能抛出异常
    > - 子类重写的方法抛出的异常不大于父类被抛出的异常

- `throw`

  > 手动向外抛异常

  - 格式

    ```java
    throw 异常类的对象;
    ```

    

  - 使用

  ```java
  // 需求 输入的年龄小于0 的时候 程序终止运行
  
  // 编译时异常
  public static void main(String[] args){
      ThrowTest tt = new ThrowTest();
      try{
          tt.setAge2(-10);
      }catch(FileNotFoundException e){
          e.printStackTrace();
      }
  }
  
  public void setAge2(int age) throws FileNotFoundException{
      if(age < 0){
          // 创建的是一个编译时异常 一般都throws
          throw new FileNotFoundException("age不能小于0");
      }else{
          System.oou.println(age)
      }
  }
  
  ===========================================================
  
  // 需求 输入的年龄小于0 的时候 程序终止运行
  // 运行时异常
  public void setAge(int age){
      if(age < 0){
          // 创建的时一个运行时异常
          throw new ArithmeticException("age不能小于0");
      }else{
          System.oou.println(age)
      }
  }
  ```

- 自定义异常类

  - 格式

    ```java
     // 1. 继承Exception(编译时异常) 或者RuntimeException(运行时异常)
     public class MyException extends Exception{
         // 3. 提供一个uid
         // 可以显示声明 也可以~~不声明~~
         prite static final long serialVersionUID  = -12381209390L;
         
         // 2. 提供两个构造器(一个空参 一个有参)
         public MyException(){
             
         }
         public MyException(String message){
             super(message)
         }
     }
        
    ```



## 5 java常用类

### 5.0 单元测试

- 使用

  - 1.导包 -> 项目上右键 ->uildPath -> AddLibraries -> JUnit -> JUnit4
  -  2.在类中写一个无返回无参的方法
  -  3.在该方法上加一个注解@Test
  -  4.运行 ： 在测试方法上 -> 右键- RunAs -> JUnitTest -> 红条表示运算失败，绿条表示成功

  ```java
  public class UnitTestTest {
  	@Test
  	public void test(){
  		System.out.println(1 / 0);
  	}
  }
  ```

- 说明

  - 单元测试的方法所在的类必须是public
  - 方法必须为**无参无返回值**

### 5.1 包装类

> 针对8种基本数据类型定义响应的引用类型
>
> 有了类的特点 就可以调用类的方法
>
> | 基本数据类型 | 包装类        |
> | ------------ | ------------- |
> | boolean      | Boolean       |
> | char         | **Character** |
> | byte         | Byte          |
> | short        | Short         |
> | int          | **Integer**   |
> | long         | Long          |
> | float        | Float         |
> | double       | Double        |
>
> 

- 数据类型转换

  - ~~基本数据类型 -> 包装类 :  通过包装类的构造器~~~

    ```java
    @Test
    public void test(){
        int a = 10;
        Integer inte = new Integer(a);
    }
    ```
    
  - ~~包装类 -> 基本数据类型  :  `包装类的对象.xxxValue()`~~
  
    ```java
    @Test
    public void test(){
        Integer inte = new Integer(10);
        int intValue = inte.intValue();
    }
    ```
  
  - 拆箱和装箱
  
    - 拆箱: 将包装类直接赋值给基本数据类型
    - 装箱: 将基本数据类型赋值给包装类
  
    ```java
    @Test
    public void test(){
        // 拆箱
        int a = new Integer(10);
        // 装箱
        int b = 20;
        Integer integer = b;
    } 
    ```
  
    - 应用场景
  
    ```java
    @Test
    public void test(){
        // 自动装箱 -> 多态
    	boolean res = "aaa".equals(10);
        // 自动装箱 打印true
        demo(10);
    } 
    
    public void demo(Object obj){
        System.out.println(obj instanceof Integer);
    }
    ```
  
  - `String`转基本数据类型: `包装类.parseXxx()`
  
    ```java
    @Test
    public void test(){
        String str = "11";
        int num = Integer.parseInt(str);
        
        str = "true";
        boolean boo = Boolean.parseBoolean(str);
        
    }
    ```
  
  - `String`转包装类型: 通过包装类的构造器
  
    ```java
    Integer integer = new Integer("111");
    
    Boolean boo = new Boolean("trueaaa"); // false 
    ```
  
  - 基本数据类型转`String`
  
    ```java
    // 方式1
    int num = 25;
    System.out.println("" + num);
    // 方式2
    String.valueOf(number);
    ```
  
  - `String`转包装类:`toString()`
  
    ```java
    new Integer = new Integer(10);
    String str = integer.toString();
    ```
  
  
  

### 5.2 `String`类

- 说明

  - `String`被`final`修饰

  - 实现了`Serializable`接口

    >  就可以被序列化，被序列化后才能在不同的进程或前后端进行数据的传输

  - 实现了`compareTo`接口

    > 可以用来比较内容

  - 实现了`CharSequence`接口

    > 可以用来获取字符串长度 字符串中的某个字符

  - 字符串在底层是通过`char`类型的数组实现的

- 创建

  ```java
  String s  = "str";
  
  String s = new String("abc");
  ```

  - 区别
    - 第一种方式`"str"`存储在常量池 多次创建一样的字符串 指向相同
    - 第二种方式`"abc"`  本质是数组 存储在堆上 new一次 创建一次 指向不同

- `String`是不可变的字符序列

  ```java
  String s = "hellojava";
  String s1 = "hello";
  String s2 = "java";
  String s3 = "hello" + "java";
  s == s3; // true
      
  String s4 = s1 + s2;
  String s5 = s1 +"java";
  // 只要有变量参与字符串拼接 底层都有new String() 
  s == s4; // false
  s4 == s5; // false
  s4.equals(s6); // true
  
  // 直接去内存中的常量池中获取该字符串
  String s7 = s6.intern();
  s == s7 // true
  
      
  ```

- **API**

  ```java
  // 获取字符串长度
  length();
  // 获取指定位置字符 返回索引值
  charAt(int index);
  
  // 比较字符串内容 返回true/false
  equals(Object obj);
  // 比较内容大小 返回编码值的差 (了解)
  compareTo(String s);
  
  // 获取指定子字符串的位置 没有返回 -1 多个返回startIndex后的第一个
  indexOf(string s[,int startIndex]);
  // 从后向前查找
  lastIndexOf(string s[,int startIndex]);
  
  // 是否以prefix开头 返回true/false 
  startsWith(String prefix);
  // 是否以suffix结尾 返回true/false 
  startsWith(String suffix);
  
  // string从索引firstStart开始的子字符串中是否包含 other从otherStart索引开始向后length(偏移量)
  // 返回true/false
  string.regionMatches(int firstStart,String other,int otherStart,int length);
  "abcdefg".regionMatches(2,"acvdeg",3,2); // true
  // "acvdeg"从索引3开始偏移2个字符 -> "de"
  // "abcdefg" 从索引2开始 -> "cdefg" 
  
  // 去除两头空格
  trim();
  // 截取子字符串 包头不包尾
  subString(int indexStart[,indexEnd]);
  
  // 字符替换
  replace(char oldChar, char newChar);
  // 字符串替换
  replaceAll(String oldStr, String newStr);
  
  //字符串拼接
  concat(String str);
  // 切割 返回数组
  split(String regex)
  // 字符串是否包含字符串s 返回true/false
  contains(CharSequence s);
  
  // 转换大小写
  toUpperCase();
  toLowwerCase();
  // 忽略大小写比较
  equalsIgnoreCase();
  
  // 字符串转化为char数组
  toCharArray();
  // char数组转字符串
  new String(charArray[,index,length]);
  
  // 转化为byte数组
  getBytes();
  // byte数组转字符串
  new String(byteArray[,index,length]);
  ```




### 5.3 StringBuffer 和StringBuilder

- 说明
  - `StringBuffer `可变的字符序列 线程安全 效率低
  - `StringBuilder`可变的字符序列 线程不安全 效率高

- `API`

  ```java
  // 添加元素 null会添加为'n','u','l','l'
  append(String str);
  append(int n);
  append(Object obj);
  append(char n);
  append(long n);
  append(boolean n);
  
  // index位置插入str
  insert(int index, String str);
  
  // 反转字符串
  reverse();
  
  // 删除 左闭右开
  delete(int startIndex,int endIndex);
  
  // 获取指定位置的元素 返回char
  charAt(int n);
  // 修改指定位置的元素 无返回值
  setCharAt(int n,char newChar);
  
  // 替换指定位置
  replace(int startIndex, int endIndex, String str);
  // str在当前字符串的位置
  indexOf(String str);
  
  // 截取子串 左闭右开 返回新的字符串
  subString(int startINdex,int endIndex);
  // 获取长度
  length();
  ```

- `String Buffer`构造器

  ```java
  StringBuffer sb = new StringBuffer();
  
  StringBuffer();
  // 无参构造器 构造一个没有字符的字符缓冲区,并构造了16个字符的初始容量 大于16的时候会扩容 2倍加2
  StringBuffer(CharSequence saq);
  // 构造一个包含指定的CharSequence相同字符的字符串缓冲区
  StringBuffer(int capacity);
  // 构造一个没有字符的字符缓冲区,并构造指定的初始容量
  StringBuffer(String str);
  // 构造一个初始化为指定字符串内容的字符串缓冲区
  // 底层创建长度str.length + 16 的数组
  // 不能传null 会抛出异常
  ```

- 效率对比

  `StringBuilder` > `StringBuffer` > `String`

### 5.4 Date类

- `java.lang.System`

  ```java
  // 1970-1-1 到此时的毫秒值
  System.currentTimeMillis();
  ```

- `java.util.Date`

  ```java
  // 获取当前日期和时间
  new java.util.Date();
  // 获取time(毫秒数)对应的时间
  new Date(long time);
  
  // 获取毫秒数
  getTime();
  // 输出日期和时间 直接输出 默认调用的就是toString()方法
  toString();
  ```

  

- `java.sql.Date`

  > 对应数据库中时间的类型
  
  ```java
  new Date(long time);
  // 只有日期 没有时间
  
  getTime();// 获取毫秒数
  
  toString();
  ```
  
  - 将当前时间插入到数据库中
  
    ```java
    java.sql.Date date = new java.sql.Date(new java.util.Date().getTime());
    ```
  
- `java.text.SimpleDateFormat`

  > `java.text`包下的都是国际化的
  
  ```java
SimpleDateFormat sdf = new SimpleDateFormat();
  // 将日期转化为String
  sdf.format(new Date()); // 20-8-31 上午10:08
  
  SimpleDateFormat stf2 = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSSZ");
  sdf2.format(new Date()); // 2020-08-31T22:23:46.845+0800
  // 字符串转时间
  parse(String name);
  ```
  
- `java.util.Calendar`（日历）类

  ```java
  // 不能 new
  Calendar instance = Calendar.getInstance();
  
  // 获取当天是当月的第几天 返回int类型
  instance.get(Calendar.DAY_OF_MONTH); //31
  // 获取当天是当年的第几天
  instance.get(Calendar.DAY_OF_YEAR); //240
  
  // 将当月的第几天修改为当月的第一天
  instance.set(Calendar.DAY_OF_MONTH,1);
  // 将当天是当月的第几天减一天
  instance.add(Calendar.DAY_OF_MONTH,-1);
  
  // 获取日历对应的日期
  Date date = instance.getTime();
  ```

- **JDK8中新API**

  - `now()`静态方法 根据当前时间创建对象/指定时区的对象

      ```java
      LocalDate date = LocalDate.now();
// 2020-08-31
      
      LocalTime time = LocalTime.now();
    // 22:55:23.233
      
    LocalDateTime dateTime = LocalDateTime.now();
      // 2020-08-31T22:55:23.233
    ```
  
  - `of()` 根据参数构建日期对象
  
    ```java
    LocalDate date = LocalDate.of(2020,08,31);
    
    LocalTime time = LocalTime.of();
    
    LocalDateTime dateTime = LocalDateTime.of();
    ```
  
  - 获取当天是当月的第几天...
  
    ```java
    date.getDayOfMonth();
    ...
    ```
  
  - ....
- Instant类
    ```java
    // 获取的是utc时间
      Instant instant = Instant.now();
    // 结合时间的偏移获取时间
      instant.atOffset(ZoneOffset.ofHours(8));

    // 时间戳 1970到现在的毫秒数
      instant.toEpochMilli();
    
    // 根据毫秒数获取对应的时间
    Instant.ofEpochMilli(1553123123);
    ```

- DateTimeFormatter类

  ```java
  // 预定义格式
  DateTimeFormatter dtf = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
  // 将时间转化为字符串
  dtf.format(LocalDateTime.now()); // 2020-09-01T07:09:23.232
  // 其他格式
  DateTimeFormatter dtf2 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG);
  
  dtf2.format(LocalDateTime.now()); // 2020年9月1日上午07点12分12秒
  
  // 自定义格式
  DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
  // 字符串转时间
  dtf.parse("2020-09-01T07:09:23.232")
  ```
| 方法                                                     | 描述                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| now() /  now(ZoneId zone)                         | 静态方法，根据当前时间创建对象/指定时区的对象                |
| of()                                                     | 静态方法，根据指定日期/时间创建对象                          |
| getDayOfMonth()/getDayOfYear()                               | 获得月份天数(1-31) /获得年份天数(1-366)                      |
| getDayOfWeek()                                       | 获得星期几(返回一个  DayOfWeek 枚举值)                       |
| getMonth()                                               | 获得月份, 返回一个  Month 枚举值                             |
| getMonthValue() / getYear()                          | 获得月份(1-12) /获得年份                                     |
| getHours()/getMinute()/getSecond()                       | 获得当前对象对应的小时、分钟、秒                             |
| withDayOfMonth()/withDayOfYear()/  withMonth()/withYear() | 将月份天数、年份天数、月份、年份修改为指定的值并返回新的对象 |
| with(TemporalAdjuster t)                | 将当前日期时间设置为校对器指定的日期时间                     |
| plusDays(), plusWeeks(),   plusMonths(), plusYears(),plusHours() | 向当前对象添加几天、几周、几个月、几年、几小时               |
| minusMonths() / minusWeeks()/  minusDays()/minusYears()/minusHours() | 从当前对象减去几月、几周、几天、几年、几小时                 |
| plus(TemporalAmount t)  /minus(TemporalAmount t) | 添加或减少一个 Duration 或 Period                            |
| isBefore()/isAfter()                                     | 比较两个 LocalDate                                           |
| isLeapYear()                                             | 判断是否是闰年（在LocalDate类中声明）                        |
| format(DateTimeFormatter t)                     | 格式化本地日期、时间，返回一个字符串                         |
| parse(Charsequence text,DateTimeFormatter t) | 将指定格式的字符串解析为日期、时间                           |

- Optional类 -> 解决空指针

  ```java
  // 创建一个空的Optional对象
  Optional empty = Optional.empty();
  // 第二种创建方式
  Optional of = Optional.of("aa");
  // 第三种创建方式
  Optional.ofNullable("aaa");
  
  String str = null;
  Optional ofNullable = Optional.ofNullable(str);
  // 判断数据是否不为空 用ofNullable 用of会抛出异常
  if(ofNullable.isPresent()){
      print("数据不是空的");
  }
  
  //获取容器中的数据
  Object obj = ofNullable.get();
  // 将容器中的null转化为ccc
  Object obj2 = ofNullable.orElse("ccc");
  ```



### 5.5 Math类

| 方法                       | 描述                                  |
| -------------------------- | ------------------------------------- |
| abs                        | 绝对值                                |
| acos,asin,atan,cos,sin,tan | 三角函数                              |
| sqrt                       | 平方根                                |
| pow(double a,doble b)      | a的b次幂                              |
| log                        | 自然对数                              |
| exp                        | e为底指数                             |
| max(double a,double b)     |                                       |
| min(double a,double b)     |                                       |
| random()                   | 返回0.0到1.0的随机数                  |
| long round(double a)       | double型数据a转换为long型（四舍五入） |
| toDegrees(double angrad)   | 弧度—>角度                            |
| toRadians(double angdeg)   | 角度—>弧度                            |

### 5.6 BigInteger 类 BigDecimal类

> Integer类作为int的包装类，能存储的最大整型值为2^31-1，BigInteger类的数值范围较Integer类、Long类的数值范围要大得多，可以支持任意精度的整数。

>  一般的Float类和Double类可以用来做科学计算或工程计算，但在商业计算中，要求数字精度比较高，故用到java.math.BigDecimal类。BigDecimal类支持任何精度的定点数。

- 方法

  ```java
  // 构造器
  BigInteger(String val);
  
  public BigInteger abs()
  public BigInteger add(BigInteger val)
  public BigInteger subtract(BigInteger val)
  public BigInteger multiply(BigInteger val)
  public BigInteger divide(BigInteger val)
  public BigInteger remainder(BigInteger val)
  public BigIntegerpow(int exponent)
  public BigInteger[] divideAndRemainder(BigInteger val)
  
  ```



## 6 java集合

> java中对对象用来存储和管理的容器
>
>  - 数组特点和缺点
>    	- 长度不可变 声明类型决定了元素的类型
>     - 缺点: 长度不可变 、类型单一、API太少有序可重复

### 6.0 分类

- `Collection`
  - Set:元素无序 不可重复
  - List: 有序可重复
    - ArrayList
- `Map`：键值对

### 6.1 Collection

```java
// 多态：创建一个集合的实现类的对象
Collection c = new ArrayList();

//----------API-------------
// 集合添加元素 返回布尔
add(E e);
// 添加多项 返回布尔
addAll(Collection c);
// 元素个数 返回int
size();
// 清除所有 无返回值
clear(); 
//是否为空
isEmpty();
// 是否包含指定元素 元素是对象时 默认比地址 equals重写比的是内容
// 向Collection集合添加对象所在的类要求必须重写e
contains(Object o);
// 是否包含另一个集合的所有元素
ContainsAll(Collection c);
// 比较两个集合是否相同 元素类型 顺序 个数
equals(Object o);
// 哈希值
hashCode();
// 删除某个元素 由于重写了equals方法 所以可以remove
remove(Object o);
// 删除另一个集合所有元素(交集部分)
removeAll();
// 保留交集部分
retainAll();
// 转数组
toArray();
```

- Iterator迭代器

  ```java
  Collection c = new ArrayList();
  // interator()返回了一个实现iterator接口的实现类的对象
  c.add(111);
    c.add(222);
    c.add(333);
    // 返回一个新的对象
    Iterator iterator = c.iterator();
    // 指针下移 返回指针指向的元素
    iterator.next();
    // 是否有下一个元素
    iterator.hasNext();
    // 打印所有元素
    while(iterator.hasNext()){
        ...print(iterator.next());
    }
  ```

- 增强for循环(forEach)

  > 用来遍历数组和集合中的元素

   ```java
   for(元素的类型 变量名: 数组或集合的对象名){
        	
    }
   ```

  ```java
  String[] names = {"jack","rose","lee"};
  Collection c = new ArrayList();
  c.add("aa");
  c.add("bb");
  c.add(11);
  
  
  // 遍历数组
  for(String name: names){
      print(name);
  }
  
  // 遍历集合
  for(Object name: c){
      print(name);
  }
  
  // 注意 并没有修改元素的值 只是修改了临时变量
  for(Object name: c){
      name = "test";
  }
  ```

  

  

### 6.2 List

| 分类       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| ArrayList  | 底层是数组<br />查找快，增删慢<br />线程不安全 效率高        |
| LibkedList | 底层是双向链表<br />增删快，查找慢                           |
| vector     | 古老的实现类（已经不用了）<br />底层也是数组<br />线程安全 效率低 |



- **ArrayList(重点)**

  - 构造器

  ```java
  new ArrayList();
  // 底层创建一个容量为10的数组
  new ArrayList(int initialCapacity);
  // 底层创建一个指定大小的数组
  ```

  

  - API

  ```java
  List list = new ArrayList();
  // 指定位置加入元素 无返回值
  list.add(int index,Object ele);
  // 指定位置加入集合中的所有元素 返回布尔
  list.addAll(int index, Collection eles);
  // 获取指定index的元素
  list.get(int index);
  // 返回obj在集合中首次出现的位置 lastIndexOf 从后向前
  list.indexOf(object obj);
  // 移除指定位置的元素并返回该元素
  list.remove(int index);
  
  list.remove(1); //remove索引为1的元素
  list.remove(new Integer(1)); // remove元素1
  
  // 设置指定位置的元素为ele
  list.set(int index,Object ele);
  // 获取子集合
  list.subList(int startIndex,int endIndex);
  ```

  

- ***LinkedList***

  ```java
  LinkedList ll = new LinkedList();
  
  // 添加头
  ll.addFirst(Object obj);
  // 添加尾
  ll.addLast(Object obj);
  // 获取头
  ll.getFirst();
  // 获取尾
  ll.getLast();
  // 删除头
  ll.removeFirst();
  // 删除尾
  ll.removeLast();
  ```

- vector

### 6.3 Set

> 元素无序 ：根据hashCode方法算出来的哈希值决定存放的位置
>
> 不可重复： 进行该对象中的`equals`方法比较 为true不可以放到集合中

- 分类

  - hashSet
    - LinkedHashSet
  - TreeSet

- Set没有新增API

- ***HashSet***

  - 原理

    > 向集合添加元素
    >
    > 1 调用a元素所在类的hashCode方法得到一个哈希值
    >
    > ​					(哈希值决定了改元素存放的位置 )
    >
    > 2 如果该位置没有其他元素 直接将改元素放入
    >
    > 2 如果有元素 调用equals方法比较
    >
    > 3 若结果相同 不能放入
    >
    > 3 结果不同 以链表形式将改元素存放起来
    >
    > (jdk8优化) 如果链表上元素达到8 将链表改为红黑树

    

- `LinkedHashSet`

  > 继承了HashSet 底层实现和HashSet一样 
  >
  > 可以按照元素添加的顺序进行遍历
  >
  > 因为linkedhashSet底层维护了一组指针用来记录添加的顺序

- *TreeSet*

  > 可以按照添加元素的属性进行排序

  - 说明
    - 底层是红黑树
    - 元素类型必须相同
  - 排序方式
    - 自然排序
    - 定制排序

  

### 6.4 Map

> 双列集合 存储的是键值对(Entry)

- 分类
  - HashMap
    - LinkedHashMap
  - *TreeMap*
  - HashTable
    - Properties

- Map

  - 规则
    - 一个`key`和`value`就是一个`Entry` .无序且不可重复
    - 所有的`key`可以看成是`Set`的集合 无序不重复 重写`hashCode`和`equals`方法
    - 所有的`value`可以看成是`Collection`的集合.无序可重复 重写`equals`方法
  - API

  ```java
  Map map = new HashMap();
  // 存放数据 键唯一 
  map.put("key","value");
  ```

- **HashMap(重点)**

  - 构造器

    ```java
    HashMap();
    // 底层创建长度16的数组 加载因子0.75
    // 加载因子: 元素大于16*0.75 = 12的时候扩容
    ```

  - 实现原理

    ```text
    当我们向集合中添加元素(k1,v1)时，会先根据k1的hashCode值来决定存放的位置。
      	当我们存放的位置没有其它元素时则直接放入。当我们存放的位置上已经有了(k2,v2)时，
      	会调用k1的equals方法和k2进行比较。如果结果为true则说明两个元素相同，那么v1将覆盖v2。
      	如果结果为false时说明两个元素不同。将以链表的形式存放(k1,v1)。但链表的数量达到8时，将链表改成红黑树（提高存放的效率）。
    ```

  - 扩展: (为什么扩容两倍)

    ```text
    
             15二进制 ：  	  0 0 0	0 0 1 1 1 1
             			& 1 1 1 1 1 1 1 1 1    //与运算的特点都是1结果为1
             			---------------------		
             			  0 0 0 0 0 1 1 1 1
             			           			  
             			  0 0 0	0 0 1 0 1 0
             			& 1 1 1 1 1 1 1 1 1    //与运算的特点都是1结果为1
             			---------------------		
             			  0 0 0 0 0 1 0 1 0
             			  
             			  0 0 0	0 0 1 0 1 0
             			& 1 1 1 1 1 0 0 0 0    //与运算的特点都是1结果为1
             			---------------------		
             			  0 0 0 0 0 0 0 0 0
             			  
    
    ```

  - API

    ```java
    HashMap map = new HashMap();
    // 添加元素
    map.put(Object key,Object value);
    
    // 删除元素 返回被删除的元素的value
    map.remove(String key);
    
    // 将t所有元素放入当前集合
    map.putAll(Map t);
    
    // 清空集合
    map.clear();
    
    // 根据key值获取元素
    map.get(Object key);
    // 是否包含key
    map.containsKey(Object key);
    // 是否包含value
    map.containsValue(Object value);
    
    // 返回元素个数
    map.size();
    
    // 判断是否为空
    map.isEmpty();
    
    // 判断相等
    map.equals(Object obj);
    ```

- LinkedHashMap

  - 说明
    - LinkedHashSet的底层 就是LinkedHashMap
    - 向LinkedHashSet添加元素 实际上是添加到了LinkedHashMap的key中
    - 可以按照元素添加的顺序进行遍历,因为底层维护了链表用来记录添加顺序
    - LinkedHashMap继承了HashMap 底层实现原理和HashMap 一样
    - TreeSet的底层就是TreeMap 
    - 向TreeSet添加元素 实际上是添加到了TreeMap 的key中

- HashTable

  - `HashMap`和`HashTable`的区别
    - `HashMap`:  
      - Map的主要实现类.线程不安全
      - 可以存储`null`的`key`和`value`
    - `HashTable`: 
      - 古老的实现类 线程安全
      - 不可以存储`null`的`key`和`value`

  

- Properties

  - 说明

    - 一般用来读取配置文件

  - 方法

    ```java
    // 创建一个流
    FileInputStream file = null;
    try{
        // 创建Properties对象
        Properties p = new Properties();
        // 指明流的资源
        file = new FileInputStream("user.properties");
        // 加载流
        p.load(file);
        // 获取内容
        String username = p.getProperty("username");
        String password = p.getProperty("password");
       
    }catch(Exception e){
        e.printStackTrace();
    }finally{
         // 关流
        if(file != null){
    	    file.close();
        }
    }
    
    ```

  

- Collections包装类

  > Collections是Collection的包装类

  - API

    ```java
    // 反转list中的元素
    reverse(List);
    // 随机排序
    shuffle(List);
    // 根据元素的自然顺序对指定List集合元素按升序排序
    sort(List);
    // 根据指定的Comparator产生的顺序进行排序
    sort(List,Comparator);
    // 将指定list集合中i处元素和j处元素进行交换
    swap(List,int,int);
    
    //根据元素的自然顺序，返回给定集合中的最大/最小元素
    Object max(Collection);
    Object min(Collection);
    
    //根据 Comparator 指定的顺序，返回给定集合中的最大/最小元素
    Object max(Collection，Comparator);
    Object min(Collection，Comparator);
    // 返回指定集合中指定元素的出现次数
    int frequency(Collection，Object);
    // 将src中的内容复制到dest中
    void copy(List dest,List src);
    // 使用新值替换 List 对象的所有旧值
    boolean replaceAll(List list，Object oldVal，Object newVal);
    
    ```

### 6.5 集合中使用泛型

- 泛型的作用

  - 解决元素存储的安全性问题(任何类型都可以添加到集合中：**类型不安全**)
  - 解决获取数据元素时，需要类型强转的问题(读取出来的对象需要强转：**繁琐**,可能有ClassCastException)

- 格式

  ```java
  List<String> list = new ArrayList<String>();
  
  HashMap<String,Integer> hp = new HashMap<String,Integer>();
  Set<String> keySet = hp.KeySet();
  Collection<Integer> values = hp.values();
  
  Set<Entry<String,Integer>> entrySet = hp.entrySet();
  
  //泛型推断用法
  List<String> list = new ArrayList();
  
  //错误写法
  List list = new ArrayList<String>();
  ```

- 优点

  - 只有指定类型才可以添加到集合中：**类型安全**
  - 读取出来的对象不需要强转：**便捷**
  - 指明泛型 类中使用泛型的地方都被具体化了
  - 没有指定泛型 泛型默认是`Object`类型

  

- 说明

  - 字节码文件中没有泛型
  - 使用泛型是能够在编译时而不是在运行时检测错误。

- 自定义泛型类

  - 格式

    ```java
    类名<泛型类型1,泛型类型2.....>
        
    K: key
    V: value
    E: Element
    
    T
    ```

    

  ```java
  Public class Person<E>{
      E e;
      public void setE(E e){
          this.e = e;
      }
      public E getE(){
          return e;
      }
  }
  
  Public class Test{
      public void test(){
          Person<String> p = new Person<String>();
      }
      
  }
  ```

  - 泛型方法中的泛型和类的泛型没有关系

    ```java
    public class MM<T> {
        T t; // 不能用static修饰
        
         // <E>是为了告诉编译器E是一个泛型的类型
        public <E> E getE(E e){
            return e;
            // 传递的实参是什么类型 泛型方法中的泛型就是什么类型
        }
        // 泛型方法可以用static修饰
        static public <E> E getE2(E e){
            return e;
        }
        // 泛型方法可以用static修饰
    }
    ```

- 泛型在继承上的体现

  ```text
  如果B是A的一个子类型（子类或者子接口），而G是具有泛型声明的类或接口，G<B>并不是G<A>的子类型！
  
  比如：String是Object的子类，但是List<String >并不是List<Object>的子类。
  
  
  ```

  ```java
  //List和ArrayList有子父类关系，那么泛型如果一致，那么List<T> 和 ArrayList<T>还存在子父类关系
  List<String> list = new ArrayList<String>();
  
  ArrayList<Number> arrayList = new ArrayList<Number>();
  ArrayList<Number> arrayList2 = new ArrayList<Number>();
  arrayList = arrayList2;
  
  A a = new B();
  A<Number> a2 = new B<Number>();
  // A类和B类存在子父类关系，但是A类和B类作为泛型后。
  // ArrayList<A>和ArrayList<B>不存在任何关系
  // 错误写法:	ArrayList<A> aaa = new ArrayList<B>();
  
  ```

- 通配符

  - 说明
    - 可以理解成所有泛型的父类
    - shi用了通配符的集合可以遍历所有元素
    - 使用了通配符的集合只能存放null

  - 格式

    ```java
    <?>
    ```

    

  - 示例

    ```java
    public void test3(ArrayList<?> list){
    		//使用了通配符的集合的元素的类型是Object
    		for (Object object : list) {
    			System.out.println(object);
    		}
    		
    		//使用了通配符的集合只能存放null
    		list.add(null);
    	}
    	
    	public void test2(ArrayList<Number> list){
    		
    	}
    
    ```

    

  - 有限制的类型

    ```java
    
    <? super Number>      
    //[Number , 无穷大)只允许泛型为Number及Number父类的引用调用
    public void test5(ArrayList<? super Number> list){
    
    }
    
    <? extends Number>     
    //(无穷小 , Number] 只允许泛型为Number及Number子类的引用调用
        
    <? extends Comparable>  
    //只允许泛型为实现Comparable接口的实现类的引用调用
    
    public void test4(ArrayList<? extends Number> list){
    
    }
    
    ```

    

## 7 I/O流

### 7.1 File类

- 说明

  - 可以表示文件或目录
  - 不能操作文件内容
  - 可以创建删除重命名文件或目录
  - File的对象一般作为参数传入(文件流...)流的构造器

- 基本使用

  ```java
  File file = new File("pathName");
  
  // 表示一个文件
  File file = new File("d:\\io\\123.txt");
  // 需要"\"转义 等同于
  File file = new File("d:/io/123.txt");
  
  // 表示一个目录
  File file = new File("d:\\io\\test");
  File file = new File("d:/io/test");
  
  // 相对路径
  File file = new File("parent","child");
  // 等同于  ("d:\\io\\123.txt");
  File file2 = new File("d:/io","123.txt");
  
  
  File file = new File("123.txt");
  
  // 第一个参数还可以传File对象
  File file3 = new File(file2,"123.txt");
  ```

- File - Api

  ```java
  // 访问文件名相关 
  
  getName();  
  //获取文件名
  getPath();  
  //获取文件的路径
  getAbsoluteFile();   
  //获取文件的绝对路径   返回File
  getAbsolutePath();   
  //获取文件的绝对路径  返回字符串
  getParent();  
  //获取上级目录
  toPath();     
  //获取文件的路径
  
  
  // 文件检测相关
  
  exists();   
  //判断是否是否存在
  canWrite(); 
  //文件是否可写
  canRead();  
  //文件是否可读
  isFile();   
  //是否是文件
  isDirectory(); 
  //是否是目录
  	
  
  // 获取常规文件信息
  
  lastModified();  
  //最后修改时间   返回long类型，表示毫秒
  length();        
  //文件大小  单位：字节
  
  
  // 文件操作相关
  
  createNewFile();  
  //创建文件
  delete();
  //删除文件
  
  
  // 目录操作相关
  
  mkdir();
  //创建目录   只能创建一层目录
  mkdirs();   
  //创建目录   可以创建一层或多层目录
  delete();   
  //删除目录   只能删除一层目录，且目录必须为空的
  list();
  //获取该目录下所有的文件和目录  字符串数组
  listFiles();  
  //获取该目录下所有的文件和目录   File数组
  
  
  renameTo(File file); 
  //重命名
  /*
  1.可以给文件进行重命名
  2.可以将文件移动到其它路径下，可以重命名
  3.也可以给文件夹重命名
  4.不能移动文件夹
  */
  ```

### 7.2 流的分类

- 按操作数据单位分类

  - 字节流(8 bit)
  - 字符流(16 bit)

- 按流向分

  - 输入流
  - 输出流

- 按流的角色分

  - 节点流
  - 处理流

  | 抽象基类 | 字节流       | 字符流 |
  | -------- | ------------ | ------ |
  | 输入流   | InputStream  | Reader |
  | 输出流   | OutputStream | Writer |

| 分类     | 字节流输入          | 字节流输出           | 字符流输入        | 字符流输出         |
| -------- | ------------------- | -------------------- | ----------------- | ------------------ |
| ...      |                     |                      |                   |                    |
| 访问文件 | FileInputStream     | FileOutputStream     | FileReader        | FileWriter         |
| 缓冲流   | BufferedInputStream | BufferedOutputStream | BufferedReader    | BufferedWriter     |
| 转换流   |                     |                      | InputStreamReader | OutputStreamWriter |
| 对象流   | ObjectInputStream   | ObjectOutputStream   |                   |                    |
| ...      |                     |                      |                   |                    |

### 7.3 字节输入/输出流

- FileInputStream 字节输入流 可以读取文件内容

  - 操作

    ```java
  public void test1() throws Exception{
        // 创建File对象
      File file = new File("hello.txt"); 
    
        // 创建 FileInputStream
        FileInputStream fis = new FileInputStream(file);
        
        //读取文件内容
        byte[] b = new byte[1024]; // 数组大小会影响读取速度
        int len;
        while((len = fis.read(b)) != -1){
            // 不能直接b.length 后面会加空格补全长度
            String str = new String(b,0,len);
            print(str);
        }
        
        // 关闭流
        fis.close();
    }
    
    ```
  
- FileOutputStream 字节输出流 

  - 说明
    - new FileOutputStream(file)   一个参是覆盖
    - new FileOutputStream(file,true) 两个参 拼接

  ```java
  @test
  public void test() throws Exception{
      // 创建File对象
       File file = new File("hello.txt"); 
      // 创建 FileOutputStream
      FileOutputStream fos = new FileOutputStream(file);
      // 写内容到文件
      fos.write("hello World".getBytes());
      // 关闭流
      fos.close();
  }
  
  ```

- 文件的复制（一边读一边写）

  ```java
  @test
  public void test() throws Exception{
      File file = new File("hello.txt");
      File file2 = new File("copy.txt");
      FileOutputStream fos = new FileOutputStream(file2);
      FileInputStream fis = new FileInputStream(file);
          // 读文件
       while((len = fis.read(b)) != -1){
           // 写文件
         fos.write(b,0,len);
      }
      // 关闭流
      fos.close();
      fis.close()
  }
  
  ```

- BufferedInputStream 字节缓冲流

  - 缓冲流可以提升文件流的效率

  ```java
  public void test1() throws Exception{
      // 创建File对象
      File file = new File("hello.txt"); 
  
      // 创建 FileInputStream
      BufferedInputStream  bis = new BufferedInputStream (file);
      
      //读取文件内容
      byte[] b = new byte[1024]; // 数组大小会影响读取速度
      int len;
      while((len = bis.read(b)) != -1){
          // 不能直接b.length 后面会加空格补全长度
          String str = new String(b,0,len);
          print(str);
      }
      
      
      // 关闭流
      bis.close();
  }
  ```

- BufferedOutputStream 

  ```java
  @test
  public void test() throws Exception{
      // 创建File对象
       File file = new File("hello.txt"); 
      // 创建 FileOutputStream
      BufferedOutputStream  bos = new BufferedOutputStream (file);
      // 写内容到文件
      bos.write("hello World".getBytes());
      // 关闭流
      bos.close();
  }
  
  ```

  - 使用缓冲流提高效率

- flush()方法

  ```java
  @test
  public void test() throws Exception{
      // 创建File对象
       File file = new File("hello.txt"); 
      // 创建 FileOutputStream
      BufferedOutputStream  bos = new BufferedOutputStream (file);
      // 写内容到文件
      bos.write("hello World".getBytes());
      // 刷新缓存 立即写出
      bos.flush();
      // 程序关闭也会将内容写出
      // 最好在程序关闭之前调用一次flush
      bos.close();
  }
  ```

### 7.4 字符流

>  字符流，只能操作纯文本的文件

- FileReader 字符输入流

- FileWriter 字符输出流

- BufferedReader 字符缓冲流-输入流 

- BufferedWriter 字符缓冲流-输出流 

  

  ```java
  // windows utf-8 一个中文字占三个字节
  public class IOTest6 {
  	
  	//字符缓冲流
  	@Test
  	public void test3() throws Exception {
  		File file = new File("abc.txt");
  		
  		FileReader fr = new FileReader(file);
  		
  		BufferedReader br = new BufferedReader(fr);
  		
  		FileWriter fw = new FileWriter(new File("d:\\io\\字符缓冲流.txt"));
  		BufferedWriter bw = new BufferedWriter(fw);
  		
  		String str = "";
  		//读取一行数据
  		while((str = br.readLine()) != null) {
  			System.out.println(str);
  			bw.write(str);
  		}
  		
  		bw.close();
  		br.close();
  	}
  	
  	/*
  	 * 字符流读取则不会出现中文乱码
  	 */
  	@Test
  	public void test1() throws Exception {
  		File file = new File("abc.txt");
  		
  		FileReader fr = new FileReader(file);
  		
  		FileWriter fw = new FileWriter(new File("d:\\io\\字符流.txt"));
  		
  		char[] c = new char[4];
  		int len;
  		while((len = fr.read(c)) != -1) {
  			fw.write(new String(c));
  		}
  		
  		fw.close();
  		fr.close();
  	}
  	
  	/**
  	 * 字节流读取中文可能会出现乱码
  	 */
  	@Test
  	public void test2() throws Exception {
  		File file = new File("abc.txt");
  		
  		FileInputStream fis = new FileInputStream(file);
  		FileOutputStream fos = new FileOutputStream(new File("d:\\io\\cba.txt"));
  		
  		byte[] b = new byte[6];
  		int len;
  		//读文件
  		while((len = fis.read(b)) != -1) {
  			System.out.println(new String(b,0,len));
  			//写文件
  			//fos.write(b, 0, len);
  		}
  		
  		fos.close();
  		fis.close();
  		
  	}
  }
  
  ```

### 7.5 对象流

- 将对象保存到文件中

  - 创建对象
  - 创建文件
  - 创建`ObjectOutputStream`
  - 将对象写入文件
  - 关闭流

  ```java
  /**
   * 对象流测试
   * 
   * 	序列化：用ObjectOutputStream类保存基本类型数据或对象的机制
  	反序列化：用ObjectInputStream类读取基本类型数据或对象的机制
  
   *  对象流也是处理流之一
   *  ObjectOutputStream 对象输出流
   *  	可以将对象写入文件中（序列化）
   *  注意：
   *  	被操作的对象对应的类必须实现序列化接口Serializable
   *  	类中必须除了基本数据类型的属性之外的属性对应的类也需要实现序列化接口Serializable
   *  
   *  ObjectInputStream 对象输入流
   *  	可以将对象从文件中读取出来（反序列化）
   *  
   *  serialVersionUID 所有实现了序列化接口Serializable的类都有一个静态的序列化版本ID：serialVersionUID
   *  serialVersionUID一致，表示版本兼容
   *  	一般开发中都需要显式的写在类中，如果不写，则系统会自动生成一个serialVersionUID，当类中的内容发生改变
   *  		则serialVersionUID也会改变
   */
  public class ObjectStreamTest {
  	
  	@Test
  	public void test2() throws Exception {
  		File file = new File("object.txt");
  		
  		FileInputStream fis = new FileInputStream(file);
  		ObjectInputStream ois = new ObjectInputStream(fis);
  		
  		Person person = (Person) ois.readObject();
  		System.out.println(person);
  	}
  
  	@Test
  	public void test1() throws Exception {
  		//1.创建对象
  		Person p = new Person(101,"小龙",39);
  		//2.创建文件
  		File file = new File("object.txt");
  		//3.创建ObjectOutputStream
  		FileOutputStream fos = new FileOutputStream(file);
  		ObjectOutputStream oos = new ObjectOutputStream(fos);
  		
  		//4.将对象写入文件中
  		oos.writeObject(p);
  		
  		//5.关闭流
  		oos.close();
  	}
  }
  
  ```

### 7.6 转换流

- InputStreamReader

  > 字节转字符

- OutputStreamWriter

  > 字符转字节

  ```java
  /*
   * 转换流 
   *   InputStreamReader  可以将字节流转化为字符流
   *   OutputStreamWriter 可以将字符流转化为字节流
   * 
   * 作用：
   * 	1.可以实现文件的复制，提升复制文件的效率  （只能是纯文本）
   *  2.可以设置文件读取和写入的编码格式
   */
  public class SwitchStreamTest {
  	
  	@Test
  	public void test1() throws Exception {
  		File file = new File("abc.txt");
  		
  		FileInputStream fis = new FileInputStream(file);
  		
  		//创建转换流  将字节流转化为字符流
  		//读取文件的时候，编码格式必须与文件的编码格式一致，否则乱码
  		InputStreamReader isr = new InputStreamReader(fis,"utf-8");
  		
  		FileOutputStream fos = new FileOutputStream(new File("d:\\io\\SwitchStreamTest.txt"));
  		OutputStreamWriter osw = new OutputStreamWriter(fos,"gbk");
  		char[] c = new char[20];
  		int len;
  		while((len = isr.read(c))!=-1 ) {
  			String str = new String(c,0,len);
  			System.out.println(str);
  			osw.write(str);
  			osw.flush();
  		}
  		
  		osw.close();
  		isr.close();
  	}
  }
  
  ```

### 7.7 打印流/标准输入\输出流

```java
/*
 * 打印流  
 * 了解
 */
public class OtherStream {
	
	//测试MyInput.java
	@Test
	public void test4() {
		float f = MyInput.readFloat();
		System.out.println(f);
		
//		Scanner in = new Scanner(System.in);
//		float f2 = in.nextFloat();
//		System.out.println(f2);
	}
	
	//模拟Scanner
	/*
	 * 
	 */
	@Test
	public void test3() throws IOException {
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		String str = br.readLine();
		System.out.println(str);
		
//		Scanner in = new Scanner(System.in);
	}
	
	
	//标准输入输出流
	/*
	 * 标准输出流：System.out  输出到控制台
	 * 标准输入流：System.in  从键盘读取数据
	 * 
	 * 步骤：
	 * 	1.先创建转换流，将字节流转化为字符流
	 *  2.使用缓冲流进行读取
	 *  3.操作数据
	 *  4.关闭流
	 */
	@Test
	public void test2() throws IOException {
		//将标准输入流传入转换流，可以接收用户从键盘输入的内容
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		String str = "";
		while((str = br.readLine()) != null) {
			if("e".equals(str) || "exit".equals(str)) {
				break;
			}
			System.out.println(str.toUpperCase()+"=============");
		}
		br.close();
		isr.close();
	}
	
	//打印流
	@Test
	public void test1() throws Exception {
		//创建输出流，将内容输出到文件中
		FileOutputStream fos = new FileOutputStream(new File("out.txt"));
		//创建打印流，将输出流传入打印流
		PrintStream ps = new PrintStream(fos);
		//修改System的out属性，将输出到控制台改为输出到文件
		System.setOut(ps);
		//输出到文件中
		System.out.println("hello PrintStream");
		
		//关闭流
		ps.close();
		fos.close();
	}
}

```



## 8 多线程

### 8.1 基本概念

- 程序

  > ●程序（program）是为完成特定任务、用某种语言编写的一-组指 令的集合。
  >
  > 即指一段静态的代码，静态对象。

- 进程

  > ●进程（process）是程序的一-次执行过程，或是**正在运行的一个程序**。是一个动态的过程：
  >
  > 有它自身的产生、存在和消亡的过程(***生命周期***)
  > ➢如：运行中的QQ，运行中的MP3播放器
  > ➢程序是静态的，进程是动态的
  > ➢**进程作为资源分配的单位**，系统在运行时会为每个进程分配不同的内存区域

- 线程

  > ●线程（thread），进程可进--步细化为线程，是一个程序内部的一条执行路径。
  > ➢若一个进程同--时间并行执行多个线程，就是支持多线程的
  > ➢**线程作为调度和执行的单位，每个线程拥有独立的运行栈和程序计数器**（pc），线程切换的开销小
  > ➢一个进程中的多个线程共享相同的内存单元/内存地址空间→它们从同-堆中分配对象，可以访问相同的变量和对象。这就使得线程间通信更简便、高效。但多个线程操作共享的系统资源可能就会带来**安全的隐患**。

- 总核CPU和多 核CPU的理解

  > ➢单核CPU，其实是一种假的多线程，因为在-一个时间单元内，也只 能执行-一个线程的任务。例如：虽然有多车道，但是收费站只有一个工作人员在收费，只有收了费才能通过，那么CPU就好比收费人员。如果有某个人不想交钱，那么收费人员可以把他“挂起”（晾着他，等他想通了，准备好了钱，再去收费）。但是因为CPU时间单元特别短，因此感觉不出来。
  > ➢如果是多核的话，才能更好的发挥多线程的效率。（现在的服务 器都是多核的）
  > ➢.一个Java应用程序java.exe，其实至少有三个线程：
  >
  > - main（）主线程，
  >
  > - gc（）垃圾回收线程，
  >
  > - 异常处理线程。
  >
  >   当然如果发生异常，会影响主线程。

  

 - 并行与并发

   > ➢并行：多个CPU同时执行多个任务。比如：多个人同时做不同的事。
   > ➢并发：一个CPU（采用时间片）同时执行多个任务。比如：秒杀、多个人做同一件事。

- 多线程的优点

  - 提高应用程序的响应。对图形化界面更有意义，可增强用户体验。
  - 提高计算机系统CPU的利用率
  - 改善程序结构。将既长又复杂的进程分为多个线程，独立运行，利于理解和修改

- 何时需要多线程

  - 程序需要同时执行两个或名个任务。
  - 程序需要实现--些需要等待的任务时，如用户输入、文件读写、网络操作、搜索等。
  - 需要一些后台运行的程序时。

### 8.2 多线程的创建和使用

### 8.2.1 方式一：继承`Thread`类

- 步骤
  - 创建一个继承thread类的子类
  - 重写Thread类的run()
  - 创建thread类的子类的对象
  - 调用此的start()
- `start()`的两个作用
  - 启动当前线程
  - 调用当前线程的run()

```java
class MyThread extends Thread{
    @Override
    public void run(){
        for(int i = 0;i<100;i++){
            // 打印偶数
            if(i%2==0){
                print(i);
            }
        }
    }
}


public class Test{
    public static main(String[] args){
    	MyThread mt = new MyThread();
    	mt.start();

        // 再启动一个线程 不能同一个对象start()两次 需要再实例化一个对象
        MyThread mt2 = new MyThread();
        mt2.start();
        
        for(int i = 0;i<100;i++){
            // 打印奇数
        	if(i%2!=0){
                print(i);
            }
        }
    }
}
// 最终结果 奇数和偶数同步打印
```

- thread匿名子类方式

  ```java
  new Thread(){
      @Override
      public void run(){
        	// 方法体
      }
  }.start();
  ```

### 8.2.2 方式二:实现`Runnable`接口

- 步骤
  - 创建一个实现了Runnable接口的类
  - 实现类去实现Runnable中的抽象方法
  - 创建实现类的对象
  - 将此对象作为参数传递到Thread类的构造器 创建Thread对象
  - 通过Thread类的对象调用start()
- 说明
  - 为什么能够调用到Mthread的run()? 
    - 构造的时候把mthread当作target传了进去 底层对run重写定向至target.run()

```java
//创建一个实现了Runnable接口的类
class Mthread implements Runnable {
    // 实现类去实现Runnable中的抽象方法
    @Override
    public void run() {
        for(int i = 0;i< 100; i++){
            if(i%2==0){
                // 这里不能直接调获取线程的方法(没有继承Thread)
                sout(Thread.currentThread().getName()+ ":" + i)
            }
            
        }
    }
}

public class ThreadTest{
    public static void main(String[] args){
        // 创建实现类的对象
        Mthread mthread = new Mthread();
        // 将此对象作为参数传递到Thread类的构造器 创建Thread对象
        Thread t1 = new Thread(mthread);
        // 通过Thread类的对象调用start()
        t1.start();
        
        Thread t2 = new Thread(mthread);
        t2.start();
        
        
    }
}

```

### 8.2.3 方式三 实现`Callable`接口(5.0 新增)

- 对比Runnable
  - 重写`call()`  可以有返回值
  - 可以抛出异常
  - 支持泛型的返回值
  - 需要借助`FutureTask`类,比如获取返回结果
- `Future`接口
  - 可以对具体Runnable、Callable任 务的执行结果进行取消、查询是否完成、获取结果等。
  - FutrueTask是Futrue接口的`唯一`的实现类
  - FutureTask同时实现了Runnable，Future接口。它既可以作为Runnable被线程执行，又可以作为Future得到Callable的返回值
- 步骤
  - 创建一个实现`Callable`的实现类
  - 实现`call`方法 将此线程需要执行的操作声明在`call()`中
  - 创建`callable`接口实现类的对象
  - 将此`callable`接口实现类的对象传递到`FutureTask`的构造器中 创建`FutureTask`的对象
  - 将`FutureTask`的对象作为参数传递到`Thread`的构造器中 创建`Thread`对象 调用`start()方法`
  - (如果需要)可以获取call的返回值

```java
class NumThread implements Callable<Integer>{
    @Override
    public Integer call() throws Exception{
		int sum = 0;
        for(int i = 0;i<100;i++){
            sum+=i;
        }
        return sum; 
    }
}

class Test{
    @Test
    NumThread num = new NumThread();
    
    FutureTask<Integer> ft = new FutureTask<Integer>(num);
    
    new Thread(ft).start();
    try{
        // get() 返回值即为FutureTask构造器参数Callable实现类重写的call()的	返回值
	    Integer sum = ft.get();    
    }catch(.){
        
    }
}
```

### 8.2.4 方式四 使用线程池(5.0新增)

- 背景：经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影响很大。

- 思路：提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。类似生活中的公共交通工具。

- 好处：

  - 提高响应速度（减少了创建新线程的时间）
  - 降低资源消耗（重复利用线程池中线程，不需要每次都创建）
  - 便于线程管理
    - corePoolSize：核心池的大小
    - maximumPoolSize：最大线程数
    - keepAliveTime：线程没有任务时最多保持多长时间后会终止

  

  

- API

```java
ExecutorSirvice 
ExecutorsExecutorService：真正的线程池接口。常见子类ThreadPoolExecutor
void execute(Runnable command);
 //执行任务/命令，没有返回值，一般用来执行Runnable
<T> Future<T> submit(Callable<T> task);
//执行任务，有返回值，一般又来执行Callable
void shutdown();
//关闭连接池
    
Executors：工具类、线程池的工厂类，用于创建并返回不同类型的线程池
    
➢Executors.newCachedThreadPool（）：创建一个可 根据需要创建新线程的线程池
➢Executors.newFixedThreadPool（n）；创建一 个可重用固定线程数的线程池
➢Executors newSingle ThreadExecutor（）：创建-一个只有 一个线程的线程池
➢Executors.newScheduledThreadPool（n）：创建一个线程池，它可安排在给定延迟后运行命令或者定期地执行。

```

- 使用

```java
psvm(){
    // 1. 提供指定线程数量的线程池
    ExecutorSirvice service = Executors.newFixedThreadPool(10);
     // 设置线程池属性
    ThreadPoolExecutor service1 = (ThreadPoolExecutor)service;
    service1.setCorePoolSize(15);
    
    
    // 2. 执行指定的线程操作 需要提供实现Runnable/Callable接口实现类的对象
    service.execute(new ClassA());
    // 适合使用于Runnable
      service.submit();
    // 适合适用于Callable
    service.shutdown();
    // 3. 关闭线程池
  
}
```





### 8.2.5 窗口卖票案例

```java
//方式一实现
class Window1 extends Thread{
    // 多次new 所以要加static 共用一个数据
    private static int ticket = 100;
    
    @Override
    public void run(){
        while(true){
            if(ticket > 0){
                sout(Thread.currentThread().getName()+ ":卖出,票号" + ticket);
                ticket--;
            }else{
                break; 
            }
        }
    }
}

public class WindowTest2 {
    @Test
    Thread t1 = new Window1();
    Thread t2 = new Window1();
    Thread t3 = new Window1();
    
    t1.start();
    t2.start();
    t3.start();
}
//===================================================================
// 方式二实现
class Window2 implements Runnable{
    //只new一次 不用加static
    private int ticket = 100;
    
    @Override
    public void run(){
        while(true){
            if(ticket > 0){
                sout(Thread.currentThread().getName()+ ":卖出,票号" + ticket);
                ticket--;
            }else{
                break; 
            }
        }
    }
}

public class WindowTest2 {
    @Test
    Window2 win2 = new Window2();
    Thread t1 = new Thread(win2);
    Thread t2 = new Thread(win2);
    Thread t3 = new Thread(win2);
    
    t1.start();
    t2.start();
    t3.start();
}
```

- 两种方式比较

  - 第二种方式没有类的单继承性的局限性
  - 实现的方式更适合处理多个线程有共享数据的情景

  - 联系:
    - Thread本身也实现了Runnable接口
  - 相同点
    - 都需要重写run()
    - 将线程要执行的逻辑声明在run()中

- Api

  ```java
  void start();
      //启动线程，并执行对象的run()方法
  run();
      //线程在被调度时执行的操作
  String getName();
      //返回线程的名称
  void setName(String name);
      //设置该线程名称 也可以利用构造器
  	/*
  	public 类名(String name){
          super(name);
      }
      */
  static Thread currentThread();
      //返回当前线程。在Thread 子类中就是this，通常用于主线程和Runnable实现类
  
  yield();
  	// 释放当前cpu执行 有可能会重新分配 让一只手
  join();
  	// 在线程a中调用线程b的join方法 a进入阻塞状态 直到b完全执行完a才结束阻塞状态 让两只手
  stop();
  	// 强制结束当前线程(已过时)
  sleep(long millitime);
  	// 阻塞指定的毫秒值
  isAlive();
  	// 判断当前线程是否存活
  
  
  suspend();
    resume();
    	// 挂起 恢复 已过时
    wait();
    	// 线程进入阻塞状态 并释放同步监视器
    notify();
    	// 唤醒被wait的一个(多个选优先级高的)进程
    notifyAll();
    	// 唤醒所有被wait的进程
  ```


  - 线程的调度
  
    - 调度策略
    
      - 时间片
      - **抢占式**：高优先级的线程抢占cpu
    
    - java的调度方法
    
      - 同优先级线程组成先进先出队列（先到先服务），使用时间片策略
      - 堆高优先级，使用优先掉地的抢占式策略
    
    - 线程的优先级等级
    
      > 高优先级抢占低优先级的cpu执行权
      >
      > 但是**只是概率上讲 高优先级的高概率执行**
      >
      > 并不意味只有当高优先级执行完低优先级才执行
    
      ```java
      // 10档 这三个属性直接设置在Thread类上
      MAX_PRIORITY: 10;
      MIN_PRIORITY: 1;
      NORMAL_PRIORITY: 5; //默认的优先级
      
      // 方法
      // 设置优先级
      setPriority(int p);
      
      h1.setPriority(Thread.MAX_PRIORITY);
      h2.setPriority(3);
      // 查看优先级
      getPriority();
          
      ```

### 8.3 线程的生命周期

- 状态
  - 新建：当一个Thread类或其 子类的对象被声明并创建时，新生的线程对象处于新建状态
  - 就绪：处于新建状态的线程被start（）后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件，只是没分配到CPU资源
  - 运行：当就绪的线程被调度并获得CPU资源时，便进入运行状态，run（）方法定义了 线程的操作和功能
  - 阻塞：在某种特殊情况下，被人为挂起或执行输入输出操作时，让出CPU并临时中止自己的执行，进入阻塞状态
  - 死亡：线程完成了它的全部工作或线程被提前强制性地中止或出现异常导致结束

### 8.4 线程的同步

- 线程安全问题

  - 多个线程执行的不确定性引起结果的不稳定
  - 多个线程对同一变量操作引起不稳定

- 解决

  - 当一个进程运行的时候 其他线程不能参与 直到a线程操作完 (即使a线程sleep())
  - java中通过同步机制解决线程安全问题
  - 好处:解决了线程的安全问题
  - 坏处:只能有一个线程参与 其他线程等待 相当于单线程 效率低

- 方式一: 同步代码块

  - 格式

    ```java
    synchronized(同步监视器){
        // 需要被同步的代码
    }
    ```

  - 说明

    - 需要被同步的代码:操作共享数据的代码(**不能多,不能少**)
    - 共享数据:多个线程共同操作的数据
    - 同步监视器(锁):任何类的对象都能充当锁 多个线程必须要共用同一把锁
      - **实现接口可以用`this`**
      - **继承`Thread`方式慎用this**,但可以用**`类名.class`**(类也是对象)

  ```java
  
  // 买票案例解决
  class Window2 implements Runnable{
      private int ticket = 100;
      //继承方式的时候要加static
    //  Object obj = new Object;
      
      @Override
      public void run(){
          while(true){
              // synchronized(obj){
              synchronized(this){
                  if(ticket > 0){
                      try{
                          Thread.sleep(100);
                      }catch(InterruptedException e){
                          e.printStackTrace();
                      }
                      sout(Thread.currentThread().getName()+ ":卖出,票号" 
                           + ticket);
                      ticket--;
                  }else{
                      break; 
                  }
              }
          }
      }
  }
  
  public class WindowTest2 {
      @Test
      Window2 win2 = new Window2();
      Thread t1 = new Thread(win2);
      Thread t2 = new Thread(win2);
      Thread t3 = new Thread(win2);
      
      t1.start();
      t2.start();
      t3.start();
  }
  ```

- 方式二: 同步方法

  > 如果操作共享数据的代码完整的声明在一个方法中,不妨将此方法声明为同步的

  - 说明
    - 同步方法仍然涉及到同步监视器 只是不需要显示声明
      - 非静态的同步方法: 同步监视器是this
      - 静态的同步方法 同步监视器是当前类本身

  ```java
  // 买票案例解决
  // 实现接口方式
  class Window2 implements Runnable{
      private int ticket = 100;
      //继承方式的时候要加static
    //  Object obj = new Object;
      
      @Override
      public void run(){
          while(ticket>0){
           	show();
          }
      }
      public synchronized void show(){
           if(ticket > 0){
               try{
                   Thread.sleep(100);
               }catch(InterruptedException e){
                   e.printStackTrace();
               }
               sout(Thread.currentThread().getName()+ ":卖出,票号" 
                    + ticket);
               ticket--;
           }
      }
  }
  
  // ==============================================
  // 继承Thread方式
  class Window2 extends Thread{
      private static int ticket = 100;
      
      @Override
      public void run(){
          while(ticket>0){
           	show();
          }
      }
      // 要加static
      private static synchronized void show(){
           if(ticket > 0){
               try{
                   Thread.sleep(100);
               }catch(InterruptedException e){
                   e.printStackTrace();
               }
               sout(Thread.currentThread().getName()+ ":卖出,票号" 
                    + ticket);
               ticket--;
           }
      }
  }
  ```

- 使用同步机制将单例模式的懒汉式改写成线程安全的

  ```java
  class Bank {
      private Bank(){}
      private static Bank instance = null;
      public static Bank getInstance(){
          /*方式一 效率稍差
          synchronized(Bank.class){
              if(instance != null){
                  instance = new Bank();
              }
              return instance;
          }
          */
          // 方式二
          if(instance != null){
              synchronized(Bank.class){
                  if(instance != null){
                      instance = new Bank();
                  }       
          	}
          }
          return instance;
      }
  }
  ```

- 线程的死锁

  > 不同进程占用对方所需要的同步资源
  >
  > 没有异常 没有提示所有进程都处于阻塞状态 无法继续
  >
  > - 解决
  >   - 尽量避免嵌套同步
  >   - 专门的算法 原则
  >   - 尽量减少同步资源的定义

- 方式三: Lock(锁)

  > jdk 5.0新增
  >
  > 和synchronized的区别:
  >
  > - synchronized自动释放同步监视器
  > - lock手动启动结束

  ```java
  class Window2 implements Runnable{
      private int ticket = 100;
      // 实例化一个Lock 
      private ReentrantLock lock = new ReentrantLock(fair:true);
      // 参数的含义 : 先进先出
      // 默认是false
      @Override
      public void run(){
          while(true){
              try{
                  // 调用lock()
                  lock.lock();
                  if(ticket > 0){
                      sout(Thread.currentThread().getName()+ ":卖出,票号" 
                           + ticket);
                      ticket--;
                  }else{
                      break; 
                  }
              }finally{
                  // 解锁
                  lock.unlock();
              }
              
          }
      }
  }
  
  public class WindowTest2 {
      @Test
      Window2 win2 = new Window2();
      Thread t1 = new Thread(win2);
      Thread t2 = new Thread(win2);
      Thread t3 = new Thread(win2);
      
      t1.start();
      t2.start();
      t3.start();
  }
  ```


### 8.5 线程的通信

- 案例

  ```java
  // 需求 两个线程交替打印1-100
  class MyNumber implements Runnable{
      private int number = 1;
      @Override
      public void run(){
          while(num<100){
              synchronized(this){
                  // b进来唤醒a 但此时this已改变 执行b线程
                  notifyAll();
                  if(num<=100){
                      // sleep()不会释放锁
                      try{Thread.sleep(10);}catch(..){..}
                      sout(num);
                      num++;
                      // 使调用wait()的线程阻塞 释放锁
                      try{wait();}catch(..){..}
                  }else{
                      break;
                  }  
              }
          }
      }
  
  // sleep() wait() 和 notifyAll() 的异同
  /* 同
  1. 这三个方法都必须使用在同步代码块或同步方法中
  2. 调用者必须是同步代码块或同步方法中的同步监视器 否则会有异常
  3. 定义在`java.lang.Object`类中(保证任何一个对象都有这三个方法)
  */
  
  /*异
  1. 声明位置不同: 
  	Thread类中声明sleep()
  	Object类中声明wait()
  2. 调用的要求不同
  	sleep()可以在任何场景下调用
  	wait()必须在同步代码块或同步方法中调用
  3. 关于是否释放同步监视器
  	如果两个方法都是用在同步代码块或同步方法中
  	sleep()不会释放锁
  	wait()会释放锁
  */
  ```

- 应用案例:生产消费者

  ```text
  ●生产者（Productor）将产品交给店员（Clerk），而消费者（Customer）从店员处取走产品，店员一次只能持有固定数量的产品（比如：20），如果生产者试图生产更多的产品，店员会叫生产者停一下，如果店中有空位放产品了再通知生产者继续生产：如果店中没有产品了，店员会告诉消费者等一下，如果店中有产品了再通知消费者来取走产品。
  ●这里可能出现两个问题：
  ➢生产者比消费者快时，消费者会漏掉--些数据没有取到。
  ➢消费者比生产者快时，消费者会取相同的数据。
  ```

  ```java
  class Clerk{
      private int count = 0;
      // 生产
      public synchronized void produceProduct(){
          if(count<20){
              count++;
              sout(Thread.currentThread().getName() + "生产第" + count + "个");
              notify();
          }else{
              try{wait();}catch(.){}
          }
      }
      // 消费
      public synchronized void consumeProduct(){
          if(count> 0){
              sout(Thread.currentThread().getName() + "消费" + count + "个");
              count--;
              notify();
          }else{
              try{wait();}catch(.){}
          }
      }
      
  }
  class Producer extends Thread{
      private Clerk clerk;
      public Producer(Clerk clerk){
          this.clerk = clerk;
      }
      @Override
      public void run(){
          sout(getName() + "开始生产");
          while(true){
              try{Thread.sleep(10);}catch(.){..}
              clerk.produceProduct();
          }
      }
  }
  
  class Consumer extends Thread{
      private Clerk clerk;
      public Consumer(Clerk clerk){
          this.clerk = clerk;
      }
       @Override
      public void run(){
          sout(getName() + "开始消费");
          while(true){
              try{Thread.sleep(10);}catch(.){..}
              clerk.consumeProduct();
          }
      }
  }
  
  public class ProductQuestion {
      psvm(..){
          Clerk clerk= new Clerk();
          Producer p = new Producer(clerk);
          p.setName("生产者");
          Producer c = new Customer(clerk);
          c.setName("消费者");
          
          p.start();
          c.start();
      }
  }
  ```

  

## 9 设计模式

### 3.5 设计模式

- 单例设计模式

  - 饿汉式

    ```java
    public static void main(String[] args){
        Bank a = Bank.getInstance();
        Bank b = Bank.getInstance();
        // a和b是同一个对象
    }
    
    class Bank{
        // 私有化构造器 限制创建该类的对象
        private Bank(){}
        // 类内部创建一个对象
        private static Bank bank = new Bank();
        // 提供一个公共方法用来返回Bank的示例
        public static Bank getInstance(){
            return bank;
        }
    }
    ```

    

  - 懒汉式

    ```java
    public static void main(String[] args){
        Bank a = Bank.getInstance();
        Bank b = Bank.getInstance();
        // a和b是同一个对象
    }
    
    class Bank{
        // 私有化构造器 限制创建该类的对象
        private Bank(){}
        // 声明一个对象的引用
        private static Bank bank = null;
        // 提供一个公共方法用来返回Bank的示例
        public static Bank getInstance(){
            if(bank == null){  
            	bank = new Bank();
            }
            return bank;
        }
    }
    ```

  - 饿汉式和懒汉式的区别

    - 饿汉式:线程安全的
    - 懒汉式:线程不安全 但是延迟了对象创建的时机**(懒加载)**

### 3.9 模板设计模式

```java
/*
需求:
	计算某个程序运行的时间
	考虑到1,2,4可以复用 使用模板设计模式
*/

public static void main(String[] args){
    // 多态
    Code code = new SubCode();
    code.runTime()
}

abstract class Code{
    public void runTime(){
        // 1,记录开始时间
        long start = System.currentTimeMillis();
        // 2,运行代码
        runCode();
        // 3, 记录结束时间
        long end = System.currentTimeMillis();
        // 4, 计算时间
        System.out.println(end-start);
    }
    public abstract void runCode();
}

class SubCode extends Code{
	@Override
     public void runCode(){
        for(int i = 1;i<10000;i++){
           System.out.println(i); 
        }
    }
}
```

- 匿名内部类

  ```java
  public static void main(String[] args){
      // 创建了一个抽象类的匿名子类的对象
    Demo demo = new Demo(){
      @Override
      public void show(){
        System.out.println("这是一个匿名内部类");
      }
    }
  }
  
  abstract class Demo {
    public void show() {
      System.out.println("demo");
    }
  }
  ```



