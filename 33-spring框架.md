# Spring

## 1 helloWorld

### 1.1 下载

```text
https://repo.spring.io/release/org/springframework/spring/
```

### 1.2 导入jar包

- Beans
- Core
- Context
- Expression
- **commons-logging**

### 1.3 hello world

- 创建普通类

  ```java
  public class User {
      public void say(){
          sout("hello world")
      }
  }
  ```

- 创建Spring配置文件 在配置文件配置创建的对象

  ```xml
  <?xml version="1.0" encoding="utf-8" ?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
             http://www.springframework.org/schema/beans/spring-beans.xsd">
      
      <bean id="user" class="com.fc.spring5.User"></bean>
  </beans>
  ```

- 测试

  ```java
  @Test
  public void test(){
      // 加载spring的配置文件
      ApplicationContext context = 
          new ClassPathXmlApplicationContext("bean1.xml");
      // 获取配置文件创建的对象
      User user = context.getBeans("user",User.class);
      user.say();
  }
  ```

## 2 IOC

### 2.1 底层原理

- 概念
  - 把创建对象和调用对象交给Spring
- 目的
  - 降低耦合
- 底层原理
  - xml反射
  - 工厂模式
  - 反射

### 2.2 ioc 接口

> ioc底层是工厂

- ~~BeanFactory~~

  > spring 内部使用的接口 一般不提供给开发使用

  ```java
  BeanFactory context = 
          new ClassPathXmlApplicationContext("bean1.xml");
  ```

  - 加载配置文件的时候不创建对象 调用的时候才创建

- **ApplicationContext**

  > 是`BeanFactory `的子接口 开发使用
  
  - 加载配置文件的时候创建对象
  - 实现类
    - `ClassPathXmlApplicationContext(相对路径)`
    - `FileSystemXmlApplicationContext(绝对路径)`

### 2.3 ~~基于xml方式~~

#### 2.3.1 基于xml方式

- 创建对象
  - 属性
    - id: 标识 别名
    - class: 类的全路径(包全路径)	
    - ~~name~~: 类似id 不用
  - 说明
    - 创建对象的时候 默认执行无参构造器

```xml
<bean id="user" class="com.fc.spring5.User"></bean>
```

- 注入属性

  - **DI:依赖注入**

    - set方式

      ```java
      public class User {
          private String name;
          private Integer age;
          public void setName(String name) {
              this.name = name;
          }
          public void setAge(Integer age) {
              this.age = age;
          }
          public void say(){
              sout("hello world")
          }
      }
      ```

      ```xml
      <bean id="user" class="com.fc.spring5.User">
      	 <property name="name" value="zhangsan"></property>
           <property name="age" value="18"></property>    
      </bean>
      ```

      

    - 有参构造

      ```java
      public class User {
      	private String name;
          private Integer age;
      
          public User(String name, Integer age) {
              this.name = name;
              this.age = age;
          }
          public void say(){
              sout("hello world")
          }
      }
      ```

      ```xml
      <bean id="user" class="com.fc.spring5.User">
          <constructor-arg name="name" value="lisi"></constructor-arg>
          <constructor-arg name="age" value="18"></constructor-arg>
          <!-- 也可以使用index属性 对应第几个参数-->
          <!--<constructor-arg index="0" value="jack"></constructor-arg>-->
      </bean>
      ```

  - ***p名称空间注入***

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           
           xmlns:p="http://www.springframework.org/schema/p"
           
           xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans.xsd">
        
        <bean id="user" class="com.fc.spring5.User" p:name="rose" p:age="18"></bean>
    </beans>
    ```

- 注入其他类型属性

  - null

  ```xml
  <bean id="user" class="com.fc.spring5.User">
  	 <property name="name">
           <null/>
      </property>
  </bean>
  ```

  - 属性包含特殊符号

  ```xml
  <bean id="user" class="com.fc.spring5.User">
      <!--转义方式 与html一致-->
  	 <property name="name" value="&lt&ltjack&gt&gt"></property>
      <!--CDATA方式 -->
      <property name="name">
          <value><![CDATA[<<jake>>]]></value>
      </property>
  </bean>
  ```

- **外部bean**

  - 创建两个类`service`和`dao`

  - 在`service`调用`dao`里面的方法

    ```java
    package com.fc.spring.service;
    
    public class UserService{
        // 1. 创建User Dao类型属性 生成set方法
        private UserDao userDao;
        public void setUserDao(UserDao userDao){
            this.userDao = userDao
        }
    }
    ```

  - 在配置文件中调用

    ```xml
    <!-- 创建service 和dao的对象-->
    <bean id="userService" class="com.fc.spring.service.UserService">
    	<!--注入userDao对象
    		name: 类里面属性名称
    		ref: 创建的对象的id
    	-->
        <property name="userDao" ref="userDaoImpl">
    </bean>
    
    <bean id="userDaoImpl" class="com.fc.spring.dao.UserDaoImpl"></bean>
    ```

    

- 内部bean和级联赋值

  ```java
  // 一对多关系 -- 员工和部门
  public class Dept {
      private String dName;
      /set,toString/
  }
  //=========================
  public class Emp{
      private String eName;
      private String gender;
      private Dept dept;
      /set,toString/
  }
  ```

  - 内部bean写法

  ```xml
  <bean id="emp" class="com.fc.spring.bean.Emp">
      <property name="eName" value="rose"></property>
      <property name="gender" value="female"></property>
      <property name="dept">
      	<bean id="dept" class="com.fc.spring.beanDept">
      		<property name="dName" value="开发部"></property>
          </bean>
      </property>
  </bean>
  ```

  - 级联赋值第一种写法

  ```xml
  <bean id="emp" class="com.fc.spring.bean.Emp">
      <property name="eName" value="rose"></property>
      <property name="gender" value="female"></property>
      <property name="dept" ref="dept"></property>
  </bean>
  <bean id="dept" class="com.fc.spring.beanDept">
      <property name="dName" value="开发部"></property>
  </bean>
  ```

  - 级联赋值第二种写法(**需要有Dept的get方法,否则报错**)

  ```xml
  <bean id="emp" class="com.fc.spring.bean.Emp">
      <property name="eName" value="rose"></property>
      <property name="gender" value="female"></property>
      <property name="dept" ref="dept"></property>
      <property name="dept.dName" value="技术部"></property>
  </bean>
  ```

  测试

  ```java
  @Test
  public void test(){
      ApplicationContext context = 
          new ClassPathXmlApplicationContext("bean.xml");
      Emp emp = context.getBeans("emp",Emp.class);
      sout(emp);
  }
  ```

- 注入集合属性

  - 注入数组
  - 注入`List`集合
  - 注入`Map`
  - 注入`Set`

  ```java
  // 创建Student类
  public class Student {
      private String[] courses;
  
      private List<String> list;
  
      private Map<String,String> maps;
  
      private Set<String> sets;
  
      /set/
  }
  ```

  ```xml
  <!--配置-->
      <bean id="student" class="com.fc.spring5.collection.Student">
          <!--数组-->
          <property name="courses" >
              <!--这里也可以用list标签-->  
              <array>
                  <value>java</value>
                  <value>vue</value>
                  <value>mysql</value>
              </array>
          </property>
          <!--list-->
          <property name="list" >
              <list>
                  <value>张三</value>
                  <value>李四</value>
                  <value>王五</value>
              </list>
          </property>
          <!--map-->
          <property name="maps">
              <map>
                  <entry key="name" value="zhangsan"></entry>
                  <entry key="gender" value="female"></entry>
              </map>
          </property>
          <!--set-->
          <property name="sets">
              <set>
                  <value>mysql</value>
                  <value>java</value>
              </set>
          </property>
      </bean>
  </beans>
  ```

  - **集合里面设置对象类型**

  ```java
  public class Course {
      private String cname;
      /set/
  }
  //===============
  public class Student {
      private List<Course> courseList;
      /set/
  }
  ```

  ```xml
  <bean id="student" class="com.fc.spring5.collection.Student">
      <property name="courseList" >
          <list>
              <ref bean="course1"></ref>
              <ref bean="course2"></ref>
          </list>
      </property>
  </bean>
   <bean id="course1" class="com.fc.spring5.collection.Course">
       <property name="cname" value="java"></property>
  </bean>
  <bean id="course2" class="com.fc.spring5.collection.Course">
      <property name="cname" value="spring"></property>
  </bean>
  ```

  - **提取集合注入部分**

  ```xml
  <?xml version="1.0" encoding="utf-8" ?>
  <!--在`spring`配置文件中引入名称空间`util`-->
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         
         xmlns:p="http://www.springframework.org/schema/p"
         xmlns:util="http://www.springframework.org/schema/util"
         
         xsi:schemaLocation="http://www.springframework.org/schema/beans
             http://www.springframework.org/schema/beans/spring-beans.xsd
                             
              http://www.springframework.org/schema/util
             http://www.springframework.org/schema/util/spring-util.xsd">
      
      <!--提取list集合类型属性注入-->
      <util:list id="bookList">
          <value>java</value>
          <value>spring</value>
      </util:list>
      <!--提取list集合类型属性注入使用-->
      <bean id="book" class="com.fc.spring.collection.Book">
      	<property name="list" ref="bookList"></property>
      </bean>
  </beans>
  ```

  

#### 2.3.3 FactoryBean

>  Spring 有两种类型bean，--种普通bean，另外一种工厂bean（FactoryBean）
> 普通bean：在配置文件中定义bean类型就是返回类型。
> 工厂bean：在配置文件定义bean类型可以和返回类型不一样。
>
> 第一步 创建类，让这个类作为工厂 bean，实现接口 FactoryBean 
>
> 第二步 实现接口里面的方法，在实现的方法中定义返回的 bean 类型

```java
public class MyBean implements FactoryBean<Course> {

    //定义返回bean
    @Override
    public Course getObject() throws Exception {
        Course course = new Course();
        course.setCname("abc");
        return course;
    }
}

```

```xml
<bean id="myBean" class="com.atguigu.spring5.factorybean.MyBean"></bean>
```

```java
@Test
public void test3() {
 ApplicationContext context =
 new ClassPathXmlApplicationContext("bean3.xml");
 Course course = context.getBean("myBean", Course.class);
    //返回值类型可以不是定义的bean类型！
 System.out.println(course);
}
```

#### 2.3.4 bean 作用域

> 在 Spring 里面，默认情况下，bean 是单实例对象，下面进行作用域设置：
>
> （1）在 spring 配置文件 bean 标签里面有属性（scope）用于设置单实例还是多实例
>
> （2）scope 属性值 
>
> - singleton(默认)，--->单实例对象
> - prototype，--->多实例对象
> - request ---> 一次请求 (不用)
> - session  ---> 一次会话(不用)

```xml
<bean id="book" class="com.atguigu.spring5.collectiontype.Book" scope="prototype">
    <!--设置为多实例-->
        <property name="list" ref="bookList"></property>
</bean>
```

- `singleton` 和`prototype`区别
  - singleton单实例，prototype 多实例+
  - 设置scope值是singleton时候，加载spring配置文件时候就会创建单实例对象。
  - 设置scope值是prototype 时候，不是在加载spring配置文件时候创建对象，在调用getBean方法时候创建多实例对象。

#### 2.3.5 bean 生命周期

- 通过构造器创建bean实例（无参数构造）
- 为bean的属性设置值和对其他bean引用（调用set方法）。
- 调用bean的初始化的方法（需要进行配置）
- bean可以使用了（对象获取到了）。
- 当容器关闭时候，调用bean的销毁的方法（需要进行配置销毁的方法）



```java
public class Orders {
    //无参数构造
    public Orders() {
        System.out.println("第一步 执行无参数构造创建 bean 实例");
    }
    private String oname;
    public void setOname(String oname) {
        this.oname = oname;
        System.out.println("第二步 调用 set 方法设置属性值");
    }
    //创建执行的初始化的方法
    public void initMethod() {
        System.out.println("第三步 执行初始化的方法");
    }
    //创建执行的销毁的方法
    public void destroyMethod() {
        System.out.println("第五步 执行销毁的方法");
    }
}

```

```xml
<!--配置文件的bean参数配置-->
<bean id="orders" class="com.atguigu.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod">	
    <!--配置初始化方法和销毁方法-->
    <property name="oname" value="手机"></property>
    <!--这里就是通过set方式（注入属性）赋值-->
</bean>

<!--配置后置处理器-->
<bean id="myBeanPost" class="com.atguigu.spring5.bean.MyBeanPost"></bean>

```

```java
 @Test
 public void testBean3() {
    // ApplicationContext context =
    // new ClassPathXmlApplicationContext("bean4.xml");
     ClassPathXmlApplicationContext context =
     new ClassPathXmlApplicationContext("bean4.xml");
     Orders orders = context.getBean("orders", Orders.class);
     System.out.println("第四步 获取创建 bean 实例对象");
     System.out.println(orders);
     //手动让 bean 实例销毁
     context.close();
 }

```

- bean 的后置处理器，bean 生命周期有七步 （正常生命周期为五步，而配置后置处理器后为七步）

   （1）通过构造器创建 bean 实例（无参数构造）

   （2）为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）

   （3）把 bean 实例传递 bean 后置处理器的方法 `postProcessBeforeInitialization`

   （4）调用 bean 的初始化的方法（需要进行配置初始化的方法）

   （5）把 bean 实例传递 bean 后置处理器的方法 `postProcessAfterInitialization`

   （6）bean 可以使用了（对象获取到了）

   （7）当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）

  ```java
  public class MyBeanPost implements BeanPostProcessor{
      @Override
      ...
      
  }
  ```

#### 2.3.6 xml自动装配(很少用)

> 根据指定装配规则（属性名称或者属性类型），Spring 自动将匹配的属性值进行注入。

1. 根据属性名称自动注入

   - bean标签属性`autowire` 配置自动装配
     - `byName`根据属性名称注入 注入bean的id和类属性名称一样
     - `byType`根据属性类型注入 

   ```xml
   
   <bean id="emp" class="com.fc.spring5.autowire.Emp" autowire="byName">
       <!--<property name="dept" ref="dept"></property>-->
   </bean>
   <bean id="dept" class="com.fc.spring5.autowire.Dept"></bean>
   ```

2. 根据属性类型自动注入

   - **不能找到多个** 

   ```xml
   <bean id="emp" class="com.fc.spring5.autowire.Emp" autowire="byType">
   </bean>
   <bean id="dept" class="com.fc.spring5.autowire.Dept"></bean>
   <!--有多个同一type的时候报错-->
   <bean id="dept2" class="com.fc.spring5.autowire.Dept"></bean>
   ```

#### 2.3.7 外部属性文件

1. 直接配置数据库的信息

   - 配置德鲁伊连接池(引入依赖jar包(druid-x.x.x.jar))

2. 引入外部属性文件配置数据库连接池

   - 创建`jdbc.property`文件

   ```properties
   prop.driverClass = com.mysql.jdbc.Driver
   prop.url=jdbc:mysql://localhost:3306/sys_demo
   prop.username=root
   prop.password=root
   ```

3. 把外部属性文件引入到spring配置文件

   ```xml
   <?xml version="1.0" encoding="utf-8" ?>
   <!--引入`context`名称空间-->
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          
          xmlns:context="http://www.springframework.org/schema/context"
          
          xsi:schemaLocation="http://www.springframework.org/schema/beans
              http://www.springframework.org/schema/beans/spring-beans.xsd
                              
              http://www.springframework.org/schema/context
              http://www.springframework.org/schema/context/spring-context.xsd">
       <context:property-placeholder location="classpath:jdbc.properties"/>
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
           <property name="driverClassName" value="${prop.driverClass}"></property>
           <property name="url" value="${prop.url}"></property>
           <property name="username" value="${prop.username}"></property>
           <property name="password" value="${prop.password}"></property>
       </bean>
   </beans>
   ```

### 2.4 基于注解实现

#### 2.4.1 注解

- 格式

  ```java
  @注解名称(属性名=属性值,属性名2=属性值2)
  ```

- 注解作用在 类 方法 属性上面

- 目的: 简化xml配置

#### 2.4.2 Bean管理

> 功能一样 都可以用来创建bean实例 建议哪个层用哪个

- `@Component`
- `@Service`
- `@Controller`
- `@Repository`

#### 2.4.3 对象创建

1. 引入依赖

   `spring-aop-x.x.x.PELEASE.jar`

2. 开启组件扫描

   - 多个包可以用逗号隔开 也可以直接写上层目录

   ```xml
   <!--先引入名称空间-->
   <context:component-scan base-package="com.fc"></context:component-scan>
   ```

3. 创建类 

   ```java
   // value值可以省略 默认类名称(首字母小写)
   // @Component(value = "student")
   @Component 
   public class Student {
       public void add(){
           System.out.println("add");
       }
   }
   ```

4. 测试

   ```java
   @Test
   public void addTest() throws Exception {
       ApplicationContext context = new ClassPathXmlApplicationContext("bean6.xml");
       Student student = context.getBean("student", Student.class);
       student.add();
   }
   ```



- 说明

  - 默认扫描所有 下面配置可以自定义选择扫描哪些注解

  ```xml
  <context:component-scan base-package="com.fc" use-default-filters="false">
      <!--扫描哪些-->
      <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
      <!--排除-->
      <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository" />
  
  </context:component-scan>
  ```



#### 2.4.4 属性注入

- `@AutoWired`: 根据属性类型自动装配

  1. Service和dao对象 添加对应注解

  2. 在service注入dao对象 在service类添加dao类型属性 在属性上面使用注解

     ```java
     @Service
     public class UserService {
         @Autowired // 根据类型自动注入
         private UserDao userDao;
         public void add(){ 
             System.out.println("service add..");
             userDao.add();
         }
     }
     ```

     

- `@Qualifier`: 根据属性名称注入

  - 和`@AutoWired`一起使用

  - 有**多个接口实现类**，无法通过类型找到的时候用这个

    ```java
    @Repository("studentImpl222")
    public class StudentImpl implements StudentDao {
        @Override
        public void add() {
            System.out.println("dao add.....");
        }
    }
    ```

    ```java
    @Service
    public class Student {
        @Autowired
        @Qualifier(value="studentImpl222")
        private StudentImpl student;
        public void add(){
            System.out.println("add");
            student.add();
        }
    }
    ```

    

- `@Resource`: 可以根据类型 也可以根据名称(属于javax 不建议使用)

  ```java
  // 根据类型
  @Resource 
  private UserDao userDao;
  
  
   // 根据名称
  @Resource(name="studentImpl2")
  private UserDao userDao;
  ```

  

- `@value`: 注入普通类型属性,不用set方法

  ```java
  @Value(value = "jack")
  private String name;
  ```

#### 2.4.5 完全注解开发

1. 创建`config/SpringConfig`(名字随意)配置类,代替xml配置文件

   ```java
   @Configuration
   @ComponentScan(basePackages = {"com.fc"})
   public class SpringConfig {
   }
   ```

2. 编写测试类

   ```java
   @Test
   public void addTest2() throws Exception {
       ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
       Student s = context.getBean("student", Student.class);
       s.add();
   }
   ```

   

## 3. AOP

### 3.1 Aop概念

- 面向切面编程(方面)
  - 对业务逻辑的各个部分进行隔离 降低耦合 提高复用 提高开发效率
  - 通俗理解: 不通过修改源代码添加新的功能

### 3.2 底层原理 - 动态代理

- 有接口情况 -- JDK动态代理

  - 创建接口实现类代理对象 增强类的方法

  - 使用`Proxy`类里面的方法创建代理对象

  - 调用`newProxyInstance`方法 

    - 第一个参数 类加载器`ClassLoader`
    - 第二个参数 增强方法所在的类,这个类实现的接口 (支持多个接口 )
    - 第三个参数 实现这个接口`InvocationHandle`,创建代理对象 写增强的方法 
  - 返回指定接口的代理类的实例
  
  
  
- 步骤
  
  - 创建接口
  
      ```java
      public interface UserDao {
          public int add(int a,int b);
          public String update(String id);
      }
    ```
  
  - 实现接口
  
      ```java
      public class UserDaoImpl implements UserDao {
          @Override
          public int add(int a, int b) {
              return a+b;
          }
      
          @Override
          public String update(String id) {
              return id;
          }
      }
    ```
  
  - 使用`Proxy`类创建接口代理对象
  
      ```java
      public class JDKProxy {
          public static void main(String[] args) {
              // 创建接口实现类代理对象
              Class[] interfaces = {UserDao.class};
              UserDaoImpl userDao = new UserDaoImpl();
              // 这里第三个参数也可以用匿名内部类
              UserDao dao = (UserDao) Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));
      
              int sum = dao.add(1, 2);
              System.out.println(sum);
          }
      }
      
      // 创建代理对象代码
      class UserDaoProxy implements InvocationHandler{
      
          // 把创建的是谁的对象 把谁传过来
          private Object obj;
          public UserDaoProxy(Object obj){
              this.obj = obj;
          }
      
          // 增强的逻辑
          @Override
          public Object invoke(Object o, Method method, Object[] args) throws Throwable {
              // 方法之前
              System.out.println("方法之前执行..."+method.getName() + "传递的参数" + Arrays.toString(args));
      
              // 被增强的方法执行
              Object res = method.invoke(obj, args);
      
              // 方法之后
              System.out.println("方法之后执行" + obj);
      
              return res;
          }
      }
      ```



- 没有接口 --- CGLIB动态代理
  
  - ~~子类重写方法~~
  
  ```java
  class User{
      public void add(){
          
      }
  }
  ========
  // 创建子类
  class Person extends User{
      public void add(){
          super.add()
          // 增强逻辑
      }
  }
  ```
  
  - `CGLIB`动态代理
  
    > 创建当前类子类的代理对象
  
    
  
  

### 3.3 术语

- 拿下面的对象为例

  ```java
  class User {
      add(){...}
      update(){...}
      select(){...}
      delete(){...}
  }
  ```

- 连接点

  > 哪些方法可以增强 这些方法就称为**连接点**

- 切入点

  > 实际被增强的方法 叫**切入点**

- 通知(增强)

  > 实际增强的逻辑部分称为通知 
  >
  > 通知有多种类型
  >
  > - 前置通知
  > - 后置通知
  > - 环绕通知
  > - 异常通知
  > - 最终通知

- 切面

  > 是动作
  >
  > 把通知应用到切入点的过程

### 3.4 Aop操作--基于AspectJ

- 依赖
  - `spring-aspects-5.2.9.RELEASE.jar`
  - `com.springsource.net.sf.cglib-x.x.x.jar`
  - `com.springsource.org.aopalliance-x.x.x.jar`

  - `com.springsource.org.aspectj.weaver-x.x.x.REALSE.jar`

- 切入点表达式

  - 作用 直到对哪个类的哪个方法进行增强

  - 语法

    ```java
    execution([权限修饰符][返回类型][全类名][方法名称]([参数列表]))
    ```

  - 例子

    ```java
    // 例 对com.fc.dao.BookDao类的add进行增强
    
    execution(* com.fc.dao.BookDao.add(..));
    // *表示 所有权限
    // 返回类型 可以省略 空格不能省略 
    // 参数可以用..代替
    
     
    // 例 对com.fc.dao.BookDao类的所有方法进行增强
    execution(* com.fc.dao.BookDao.*(..));
    
    // 例 对com.fc.dao包里面所有类 所有方法进行增强
    execution(* com.fc.dao.*.*(..));
    ```

- 基于注解

  1. 创建类 定义方法

     ```java
     public class User {
         public void add(){
             System.out.println("add...");
         }
     }
     ```

  2. 创建增强类 编写增强逻辑

     1. 在增强类里面 创建方法 让不同方法代表不同的通知类型

        ```java
        public class UserProxy {
            public void before(){
                System.out.println("before");
            }
        }
        ```

  3. 进行通知的配置

     1. ~~开启注解扫描~~

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        
               xmlns:context="http://www.springframework.org/schema/context"
               xmlns:aop="http://www.springframework.org/schema/aop"
               xsi:schemaLocation="http://www.springframework.org/schema/beans
                   http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/context
                   http://www.springframework.org/schema/context/spring-context.xsd
                    http://www.springframework.org/schema/aop
                   http://www.springframework.org/schema/aop/spring-aop.xsd">
        <!--开启注解扫描-->
            <context:component-scan base-package="com.fc.spring5.aopanno">
            </context:component-scan>
        ```

     - **注解扫描(完全注解方式)**

       - 创建ConfigAop文件

       ```java
       @Configuration
       @ComponentScan(basePackage={"com.fc"})
       @EnableAspectJAutoProxy(proxyTargetClass = true)
       public class ConfigAop{
           
       }
       ```

       

     2. 使用注解创建User和UserProxy对象

        ```java
        @Component
        public class User {
        
        ```

        ```java
        @Component
        public class UserProxy {
        
        ```

        

     3. 在增强类上面添加注解@Aspect

        ```java
        @Component
        @Aspect
        public class UserProxy {
        ```

        

     4. 在spring配置文件中 开启生成代理对象

        ```xml
           
        <!--开启注解扫描-->
        <context:component-scan base-package="com.fc.spring5.aopanno">
        <!--开启Aspect生成代理对象-->
        <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
        ```

  4. 配置不同类型的通知

     1. 在增强类里面 在作为通知方法上面添加通知类型注解 使用切入点表达式

        ```java
        @Component
        @Aspect
        public class UserProxy {
            @Before("execution(* com.fc.spring5.aopanno.User.add(..))")
            public void before(){
                System.out.println("before");
            }
        }
        ```

  5. 测试

     ```java
     public class TestAop {
         @Test
         public void test(){
             ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
             User user = context.getBean("user", User.class);
             user.add();
         }
     }
     
     // 结果
     // before
     // add...
     ```

- 其他类型

  ```java
  @Before("execution(* com.fc.spring5.aopanno.User.add(..))")
  public void before(){
      System.out.println("before");
  }
  
  @After("execution(* com.fc.spring5.aopanno.User.add(..))")
  public void after(){
      System.out.println("after");
  }
  
  @AfterReturning("execution(* com.fc.spring5.aopanno.User.add(..))")
  public void afterReturning(){
      System.out.println("afterReturning");
  }
  
  @AfterThrowing("execution(* com.fc.spring5.aopanno.User.add(..))")
  public void afterThrowing(){
      System.out.println("afterReturning");
  }
  @Around("execution(* com.fc.spring5.aopanno.User.add(..))")
  public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{
      System.out.println("环绕之前");
  
      proceedingJoinPoint.proceed();
  
      System.out.println("环绕之后");
  }
  ```

  - 执行结果

    ```text
    环绕之前
    before
    add...
    afterReturning(有异常不执行)
    after --- > 最终通知
    环绕之后
    ```

  - 执行结果(有异常的时候)

    ```text
    环绕之前
    before
    after
    afterThrowing
    ```

- 相同切入点

  ```java
  // 相同切入点抽取
  @Pointcut("execution(* com.fc.spring5.aopanno.User.add(..))")
  public void pointDemo(){
  }
  // 参数写方法名称
  @Before("pointDemo()")
  public void before(){
      System.out.println("before");
  }
  ```

- 多个增强类对同一个方法进行增强 设置增强类优先级

  ```java
  @Orader(数值类型的值) // 数字越小 优先越高
  ```
  
- 基于配置文件(很少用)

  1. 创建两个类和方法

  2. 在配置文件中创建两个对象

  3. 在spring配置文件中配置切入点

     ```xml
     <aop:config>
         <!--切入点-->
     	<aop:point id="p" expression=execution(* com.fc.spring.Book.buy(..))/>
         <!--配置切面-->
         <aop:aspect ref="bookProxy">
             <!--增强作用在具体方法上-->
         	<aop:before method="before" pointcut-ref="p">
         </aop:aspect>
     </aop:config>
     ```

     

## 4  JdbcTemplate

### 4.1 概念和准备

### 4.1.1概念

> Spring 框架对JDBC进行封装 	方便对数据库的操作

### 4.1.2准备

- 导入jar包

  - `mysql-connector-java-x.x.x-bin.jar`

  - `druid-x.x.x.jar`
  - `spring-jdbc-x.x.x.REALEASE.jar`
  - `spring-tx-x.x.x.REALEASE.jar`
  - `spring-orm-x.x.x.REALEASE.jar`

- 配置数据库连接池

  ```xml
  <!--数据库连接池配置略-->
   <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
          <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
          <property name="url" value="jdbc:mysql://localhost:3306/user_db?characterEncoding=UTF-8"></property>
          <property name="username" value="root"></property>
          <property name="password" value="123"></property>
      </bean>
  ```

- 配置`JdbcTemplate`对象 注入`DataSource`

  ```xml
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
      <propertity name="dataSource" ref="dataSource"></propertity>
  </bean>
  ```

- 创建servive类，创建dao类，在dao注入jdbcTemplate对象

  - service

    ```java
    @Service
    public class BookService {
        // 注入dao
        @Autowired
        private BookDao bookDao;
    }
    ```

  - daoimpl

    ```java
    @Repository
    public class BookDaoImpl implements BookDao {
        // 注入jdbcTemplate
        @Autowired
        private JdbcTemplate jdbcTemplate;
    }
    ```

- 开启组件扫描

  ```xml
  <!--    开启组件扫描 -->
      <context:component-scan base-package="com.fc"></context:component-scan>
  ```

### 4.1.3 操作数据库

- 对应数据库创建对应实体类

  - com.fc.spring5.entity
  
  ```java
  public class Book {
      private String userId;
      private String userName;
      private String userStatus;
      // set and get
}
  ```
  
- 编写service和dao

  - 在dao进行数据库添加操作

  - 调用`jdbcTemplate`对象里面`update`方法实现添加操作(增\删\改)

    - 两个参数

      - 第一个参数:`sql`语句
      - 第二个参数: 可变参数,设置`sql`语句值

      ```java
      public class BookDaoImpl implements BookDao {
          // 注入jdbcTemplate
          @Autowired
          private JdbcTemplate jdbcTemplate;
      	// 新增
          @Override
          public void add(Book book) {
              // 创建sql语句
              String sql = "insert into t_book values(?,?,?)";
              // 调用方法实现
              Object[] args = {book.getBookId(), book.getBookName(), book.getBookStatus()};
              int update = jdbcTemplate.update(sql,args);
              System.out.println(update);
          }
          // 删除
          @Override
          public void delete(int bookId) {
              String sql = "delete from t_book where book_id = ?";
              int update = jdbcTemplate.update(sql,bookId);
              System.out.println(update);
          }
      	// 修改
          @Override
          public void update(Book book) {
              String sql = "update t_book set book_name=?,book_status=?where book_id = ?";
              // 调用方法实现
              Object[] args = {book.getBookName(), book.getBookStatus(),book.getBookId()};
              int update = jdbcTemplate.update(sql,args);
              System.out.println(update);
          }
      }
      ```

  - 查找

    - 返回总记录条数`queryForObject`

      - 两个参数
        - sql语句
        - 返回值类型.class

      ```java
       @Override
          public int count() {
              String sql = "select COUNT(*) from t_book";
              Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
              return count;
          }
      ```

    - 返回对象`queryForObject`

      - 三个参数
        - `sql`语句
        - `RowMapper`是一个接口,返回不同类型数据 ,使用这个接口实现类完成数据封装.  

      ```java
       @Override
          public Book findOne(int id) {
              String sql = "select * from t_book where book_id = ?";
              Book book = jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<Book>(Book.class),id);
              return book;
          }
      ```

    - 返回集合`queryForObject`

      - 两个参数

        - sql语句
        - `RowMapper`

        ```java
        @Override
            public List<Book> findAll() {
                String sql = "select * from t_book";
                List<Book> list = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
                return list;
            }
        ```

  - 测试

    ```java
    @Test
        public void testJdbcTemplate(){
            ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
            BookService bookService = context.getBean("bookService", BookService.class);
                // 添加
    //            Book book = new Book();
    //            book.setBookId(3);
    //            book.setBookName("《小妇人》");
    //            book.setBookStatus("上架");
    //            bookService.addBook(book);
                // 修改
        //        book.setBookId(2);
        //        book.setBookName("《坏蛋是怎样炼成的》");
        //        book.setBookStatus("上架");
                // 删除
    //            bookService.deleteBook(2);
            // 查找总条数
    //        int i = bookService.selectBookCount();
    //        System.out.println("表中有" + i +"条记录");
            // 查找一条数据
    //        Book book = bookService.selectOne(2);
    //        System.out.println(book);
            // 查找多条
            List<Book> books = bookService.selectAll();
            System.out.println(books);
        }
    ```

- 批量操作

  > 操作表里的多条记录 使用`batchUpdate`
  >
  > 两个参数`batchUpdate(String sql,List<Object[]> batchArgs)`
  >
  > - 第一个参数,`sql`语句
  > - 第二个参数:List集合,添加多条记录数据 

  ```java
   @Override
      public void batchAddBook(List<Object[]> bookList) {
          String sql = "insert into t_book values(?,?,?)";
          int[] ints = jdbcTemplate.batchUpdate(sql, bookList);
          System.out.println(Arrays.toString(ints));
      }
  ```

  测试

  ```java
   List<Object[]> bookList = new ArrayList<>();
          Object[] o1 = {4,"《老人与海》","上架"};
          Object[] o2 = {5,"《放风筝的人》","下架"};
          bookList.add(o1);
          bookList.add(o2);
          bookService.batchAdd(bookList);
  ```

  - 批量修改删除过程类似

## 5 事务操作

### 5.1 概念

- 什么是事务
  - 事务是数据库操作的最基本单元,逻辑上一组操作,要么都成功,如果有一个失败那么都失败
- 四个特性（ACID特性）
  - 原子性 - 不可分割
  - 一致性 - 操作前后总量不变
  - 隔离性 - 多个操作互不影响
  - 持久性 - 提交之后数据变化

### 5.2 事务操作

- 环境搭建

  > 转账场景

  - 创建数据库及表 添加记录

  - 创建service,搭建dao,完成对象创建

  - 在dao里面创建两个方法(增加钱,减少钱) 

    ```java
    @Override
        public void addMoney() {
           String sql = "update bank set money=money+? where user = ?";
           jdbcTemplate.update(sql,100,"mary");
        }
    
        @Override
        public void reduceMoney() {
            String sql = "update bank set money=money-? where user=?";
            jdbcTemplate.update(sql, 100,"lucy");
        }
    ```

- 存在的问题 

  > a给b转账之后 发生异常 程序终止 

### 5.3 spring事务管理

- 事务添加到JJavaEE三层架构里面的service层
- 在Spring进行事务管理操作
  - ~~编程式事务管理~~
  - 声明式事务管理，底层使用`AOP`

- 事务管理`API`

  - 提供接口`platformTransactionManager`，代表事务管理器，这个接口针对不同框架提供不同的实现类


#### 5.3.1 基于注解

- 事务操作(注解声明式事务管理)

  1. 配置文件配置事务管理器(`jdbc`)

     ```xml
     <!--创建事务管理器-->
     <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" 			id="transactionManager">
     	<!--注入数据源-->
         <property name="dataSource" ref="dataSource"></property>
     </bean>
     
     ```

  2. 开启事务注解

     1. 引入名称空间

        ```xml
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:context="http://www.springframework.org/schema/context"
               xmlns:aop="http://www.springframework.org/schema/aop"
               
               xmlns:tx="http://www.springframework.org/schema/tx"
        
               xsi:schemaLocation="http://www.springframework.org/schema/beans
                   http://www.springframework.org/schema/beans/spring-beans.xsd
                   http://www.springframework.org/schema/context
                   http://www.springframework.org/schema/context/spring-context.xsd
                    http://www.springframework.org/schema/aop
                   http://www.springframework.org/schema/aop/spring-aop.xsd
                                   
                    http://www.springframework.org/schema/tx
                   http://www.springframework.org/schema/tx/spring-tx.xsd"
        >
        ```

     2. 开启事务注解

        ```xml
        <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
        ```

  3. 在service类上面(获取service类里面方法上面)添加事务注解

     `@Transactional` 

     - 可以加在类上面 也可以加在方法上面
     - 加在类上面,这个类所有方法都添加事务
     - 加在方法上面,为这个方法添加事务 

     ```java
     @Service
     @Transactional
     public class UserService {
     }
     ```

- 参数配置

  ```java
  @Service
  @Transactional(参数)
  public class UserService {
      
  }
  ```
  
  - 事务传播行为
  
    ```java
  @Transactional(propagation=Propagation.REQUIRED) // 默认REQUIRED
    ```

      Spring框架事务传播行为有7种
  
    | 传播属性     | 描述                                                         |
  | ------------ | ------------------------------------------------------------ |
    | REQUIRED     | 如果有事务,当前事务就在这个事务内运行<br />否则启动新事务,并在自己的事务内运行 |
  | REQUIRED_NEW | 当前方法必须启动新事务,并在他自己的事务内运行<br />如果有事务正在运行,应该将他挂起 |
    | ...          |                                                              |
    |              |                                                              |
    |              |                                                              |
    |              |                                                              |
    | ...          |                                                              |
  
  - 事务隔离级别
  
    ```java
  @Transactional(isolution=Isolution.REPEATABLE_READ) // mysql 默认使用这一级别
    ```

    事务隔离级别有四种
  
    |                  |          | 脏读 | 不可重复读 | 幻读 |
  | :--------------- | -------- | ---- | ---------- | ---- |
    | READ_UNCOMMITTED | 读未提交 | T    | T          | T    |
  | READ_COMMITTED   | 读已提交 | F    | T          | T    |
    | REPEATABLE_READ  | 可重复读 | F    | F          | T    |
    | SERIALIZABLE     | 串行化   | F    | F          | F    |
  
  - timeout : 超时时间
  
    - 事务需要在一定时间内提交,不提交进行回滚
  - 默认值 - 1,设置时间以秒为单位
  
    ```java
    @Transactional(timeout =-1)
    ```
  
  - readOnly : 是否只读
  
    - 读:查询操作
    - 写:增删改
    - 默认值:false,可以查询,可以修改删除添加
  
    ```java
    @Transactional(timeout =true) // 只能查询
    ```
  
  - rollbackFor: 回滚
  
    - 设置出现哪些异常回滚
  
    ```java
    @Transactional(rollbackFor =异常类型的class)
    ```
  
  - noRollbackFor:不回滚
  
    - 设置出现哪些异常回滚

#### 5.3.2 ~~基于XML~~

1. 配置事务管理器

   ```xml
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   ```

2. 配置通知(事务就是通知)

   ```xml
   <tx:advice id="txadvice">
           <!--配置事务参数-->
           <tx:attributes>
               <!--指定那种规则的方法上面添加事务-->
               <tx:method name="accountMoney"  propagation="REQUIRED"/>
   			<!--以account开头的-->
               <!--<tx:method name="account*"/>-->
           </tx:attributes>
       </tx:advice>
   ```

3. 配置切入点和切面

   ```xml
    <aop:config>
           <!--配置切入点-->
           <aop:pointcut id="pt" expression="execution(* com.fc.spring5.service.UserService.*(..))"/>
           <!--配置切面-->
           <aop:advisor advice-ref="txadvice" pointcut-ref="pt" />
       </aop:config>
   ```

#### 5.3.3 完全注解

1. 创建配置类,替代xml配置文件

   ```java
   @Configuration //配置类
   @ComponentScan(basePackages = "com.fc") //组件扫描
   @EnableTransactionManagement // 开启事务
   public class TxConfig {
       // 创建数据库连接池
       @Bean
       public DruidDataSource getDruidDataSource(){
           DruidDataSource dataSource = new DruidDataSource();
           dataSource.setDriverClassName("com.mysql.jdbc.Driver");
           dataSource.setUrl("jdbc:mysql://localhost:3306/user_db?characterEncoding=UTF-8");
           dataSource.setUsername("root");
           dataSource.setPassword("123");
           return dataSource;
       }
   
       // 创建jdbcTemplate对象
       @Bean
       public JdbcTemplate getJdbcTemplate(DataSource dataSource){
           JdbcTemplate jdbcTemplate = new JdbcTemplate();
           // 到ioc容器中根据类型找到dataSource
           jdbcTemplate.setDataSource(dataSource);
           return jdbcTemplate;
       }
   
       // 创建事务管理器对象
       @Bean
       public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource){
           DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
           dataSourceTransactionManager.setDataSource(dataSource);
           return dataSourceTransactionManager;
       }
   }
   ```

2. 测试

   ```java
    @Test
       public void testBank2(){
           ApplicationContext context = new AnnotationConfigApplicationContext(TxConfig.class);
           UserService userService = context.getBean("userService", UserService.class);
           userService.accountMoney();
       }
   ```

   

## 6 spring5框架新特性

- 框架基于java8,运行兼容jdk9,许多不建议使用的方法和类在代码库中删除

- 自带日志封装,也可以整合其他日志工具

  - spring5移除了Log4jConfigListener,官方建议使用Log4j2

- 核心容器

  - 支持`@Nullable`注解

    > `@Nullable`注解可以使用在方法,属性,参数上面,表示方法可以返回空,属性,参数可以为空

  - 函数式风格(lamda表达式相关)

    ```java
     @Test
        public void test(){
            GenericApplicationContext context = new GenericApplicationContext();
            // 调用context的方法对象注册
            context.refresh();
            context.registerBean("user1",UserService.class,()->new UserService());
            // 获取在spring注册的对象
            UserService user = (UserService)context.getBean("user1");
        }
    ```

- 支持整合junit5

- SpringWebFlux