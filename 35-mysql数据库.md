# mysql

## 1.数据库的相关概念

- DB

  > 数据库(database) : 存储数据的仓库。保存了一系列有组织的数据

- DBMS

  > 数据库管理系统(Database Management System).数据库是通过DBMS创建和操作的容器\

  - 基于共享文件系统的DBMS(access)
  - 基于客户机-服务端的DBMS(MySQl,Oracle,SqlServer)

- SQL

  > 结构化查询语言(Structure Query Language). 专门用来与数据库通信的语言

## 2. 安装

- 启动/停止服务

  ```shell
  命令行
  net start mysql
  # (服务名安装的时候可以自定义)
  
  net stop mysql
  ```
  
- 服务端的登录

  ```shell
  mysql -h localhost -P 3306 -u root -ppassword
  # h - host
  # P - Port
  # 3306 - 端口号
  # u - user
  # 前三个有无空格都可以
  # p - password 和-p不能有空格
  
  # 简写
  mysql -uroot -ppassword
  exit 
  # 退出
  ```

- 常见命令

  ```shell
  # 先登录
  show databases;
  # 显示当前数据库
  drop database if exists 库名
  # 如果存在这个库删除这个库
  create database 库名 character set utf8
  # 创建数据库 设置字符编码
  use test
  # test(库名) 进入test库
  show tables;
  # 显示表
  show tables from test;
  # 显示test下的表
  select database();
  # 显示当前在哪个库
  select version();
  # 显示当前版本
  mysql --version 
  mysql -V
  # 显示当前版本 要先退出服务
  ```
  
- 语法规范

  - 不区分大小写(建议关键字大写,其他小写)

  - 分号结尾

  - 可以缩进,换行

  - 注释

    ```mysql
    #注释
    -- 注释
    /*多行注释*/
    ```




## 3. 数据类型

### 3.1 整形

- 整形

| 整数类型     | 字节 | 范围     | 范围(无符号) |
| ------------ | ---- | -------- | ------------ |
| Tinyint      | 1    | -128~127 | 0~2^16-1     |
| Smallint     | 2    | ...      | .            |
| Mediumint    | 3    | ...      | .            |
| Int,interger | 4    | ...      | .            |
| Bigint       | 8    | ...      | .            |

```sql
CREATE TABLE demo(
	t1 INT,
    t2 INT UNSIGNED,
    t3 INT(7) ZEROFILL
)
```

- 特点	
  -  默认有符号
  - 超范围报异常 并插入临界值
  - 不设置长度,会设置默认的长度
  - 长度表示最大宽度 不够填充0 必须搭配`ZEROFILL` 使用

### 3.2 小数

- 定点数

  ```sql
  DEC(M,D)
  DECIMAL 
  -- 字节 m+2 范围与double相同
  
  -- DECIMAL默认 M = 10, D = 0
  ```

- 浮点数

  - 单精度浮点：

    ```sql
    float(M,D) -- 字节 4
    ```

  - 双精度浮点： 

    ```sql
    double(M,D) -- 字节8
    ```

- 特点

  - `M` - 整数部分 + 小数部分
  - `D` - 小数部分
  - `M`和`D`都可以省略
  - 超范围插入临界值

### 3.3 字符串

| 分类    | 写法       | 特点     | 空间耗费 | 效率 | M            |
| ------- | ---------- | -------- | -------- | ---- | ------------ |
| char    | char(M)    | 固定长度 | 比较耗费 | 高   | 可省略 默认1 |
| varchar | varchar(M) | 可变长度 | 比较节省 | 低   | 不可省略     |

- set
  - 集合
- enum
  - 枚举
- binary varbinary
  - 用域保存较短的二进制

- text
  - 较长的文本
- blob
  - 较长的二进制数据

### 3.4 日期类型

| 类型      | 字节 | 最小值              | 最大值              |
| --------- | ---- | ------------------- | ------------------- |
| date      | 4    | 1000-01-01          | 9999-12-31          |
| datetime  | 8    | 1000-01-01 00:00:00 | 9999-12-31 23:59:59 |
| timestamp | 4    | 19700101 080001     | 2038年的某个时刻    |
| time      | 3    | -838:59:59          | 838:59:59           |
| year      | 1    | 1901                | 2155                |

## 4. DQL语言学习

> ```mysql
> /*
> SQLyog Ultimate v10.00 Beta1
> MySQL - 5.5.15 : Database - myemployees
> *********************************************************************
> */
> 
> 
> /*!40101 SET NAMES utf8 */;
> 
> /*!40101 SET SQL_MODE=''*/;
> 
> /*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
> /*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
> /*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
> /*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
> CREATE DATABASE /*!32312 IF NOT EXISTS*/`myemployees` /*!40100 DEFAULT CHARACTER SET gb2312 */;
> 
> USE `myemployees`;
> 
> /*Table structure for table `departments` */
> 
> DROP TABLE IF EXISTS `departments`;
> 
> CREATE TABLE `departments` (
>   `department_id` int(4) NOT NULL AUTO_INCREMENT,
>   `department_name` varchar(3) DEFAULT NULL,
>   `manager_id` int(6) DEFAULT NULL,
>   `location_id` int(4) DEFAULT NULL,
>   PRIMARY KEY (`department_id`),
>   KEY `loc_id_fk` (`location_id`),
>   CONSTRAINT `loc_id_fk` FOREIGN KEY (`location_id`) REFERENCES `locations` (`location_id`)
> ) ENGINE=InnoDB AUTO_INCREMENT=271 DEFAULT CHARSET=gb2312;
> 
> /*Data for the table `departments` */
> 
> insert  into `departments`(`department_id`,`department_name`,`manager_id`,`location_id`) values (10,'Adm',200,1700),(20,'Mar',201,1800),(30,'Pur',114,1700),(40,'Hum',203,2400),(50,'Shi',121,1500),(60,'IT',103,1400),(70,'Pub',204,2700),(80,'Sal',145,2500),(90,'Exe',100,1700),(100,'Fin',108,1700),(110,'Acc',205,1700),(120,'Tre',NULL,1700),(130,'Cor',NULL,1700),(140,'Con',NULL,1700),(150,'Sha',NULL,1700),(160,'Ben',NULL,1700),(170,'Man',NULL,1700),(180,'Con',NULL,1700),(190,'Con',NULL,1700),(200,'Ope',NULL,1700),(210,'IT ',NULL,1700),(220,'NOC',NULL,1700),(230,'IT ',NULL,1700),(240,'Gov',NULL,1700),(250,'Ret',NULL,1700),(260,'Rec',NULL,1700),(270,'Pay',NULL,1700);
> 
> /*Table structure for table `employees` */
> 
> DROP TABLE IF EXISTS `employees`;
> 
> CREATE TABLE `employees` (
>   `employee_id` int(6) NOT NULL AUTO_INCREMENT,
>   `first_name` varchar(20) DEFAULT NULL,
>   `last_name` varchar(25) DEFAULT NULL,
>   `email` varchar(25) DEFAULT NULL,
>   `phone_number` varchar(20) DEFAULT NULL,
>   `job_id` varchar(10) DEFAULT NULL,
>   `salary` double(10,2) DEFAULT NULL,
>   `commission_pct` double(4,2) DEFAULT NULL,
>   `manager_id` int(6) DEFAULT NULL,
>   `department_id` int(4) DEFAULT NULL,
>   `hiredate` datetime DEFAULT NULL,
>   PRIMARY KEY (`employee_id`),
>   KEY `dept_id_fk` (`department_id`),
>   KEY `job_id_fk` (`job_id`),
>   CONSTRAINT `dept_id_fk` FOREIGN KEY (`department_id`) REFERENCES `departments` (`department_id`),
>   CONSTRAINT `job_id_fk` FOREIGN KEY (`job_id`) REFERENCES `jobs` (`job_id`)
> ) ENGINE=InnoDB AUTO_INCREMENT=207 DEFAULT CHARSET=gb2312;
> 
> /*Data for the table `employees` */
> 
> insert  into `employees`(`employee_id`,`first_name`,`last_name`,`email`,`phone_number`,`job_id`,`salary`,`commission_pct`,`manager_id`,`department_id`,`hiredate`) values (100,'Steven','K_ing','SKING','515.123.4567','AD_PRES',24000.00,NULL,NULL,90,'1992-04-03 00:00:00'),(101,'Neena','Kochhar','NKOCHHAR','515.123.4568','AD_VP',17000.00,NULL,100,90,'1992-04-03 00:00:00'),(102,'Lex','De Haan','LDEHAAN','515.123.4569','AD_VP',17000.00,NULL,100,90,'1992-04-03 00:00:00'),(103,'Alexander','Hunold','AHUNOLD','590.423.4567','IT_PROG',9000.00,NULL,102,60,'1992-04-03 00:00:00'),(104,'Bruce','Ernst','BERNST','590.423.4568','IT_PROG',6000.00,NULL,103,60,'1992-04-03 00:00:00'),(105,'David','Austin','DAUSTIN','590.423.4569','IT_PROG',4800.00,NULL,103,60,'1998-03-03 00:00:00'),(106,'Valli','Pataballa','VPATABAL','590.423.4560','IT_PROG',4800.00,NULL,103,60,'1998-03-03 00:00:00'),(107,'Diana','Lorentz','DLORENTZ','590.423.5567','IT_PROG',4200.00,NULL,103,60,'1998-03-03 00:00:00'),(108,'Nancy','Greenberg','NGREENBE','515.124.4569','FI_MGR',12000.00,NULL,101,100,'1998-03-03 00:00:00'),(109,'Daniel','Faviet','DFAVIET','515.124.4169','FI_ACCOUNT',9000.00,NULL,108,100,'1998-03-03 00:00:00'),(110,'John','Chen','JCHEN','515.124.4269','FI_ACCOUNT',8200.00,NULL,108,100,'2000-09-09 00:00:00'),(111,'Ismael','Sciarra','ISCIARRA','515.124.4369','FI_ACCOUNT',7700.00,NULL,108,100,'2000-09-09 00:00:00'),(112,'Jose Manuel','Urman','JMURMAN','515.124.4469','FI_ACCOUNT',7800.00,NULL,108,100,'2000-09-09 00:00:00'),(113,'Luis','Popp','LPOPP','515.124.4567','FI_ACCOUNT',6900.00,NULL,108,100,'2000-09-09 00:00:00'),(114,'Den','Raphaely','DRAPHEAL','515.127.4561','PU_MAN',11000.00,NULL,100,30,'2000-09-09 00:00:00'),(115,'Alexander','Khoo','AKHOO','515.127.4562','PU_CLERK',3100.00,NULL,114,30,'2000-09-09 00:00:00'),(116,'Shelli','Baida','SBAIDA','515.127.4563','PU_CLERK',2900.00,NULL,114,30,'2000-09-09 00:00:00'),(117,'Sigal','Tobias','STOBIAS','515.127.4564','PU_CLERK',2800.00,NULL,114,30,'2000-09-09 00:00:00'),(118,'Guy','Himuro','GHIMURO','515.127.4565','PU_CLERK',2600.00,NULL,114,30,'2000-09-09 00:00:00'),(119,'Karen','Colmenares','KCOLMENA','515.127.4566','PU_CLERK',2500.00,NULL,114,30,'2000-09-09 00:00:00'),(120,'Matthew','Weiss','MWEISS','650.123.1234','ST_MAN',8000.00,NULL,100,50,'2004-02-06 00:00:00'),(121,'Adam','Fripp','AFRIPP','650.123.2234','ST_MAN',8200.00,NULL,100,50,'2004-02-06 00:00:00'),(122,'Payam','Kaufling','PKAUFLIN','650.123.3234','ST_MAN',7900.00,NULL,100,50,'2004-02-06 00:00:00'),(123,'Shanta','Vollman','SVOLLMAN','650.123.4234','ST_MAN',6500.00,NULL,100,50,'2004-02-06 00:00:00'),(124,'Kevin','Mourgos','KMOURGOS','650.123.5234','ST_MAN',5800.00,NULL,100,50,'2004-02-06 00:00:00'),(125,'Julia','Nayer','JNAYER','650.124.1214','ST_CLERK',3200.00,NULL,120,50,'2004-02-06 00:00:00'),(126,'Irene','Mikkilineni','IMIKKILI','650.124.1224','ST_CLERK',2700.00,NULL,120,50,'2004-02-06 00:00:00'),(127,'James','Landry','JLANDRY','650.124.1334','ST_CLERK',2400.00,NULL,120,50,'2004-02-06 00:00:00'),(128,'Steven','Markle','SMARKLE','650.124.1434','ST_CLERK',2200.00,NULL,120,50,'2004-02-06 00:00:00'),(129,'Laura','Bissot','LBISSOT','650.124.5234','ST_CLERK',3300.00,NULL,121,50,'2004-02-06 00:00:00'),(130,'Mozhe','Atkinson','MATKINSO','650.124.6234','ST_CLERK',2800.00,NULL,121,50,'2004-02-06 00:00:00'),(131,'James','Marlow','JAMRLOW','650.124.7234','ST_CLERK',2500.00,NULL,121,50,'2004-02-06 00:00:00'),(132,'TJ','Olson','TJOLSON','650.124.8234','ST_CLERK',2100.00,NULL,121,50,'2004-02-06 00:00:00'),(133,'Jason','Mallin','JMALLIN','650.127.1934','ST_CLERK',3300.00,NULL,122,50,'2004-02-06 00:00:00'),(134,'Michael','Rogers','MROGERS','650.127.1834','ST_CLERK',2900.00,NULL,122,50,'2002-12-23 00:00:00'),(135,'Ki','Gee','KGEE','650.127.1734','ST_CLERK',2400.00,NULL,122,50,'2002-12-23 00:00:00'),(136,'Hazel','Philtanker','HPHILTAN','650.127.1634','ST_CLERK',2200.00,NULL,122,50,'2002-12-23 00:00:00'),(137,'Renske','Ladwig','RLADWIG','650.121.1234','ST_CLERK',3600.00,NULL,123,50,'2002-12-23 00:00:00'),(138,'Stephen','Stiles','SSTILES','650.121.2034','ST_CLERK',3200.00,NULL,123,50,'2002-12-23 00:00:00'),(139,'John','Seo','JSEO','650.121.2019','ST_CLERK',2700.00,NULL,123,50,'2002-12-23 00:00:00'),(140,'Joshua','Patel','JPATEL','650.121.1834','ST_CLERK',2500.00,NULL,123,50,'2002-12-23 00:00:00'),(141,'Trenna','Rajs','TRAJS','650.121.8009','ST_CLERK',3500.00,NULL,124,50,'2002-12-23 00:00:00'),(142,'Curtis','Davies','CDAVIES','650.121.2994','ST_CLERK',3100.00,NULL,124,50,'2002-12-23 00:00:00'),(143,'Randall','Matos','RMATOS','650.121.2874','ST_CLERK',2600.00,NULL,124,50,'2002-12-23 00:00:00'),(144,'Peter','Vargas','PVARGAS','650.121.2004','ST_CLERK',2500.00,NULL,124,50,'2002-12-23 00:00:00'),(145,'John','Russell','JRUSSEL','011.44.1344.429268','SA_MAN',14000.00,0.40,100,80,'2002-12-23 00:00:00'),(146,'Karen','Partners','KPARTNER','011.44.1344.467268','SA_MAN',13500.00,0.30,100,80,'2002-12-23 00:00:00'),(147,'Alberto','Errazuriz','AERRAZUR','011.44.1344.429278','SA_MAN',12000.00,0.30,100,80,'2002-12-23 00:00:00'),(148,'Gerald','Cambrault','GCAMBRAU','011.44.1344.619268','SA_MAN',11000.00,0.30,100,80,'2002-12-23 00:00:00'),(149,'Eleni','Zlotkey','EZLOTKEY','011.44.1344.429018','SA_MAN',10500.00,0.20,100,80,'2002-12-23 00:00:00'),(150,'Peter','Tucker','PTUCKER','011.44.1344.129268','SA_REP',10000.00,0.30,145,80,'2014-03-05 00:00:00'),(151,'David','Bernstein','DBERNSTE','011.44.1344.345268','SA_REP',9500.00,0.25,145,80,'2014-03-05 00:00:00'),(152,'Peter','Hall','PHALL','011.44.1344.478968','SA_REP',9000.00,0.25,145,80,'2014-03-05 00:00:00'),(153,'Christopher','Olsen','COLSEN','011.44.1344.498718','SA_REP',8000.00,0.20,145,80,'2014-03-05 00:00:00'),(154,'Nanette','Cambrault','NCAMBRAU','011.44.1344.987668','SA_REP',7500.00,0.20,145,80,'2014-03-05 00:00:00'),(155,'Oliver','Tuvault','OTUVAULT','011.44.1344.486508','SA_REP',7000.00,0.15,145,80,'2014-03-05 00:00:00'),(156,'Janette','K_ing','JKING','011.44.1345.429268','SA_REP',10000.00,0.35,146,80,'2014-03-05 00:00:00'),(157,'Patrick','Sully','PSULLY','011.44.1345.929268','SA_REP',9500.00,0.35,146,80,'2014-03-05 00:00:00'),(158,'Allan','McEwen','AMCEWEN','011.44.1345.829268','SA_REP',9000.00,0.35,146,80,'2014-03-05 00:00:00'),(159,'Lindsey','Smith','LSMITH','011.44.1345.729268','SA_REP',8000.00,0.30,146,80,'2014-03-05 00:00:00'),(160,'Louise','Doran','LDORAN','011.44.1345.629268','SA_REP',7500.00,0.30,146,80,'2014-03-05 00:00:00'),(161,'Sarath','Sewall','SSEWALL','011.44.1345.529268','SA_REP',7000.00,0.25,146,80,'2014-03-05 00:00:00'),(162,'Clara','Vishney','CVISHNEY','011.44.1346.129268','SA_REP',10500.00,0.25,147,80,'2014-03-05 00:00:00'),(163,'Danielle','Greene','DGREENE','011.44.1346.229268','SA_REP',9500.00,0.15,147,80,'2014-03-05 00:00:00'),(164,'Mattea','Marvins','MMARVINS','011.44.1346.329268','SA_REP',7200.00,0.10,147,80,'2014-03-05 00:00:00'),(165,'David','Lee','DLEE','011.44.1346.529268','SA_REP',6800.00,0.10,147,80,'2014-03-05 00:00:00'),(166,'Sundar','Ande','SANDE','011.44.1346.629268','SA_REP',6400.00,0.10,147,80,'2014-03-05 00:00:00'),(167,'Amit','Banda','ABANDA','011.44.1346.729268','SA_REP',6200.00,0.10,147,80,'2014-03-05 00:00:00'),(168,'Lisa','Ozer','LOZER','011.44.1343.929268','SA_REP',11500.00,0.25,148,80,'2014-03-05 00:00:00'),(169,'Harrison','Bloom','HBLOOM','011.44.1343.829268','SA_REP',10000.00,0.20,148,80,'2014-03-05 00:00:00'),(170,'Tayler','Fox','TFOX','011.44.1343.729268','SA_REP',9600.00,0.20,148,80,'2014-03-05 00:00:00'),(171,'William','Smith','WSMITH','011.44.1343.629268','SA_REP',7400.00,0.15,148,80,'2014-03-05 00:00:00'),(172,'Elizabeth','Bates','EBATES','011.44.1343.529268','SA_REP',7300.00,0.15,148,80,'2014-03-05 00:00:00'),(173,'Sundita','Kumar','SKUMAR','011.44.1343.329268','SA_REP',6100.00,0.10,148,80,'2014-03-05 00:00:00'),(174,'Ellen','Abel','EABEL','011.44.1644.429267','SA_REP',11000.00,0.30,149,80,'2014-03-05 00:00:00'),(175,'Alyssa','Hutton','AHUTTON','011.44.1644.429266','SA_REP',8800.00,0.25,149,80,'2014-03-05 00:00:00'),(176,'Jonathon','Taylor','JTAYLOR','011.44.1644.429265','SA_REP',8600.00,0.20,149,80,'2014-03-05 00:00:00'),(177,'Jack','Livingston','JLIVINGS','011.44.1644.429264','SA_REP',8400.00,0.20,149,80,'2014-03-05 00:00:00'),(178,'Kimberely','Grant','KGRANT','011.44.1644.429263','SA_REP',7000.00,0.15,149,NULL,'2014-03-05 00:00:00'),(179,'Charles','Johnson','CJOHNSON','011.44.1644.429262','SA_REP',6200.00,0.10,149,80,'2014-03-05 00:00:00'),(180,'Winston','Taylor','WTAYLOR','650.507.9876','SH_CLERK',3200.00,NULL,120,50,'2014-03-05 00:00:00'),(181,'Jean','Fleaur','JFLEAUR','650.507.9877','SH_CLERK',3100.00,NULL,120,50,'2014-03-05 00:00:00'),(182,'Martha','Sullivan','MSULLIVA','650.507.9878','SH_CLERK',2500.00,NULL,120,50,'2014-03-05 00:00:00'),(183,'Girard','Geoni','GGEONI','650.507.9879','SH_CLERK',2800.00,NULL,120,50,'2014-03-05 00:00:00'),(184,'Nandita','Sarchand','NSARCHAN','650.509.1876','SH_CLERK',4200.00,NULL,121,50,'2014-03-05 00:00:00'),(185,'Alexis','Bull','ABULL','650.509.2876','SH_CLERK',4100.00,NULL,121,50,'2014-03-05 00:00:00'),(186,'Julia','Dellinger','JDELLING','650.509.3876','SH_CLERK',3400.00,NULL,121,50,'2014-03-05 00:00:00'),(187,'Anthony','Cabrio','ACABRIO','650.509.4876','SH_CLERK',3000.00,NULL,121,50,'2014-03-05 00:00:00'),(188,'Kelly','Chung','KCHUNG','650.505.1876','SH_CLERK',3800.00,NULL,122,50,'2014-03-05 00:00:00'),(189,'Jennifer','Dilly','JDILLY','650.505.2876','SH_CLERK',3600.00,NULL,122,50,'2014-03-05 00:00:00'),(190,'Timothy','Gates','TGATES','650.505.3876','SH_CLERK',2900.00,NULL,122,50,'2014-03-05 00:00:00'),(191,'Randall','Perkins','RPERKINS','650.505.4876','SH_CLERK',2500.00,NULL,122,50,'2014-03-05 00:00:00'),(192,'Sarah','Bell','SBELL','650.501.1876','SH_CLERK',4000.00,NULL,123,50,'2014-03-05 00:00:00'),(193,'Britney','Everett','BEVERETT','650.501.2876','SH_CLERK',3900.00,NULL,123,50,'2014-03-05 00:00:00'),(194,'Samuel','McCain','SMCCAIN','650.501.3876','SH_CLERK',3200.00,NULL,123,50,'2014-03-05 00:00:00'),(195,'Vance','Jones','VJONES','650.501.4876','SH_CLERK',2800.00,NULL,123,50,'2014-03-05 00:00:00'),(196,'Alana','Walsh','AWALSH','650.507.9811','SH_CLERK',3100.00,NULL,124,50,'2014-03-05 00:00:00'),(197,'Kevin','Feeney','KFEENEY','650.507.9822','SH_CLERK',3000.00,NULL,124,50,'2014-03-05 00:00:00'),(198,'Donald','OConnell','DOCONNEL','650.507.9833','SH_CLERK',2600.00,NULL,124,50,'2014-03-05 00:00:00'),(199,'Douglas','Grant','DGRANT','650.507.9844','SH_CLERK',2600.00,NULL,124,50,'2014-03-05 00:00:00'),(200,'Jennifer','Whalen','JWHALEN','515.123.4444','AD_ASST',4400.00,NULL,101,10,'2016-03-03 00:00:00'),(201,'Michael','Hartstein','MHARTSTE','515.123.5555','MK_MAN',13000.00,NULL,100,20,'2016-03-03 00:00:00'),(202,'Pat','Fay','PFAY','603.123.6666','MK_REP',6000.00,NULL,201,20,'2016-03-03 00:00:00'),(203,'Susan','Mavris','SMAVRIS','515.123.7777','HR_REP',6500.00,NULL,101,40,'2016-03-03 00:00:00'),(204,'Hermann','Baer','HBAER','515.123.8888','PR_REP',10000.00,NULL,101,70,'2016-03-03 00:00:00'),(205,'Shelley','Higgins','SHIGGINS','515.123.8080','AC_MGR',12000.00,NULL,101,110,'2016-03-03 00:00:00'),(206,'William','Gietz','WGIETZ','515.123.8181','AC_ACCOUNT',8300.00,NULL,205,110,'2016-03-03 00:00:00');
> 
> /*Table structure for table `jobs` */
> 
> DROP TABLE IF EXISTS `jobs`;
> 
> CREATE TABLE `jobs` (
>   `job_id` varchar(10) NOT NULL,
>   `job_title` varchar(35) DEFAULT NULL,
>   `min_salary` int(6) DEFAULT NULL,
>   `max_salary` int(6) DEFAULT NULL,
>   PRIMARY KEY (`job_id`)
> ) ENGINE=InnoDB DEFAULT CHARSET=gb2312;
> 
> /*Data for the table `jobs` */
> 
> insert  into `jobs`(`job_id`,`job_title`,`min_salary`,`max_salary`) values ('AC_ACCOUNT','Public Accountant',4200,9000),('AC_MGR','Accounting Manager',8200,16000),('AD_ASST','Administration Assistant',3000,6000),('AD_PRES','President',20000,40000),('AD_VP','Administration Vice President',15000,30000),('FI_ACCOUNT','Accountant',4200,9000),('FI_MGR','Finance Manager',8200,16000),('HR_REP','Human Resources Representative',4000,9000),('IT_PROG','Programmer',4000,10000),('MK_MAN','Marketing Manager',9000,15000),('MK_REP','Marketing Representative',4000,9000),('PR_REP','Public Relations Representative',4500,10500),('PU_CLERK','Purchasing Clerk',2500,5500),('PU_MAN','Purchasing Manager',8000,15000),('SA_MAN','Sales Manager',10000,20000),('SA_REP','Sales Representative',6000,12000),('SH_CLERK','Shipping Clerk',2500,5500),('ST_CLERK','Stock Clerk',2000,5000),('ST_MAN','Stock Manager',5500,8500);
> 
> /*Table structure for table `locations` */
> 
> DROP TABLE IF EXISTS `locations`;
> 
> CREATE TABLE `locations` (
>   `location_id` int(11) NOT NULL AUTO_INCREMENT,
>   `street_address` varchar(40) DEFAULT NULL,
>   `postal_code` varchar(12) DEFAULT NULL,
>   `city` varchar(30) DEFAULT NULL,
>   `state_province` varchar(25) DEFAULT NULL,
>   `country_id` varchar(2) DEFAULT NULL,
>   PRIMARY KEY (`location_id`)
> ) ENGINE=InnoDB AUTO_INCREMENT=3201 DEFAULT CHARSET=gb2312;
> 
> /*Data for the table `locations` */
> 
> insert  into `locations`(`location_id`,`street_address`,`postal_code`,`city`,`state_province`,`country_id`) values (1000,'1297 Via Cola di Rie','00989','Roma',NULL,'IT'),(1100,'93091 Calle della Testa','10934','Venice',NULL,'IT'),(1200,'2017 Shinjuku-ku','1689','Tokyo','Tokyo Prefecture','JP'),(1300,'9450 Kamiya-cho','6823','Hiroshima',NULL,'JP'),(1400,'2014 Jabberwocky Rd','26192','Southlake','Texas','US'),(1500,'2011 Interiors Blvd','99236','South San Francisco','California','US'),(1600,'2007 Zagora St','50090','South Brunswick','New Jersey','US'),(1700,'2004 Charade Rd','98199','Seattle','Washington','US'),(1800,'147 Spadina Ave','M5V 2L7','Toronto','Ontario','CA'),(1900,'6092 Boxwood St','YSW 9T2','Whitehorse','Yukon','CA'),(2000,'40-5-12 Laogianggen','190518','Beijing',NULL,'CN'),(2100,'1298 Vileparle (E)','490231','Bombay','Maharashtra','IN'),(2200,'12-98 Victoria Street','2901','Sydney','New South Wales','AU'),(2300,'198 Clementi North','540198','Singapore',NULL,'SG'),(2400,'8204 Arthur St',NULL,'London',NULL,'UK'),(2500,'Magdalen Centre, The Oxford Science Park','OX9 9ZB','Oxford','Oxford','UK'),(2600,'9702 Chester Road','09629850293','Stretford','Manchester','UK'),(2700,'Schwanthalerstr. 7031','80925','Munich','Bavaria','DE'),(2800,'Rua Frei Caneca 1360 ','01307-002','Sao Paulo','Sao Paulo','BR'),(2900,'20 Rue des Corps-Saints','1730','Geneva','Geneve','CH'),(3000,'Murtenstrasse 921','3095','Bern','BE','CH'),(3100,'Pieter Breughelstraat 837','3029SK','Utrecht','Utrecht','NL'),(3200,'Mariano Escobedo 9991','11932','Mexico City','Distrito Federal,','MX');
> 
> /*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
> /*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
> /*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
> /*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
> 
> ```
>
> 

### 4.1 基础查询

- 语法

  ```sql
  select 查询列表 from 表名
  ```

- 特点

  1. 查询列表可以是 表中的字段 常量值 表达式 函数
  2. 查询的结果式一个虚拟的表格

- 查询单个字段

  ```sql
  SELECT
  	last_name
  FROM
  	employees;
  ```

  

- 查询多个字段

  ```mysql
  SELECT
  	last_name,
  	salary,
  	email
  FROM
  	employees;
  # 关键字和字段重复可以用反引号`NAME`包裹
  ```

- 查询所有

  ```sql
  SELECT
  	* 
  FROM
  	employees;
  ```

- 查询常量

  ```mysql
  SELECT 100;
  SELECT 'john';
  ```

- 查询表达式

  ```mysql
  SELECT 100*98;
  ```

- 查询函数

  ```mysql
  SELECT VERSION();
  ```

- 别名

  > 区分重复
  >
  > 便于理解
  >
  > 别名要有空格加""

  ```mysql
  SELECT
  	last_name AS 结果,
  	salary AS 薪水,
  	email AS 邮箱 
  FROM
  	employees;
  # 方式2
  SELECT
  	last_name 结果,
  	salary 薪水,
  	email 邮箱 
  FROM
  	employees;
  ```

- 去重

  ```mysql
  SELECT DISTINCT
  	department_id 
  FROM
  	employees;
  ```

- `+`的作用

  

  ```mysql
  /*
  mysql中的sql仅有一个运算功能
  
  '123' + 23 会试图将字符串转换伪数字再进行运算
  'john' + 23 转换失败会转换为0
  null + 23 只要有一个为null 结果为null
  */
  # 实例查询名和姓
  
  # 错误查询
  /* SELECT
  	last_name + first_name AS 姓名 
  FROM
  	employees; */
  
  SELECT
  	CONCAT( last_name,' ', first_name ) AS 姓名 
  FROM 
  	employees;
  ```

- `IFNULL`函数

  > 第一个参数 字段 
  >
  > 第二个参数 为null时显示的内容

  ```sql
  SELECT
  	IFNULL(commission_pct,'暂无奖金') AS 奖金率 
  FROM
  	employees;
  ```



### 4.2 条件查询

1. 按条件表达式筛选

   ```mysql
   # 条件运算符号
   =  >  >=  <  <=  !=  <> 不等于
   ```

   ```mysql
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	salary > 12000;
   #-------------
   
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	department_id <> 90;
   ```

   

2. 按逻辑表达式筛选

   ```mysql
   && || ! and or not
   ```

   ```mysql
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	department_id <> 90 
   	AND salary > 10000;
   ```

   

3. 模糊查询

   ```mysql
   like 
   between and
   in
   is null
   is not null
   <=> -- 安全等于
   # 一般和通配符搭配使用
   # % 任意多个(包括0)通配符
   # _ 单个任意字符
   # \ 转义 
   # ESCAPE 转义的另一种写法
   ```

   ```mysql
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	first_name LIKE 'a_\_a%';
   	
   -- ------
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	last_name LIKE '_$_%' ESCAPE '$';
   -- ---------------------
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	employee_id BETWEEN 100 
   	AND 120;
   -- ---------------------------
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	job_id IN ( 'AD_PRES', "IT_PROG", 'FI_ACCOUNT' );
   -- IN 对应 = 不能写通配符
   -- -------------------------
   SELECT
   	* 
   FROM
   	`employees` 
   WHERE
   	commission_pct IS NULL;
   -- 不能写 = null 可以 <=> null
   -- <null> 既可以判断null 也可以判断普通的数值
   ```
### 4.3 排序查询

- 普通排序

  ```sql
  SELECT
  	* 
  FROM
  	`employees` 
  ORDER BY
  	salary DESC;
  -- desc 降序
  -- asc 升序（默认）
  -- order by一般放在语句的最后面 (limit子句除外)
  ```

- 按表达式排序

  ```sql
  SELECT
  	*,
  	salary * 12 * ( 1 + IFNULL( commission_pct, 0 ) ) 年薪 
  FROM
  	employees 
  ORDER BY
  	salary * 12 * ( 1 + IFNULL( commission_pct, 0 ) ) DESC;
  
  ```

- 按别名排序

  ```sql
  SELECT
  	*,
  	salary * 12 * ( 1 + IFNULL( commission_pct, 0 ) ) 年薪 
  FROM
  	employees 
  ORDER BY
  	年薪 DESC;
  ```

- 按函数排序

  ```sql
  -- 案例: 按姓名长度排序
  SELECT
  	*,
  	LENGTH( last_name ) 字节长度 
  FROM
  	employees 
  ORDER BY
  	字节长度 DESC;
  ```

- 多个字段

  ```sql
  SELECT
  	* 
  FROM
  	employees
  ORDER BY
  	salary,
  	employee_id DESC;
  ```

### 4.4 常见函数

- 单行函数

  - 字符函数

    ```sql
    -- length
    select 
    	LENGTH("join"); -- 4
    select 
    	LENGTH("张三"); -- 6 一个汉字三个字节(utf8) 和字符集有关
    
    -- concat
    SELECT
    	CONCAT( first_name, "_", last_name ) 'name' 
    FROM
    	employees;
    
    -- upper/lower
    SELECT 
    	UPPER("str"); -- STR
    
    -- substr, substring
    SELECT 
    	SUBSTR("sql中索引从1开始",4); -- 中索引从1开始
    SELECT 
    	SUBSTR("三个参时第三个参是长度",1,3); -- 三个参 
    	
    -- instr
    SELECT
    	INSTR('aabcaabc','aa'); -- 1 首次出现的索引 不存在返回0
    	
    -- trim
    SELECT
    	TRIM("   aaa   "); -- 'aaa'
    	
    SELECT
    	trim( 'a' FROM 'aaaabbbaaabbbaaa' ); -- bbbaaabbb
    	
    	
    -- lpad/rpad(左右填充)
    SELECT
    	lpad("aa",10,'bnb'); -- bnbbnbbnaa
    	
    -- replace
    SELECT
    	REPLACE("string",'ing',''); -- str
    
    ```

  - 数字函数

    ```sql
    -- round
    SELECT
    	ROUND(1.90); -- 2 四舍五入
    
    SELECT
    	ROUND(1.90,1); -- 1.9 第二个参表示保留几位
    	
    -- ceil/floor 向上/下取整
    
    -- truncate 小数点后保留几位
    SELECT
    	TRUNCATE(1.65,1); -- 1.6
    	
    -- mod
    SELECT
    	mod(1.65,1); -- 0.65
    ```

  - 日期函数

    ```sql
    -- now 
    SELECT
    	now( ); -- 2021-03-20 21:31:18
    	
    -- current_date/curdate
    SELECT
    	CURRENT_DATE( ); -- 2021-03-20
    	
    -- current_time/curtime
    SELECT
    	current_time( ); -- 21:33:10
    
    -- year/month/monthname.....
    SELECT YEAR
    	( now( ) ); -- 2021
    		
    -- str_to_date/date_format
    SELECT
    	STR_TO_DATE( '1919-01-01', '%Y-%m-%d' ); -- 1919-01-01
    	
    SELECT
    	DATE_FORMAT( '2018/6/6', '%Y年%m月%d日' ); -- 2018年06月06日
    ```

    | 序号 | 格式 | 功能             |
    | ---- | ---- | ---------------- |
    | 1    | %Y   | 四位的月份       |
    | 2    | %y   | 两位的年份       |
    | 3    | %m   | 月份(01,02)      |
    | 4    | %c   | 月份(1,2)        |
    | 5    | %d   | 日（01，02）     |
    | 6    | %H   | 小时（24小时制） |
    | 7    | %h   | 小时（12小时制） |
    | 8    | %i   | 分钟（00，01）   |
    | 9    | %s   | 秒（00，01）     |

    

  - 其他函数

    ```sql
    -- version
    SELECT
    	VERSION(); -- 8.0.21
    	
    -- databases
    SELECT DATABASE
    	( ); -- myemployees
    	
    -- user
    SELECT USER
    	( ); -- root@localhost
    
    ```

    

  - 流程控制函数

    ```sql
    -- if
    
    SELECT
    IF
    	( 10 > 5, "大", "小" );
    	
    /*
    case 要判断的字段或者表达式
    when 常量1 then 要显示的值1或语句1;
    when 常量2 then 要显示的值2或语句2;
    ...
    else 要显示的值n或语句n;
    end
    */
    -- 值不用加分号 语句加分号
    
    SELECT
    	salary 旧工资,
    	department_id,
    CASE
    	department_id 
    	WHEN 30 THEN
    	salary * 1.3 
    	WHEN 40 THEN
    	salary * 1.2 
    	ELSE 
    	salary 
    	END AS 最终工资 
    FROM
    	employees;
    	
    -- 用法2
    /*
    case
    when 常量1 then 要显示的值1或语句1;
    when 常量2 then 要显示的值2或语句2;
    ...
    else 要显示的值n或语句n;
    end
    */
    
    SELECT
    	salary 旧工资,
    CASE
    	
    	WHEN salary > 20000 THEN
    	'A' 
    	WHEN salary > 15000 THEN
    	'b' 
    	WHEN salary > 10000 THEN
    	'c' ELSE 'd' 
    	END AS 最终工资 
    FROM
    employees;
    ```

    


### 4.5 分组函数

> 统计使用 又称聚合函数挥着统计函数或者组函数

```sql
-- sum 忽略null
SELECT
	SUM( salary ) 
FROM
	employees;
	
-- avg
SELECT
	AVG( salary ) 
FROM
	employees;
	
-- max 支持字符 日期
SELECT
	MAX( salary ) 
FROM
	employees;
	
-- min
SELECT
	MIN( salary ) 
FROM
	employees;
	
-- count 非空个数
SELECT
	COUNT( * ) 
FROM
	employees; -- 统计总行数
	
-- 和distinct搭配使用 去重

-- 和分组函数一同查询的字段要求是group by后的字段
```

### 4.6 分组查询

- 语法

  ```sql
  select columns,group_function(column)
  from table
  [where condition]
  [group by group_by_expression]
  [order by column]
  ```

  > group by 支持单个字段、 多个字段、 表达式或函数分组

- 案例

  ```sql
  -- 案例一
  SELECT
  	max( salary ),
  	job_id 
  FROM
  	employees 
  GROUP BY
  	job_id;
  -- 案例二
  SELECT
  	count(*),
  	location_id 
  FROM
  	departments 
  GROUP BY
  	location_id;
  
  
  ```

- 有筛选条件的分组

  ```sql
  -- 添加筛选条件
  -- 案例一
  SELECT
  	avg( salary ),
  	department_id
  FROM
  	employees 
  WHERE
  	email LIKE '%A%' 
  GROUP BY
  	department_id;
  -- 案例二
  SELECT
  	max( salary ),
  	manager_id 
  FROM
  	employees 
  WHERE
  	commission_pct IS NOT NULL 
  GROUP BY
  	manager_id
  	
  -- HAVING 复杂筛选条件
  
  -- 案例1 ：查询哪个部门员工个数大于2
  SELECT
  	count( * ) 部门员工数,
  	department_id 部门 id 
  FROM
  	employees 
  GROUP BY
  	department_id 
  HAVING
  	COUNT( * ) > 2;
  
  -- 案例2 查询每个工种有奖金且最高工资大于12000的工种编号
  SELECT
  	max( salary ),
  	job_id 
  FROM
  	employees 
  WHERE
  	commission_pct IS NOT NULL 
  GROUP BY
  	job_id 
  HAVING
  	max(salary) > 12000;
  	
  -- 案例3 查询领导编号>102的 手下最低工资>5000的领导编号,及最低工资
  SELECT
  	min( salary ),
  	manager_id 
  FROM
  	employees 
  WHERE
  	manager_id > 102 
  GROUP BY
  	manager_id 
  HAVING
  	min( salary ) > 5000
  ```

  - `having`和 `where` 的区别

    - 分组函数做条件肯定是放在`having`子句中
    - 能用分组前筛选的优先使用分组前筛选

    |          | having         | where  |
    | -------- | -------------- | ------ |
    | 筛选时机 | 分组后         | 分组前 |
    | 数据源   | 分组后的结果集 | 原始表 |

- 按表达式或者函数分组

  ```sql
  -- 按员工姓名长度分组 查询每一组的员工个数,筛选员工个数大于5的
  SELECT
  	count( * ) 员工个数个数,
  	length( last_name ) 姓名长度 
  FROM
  	employees 
  GROUP BY
  	姓名长度
  HAVING
  	员工个数个数 > 5
  ```

- 按多个字段分组

  ```sql
  -- 查询每个部门每个工种的员工的平均工资
  SELECT
  	avg( salary ) 平均工资,
  	department_id 部门,
  	job_id 工种 
  FROM
  	employees 
  GROUP BY
  	部门,
  	工种
  ```

- 添加排序

  ```sql
  SELECT
  	avg( salary ) 平均工资,
  	department_id 部门,
  	job_id 工种 
  FROM
  	employees 
  WHERE
  	部门 IS NOT NULL 
  GROUP BY
  	部门,
  	工种 
  HAVING
  	平均工资 > 10000 
  ORDER BY
  	平均工资
  ```

  

### 4.7 连接查询

> 笛卡尔乘积现象
>
> 表1 m行 表2n行 结果m*n行
>
> - 原因： 没有有效的连接条件
> - 避免： 添加连接条件
>   - 按年代分类
>     - sql92：仅支持内连接
>     - **sql99**：支持内外(左+右)连接 + 交叉
>   - 按功能分类
>     - 内连接
>       - 等值连接
>       - 非等值连接
>       - 自连接
>     - 外连接
>       - 左外连接
>       - 右外连接
>       - 全外连接
>     - 交叉连接

- ~~sql92连接~~

  1. 等值连接

     > - 结果为多表的交集部分
     > - n表连接 至少需要n-1个连接条件
     > - 多表顺序没有要求
     > - 一般要为表起别名
     > - 可以搭配前面所有子句使用

     ```sql
     -- 查询女生名和对应的男生名
     SELECT
     	girl_name,
     	boy_name 
     FROM
     	boys,
     	beauty 
     WHERE
     	beauty.bf_id = boys.id;
     	
     -- 查询员工对应的部门名
     SELECT
     	CONCAT( first_name, ' ', last_name ) 员工,
     	department_name 部门 
     FROM
     	employees,
     	departments 
     WHERE
     	employees.department_id = departments.department_id;
     
     -- 对表起别名
     -- **如果起了表名 查询的字段就不能使用原来的表名**
     SELECT
     	CONCAT( first_name, ' ', last_name ) 员工,
     	b.job_title 工作 
     FROM
     	employees a,
     	jobs b 
     WHERE
     	a.job_id = b.job_id;
     	
     -- 添加筛选
     SELECT
     	CONCAT( first_name, ' ', last_name ) 员工,
     	job_title 工作 
     FROM
     	employees a,
     	jobs b 
     WHERE
     	a.job_id = b.job_id 
     	AND a.commission_pct IS NOT NULL
     
     -- 添加分组
     -- 查询每个城市的部门个数
     SELECT
     	city 城市,
     	COUNT( * ) 
     FROM
     	locations l,
     	departments d 
     WHERE
     	d.location_id = l.location_id 
     GROUP BY
     	city;
     	
     -- 案例
     SELECT
     	department_name,
     	d.manager_id,
     	min( salary ) 
     FROM
     	departments d,
     	employees e 
     WHERE
     	d.department_id = e.department_id 
     	AND e.commission_pct IS NOT NULL 
     GROUP BY
     	department_name,
     	manager_id;
     	
     -- 三表
     SELECT
     	first_name e_name,
     	department_name,
     	city 
     FROM
     	employees e,
     	departments d,
     	locations l 
     WHERE
     	e.department_id = d.department_id 
     	AND d.location_id = l.location_id;
     ```

  2. 非等值连接

     ```sql
     SELECT
     	salary,
     	grade_level 
     FROM
     	employees e,
     	job_grades j 
     WHERE
     	e.salary BETWEEN j.lowest_sal 
     	AND j.highest_sal;
     ```

  3. 自连接

     ```sql
     SELECT
     	e.employee_id,
     	e.last_name,
     	m.employee_id,
     	m.last_name 
     FROM
     	employees e,
     	employees m 
     WHERE
     	e.manager_id = m.employee_id;
     ```

- sql99连接

  - 语法

    ```sql
    select 查询列表
    from 表1 别名 
    [连接类型] join 表2 别名 
    on 连接条件
    [where 筛选条件]
    [group by 分组]
    [having 筛选条件]
    [order by 排序列表]
    ```

    

  1. 内连接 `inner`

     > - `inner`可以省略
     > - 筛选条件放在`where`后面,连接条件放在`on`后面 提高分离性 
     > - 可以添加排序分组筛选

     1. 等值连接

        > `inner join`和`sql92`的等值连接效果一样

        ```mysql
        SELECT
        	last_name,
        	department_name 
        FROM
        	employees e
        	INNER JOIN departments d ON e.department_id = d.department_id;
        ```

        

     2. 非等值连接

     3. 自连接

  2. 外连接 `outer`

     > 外连接的查询结果为主表中的所有记录
     >
     > 如果从表中没有和它匹配的,显示为`null`
     >
     > 外连接的查询结果 = 内连接结果 + 主表右从表没有的记录
     >
     > 1. left   -> left左边的时主表
     > 2. right - > right右边的是
     > 3. 左外和右外交换两个表的顺序 可以实现同样的效果

     1. 左外 `left`

        ```mysql
        -- 查询没有员工的部门
        SELECT
        	d.*,
        	employee_id 
        FROM
        	departments d
        	LEFT OUTER JOIN employees e ON d.department_id = e.department_id 
        WHERE
        	e.employee_id IS NULL;
        ```

     2. 右外 `right`

        ```mysql
        SELECT
        	d.*,
        	employee_id 
        FROM
        	employees e
        	RIGHT OUTER JOIN departments d ON d.department_id = e.department_id 
        WHERE
        	e.employee_id IS NULL;
        ```

     3. 全外 `full`

        > `mysql` **不支持**全外
        >
        > 查询结果 = 表一有 表二没有 + 表一没有表二有

  3. 交叉连接 `cross`

     > 结果为笛卡尔乘积

     ```mysql
     SELECT
     	a.*,
     	b.* 
     FROM
     	table_a a
     	CROSS JOIN table_b b;
     ```

### 4.8 子查询

- 含义:

  ​	出现在其他语句的select语句 称为子查询或内查询

  ​	外部的查询语句 称为主查询

- 分类
  - 按子查询出现的位置
    - `select`后面 (仅支持标量子查询)
    - `from`后面(支持表子查询)
    - **`where`或`having`后面**(**标量子查询/ 列子查询**/~~行子查询~~)
    - `exists`或`not exists`后面(又名相关子查询 ,支持表子查询)
  - 按结果集的行列数不同
    - 标量子查询(结果集只有一行一列)
    - 列子查询(又名多行子查询,结果为一列多行)
    - 行子查询(结果多列一(多)行)
    - 表子查询(结果集一般为多行多列)

- `where`或`having`后面

  > - 子查询放在小括号内
  > - 子查询一般放在条件的右侧
  > - 子查询优先于主查询

  - 标量子查询(单行子查询)

    > 标量子查询 一般搭配使用单行操作符(>, <, !=, =)

    ```sql
    -- 案例1
    SELECT
    	last_name 
    FROM
    	employees 
    WHERE
    	salary > ( SELECT salary FROM employees WHERE last_name = 'ABEL' );
    	
    -- 案例2
    SELECT
    	* 
    FROM
    	employees 
    WHERE
    	job_id = ( SELECT job_id FROM employees WHERE employee_id = 141 ) 
    	AND salary > ( SELECT salary FROM employees WHERE employee_id = 143 );
    ```

  - 列子查询

    > 列子查询 一般搭配多行子查询使用(in,any,some,all)
    >
    > | 操作符      | 含义                                                   |
    > | ----------- | ------------------------------------------------------ |
    > | IN/ NOT IN  | 等于列表中的**任意一个**                               |
    > | ANY \| SOME | 和子查询返回的**某一个**值比较(可以使用min,max等代替 ) |
    > | ALL         | 和子查询返回的**所有值**作比较                         |

    ```sql
    SELECT
    	last_name 
  FROM
    	employees 
  WHERE
    	department_id IN ( SELECT DISTINCT department_id FROM departments WHERE location_id IN ( 1400, 1700 ) );
  	
    	
    -- 案例
    SELECT
    	employee_id,
    	last_name,
    	job_id,
    	salary 
    FROM
    	employees 
    WHERE
    	department_id != ( SELECT DISTINCT department_id FROM employees WHERE job_id = 'IT_PROG' ) 
    	AND salary < ANY ( SELECT DISTINCT salary FROM employees WHERE job_id = 'IT_PROG' );
    ```
  
  - 行子查询
  
    ```mysql
    SELECT
    	* 
    FROM
    	employees 
    WHERE
    	( employee_id, salary ) = ( SELECT MIN( employee_id ), MAX( salary ) FROM employees )
    ```
  
- `select`后面的(仅支持标量子查询)

  ```mysql
  SELECT
  	d.*,
  	( SELECT COUNT( * ) FROM employees e WHERE e.department_id = d.department_id ) 个数 
  FROM
  	departments d;
  	
  -- 案例2
  SELECT
  	( SELECT department_name FROM departments d INNER JOIN employees e on d.department_id = e.department_id WHERE e.employee_id = 102) 部门名称
  ```

- `from`后面

  ```mysql
  SELECT
  	ag_dep.*,
  	g.grade_level 
  FROM
  	( SELECT AVG( salary ) ag, department_id FROM employees GROUP BY department_id ) ag_dep
  	INNER JOIN job_grades g ON ag_dep.ag BETWEEN g.lowest_sal 
  	AND g.highest_sal
  ```

- `exists`/`not exists`后面

  ```sql
  SELECT EXISTS
  	( SELECT employee_id FROM employees WHERE salary = 100000 ) -- 0
  	
  SELECT EXISTS
  	( SELECT employee_id FROM employees) -- 1
  ```

### 4.9 分页查询

- 语法

  ```mysql
  SELECT 查询列表 FROM 
  [join type join 表2
  on 连接条件
  WHERE 筛选条件	
  GROUP BY 分组字段
  HAVING 分组后的筛选
  ORDER BY 排序的字段]
  LIMIT offset 要显示的起始索引（从0开始）,size 条目个数;
  ```

- 特点

  - 放在查询语句的最后面
  - 公式`( page - 1) * size, size`

- 案例

  ```mysql
  SELECT
  	* 
  FROM
  	employees 
  	LIMIT 0,5; -- 0 可以省略
  ```

### 4.10 联合查询

> 将多条查询语句的结果合并成一个结果

- 语法

  ```mysql
  查询语句1
  union
  查询语句2
  union
  ...
  ```

- 场景

  > 数据来源于多张表 表之间没有关联关系 但查询的信息一致

- 特点

  - 多条查询语句的查询列数是一致的
  - 多条查询语句的每一列的类型和顺序最好一致
  - `union`关键字默认去重 使用`union all`可以保留重复项

- 案例

  ```sql
  SELECT
  	* 
  FROM
  	employees 
  WHERE
  	last_name LIKE '%a%' 
  	OR department_id > 90;
  
  -- 等价于
  SELECT
  	* 
  FROM
  	employees 
  WHERE
  	last_name LIKE '%a%' UNION
  SELECT
  	* 
  FROM
  	employees 
  WHERE
  	department_id > 90;
  	
  -- 最终字段为查询语句1的字段
  ```


## 5. DML语言学习

### 5.1 插入

- 语法

  - 插入的值要与列的类型一致或兼容
  - 字符 日期用单引号包裹
  - 空值写`null`
  - 不能为`null`的列必须插入值
  - 列的顺序可以调换
  - 可以省略列明,默认所有列,列顺序和表中列的顺序一致

- **方式一**

  ```sql
  INSERT INTO 表名 ([列名...])
  VALUES
  	( 值 1,...),(记录2);
  ```

- 方式二

  ```sql
  INSERT INTO 表名 ( 列名...) 
  SET 列名 = 值,列名= 值,...
  ```

- 比较

  - 方式一支持多行插入

  - 方式一支持子查询

    ```sql
    INSERT INTO beauty(id,name,phone)
    select 26,'rose','12213123'
    ```

### 5.2 修改

- 修改单表

  ```sql
  UPDATE 表名 
  SET 列 = 新值,列=新值 
  WHERE
  	筛选条件;
  ```

- 修改多表

  - 92语法

    ```sql
    UPDATE 表 1 别名 1,表 2 别名 2 
    SET 列 = 新值,列=新值...
    WHERE
    	连接条件 
    	AND 筛选条件;
    ```

  - 99语法

    ```sql
    UPDATE 表 1 别名 1 连接类型 表 2 别名 2 ON 连接条件 
    SET 列 = 新值,列=新值...
    WHERE
    	筛选条件;
    ```

### 5.3 删除

- 单表删除

  ```sql
  DELETE 
  FROM
  	表名 
  WHERE
  	筛选条件;
  ```

- 多表删除

  - 92语法

    ```sql
    DELETE 表 1的别名,表 2的别名 
    FROM
    	表 1 别名 1,表 2 别名 2 
    WHERE
    	连接条件 
    	AND 筛选条件
    ```

  - 99语法

    ```sql
    DELETE 表 1的别名,表 2的别名 
    FROM
    	表 1 别名 1 连接类型 JOIN 	表 2 别名 2 ON 连接条件 
    WHERE
    	筛选条件
    ```

- 删除表

  ```sql
  TRUNCATE TABLE 表名
  ```

- 删除库

  ```sql
  drop database sd_db
  ```

  

- 比较

  - `delete`可以加where条件
  - `truncate`删除效率高
  - 自增字段 `delete`删除后再新增从断点开始 `truncate`从1开始
  - `truncate`删除没有返回值,`delete`删除有返回值
  - `truncate`删除不能回滚

## 6. DDL语言学习

### 6.1 库的管理

- 创建

  ```mysql
  CREATE DATABASE 库名;
  
  -- 如果没有再创建
  CREATE DATABASE IF NOT EXISTS 库名;
  ```

- ~~库的修改~~

  ```mysql
  RENAME DATABASE 旧库名 TO 新库名; -- 已失效
  ```

  

- 更改库的字符集

  ```mysql
  ALTER DATABASE 库名 CHARCTER SET 字符集;
  ```

- 删除库

  ```mysql
  DROP DATABASE IF EXISTS 库名;
  ```

### 6.2 表的管理

- 创建表

  ```mysql
  CREATE TABLE 表名(
  	列名 列类型 [(长度) 约束],
  	...
  )
  ```

- 查看表信息

  ```mysql
  DESC 表名;
  ```

- 查看当前库所有表

  ```mysql
  SHOW TABLES;
  ```

  

- 表的修改

  ```MySQL
  -- 修改列名
  ALTER TABLE 表名 CHANGE COLUMN(可以省略) 旧列名 新列名 新列类型(不能省略);
  
  -- 修改列的类型/约束
  ALTER TABLE 表名 MODIFY COLUMN 列名 新类型
  
  -- 添加列
  ALTER TABLE 表名 ADD COLUMN 列名 新类型;
  
  -- 删除列
  ALTER TABLE 表名 DROP COLUMN 列名;
  
  -- 修改表名
  ALTER TABLE 表名 RENAME TO 新表名;
  ```

- 表的删除

  ```mysql
  DROP TABLE IF EXISTS 表名;
  ```

- 表的复制

  ```mysql
  -- 仅复制表的结构
  CREATE TABLE 表名 LIKE 旧表;
  
  -- 复制结构 + 数据
  CREATE TABLE 表名 
  SELECT * FROM 旧表;
  
  -- 只复制部分数据
  CREATE TABLE 表名 
  SELECT 字段... FROM 旧表
  WHERE 筛选条件;
  
  -- 只复制某些字段
  CREATE TABLE 表名 
  SELECT 字段... FROM 旧表
  WHERE 1=2;
  ```


### 6.3 常见约束

> 用于限制表中的数据
>
> 保证表中数据的准确性和可靠性

- 时机
  - 创建表
  - 修改表(数据添加之前)

- 分类

  - `NOT NULL`
    - 非空 保证该字段不能为空
  - `DEFAULT`
    - 默认 保证该字段有默认值
  - `PRIMARY KEY`
    - 主键 保证唯一性
  - `UNIQUE`
    - 唯一约束 保证该字段唯一 但可以为空
  - `CHECK`
    - 检查约束(`mysql`不支持)
  - `FOREIGN KEY`
    - 外键约束 用于限制两个表的关系

- 约束的添加分类

  - 列级约束

    - 六大约束语法上都支持 但外键约束没有效果

    ```mysql
    CREATE TABLE stuinfo (
        id INT PRIMARY KEY,#主键
        stuName VARCHAR ( 20 ) NOT NULL,# 非空
        gender CHAR ( 1 ) CHECK ( gender = '男' OR gender = '女' ),# 检查
        seat INT UNIQUE,# 唯一
        age INT DEFAULT 18,# 约束
        majorId INT FOREIGN KEY REFERENCES major ( id ) # 外键 (不生效)
    );
    CREATE TABLE major ( id INT PRIMARY KEY, majorName VARCHAR ( 20 ) );
    ```

  - 表级约束

    - 除了非空 默认 其他都支持

    ```mysql
    CREATE TABLE major ( id INT PRIMARY KEY, majorName VARCHAR ( 20 ) );
    CREATE TABLE stuinfo (
        id INT,
        stuName VARCHAR ( 20 ),
        gender CHAR ( 1 ) ,
        seat INT,
        age INT,
        majorId INT,
    
        CONSTRAINT pk PRIMARY KEY(id), # 主键 -- 名字固定 pk无效(mysql)
        CONSTRAINT uq UNIQUE(seat),# 唯一键
        CONSTRAINT ck CHECK(gender='男' OR gender='女'),#检查
        CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id);
    
    );
    
    ```

    

  ```sql
  CREATE TABLE 表名(
  	字段 类型 列约束,
      表约束
  )
  ```

- 查看索引

  > 包括主键 外键 唯一

  ```mysql
  SHOW INDEX FROM 表名
  ```

- ~~主键,唯一键的组合~~

  ```mysql
  CREATE TABLE stuinfo (
      id INT,
      stuName VARCHAR ( 20 ),
      
      PRIMARY KEY(id,stuName), # 主键 
  );
  ```

- 外键

  - 要求再从表设置外键关系
  - 从表的外键列的类型和主表的关联列的类型要求一致或兼容,名称无要求
  - 主表的关联列必须是一个key(主键,唯一键)
  - 插入数据时,先插入主表,再插入从表
  - 删除数据时,先删除从表,再删除主表

- 修改表时添加约束

  ```mysql
  ALTER TABLE 表名 modify column 字段 varchar(20) NOT NULL
  ```

### 6.4 标识列

> 又称自增长列

- 含义

  > 不用手动插入值,系统提供默认的序列值

- 创建表时设置标识列

  ```mysql
  DROP TABLE IF EXISTS tab_indentify;
  CREATE TABLE tab_identity(
  	id INT PRIMARY KEY AUTO_INCREMENT,
  )
  ```

- 修改步长

  ```mysql
  SHOW VARIABLES LIKE '%auto_increment%';
  SET auto_increment_increment = 3
  ```

- 特点

  - 和`key`搭配,不一定要和主键搭配
  - 一个表至多一个标识列
  - 标识列的类型只能是数值型
  - 可以`SET auto_increment_increment`设置步长
  - 也可以通过手动插入值 设置起始值

- 修改表时设置标识列

  ```mysql
  ALTER TABLE 表名 MODIFY COLUMN id INT PRIMARY KEY AUTO_INCREMENT
  ```

  

## 7 TCL语言学习

> **事务由单独单元的一个或者多个sql语句组成,在这个单元中,每个sql语句是互相依赖的**,
>
> 而整个单独单元作为不可分割的整体,如果单元中某条sql语句执行失败发生错误,整个单元将会回滚.所有受到影响的数据返回开始以前的状态;
>
> 如果单元中的所有sql语句只想成功,则事务被顺利执行

- 查看mysql支持的存储引擎

  ```sql
  show engines
  ```

  - `mysql`使用最多的引擎:
    - `innodb`(支持)
    - `myisam`(不支持)
    - `memory`(不支持)
  
- 四个特性（ACID特性）

  - 原子性 - 不可分割
  - 一致性 - 操作前后总量不变
  - 隔离性 - 多个操作互不影响
  - 持久性 - 提交之后数据变化

- 事务的创建

  - 隐式事务

    > 事务没有明显的开启和结束的标记
    >
    > 比如`insert`,`update`,`delete`语句

  - 显示事务

    > 前提:必须先设置自动提交功能为禁用
    >
    > ```mysql
    > set autocommit = 0;
    > start transaction; -- (可选)
    > 语句1;
    > 语句2;
    > ...
    > 语句n;
    > commit; -- 提交事务
    > rollback; -- 回滚事务
    > ```

- `delete`和`truncate`在事务中的区别

  - `delete`回滚后记录还在
  - `truncate`回滚后记录仍然删除

  

## 8. 视图

- 场景
  - sql较复杂
  - 多个地方用到同样的查询结果
- 特点
  - 重用sql
  - 简化操作
  - 保护数据

- 创建

  - 语法

    ```sql
    CREATE 视图名 AS 语句;
    ```

  - 案例

    ```sql
    CREATE VIEW v1 AS SELECT
    last_name,
    department_name 
    FROM
    	employees e
    	INNER JOIN departments d ON e.department_id = d.department_id 
    WHERE
    	last_name LIKE '%a%';
    ```

- 使用

  ```mysql
  SELECT
  	* 
  FROM
  	v1;
  ```

- 修改

  ```mysql
  CREATE OR REPLACE VIEW 视图名
  AS
  查询语句;
  
  -- 方式二
  ALTER VIEW 视图名
  AS
  查询语句;
  ```

- 删除视图

  ```mysql
  DROP VIEW 视图1,视图2,视图3
  ```

- 查看视图

  ```mysql
  DESC 视图;
   
  SHOW CREATE VIEW 视图;
  ```

- 更新

  ```sql
  -- 插入
  INSERT INTO 视图 values (值);
  
  -- 修改
  UPDATE 视图 SET 字段 键=值 ...
  
  -- 删除
  DELETE FROM 视图 WHERE 条件
  ```

- 具备以下条件的视图不允许更新

  - sql包含以下关键词
    - 分组函数
    - `DISTINCT`
    - `GROUP BY`
    - `HAVING`
    - `UNION`
    - `UNION ALL`
  - 常量视图
  - `select`中包含子查询
  - `join`等连接
  - `from`一个不能更新的视图
  - `where`子句的子查询引用了`from`子句中的表

## 9. 变量

### 9.1 系统变量

> 由系统提供 不是用户定义 属于服务器层面
>
> - 分类
>   - 全局变量
>     - 不能跨重启
>   - 会话变量

- 查看所有系统变量

  ```mysql
  -- 全局
  SHOW GLOBAL VARIABLES;
  
  -- 会话
  SHOW SESSION VARIABLES;
  
  -- 筛选
  SHOW VARIABLES LIKE '%%'; -- 不写默认session
  
  -- 查看指定的某个系统变量
  SELECT @@[GLOBAL|SESSION].系统变量名;
  
  -- 赋值
  SET [GLOBAL|SESSION] 变量 = 值
  SET @@[GLOBAL|SESSION].变量 = 值
  ```



### 9.2 自定义变量

> 由用户定义
>
> - 分类
>   - 用户变量
>   - 局部变量

- 使用步骤
  - 声明
  - 赋值
  - 使用(查看 比较 运算)



- 用户变量

  > 针对当前会话(连接)有效
  >
  > 应用在任何地方

  1. 声明并初始化

     ```mysql
     SET @用户变量名=值;
     SET @用户变量名:=值;
     SELECT @用户变量名:=值;
     ```

  2. 赋值

     ```mysql
     SET @用户变量名=值;
     SET @用户变量名:=值;
     SELECT @用户变量名:=值;
     
     SET 字段 INTO @变量名 FROM 表;
     ```

     ```sql
     SET COUNT(*) INTO @count FROM tb;
     ```

  3. 查看

     ```sql
     SELECT @用户变量名
     ```

- 局部变量

  > 作用域:仅仅在定义它的`begin`,`end`有效
  >
  > **应用在`begin`,`end`的第一句话**

  1. 声明(初始化)

     ```mysql
     DECLARE 变量名 类型;
     DECLARE 变量名 类型 DEFAULT 默认值;
     ```

  2. 赋值

     ```sql
     SET 局部变量名=值;
     SET 局部变量名:=值;
     SELECT @局部变量名:=值;
     
     SET 字段 INTO 局部变量名 FROM 表;
     ```

  3. 使用

     ```sql
     select 局部变量名;
     ```

     

## 10. 存储过程和函数

### 10.1 存储过程

- 存储过程

  > 一组预先编译好的sql语句的集合 理解成批处理语句
  >
  > - 提高重用
  > - 简化操作
  > - 减少编译次数
  > - 减少和数据库服务器的连接次数,提高了效率

- 创建

  ```mysql
  CREATE PROCEDURE 存储过程名 ( 参数列表 ) BEGIN
  	存储过程体
  END
  ```

- 实例

  ```sql
  IN stuname VARCHAR(20)
  ```

- 参数

  1. 参数模式
     - `IN`
       - 该参数可以作为输入
     - `OUT`
       - 该参数可以作为输出(返回值)
     - `INOUT`
       - 既可以作为参数,也可以作为输出

  2. 参数名

  3. 参数类型

- 规则

  - 如果 存储过程体只有一句 可以省略`BEGIN`,`END`

  - 存储过程体中的每条`sql`必须以`;`结尾

  - 存储过程的 结尾可以使用`DELIMITER`重新设置

    ```mysql
    CREATE PROCEDURE `func`()
    BEGIN
    	#Routine body goes here...
    END
    ```

  - 调用

    ```sql
    CALL 存储过程名(实参列表);
    ```

    

- 案例

  - 空参列表案例

    ```mysql
    CREATE DEFINER=`root`@`localhost` PROCEDURE `NewProc`()
    BEGIN
    	#Routine body goes here...
    	INSERT INTO `myemployees`.`admin`(`id`, `username`, `psd`) VALUES (1, 'join', 123);
    
    END
    ```

    - 调用
  
    ```sql
  call addOneUser();
    ```

  - 有参
  
    ```mysql
    CREATE DEFINER = `root` @`localhost` PROCEDURE `findCp` ( IN girlName VARCHAR ( 20 ) ) BEGIN
        SELECT
            b.* 
        FROM
            boys b
            RIGHT JOIN girls g ON b.id = g.cp_id 
        WHERE
            g.NAME = girlName 
  END
    ```
  
    - 调用

    ```sql
  call findCp('rose');
    ```
  
  - 案例: 检查用户名和密码
  
    ```mysql
    CREATE DEFINER=`root`@`localhost` PROCEDURE `check`( IN uname VARCHAR ( 20 ),IN psd VARCHAR(20) )
    BEGIN
    	DECLARE result VARCHAR(20) DEFAULT ''; # 声明变量并初始化
    	SELECT COUNT(*) INTO result # 赋值
    	FROM admin a
  	WHERE a.username = uname AND a.psd = psd;
    	SELECT IF(result>0,'正确','错误') 结果;
    END
    ```

    - 调用
  
    ```mysql
CALL `check`( 'join', '123' ); -- check是关键字 加``
    ```

  - `out`模式的参数
  
    ```mysql
    CREATE PROCEDURE findBoy(IN girlName VARCHAR(20),OUT boyName VARCHAR(20))
    BEGIN
    	SELECT b.bName INTO boyName
    	FROM boys b
    	INNER JOIN girls g ON b.id = g.cp_id
    	WHERE g.gName = girlName;
    END
    ```
  
    - 调用
  
    ```mysql
    SET @boyName 
    CALL findBoy('ROSE',@boyName)
    
    SELECT @boyName
    ```
  
    
  
  - `INOUT`模式参数的存储过程
  
    ```mysql
    CREATE PROCEDURE doubleNum(INOUT a INT,INOUT b INT)
    BEGIN
    	SET a = a*2;
    	SET b = b*2;
    END
    ```
  
    - 调用
  
    ```sql
    set @param1 = 10;
    set @param2 = 20;
    CALL doubleNum(@param1,@param2);
    
    SELECT @param1,@param2
    ```
  
- 删除存储过程

  ```sql
  DROP PROCEDURE 存储过程名,存储过程名2;
  ```

- 查看存储过程

  ```sql
  SHOW CREATE PROCEDURE 存储过程名
  ```

### 10.2 函数

> 和存储过程的区别:
>
> 存储过程可以有任意多个返回,适合做批量插入,批量更新
>
> **函数只能有一个返回**,适合做处理数据后返回的一个结果

- 创建

  ```mysql
  CREATE FUNCTION 函数名(参数列表) RETURNS 返回类型 -- 一定有returns
  BEGIN
  	函数体
  END
  ```

- 规则

  - 参数列表
    1. 参数名
    2. 参数类型

  - 肯定有`returns` 没有会报错 return一般放最后
  - 函数体只有一句话 可以省略`begin`,`end`
  - 使用`delimiter`设置结束标记

- 调用

  ```sql
  SELECT 函数名(参数列表);
  ```

- 案例

  ```mysql
  CREATE FUNCTION e_count() RETURNS INT
  BEGIN
  	DECLARE c INT DEFAULT 0;
  	SELECT COUNT(*) INTO c
  	FROM students;
  	RETURN c;
  END
  ```

  - 调用

  ```sql
  SELECT e_count();
  ```

- 有参案例

  ```mysql
  CREATE FUNCTION getSalary(ename VARCHAR(20)) RETURNS DOUBLE
  BEGIN
  	SET @sal = 0;
  	SELECT salary INTO @sal
  	FROM employees
  	WHERE last_name = ename;
  	RETURN @sal;
  END
  ```

  - 调用

  ```sql
  SELECT getSalary("jack")
  ```

- 查看函数

  ```mysql
  SHOW CREATE FUNCTION 函数名;
  ```

- 删除函数

  ```mysql
  DROP FUNCTION 函数名;
  ```

  

## 11. 流程控制结构



### 11.1 顺序结构

> 程序从上往下依次执行

### 11.2 分支结构

> 程序从两条或多条路径中选择一条去执行

- `IF`

  - 语法

    ```sql
    IF(条件,表达式2,表达式3)
    ```

  - 应用:任何地方

- `CASE`

- `IF...THEN`

  - 语法

    ```mysql
    if 条件1 then 语句1;
    elseif 条件2 then 语句2;
    ...
    else
    ...
    ```

  - 应用: `begin-end`中

### 11.3 循环结构

> 程序在满足一定条件的基础上,重复执行一段代码
>
> 使用位置: `begin-end`中
>
> `iterate` - > 类似于`countine`
>
> `leave` -> 类似于`break`

- `while`

  - 语法

    ```mysql
    [标签:] while 循环条件 do
    	循环体;
    end while [标签];
    ```

  - 特点: 先判断后执行

- `loop`

  - 语法

    ```mysql
    [标签:] loop
    	循环体;
    end while [标签];
    ```

  - 特点: 死循环

- `repeat`
  - 语法

    ```mysql
    [标签:] repeat
    	循环体;
    until 结束循环的条件
    end repeat [标签];
    ```

  - 特点:先执行后判断