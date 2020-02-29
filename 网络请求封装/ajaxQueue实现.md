在传统开发中，对于多个ajax有时候需要按顺序发起请求。

虽然现在可以通过 多层Promise的then方法 或者 async+await 来实现按顺序发起请求，但我们有必要学习一下如何通过队列实现同步ajax。

使用方法：

```
$.newAjaxQueue().post([url], [params], [callback]).post( [url], [params], [callback] ).post( [url], [params], [callback] );
```

- 首先创建一个ajax queue
- 为队列添加需要执行的ajax操作，如post
- 队列里的ajax会按顺序执行，一个执行完毕获得返回值再继续执行下一个


实现源码：

```javascript

$.newAjaxQueue = function () {
  var queue = [];
  var posting = false;
  var fn = function() {
    if(queue.length) {
      posting = true;
      var request = queue.shift();
      var url = request.url;
      var params = request.params;
      var callback = request.callback;
      if(typeof(params) === 'function') { 
        callback = params; params = {}; 
      }

      $.post(url, params, function(res, status, xhr) {
        try {
          if(typeof(callback) === 'function') { callback(res); }
        } finally {
          fn();
          posting = false;
        }
      }, 'text');
    }
  };
  var instance = ({
    post: function(url ,params, callback) {
      queue.push({
        url: url, 
        params: params,
        callback: callback
      });
      if(posting === false) fn();
       return instance;
    }
  });
  return instance;
}

```
