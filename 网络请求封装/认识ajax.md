一、Ajax
Asynchronous JavaScript and XML。

Ajax 是一种用于创建快速动态网页的技术。其最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。

常用于以下场景：

运用 JavaScript 渲染/操作 DOM 来执行动态效果
运用 XMLHttpRequest 或新的 Fetch API 来与服务器进行异步数据交互（json或xml）
流程：

Create a XMLHttprequest object -> httprequest -> Server -> process the returned data using javascript -> render page



1. 原生 JavaScript 实现 Ajax
var xmlhttp；
if(window.XMLHttpRequest) { xmlhttp = new XMLHttpRequest(); }
else{ xmlhttp = new ActiveXObject("Microsoft.XMLHTTP"); }

/* 版本的浏览器IE6 IE7等不支持XMLHttpRequest对象，因此要使用ActiveXObject()对象来实现 */

/* get */
xml.onreadystatechange = function() {
    if(xml.readyState==4&&xml.status==200) { var data = JSON.parse(xml.responseText); }
    else { alert("请求失败"); }        
}
xml.open("get", "url", "true"); //true表示置为异步请求
xml.send(null);

/* post */
xml.onreadystatechange = function(){
    if(xml.readyState==4&&xml.status==200) { var data=JSON.parse(xml.responseText); }
    else { alert("请求失败"); }        
}
xml.open("post", "url", "true");
xml.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

/* 默认情况下 Ajax 会以 Content-Type: text/plain 方式来提交数据，此时服务器将忽略 post body 部分的数据，导致服务器无法获得请求的数据 */

xml.send(null);



2. readState 和 status
XMLHttpRequest.readyState：请求的状态

0 - 未初始化，还没有调用send方法
1 - 载入，已调用send方法，正在发送
2 - 载入完成，send方法执行完成，已经接收到全部相应内容
3 - 交互，正在解析相应内容
4 - 完成，响应内容解析完成，可以在客户端调用
XMLHttpRequest.status：请求过程中的状态说明

2xx - 成功
1xx - 表示临时响应，客户端在收到常规响应时，应配置接收一个或多个1xx 响应
3xx - 表示重定向，客户端必须采取更多操作来实现请求。例如，浏览器不得不请求服务器上的不同的页面，或需要通过代理服务器重复该请求。如 304 表示可以使用缓存，该请求重复了
4xx - 表示客户端错误。例如，客户端请求不存在的页面，或客户端未提供有效的身份验证信息
5xx - 表示服务器错误



