# Spring MVC

## 1. 简介

- MVC

  > MVC是一种软件架构的思想 将软件分为模型视图 控制
  >
  > - M： Model 模型层 指工程种的javaBean 作用是处理数据
  >   - 实体类Bean： 专门储存业务数据的
  >   - 业务处理Bean：Service Dao，专门用域处理逻辑业务和数据访问
  > - V: View 视图层 html jsp
  > - Controller，控制层 servlet 接受请求 响应浏览器

- SpringMVC特点
  - spring全家桶成员 与IOC无缝对接
  - 基于原生servlet 通过前端控制器DispatherServlet对请求进行统一处理
  - 代码简洁
  - 组件化程度高
  - 性能卓著

## 2. Helloworld

- 开发工具

- 创建maven工程

  - 添加web模块

  - 打包方式：war

  - 引入依赖

    ```xml
      <dependencies>
            <!--mvc-->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>5.3.1</version>
            </dependency>
            <!--log-->
            <dependency>
                <groupId>ch.qos.logback</groupId>
                <artifactId>logback-classic</artifactId>
                <version>1.2.3</version>
            </dependency>
            <!--servlet-->
            <dependency>
                <groupId>javax.servlet</groupId>
                <artifactId>javax.servlet-api</artifactId>
                <version>3.1.0</version>
                <scope>provided</scope>
            </dependency>
            <!--spring5和thymeleaf-->
            <dependency>
                <groupId>org.thymeleaf</groupId>
                <artifactId>thymeleaf-spring5</artifactId>
                <version>3.0.12.RELEASE</version>
            </dependency>
        </dependencies>
    ```

## 3. @RequestMapping注解

### 3.1 功能

> 将请求和处理请求的控制器方法关联起来 建立映射关系
>
> SpringMVC接收到指定的请求 就会来找到在映射关系中对应的控制器方法来处理这个请求

### 3.2 位置

- 标识一个类 设置映射请求的请求路径的初始信息
- 标识一个方法 设置映射请求请求路径的具体信息

```java
@Controller
@RequestMapping("/test")
public class MainController {
    // 请求路径为 /test/1
    @RequestMapping("1")
    public String test(){
        return "success";
    }
}

```

### 3.3 Value属性

- 字符串类型的数组 标识请求映射可以匹配多个请求地址所对应的请求

- 必填属性 至少通过请求地址匹配请求映射

  ```java
  @RequestMapping(value = {"1","2"})
  public String test(){
      return "success";
  }
  ```

### 3.4 Method属性

- 是一个`RequestMethod`类型的数组,标识该请求映射能够匹配多种请求方式

- 路径正确 方式不正确 报405错误 Method not allowed

  ```java
  @RequestMapping(value = {"1","2"},method = {RequestMethod.GET,RequestMethod.POST})
  public String test(){
      return "success";
  }
  ```

- 派生注解

  ```java
  @GetMapping()
  @PostMapping()
  @PatchMapping()
  @PutMapping()
  @DeleteMapping()
  ```

### 3.5 params属性

- 通过请求的参数匹配请求映射

- 多个条件必须同时满足

- 四种类型

  - `param`:必须携带param请求参数
  - `!param`:必须不能携带param请求参数
  - `param=value`
  - `param!=value`

  ```java
  @RequestMapping(value = "2",
              params = {
                      "name",
                      "!userName",
                      "Password=123456",
                      "token!=null"
              })
      public String test() {
          return "success";
      }
  ```

### 3.6 headers属性

- 规则同params

### 3.7 ant风格的路径

1. `?`:任意的单个字符
2. `*`:任意的0个或多个字符
3. `**`:人以的一层或者多层目录
   - 使用`**`,只能使用`/**/xxx/`

### 3.8 路径中的占位符

> 原始方式: /deleteUser?id=1
>
> rest方式: /deleteUser/1

```java
 @DeleteMapping("user/{id}")
public void DeleteUser(@PathVariable("id")String  id){
    System.out.println(id);
}
```



## 4 获取请求参数

### 4.1 通过sqrvletApi获取

```java
@RequestMapping("test")
public String testAnt(HttpServletRequest request) {
    String name = request.getParameter("name");
    return name;
}
```

### 4.2 使用控制器方法形参

```java
@RequestMapping("test")
public String testAnt(String name, String password,String[] hobbies) {
    System.out.println(Arrays.toString(hobbies));
    return name + " " + password;
}

// 多个同名参数会自动转换为用逗号隔开的字符串 或者可以用String[]接收
```

### 4.3 `@Requestparam`

```java
@RequestMapping("test")
public String testAnt(@RequestParam("user_name") String name) {
    return name;
}

@RequestMapping("test")
public String testAnt(@RequestParam(value="user_name",required=true,defaultValue="admin") String name) {
    return name;
}
```

- 属性
  - `value`
  - `required` 默认为`true`
  - defaultValue

### 4.4 `@RequestHeader`,`@CookieValue`

- 同`@Requestparam`

### 4.5 通过`pojo`获取

```java
package pojo;

public class User {
    private String name;
    private String password;
}
```

```java
@RequestMapping("test")
public String testAnt(User user) {
    return user.getName() +" "+ user.getPassword();
}
```



## 5.Restful

### 5.0 概念

> `Representational State Transfer` 表现层状态转移
>
> - 资源
>
>   > 资源是一种看待服务器的方式,即,将服务器看作是由很多高散的资源组成。每个资源是服务器上一个可命名的抽象概念。因为资源是一个抽象的概念,所以它不仅仅能代表服务器文件系统中的一个文件、数据库中的一张表等等具体的东西,可以将资源设计的要多抽象有多抽象,只要想象力允许而且客户端应用开发者能够理解。与面向对象设计类似,资源是以名词为核心来组织的,首先关注的是名词。一个资源可以由一个或多个URI来标识。URI既是资源的名称,也是资源在Web上的地址。对某个资源感兴趣的客户端应用,可以通过资源的URI与其进行交互。
>
> - 资源的表述
>
>   > 资源的表述是一段对于资源在某个特定时刻的状态的描述,可以在客户端服务器端之间转移(交换) 。资源的表述可以有多种格式,例如HTML/XMLJSON/纯文本/图片/视频/音频等等。资源的表述格式可以通过协商机制来确定。请求响应方向的表述通常使用不同的格式。
>
> - 状态转移
>
>   > 状态转移说的是:在客户端和服务器端之间转移(transfer)代表资源状态的表述。通过转移和操作资源的表述来间接实现操作资源的目的。

### 5.1 实现

| 操作 | 传统               | rest              |
| ---- | ------------------ | ----------------- |
| 查询 | `getUserInfo?id=1` | `user/1` ->get    |
| 保存 | `saveUser`         | `user`  ->post    |
| 删除 | `deleteUser?id=1`  | `user/1` ->delete |
| 更新 | `updateUser`       | `user`->put       |

## 6.HttpMessageConverter

> 报文信息转换器
>
> - @RequestBody
> - @ResponseBody
> - RequestEntity - 请求头 请求体
>   - RequestEntity.getHeaders()
>   - RequestEntity.getBody()
> - ResponseEntity - 响应头 响应体

- 使用@ResponseBody处理json

## 7.文件上传下载

### 7.1 下载

```java
@PostMapping("download")
public ResponseEntity<byte[]> test(HttpSession session) throws IOException {
    //获取servletContext对象
    ServletContext servletContext = session.getServletContext();
    // 获取文件真实路径
    String realPath = servletContext.getRealPath("/static/img/1.jpg");
    // 创建输入流
    FileInputStream is = new FileInputStream(realPath);
    // 创建字节数组
    byte[] bytes = new byte[is.available()];
    // 将流读写到数组中
    is.read(bytes);
    // 设置响应头
    MultiValueMap<String, String> headers = new HttpHeaders();
    // 设置下载方式及文件名
    headers.add("Content-Disposition", "attachment;filename=1.jpg");
    // 设置响应码
    HttpStatus ok = HttpStatus.OK;
    // 创建ResponseEntity对象
    ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, ok);
    // 关闭流
    is.close();
    return responseEntity;
}
```

### 7.2 上传

- 添加依赖

  ```xml
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
  </dependency>
  ```

- 配置文件上传解析器,将上传的文件转换为

  ```xml
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  </bean>
  
  ```

- 实现

  ```java
   @PostMapping("upload")
  public String testUpload(MultipartFile file){
      try {
          file.transferTo(new File("/static/",file.getOriginalFilename()));
      } catch (IOException e) {
          e.printStackTrace();
      }
      return "success";
  }
  ```

## 8.拦截器

```java
public class AdminInterceptor implements  HandlerInterceptor {

    /**
     * 在请求处理之前进行调用（Controller方法调用之前）
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
//        System.out.println("执行了TestInterceptor的preHandle方法");
        try {
            //统一拦截（查询当前session是否存在user）(这里user会在每次登陆成功后，写入session)
            User user=(User)request.getSession().getAttribute("USER");
            if(user!=null){
                return true;
            }
            response.sendRedirect(request.getContextPath()+"你的登陆页地址");
        } catch (IOException e) {
            e.printStackTrace();
        }
        return false;//如果设置为false时，被请求时，拦截器执行到此处将不会继续操作
                      //如果设置为true时，请求将会继续执行后面的操作
    }
 
    /**
     * 请求处理之后进行调用，但是在视图被渲染之前（Controller方法调用之后）
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {
//         System.out.println("执行了TestInterceptor的postHandle方法");
    }
 
    /**
     * 在整个请求结束之后被调用，也就是在DispatcherServlet 渲染了对应的视图之后执行（主要是用于进行资源清理工作）
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
//        System.out.println("执行了TestInterceptor的afterCompletion方法");
    }
    
}
```



