## 实体类定义规则

- PO： 持久对象

  > 有时也被称为Data对象，对应数据库的entity，可以简单认为一个PO对应数据库中的一条记录

- VO： 表现层对象

  > - 主要对应页面显示的数据对象
  > - 可以和表对应，也可以不，根据业务需要

- TO：（DTO）数据传输对象

  > 一张表有100个字段，view只需要10个 可以用这10个属性的DTO来传输数据道client。到达客户端后，如果用这个对象对应界面显示，那么此时它的身份就转为VO

- POJO：无规则简单java对象

  > 一个中间对象 可以转化为PO ，DTO, VO
  >
  > - 持久化 =》 PO
  > - 用作表示层 =》 VO
  > - 传输过程中 =》 DTO

### Lombok

- 安装

- 依赖

  ```xml
  <dependency>
  	...
  </dependency>
  ```

- 使用

  - `@NonNull`
    - 用在方法参数前
    - 非空校验 为空抛出NPE
  - `@Cleanup`   
    - 自动释放资源
  - `@Getter`
    - 用在属性上
  - `@Setter`
  - `@ToString`
    - 用在类上
  - `@EqualsAndHashCode`
    - 用在类上
  - `@NoArgsConstructor`
  - `@RequiredArgsConstructor`
  - `@AllArgsConstructor`
    - 用在类上,自动生成无参 有参构造
  - `@Data`
    - 用在类上
    - 等于上面`getter`...
  - `@value`
    - 用在类上
    - `@Data`的不可变性是 相当于添加`final`声明
    - 只能`get` 不能`set`
  - `@SneakyThrows`
    - 自动抛受检异常
  - `@Synchronized`
    - 用在方法上,声明为同步,自动加锁
  - `@Getter(lazy=true)`
    - 懒加载

