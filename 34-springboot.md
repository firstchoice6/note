# Spring boot

## 一 入门

### 1. helloworld

- 创建maven工程

- 导入依赖

  ```xml
  <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.0.0.RELEASE</version>
      </parent>
      <dependencies>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
      </dependencies>
  ```

- 编写主程序

  ```java
  package com.fc;
  /**
   * @SpringBootApplication 来标注一个主程序类 说明这个是一个SpringBoot应用
   * */
  @SpringBootApplication
  public class HelloWorldMain {
      public static void main(String[] args) {
          // spring应用启动
          SpringBootApplication.run(HelloWorldMain.class,args);
      }
  }
  
  ```

- 编写相关Controller，service

  ```java
  package com.fc.controller;
  @Controller
  public class HelloController {
      @ResponseBody
      @RequestMapping("/hello")
      public String hello(){
          return "hello world";
      }
  }
  
  // 抽离@ResponseBody注解
    /**@Controller
    @ResponseBody 
    这两个注解等同于 @RestController
    */ 
    @RestController
    public class HelloController {
          @RequestMapping("/hello")
        public String hello(){
            return "hello world";
        }
        
      }
  ```

#### 2 yaml

> 配置文件名 application.propertity 和 application.yaml（yam）

- 语法

  - 使用缩进表示层级关系
  - 缩进不允许使用tab 只能用空格
  - 缩进空格数量不重要 只要相同层级的元素左侧对齐
  - 大小写敏感

- 支持的数据结构

  - 对象
  - 数组
  - 字面量

  ```yaml

  server:
    port: 8081
    path: /hello
    # 冒号后面必须有空格
    # 字符串不用加上单双引号
    	 # 单引号 'a\nb' -> a\nb
    	 # 双引号 "a\nb" -> a 换行 b
   	# last-name 等同于 lastName
  friends:
    name: zhangsan
    age: 18
  friends: {name: zhangsan,age: 18}
  
  lists:
   - 1
   - 2
   - 3
  lists: [1,2,3]
    
  ```

- 配置文件注入

  - 生成配置信息

    ```xml
    <!--导入配置文件处理器 配置文件进行绑定就会有提示-->
    <dependency>
    	<groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
</dependency>
    
  ```
    
  - `@Value`注解方式
  
    ```java
    @Component
    public class Person{
        
        @Value("${person.last-name}")
        private String name;
        @Value("#{10*2}")
        private Integer age;
        @Value("true")
        private Boolean isBoss;
        
        ...
    }
    ```
  
    

  - `@ConfigurationProperties`注解方式
  
  ```java
  /**
  *将配置文件中配置的每一个属性的值 映射到这个组件中
  *@ConfigurationProperties： 告诉Springboot将本类中的所有属性和配置文件中的相=相关配置进行绑定
  prefix 配置文件中 哪个下面所有属性进行意义映射
  
  只有这个组件是容器中的组件 才能容器提供的@ConfigurationProperties
  */
  @Component
  @ConfigurationProperties(prefix = "person")
  // @Valitated 支持校验
  public class Person{
      // @Email => 必须为email
      private String name;
      private Integer age;
      private Boolean isBoss;
      private Date birthday;
      
      private Map<String,Object> maps;
      private List<Object> lists;
      private Dog dog;// name age
  }
  ```
  
  - yaml文件方式
  
    ```yaml
    server:
      port: 8080
     
     person:
       name: zhangsan
       age: 18
       isBoss: false
       birth: 2017/2/2
       maps: {k1: v1,k2: v2}
       lists:
         - 1
         - 2
         - 3
       dog:
        name: 小狗
      age: 2
    ```
  
  - `application.properties方式`
  
    ```properties
    # idea properties默认使用utf-8 设置里设置转为ascii码 否则中文会乱码
    
      person.last-name=zhangsan
      person.age=18
      person.isBoss=false
      person.birth: 2017/2/2
      person.maps.k1=v1
      person.maps.k2=v2
      person.lists=1,2,3
      person.dog.name=小狗
      person.dog.age=2
      
    ```
  
    
  
- 对比

  | --             | @ConfigurationProperties | @Value       |
  | -------------- | ------------------------ | ------------ |
  | 功能           | 批量绑定                 | 一个一个绑定 |
  | 松散语法       | 支持                     | 不支持       |
  | SpEL语言支持   | 不支持                   | 支持         |
  | JSR303数据校验 | 支持                     | 不支持       |
  | 复杂类型封装   | 支持                     | 不支持       |

- 读取指定配置文件

  ```java
  // ConfigurationProperties默认读取全局配置文件
  // 如果单独创建配置文件 则需要添加@PropertySource注解
  @PropertySource(value = {classpath:person.properties})
  @Component
  @ConfigurationProperties(prefix = "person")
  
  public class Person{
      private String name;
      private Integer age;
      ...
  }
  ```

- `@ImportResource`

  > ~~导入spring的配置文件 让配置文件里的内容生效~~
  >
  > springboot没有spring的配置文件 自己编写的配置也不会生效
  >
  > ~~需要在**配置类**上面添加`@ImportResource`~~

  ```java
  @ImportResource(locations = {"classpath:beans.xml"})
  // 导入spring的配置文件
  ```

  - **springboot推荐给容器中添加组件的方式(全注解)**

    ```java
    // 配置类
    ```

    

#### 3 测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class test{
    @Autowired
    Person person;
    
    @Test
    public void contextLoads(){
        sout(person)
    }
}
```



## 二 配置

## 三 日志

## 四 web开发

## 五 Docker

## 六 数据访问

##  七 启动配置原理

## 八 自定义starters

## 九 缓存

## 十 消息

## 十一 检索

## 十二 任务

## 十三 安全

## 十四 分布式

## 十五 开发热部署

## 十六 



