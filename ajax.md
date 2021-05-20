# ajax



## 同步操作

比如我们有3个操作a,b,c

同步操作的意思是操作完了a，再操作b，b完了再操作c，这种操作就叫做同步操作

```
下面的两个循环，一定是上面的循环结束了，才会执行下面的循环

for(let i=0;i<100;i++){
 	document.write("跑")
  }
	
  for(let i=0;i<100;i++){
    document.write("打")
  }
```



## 异步操作

比如我们有3个操作a，b，c

异步操作的意思是操作a的同时也在操作b和c，这种操作就叫做异步操作

从时间上来讲，异步操作优于同步操作。常见的异步操作包括setTimeout以及setInterval

```
如果同时点了开枪和跑，那么开枪和跑就会同时运行，不会存在要等另一个执行完了才能执行的问题
<button onclick="f2()">开枪</button>
<button onclick="f1()">跑</button>
<div id="div1"></div>
<script>
	function f1(){
		div1.innerHTML+="  跑  ";
		setTimeout(f1,1000);
	}
	
	function f2(){
		div1.innerHTML+="  打  ";
		setTimeout(f2,1000);
	}
</script>
```

## 同步请求

1.操作的结果，会马上回馈到网页上

2.操作过程中，网页会出现卡死的现象

3.地址栏会发生改变

常见的同步请求方式：

```
<!-- 超链接发送同步请求 -->
<a href="book.json">加载数据</a>

<!-- form表达发送同步请求 -->
<form action="book.json">
<input type="submit" />
</form>

<!-- 使用javascript中内置window对象的open方法发送同步请求 -->
<button onclick="f1()">open方法</button>

<!-- 使用javascript中的内容location对象来发送同步请求 -->
<button onclick="f2()">location对象</button>
		
<script>
	function f1(){
		window.open("book.json");
	}
	function f2(){
		location.href="book.json";
	}
</script>
```

## 异步请求：

1.需要委托ajax引擎执行操作，对网页本身不会有任何影响

2.请求的结果，需要监听ajax引擎主动获取。



温馨提醒：

​	如果是页面之间的跳转，建议使用同步，如果是页面上局部数据的变化或者交互建议使用异步请求。



发送异步请求：

```
	// 1.创建ajax引擎
		let xhr = new XMLHttpRequest();
		console.log(xhr.readyState)
		
		// 2.设置参数(地址，数据)   请求方式，由后台决定
		xhr.open("GET","book.json");
		
		// 3.下命令，走
		xhr.send();
		/*
			 0 － （未初始化）还没有调用send()方法
			 1 － （载入）已调用send()方法，正在发送请求
			 2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
			 3 － （交互）正在解析响应内容
			 4 － （完成）响应内容解析完成，可以在客户端调用了
		*/
	   console.log(xhr.readyState)
		
		// 4.监视他，在合适的时候获取结果
		xhr.onreadystatechange=function(){
			/*
				xhr.status(响应状态码)
				200:表示正常情况
			*/
			//这个函数，每有任何风吹草动，都会执行
			if(xhr.readyState == 4 && xhr.status == 200){
				//获取结果--responseText方法默认拿回来的是字符串类型的数据
				let rs = xhr.responseText;
				//转成数组(json)类型  放进去的是字符串，返回的是json类型的数据
				let json = JSON.parse(rs);
				
				json.forEach(item=>{
					//默认找id为div1的节点
					div1.innerHTML+=`<li>${item}</li>`
				})
			}
		}
```

## ajax封装

```
function ajax(method,url,param,callback,datatype){
	let xhr = new XMLHttpRequest();
	xhr.open(method,url);
	//如果是post请求，就要设置参数格式
	if("POST" == method){
		xhr.setRequestHeader("Content-type","application/json;charset=UTF-8");
		xhr.send(param);
	}else{
		xhr.send();
	}
	xhr.onreadystatechange=function(){
		if(xhr.readyState == 4 && xhr.status == 200){
			//处理结果
			let data = xhr.responseText;
			//转成json
			data = "json" == datatype ? JSON.parse(data) : data;
			
			//如果callback不为空，就调用callback方法，把data传过去
			callback && callback(data);
		}
	}
}
```

## 回调地狱

- 代码耦合，一旦修改，原地爆炸。
- 无法使用try catch，就无法排错，也是原地爆炸

```
ajax("GET","./b1.json",null,function(data){
	data.forEach((item)=>{
		t1.innerHTML+=`
			<tr><td>${item.id}</td><td>${item.name}</td></tr>
		`
	})
	ajax("GET","./b2.json",null,function(data){
		data.forEach((item)=>{
			t1.innerHTML+=`
				<tr><td>${item.id}</td><td>${item.name}</td></tr>
			`
		})
		ajax("GET","./b3.json",null,function(data){
			data.forEach((item)=>{
				t1.innerHTML+=`
					<tr><td>${item.id}</td><td>${item.name}</td></tr>
				`
			})
		},'json')
	},'json')
},'json')

```

避免回调地狱

怎么避免回调地狱？

1、写浅一些。别嵌套太深。

2、使用模块化技术，别一个模块里做很多事，而是分成不同模块。

3、对错误进行单独处理，而不是套很多错误处理的回调。



怎么解决回调地狱

我们可以使用promise这个状态对象来解决地狱回调问题

promise有三种状态。

```

	let promise = new Promise(function(resove,reject){
		console.log("执行操作")
		try{
			console.log(111);
			resove();  //成功
		}catch(e){
			reject();
		}
	});
	
	//如果调用的是resolve，其实执行就是then里面的第一个函数，否则就是第二个函数
	promise.then(function(){
		console.log("成功的调用")
	},function(){
		console.log("失败的调用")
	})
```

promise解决地狱回调

```
sendReq(1)
	.then(()=>sendReq(2))
	.then(()=>sendReq(3))
	
	
	function sendReq(num){
		return new Promise((resove,reject)=>{
			ajax("GET","./b"+num+".json",null,function(data){
				try{
					data.forEach((item)=>{
						t1.innerHTML+=`
							<tr><td>${item.id}</td><td>${item.name}</td></tr>
						`
					})
					//如果执行成功，将状态改成成功状态
					console.log("111")
					resove();
				}catch(e){
					//如果失败，执行reject，将状态改成失败状态
					console.log("222")
					reject();
				}
			},'json')
		})
	}
```

## jquery中的异步请求

jquery为我们封装好了异步请求的发送方法

+ load
+ get
+ post
+ ajax

```
/*
		第一个参数：路径
		第二个参数：参数
		第三个参数：成功的回调函数,返回的数据默认都是json格式
	*/
	$.get("./b1.json",null,function(data){
		data.forEach((item)=>{
			$("#t1").append($(`<tr><td>${item.id}</td><td>${item.name}</td></tr>`));
		})
	});
	
	/*
		load方法将加载到的数据直接放到页面上
	*/
   $("div").load("./b2.json",null,function(data){})
   
   /*
	ajax方法   功能是最齐全的
   */
  $.ajax({
	  // 本地文件请求只能用get不能用post
	  method:"get",
	  url:"./b1.json",
	  success:function(data){
		  data.forEach((item)=>{
		  	$("#t1").append($(`<tr><td>${item.id}</td><td>${item.name}</td></tr>`));
		  })
	  },
	  datatype:"json",
	  fail:function(){
		  console.log("失败的回调")
	  }
  })
```















