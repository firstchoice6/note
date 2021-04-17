#### 重要性

webserversive = HTTP协议 + XML

REST = HTTP协议 +json

各种api也一般是http + xml/json实现的

#### URL与URI

URL表示互联网中资源的地址

URN表示资源的唯一名称

URI是URL与URN 的超集

<img src="C:\Users\何镇武\AppData\Roaming\Typora\typora-user-images\image-20200702081012669.png" alt="image-20200702081012669" style="zoom: 50%;" />

<img src="C:\Users\何镇武\AppData\Roaming\Typora\typora-user-images\image-20200702081730973.png" alt="image-20200702081730973" style="zoom:50%;" />

#### 三次握手与四次挥手



#### http请求信息和响应信息的格式

![image-20200701211409277](C:\Users\何镇武\AppData\Roaming\Typora\typora-user-images\image-20200701211409277.png)

请求：

- 请求行
  - 请求方法（这些方法虽然http里规定，但是webserver未必支持）
    - POST
    - GET
    - HEAD（head 和 get 基本一致 只是不返回内容）
    - PUT
    - DELETE
    - TRACE（测试代理有没有修改http请求）
    - OPTIONS（返回服务器可用的方法）
  - 请求路径
  - 所用协议
- 请求头信息
- 请求主体信息（可以没有）
- 头信息结束后和主体信息之间要空一行

![image-20200701211600070](C:\Users\何镇武\AppData\Roaming\Typora\typora-user-images\image-20200701211600070.png)

![image-20200701212402141](C:\Users\何镇武\AppData\Roaming\Typora\typora-user-images\image-20200701212402141.png)

![image-20200701212646643](C:\Users\何镇武\AppData\Roaming\Typora\typora-user-images\image-20200701212646643.png)

响应：

- 响应行
  - 协议版本
  - 状态码
  - 状态文字
- 响应头信息
- 响应主体信息

#### 状态码

| 状态码 | 定义       | 说明                             |
| ------ | ---------- | -------------------------------- |
| 1xx    | 信息       | 接收到请求，继续处理             |
| 2xx    | 成功       | 操作成功的收到，理解和接受       |
| 3xx    | 重定向     | 为了完成请求，必须进一措施       |
| 4xx    | 客户端错误 | 请求的语法有错误或不能完全被满足 |
| 5xx    | 服务端错误 | 服务器无法完成明显有效的请求     |

| 状态码 |                       | 定义                                                         |
| ------ | --------------------- | ------------------------------------------------------------ |
| 100    | Continue              | 请求者应当继续提出请求                                       |
| **200  | OK                    | 成功返回网页                                                 |
| 201    | Created               | 请求成功并且服务器创建了新的资源                             |
| 202    | Accepted              | 服务器已接受请求，但尚未处理                                 |
| **301  | Moved Permanently     | 永久 重定向                                                  |
| **302  | Found                 | 临时 重定向                                                  |
| 303    | See Other             | 临时性重定向，且总是使用 GET 请求新的 URI                    |
| **304  | Not Modified          | 未修改（刷新的时候从缓存中请求，减轻服务器压力）             |
| **307  | Temporary Redirect    | 重定向中保持原有的信息                                       |
| 400    | Bad Request           | 服务器无法理解请求的格式，客户端不应当尝试使用相同的内容发起请求 |
| 401    | Unauthorized          | 请求未授权                                                   |
| 403    | Forbidden             | 禁止访问                                                     |
| **404  | Not found             | 找不到如何与 URI 相匹配的资源                                |
| **500  | Internal Server Error | 服务器内部错误                                               |
| 503    | Service Unavailable   | 服务器暂时不可用                                             |

#### HTTP为什么是不安全的？。
1. 不能这么问.首先HTTP协议本身并不存在安全问题，协议本身也几乎不会成为攻击的对象.所谓的”HTTP不安全”是指应用HTTP协议的服务器和客户端，以及运行在服务器上的web应用资源容易受到攻击，这些是不安全的..
2. 因为http 协议非常单纯.它不具备会话管理，不具备加密处理.如果有需要，我们得重新开发.开发者需要自行设计（认证，会话管理）.自行设计就有多样性.那么这些多样性的设计，就可能存在了安全问题.。
3. 使用的软件（web软件存在漏洞）.
   
   - https是http的一个安全通道，安全保证是由SSL提供的。



#### 常见的WEB攻击
1. XSS跨站脚本攻击
2. SQL注入与OS命令注入
3. 远程文件的漏洞
4. 会话劫持与伪请求
5. 密码破解

