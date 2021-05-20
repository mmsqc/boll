# nodejs

## 介绍

简单的说 Node.js 就是运行在服务端的 JavaScript。Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

通俗来讲，nodejs其实就是安装在服务端(window，linux，。。。)的javascript引擎。是基于chrome的v8引擎的。他能够支撑js代码的运行。由于他摆脱了浏览器的束缚，为我们拓展出了很多新的功能。比如io操作，比如数据库操作，比如。

## 下载安装

https://nodejs.org/zh-cn/download/releases/

win7系统要使用12以前(包括)的版本，win10系统必须使用最新的版本。

例如(node-v12.22.1-win-x64.zip );  

检测安装：

打开cmd窗口-->node -v-->正常应该输出版本号

如果出现不是内部或外部命令。检查是否安装nodejs，如果确认安装了，就检查环境变量是否正确配置。

## 执行js代码

执行文件

```
编写js文件   a.js
执行js文件	 node a.js
```

直接执行js

```
node
输入任意js代码即可
退出 ctrl+c
```

## npm

NPM是随同NodeJS一起安装的当下最流行，最大的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

下载包

```
npm -v		查看npm的版本

npm install 包名@版本号	-g	--save -dev
			-g	表示全局安装
			[
			--save	保存的意思，如果加了，就会在package.json中记录安装的包
					否则，只是在node_modules中下载了包，但是不在package.json中记录.
			]
			--save-dev
			
	node_modules：是npm自动为我们创建的用来装包的一个文件夹
	package.json	这个文件里面的depenedecis里面配置的是我们项目的依赖包。
    		devDependencies: 开发依赖
    		dependencies:	 全局依赖

npm	i		当我们执行该命令，就会读取当前目录下的package.json文件，根据该文件中dependencis中的配置，去仓库中			下载相应的包，放到node_modules目录下面去，供我们的自己的项目使用
```

由于直接使用npm的官方镜像，速度比较慢，我们可以下载一个npm的替换工具cnpm，cnpm默认使用的是淘宝镜像，通常速度会比较快。

下载cnpm

npm i cnpm -g 

使用cnpm

用法跟npm一样，就是在npm前面加个c(中国);

更新包

cnpm update 包名

搜索包

cnpm search 包名

卸载包

cnpm uninstall 包名

清除缓存

cnpm clear cache

## 模块系统

比如我们的网页，依赖两个js文件

模块输出

```
var app = {
	name:"jack",
	age:"21",
	eat:function(){
		console.log("eat..eat")
	}
}

//输出了一个name
exports.app = app;
```

模块输入

```
//输入
let m1 = require('./m1');

/*
	模块系统
		就是将不同的内容放到不同的文件中作为模块去写
		文件与文件中的关系，就是相互引用的关系
		每个模块都有自己的输入和输出
		要想被别人用，他就要有输出
		要想用别的模块，就要有输入
*/

m1.app.eat();
```

## 文件系统

 Node.js 提供一组类似 UNIX（POSIX）标准的文件操作API。 Node.js 文件系统（fs 模块）模块中的方法均有异步和同步版本，例如读取文件内容的函数有异步的 fs.readFile() 和同步的 fs.readFileSync()。建议大家使用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。

引入文件系统

```
let fs = require('fs');
```

获取文件信息

```
文件大小		stat.size;
是否文件		stat.isFile()
是否目录		stat.isDirection()
```

读取文件内容

```
fs.readFile(url,function(err,data){
	err报错  data数据
})
```

写入内容

```
fs.writeFile(url,data,function(){
	data就是我们要写入url里面的内容
})
```

读取文件夹

```
fs.readdir("d://",function(err,data){
	console.log(data)
})
```



## 路径模块



## 服务器

服务器的主要作用就是能够对外提供服务，网页上能够通过请求的方式向服务器发起请求，然后服务器对于不同的请求给予不同的响应，比如文件，网页，json数据等。服务器为了完成请求，还可以操作文件，连接数据库等等。

服务器创建

```
// 引入http模块
let http =  require("http");

//使用该模块创建服务器
http.createServer(function(request,response){
	//request 表示请求的数据	//response表示响应的数据
	
	//设置响应数据格式--utf8支持中文
	response.writeHead(200, {'Content-Type': 'text/html;charset=utf-8'});
	
	//响应给用户的数据
	response.end("别问我是谁");
	
}).listen(8888);	//8888表示端口号。很重的啊，不能不设置。 
```

服务器访问

	组成：   服务器地址	:	端口
	
		 服务器地址：IP地址(192.168.2.22)   域名(baidu.com)
			本机的ip地址可以直接写成：127.0.0.1
			本机的域名可以直接写成:localhost
	
		 端口:		每个程序都有一个唯一的端口		端口号尽量用1024以后的端口号   6万5千多
		 
	注意：
		浏览器发出去请求，在没有写明端口的情况下，默认都是访问80端口
## express

Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。

使用 Express 可以快速地搭建一个完整功能的网站。

Express 框架核心特性：

- 可以设置中间件来响应 HTTP 请求。
- 定义了路由表用于执行不同的 HTTP 请求动作。
- 可以通过向模板传递参数来动态渲染 HTML 页面。

安装

```
cnpm i express --save
```

启动服务器

```
//引入express模块
const express = require("express")

//通过这个模块来创建服务器
const app = express();

//启动监听端口
app.listen(8888)
```

编写接口

接口是给别人来访问的，别人通过一个地址来进行访问，我们就认为别人访问了我们的一个接口，我们通过接口来接收用于的请求，处理请求，响应结果。

每个接口都会提供一个独立的功能。



```
过滤所有的请求
app.use("/*",function(req,resp,next){
	resp.setHeader("content-type","text/html;charset:utf-8");
	next()
})

处理get请求
app.get(url,function(){

})

处理post请求
app.post(url,function(){

})

处理get或者post请求
app.use(url,function(){

})
```



获取get参数

不需要任何特殊处理，直接获取即可

```
app.get(url,function(req,resp){
	let name = req.query.name;
})
```

获取post参数

post参数比较特殊，需要引入专门的处理模块

```
//引用post参数解析模块
const bodyParser=require("body-parser");//有些模块是nodejs自带的，不需要安装的
// 解析application/json数据
var jsonParser = bodyParser.json();
// 解析application/x-www-form-urlencoded数据
var urlencodedParser = bodyParser.urlencoded({ extended: false });

app.post(url,urlencodedParser,function(req,resp){
	let name = req.body.name;
})
```

## mysql

我们之前学习mysql，要对数据进行操作，必须使用myslq的客户端连接上数据库，然后才能进行操作。但是呢，实际项目当中，我们肯定没办反通过客户端去操作数据库，你想想一下，用代码去操作sqlyog，是不是感觉很反人类呢？肯定反人类。我们在做项目的时候，都是通过代码直接去操作数据库的。那么，我们如果通过nodejs代码来操作数据库呢?这个就是我们接下来要讲的内容了。

前提：打开数据库服务。

1.安装驱动

```
cnpm i mysql 
```

2.连接数据库案例

```
//引入mysql数据库驱动
var mysql  = require('mysql');
//设置连接的参数
let conn = mysql.createConnection({
	//用户名
	user:"sun",
	//密码
	password:"sun",
	//主机(ip)
	host:"localhost",
	
	//数据库
	database:"k4"
	//端口
	// port:"3306"   不写默认就是3306，这时mysql数据库的默认端口号
	
})

//进行连接
conn.connect();

//执行sql语句--更新
let sql = "insert into student values(16,'男',1,'老王')";//执行添加
sql = "update student set sname = '老王炸' where sid = 16";//执行修改
sql = "delete from student where sid = 16";//执行删除
conn.query(sql,function(error,result,field){
	//只要受影响的行数大于0，就说明更新成功
	if(result && result.affectedRows>0){
		console.log("说明更新成功");
	}
})

//执行查询操作
sql = "select * from student";
/* error:报错信息    result：执行语句的结果信息，如果是查询返回的就是数组,如果是修改，返回的是对象 
   field:执行查询的时候，返回的字段信息(有哪些列)
*/   
conn.query(sql,function(error,result,field){
	// console.log(error);
	// console.log(result);
	// console.log(field);
	if(result){
		result.forEach(item=>{
			console.log(item.sid,item.gender,item.sname,item.class_id);
		})
	}
})

//关闭连接通道，释放内存
conn.end();
```

## 封装mysql

















# 练习

2.假如桌面上有一张图片，要求编写一个复制图片的方法，当调用了这个方法，桌面上就多了一张图片，要求图片能正常打开

1.加入有一个文件夹，要求使用nodejs编写一个方法，遍历出这个文件夹里面的所有文件

3.大家自行完成一个登陆注册系统。

​	1.编写注册页面

​    2.编写注册接口

​			1.获取注册的用户名，密码，手机号等数据

​			2.读取本地的data.json文件

​			3.将获取的用户名， 密码，手机号等数据，追加到data.json文件当中(如果用户名重复，直接返回注册失败)

​			4.返回注册成功，

​	3.编写登录页面

​	4.编写登录接口

​			0.获取登录的用户名和密码

​			1.读取本地的data.json文件

​			2.判断用户名和密码是否匹配

​			3.如果匹配，返回登录成功，否则，返回登录失败



4.注册完毕之后想要自动跳转到登录页面，该怎么做？



a

​	a1

​		a111.txt

​	a2

​		a111.txt

​	a3.txt

b

c

d

e

f

g

aa.txt

bb.txt

cc.mp3



















