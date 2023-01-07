# 1 快捷键
ctrl+X :清空所有记录
Ctrl+F：查找
F12：启动或者停止抓包


# 2 fiddler原理
Fiddler是位于客户端和服务器端的HTTP代理

- 监控阅览器所有的http/https流量
- 查看分析请求内容细节
- 伪造客户端请求和服务器响应
- 全局, 局部断电功能那个
- 第三方插件 

- 请求分析, 请求修改, 相应修改, 网络限速. 断电调试. 设计请求, 自动相应, mock 测试 

# 3 B/S架构

●编写程序部署到web服务器
●web服务器运行在服务器上，绑定ip地址并监听某端口，接收和处 理http请求
●客户端 (web client端为前端), 通过http协议获取服务器 (HTTPS server, 为后端 )上的网页、文档等资源
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330160659334.png)


# 4 工作原理
作为系统代理，发送请求或接受响应
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330161859198.png)

# 5 HTTP协议
Hyper Text Transfer Protocol (超文本传输协议)
用于从万维网服务器传输超文本到本地浏览器的传送协议
●HTTP协议是基于TCP的应用层协议，它不关心数据传输的细节，主要是用来规定客户端和服务端的数据传输格式，最初是用来向客户端传输HTML页面的内容。默认端口是80 .
●http是基于请求与响应模式的、无状态的、应用层的协议
下面为http请求构成的两部分

## 5.1 请求报文
客户端发给服务器，HTTP请求报文主要由请求行，请求头部、空一行、请求正文4部分组成。
  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330164310273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoYW5lX0NoZW5nMDIwMg==,size_16,color_FFFFFF,t_70)

请求体可以为空，例如get请求

### 5.1.1 请求行

请求行由请求方法字段、URL字段和HTTP协议版本字段3个字段组成，它们用空格分隔。

#### 5.1.1.1 请求方法字段
请求方法	备注
GET	请求资源
POST	提交资源
HEAD	获取响应头
PUT	替换资源
DELETE	删除资源
OPTIONS	允许客户端查看服务器的性能
TRACE	回显服务器收到的请求，用于测试或诊断

#### 5.1.1.2 统一资源定位符（URL）

Uniform Resource Locator:统一 资 源定位符
●用于描述网.上的资源
格式: schema:/ /host[:port#]/path/ …/ [?query-string ]
●　scheme:协议，如http, https, ftp等
●　host:域名或者IP地址
●　port: 端口
●　path:资源路径
●　query-string:发送的参数

资源定位符http:// test. lemonban.com/ningmengban/images/logo.png
协议 http://
域名 test.lemonban.com 对应主机IP，为了查找主机 cmd命令行可以用ping域名的方式命令查找IP

#### 5.1.1.3 http版本
目前普遍使用的为1.1版本,即http/1.1


### 5.1.2 请求头部

请求头可以是任意信息,根据服务器需要进行组合

请求头	描述
Host	主机ip地址或域名
User- Agent	客户端相关信息，如操作系统、刘览器等信息
Accept	指定客户端接收信息类型，如: image/jpg, text/html, application/json
Accept-Charset	替换资源
Accept-charaet	客户端接受的字符集，如gb2312,iso-8859-1
Accept-Encoding	可接受的内容编码，gzip
Accept-Language	接受的培言，如Accept-Langunge:zh-cn
Authorization	客户瑞提供给服务端，进行权限认证的信息
Cookie	携带的cookie信息
Referer	当前文档的URL，即从哪个链接过来的
Content-Type	请求体内容类型，如Content-Type: application/x www form urlencoded
Content -Length	数据长度
Cache-Control	缓存机制，如Cache-Control:no-cache
Pragma	防止页面被缓存，和Cache-control:no-cache作用一样


### 5.1.3 请求体
真正发送给服务器的一串文本


## 5.2 响应报文

服务器返回给客户端，HTTP响应报文主要由状态行，消息头部、空一行、响应体4部分组成。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200412101109952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NoYW5lX0NoZW5nMDIwMg==,size_16,color_FFFFFF,t_70)

### 5.2.1 状态行/相应行

请求行由HTTP协议版本字段、状态码字段及其描述3个字段组成，它们用空格分隔。
状态码：用以表示网页服务器HTTP响应状态的3位数字代码

状态码	描述
1XX	提示信息，请求被成功接收
2XX	成功，请求被成功处理
3XX	重定向相关
4XX	客户端错误
5XX	服务器端错误
常用状态码：https://blog.csdn.net/qq_35689573/article/details/82120851

### 5.2.2 响应头
响应头	描述
Server	HTTP服务器的软件信息
Date	响应报文的时间
Expires	指定缓存过期时间
Set-Cookie	设置Cookie
Last-Modified	资源最后修改时间
Content-Length	内容长度
Connection	如：Content-Type：text/html；charset=utf-8
Connection	如keep-Alive，表示保持tcp链接不关闭，不回永久保持链接，服务器可设置
Location	指明重定向的位置，新的URL地址，如304的情况

