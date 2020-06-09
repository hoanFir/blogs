Ajax，Asynchronous JavaScript And XML.


## 一、基于 XMLHttpRequest对象

XMLHttpRequest 对象用于和服务器交换数据。

- 支持在不重新加载页面的情况下更新网页

- 支持在页面已加载后从服务器请求/接收数据


栗子：

```javascript

var xmlhttp = null;

function request(url) {
  
  if(window.XMLHttpRequest) {
    xmlhttp = new XMLHttpRequest();
  } else if(window.ActiveXObject) {
    //老版本的 Internet Explorer（IE5/IE6）使用 ActiveX 对象：
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  } 
  
  if(xmlhttp != null) {
  
    xmlhttp.onreadystatechange=state_Change;
    
    xmlhttp.open("GET", url, true);    
    xmlhttp.send(null);
    
    //xmlhttp.open("POST", url, true);
    //xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    //xmlhttp.send("x=1&y=2")
    
  } else {
    alert("Your browser does not support XMLHTTP.");
  }
}

function state_Change() {
  if (xmlhttp.readyState==4) {
    if (xmlhttp.status == 200) {
      //...
    } else {
      alert("Problem getting data");
    }
  }
}


```

为什么需要 Async 参数？

```

该参数规定请求是否异步处理。

true - 表示脚本会在 send() 方法之后继续执行，而不等待来自服务器的响应，当响应就绪后再对响应进行处理。当使用 async=true 时，需要规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数

false - 如果在请求失败时是否执行其余的代码无关紧要，可以通过把该参数设置为 "false"。当使用 async=false 时，无需编写 onreadystatechange 函数，把代码放到 send() 语句后面即可


不过，XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true。毕竟是 asynchronous嘛

```


## Ajax大致流程

1. 创建 XMLHTTPRequest 对象，`var xhr = new XMLHttpRequest()`

2. 创建一个新的 http 请求，`xhr.open("get", url, true)`

3. 发送 http 请求

4. 设置响应 http 请求状态变化的函数

5. 获取异步请求返回的数据

状态：

- 0: 请求未初始化

- 1: 服务器连接已建立

- 2: 请求已接收

- 3: 请求处理中

- 4: 请求已完成，且响应已就绪



