
jsonp 是发起 get 请求实现跨域的一种纯前端的手段。

不管是 jquery 或 extjs，实现的 jsonp 其原理都是一样的。

### 背后的原理

前端

```javascript

var localHandler = function(data){
  console.log(data);
};

var script = document.createElement("script");
script.src = "http://remoteserver.com/remote.js?callback=localHandler";
    
document.body.insertBefore(script, document.body.firstChild);

```

服务器：动态根据参数添加 callback 放在 js 文件里

```

localHandler({"data": "response"});

```

运行后，调用成功，控制台输出


