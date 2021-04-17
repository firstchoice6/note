# Mybatis

## 1 基本概念

### 1.1 三层架构

- 表现层
  - 展示数据
- 业务层
  - 处理业务需求
- 持久层
  - 和数据据库交互

### 1.2 环境搭建

- 数据库初始化

  ```sql
  create database mybatis;
  
  create table t_user(
  	`id` int primary key auto_increment,
  	`last_name`	varchar(50),
  	`sex` int
  );
  
  insert into t_user(`last_name`,`sex`) values('wzg168',1);
  ```

- mybatis环境搭建

  - 创建java工程

  - 添加mybatis核心jar

  - 添加mysql数据库连接驱动

  - 添加log4j日记所需核心jar 

  - 在src目录下添加 log4j.properties 日记配置文件 log4j.properties

   ```text
    # Global logging configuration
    log4j.rootLogger=DEBUG, stdout
    # Console output...
    log4j.appender.stdout=org.apache.log4j.ConsoleAppender
    log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
    log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
   ```
  
    

### 1.3 查询数据库案例

- 创建PoJo对象User

  ```java
  public class User {
  
  	private int id;
  	private String lastName;
      private int sex;
      
      // 重写toString方法 方便展示
  }
  ```

- 创建UserMapper.xml配置文件

  > 在Pojo对象User所在的包下，创建一个UserMapper.xml配置文件。这个配置文件用来配置对User对象所对应的表的增，删，改，查的语句。
  >
  > 一般这个配置文件的命名规则为：类名Mapper.xml
  >
  >  
  >
  > 如果是User类，那么对应的配置文件名为UserMapper.xml
  >
  > 如果是Teacher类，那么对应的配置文件名为TeacherMapper.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <!-- 
    	namespace属性是名称空间的意思。
    	功能相当于 给配置文件定义一个包名。
    	一般情况下。可以写两种值，一种是对应类的全类名
    	一种情况是。对应类的处理接口全类名（后面讲）。
  -->
  <mapper namespace="com.fc.pojo.User">
  	<!-- 
  		select 标签用于定义一个select查询语句
  		
  		id属性，又称之为statementId
  		id属性可以给select语句定义一个唯一的标识
  		
  		parameterType 属性定义参数的类型，int 表示基本的Integer类型
  		resultType 属性定义返回值的数据类型
  	 -->
  	<select id="selectUserById" parameterType="int" resultType="com.fc.pojo.User">
  		select id, last_name lastName, sex from t_user where id = #{id}
  	</select>
  </mapper>
  ```

- 在src目录创建mybatis-config.xml核心配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
      <!-- environments 是配置多个jdbc环境 
  		default表示使用的默认环境 
  	 -->
  
      <environments default="development">
          <!-- 
  			environment 标签用来配置一个环境 
  				id 是环境的标识
  		 -->
  
          <environment id="development">
              <!-- 
  				transactionManager	配置使用什么样类型的数据库事务管理
  					 type="JDBC"  	表示启用事务，有commit和rollback操作
  					 type="MANAGED" 表示不直接控制事务。交给容器处理---几乎不用。
  			 -->
  
              <transactionManager type="JDBC"/>
              	<!-- 
  				dataSource标签配置连接池
  					type="POOLED"	表示启用数据库连接池
  					type="UNPOOLED"	表示不启用数据库连接池
  			 -->
  
              <dataSource type="POOLED">
                  <!-- 连接数据库的驱动类 -->
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <!--数据库访问地址 后面的?解决字符编码报错 不报错可不加-->
                  <property name="url" value="jdbc:mysql://localhost:3306/sys_demo?useUnicode=true&amp;characterEncoding=utf8"/>
                  <!-- 数据库用户名 -->	
                  <property name="username" value="root"/>
                  <!-- 数据库密码 -->
                  <property name="password" value="123"/>
              </dataSource>
          </environment>
      </environments>
      <!-- 导入mapper配置文件 -->
      <mappers>
          <mapper resource="com/fc/pojo/UserMapper.xml"/>
      </mappers>
  </configuration>
  ```

- 执行搜索

  ```java
   @Test
      public void test2() throws Exception{
          InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
          SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
          SqlSession session = sqlSessionFactory.openSession();
  
          try {
              User user = session.selectOne("com.fc.pojo.User.selectUserById",1);
              System.out.println(user);
          }finally {
              session.close();
          }
      }
  ```

  

