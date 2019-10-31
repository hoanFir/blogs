🐾 axios基本使用

🕘 2019.10.31 由 hoanfirst 编辑


### axios的特性

Promise based HTTP client for the browser and node.js

- 使用Promise管理异步

- node（http）和浏览器（XMLHttpRequest）同一套api

- 支持拦截器（intercept request and response）等高级配置

- 支持取消请求

- 自动转换json数据

- 客户端支持预防csrf


### basic use of get/post request

```javascript

const axios = require('axios');

//basic use - axios(config)

axios({
  method:'get',
  url:'//xxx.png',
  responseType:'stream'
})
  .then(function (response) {
    /* handle success */
  });

axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
})
  .then(function (res) {
    /* handle success */
  });


//basic use - axios(url[, config])

axios('/user/123'); //send a default get request

//basic use - axios.get(url[, config]) and axios.post(url[, data[, config]])

axios.get('/user?id=123') or
axios.get('/user', { params: { id: 123 } }) or
axios.post('/user', { id: 123 })
  .then(function (res) { /* handle success */ })
  .catch(funciton (err) { /* handle error */ }) or
  .then(function (res) { /* handle success */ }, function(err) { /* handle error */ })
  .then(function() { /* always executed */ })


//use async

async function getUser() {
  try {
    const res = await axios.get('/user?id=123');
    /* handle success */
  } catch (err) {
    /* handle error */
  }
}

```

### use of intercept requests or response

before then or catch.

```javascript

import axios from 'axios';

axios.interceptor.request.use(function(config) {
  /* do something before request is sent. */
  config.headers['Content-Type'] = 'application/json;charset=UTF-8';
  config.headers['X-Request-With'] = 'XMLHttpRequest';
  
  return config;
}, function(error) {
  /* do something with request error. */

  return Promise.reject(error);
});

axios.interceptors.response.use(function(res) {
  /* do something with response data. */
  console.log(res);
  
  return res;
}, function(error) {
  /* do something with response error. */

  rerturn Promise.reject(error);
})

const Axios = {
  get: (url, option) => {
    return new Promise(function(resolve, reject) {
      axios.get(url, { params: option }).then(function(res) {
        resolve.apply(undefined, arguments);
      }).catch(function() {
        resolve.apply(undefined, arguments);
      })
    })
  },
  post: (url, option) => {
    return new Promise(function (resolve, reject) {
        axios.post(url, option).then(function (res) {
        
            //after response interceptor
            resolve.apply(undefined, arguments);
        }).catch(function () {
        
            //after response interceptor
            resolve.apply(undefined, arguments);
        });
    });
  },
}

export default Axios;

```

### use of multiple concurrent requests

基于Promise.all()。

axios.all(iterable) and axios.spread(callback).

```javascript

function getUserAccount() { return axios.get('/user/123') }

function getUserPermissions() { return axios.get('/user/123/permissions') }

axios.all([getUserAccount()， getUserPermissions()])
  .then(axios.spread(funciton (acct, perms) {
    //all requests are now complete
  }))

```

### 设置application/x-www-form-urlencoded格式

在axios中，默认会将js对象序列化成json格式，如果要发送application/x-www-form-urlencoded格式，如下：

```javascript

//方法1

const params = new URLSearchParams();
params.append('param1', 'value1');
params.append('param2', 'value2');
axios.post('/foo', params);

tips：URLSearchParams is not supported by all browsers, but there is a polyfill available.

//方法2

import qs from 'qs';
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify({ 'bar': 123 }),
  url,
};
axios(options);

```

### 捕获axios的errors

在axios中，只要返回的http status code不是2xx，就会执行catch()或reject()。

```javascript

axios.post('/user', { id: 123 })
  .then(function (res) { /* handle success */ })
  .catch(funciton (err) {
    /* handle error */ 
    
    if(err.response) {
      /* the request was made and the server responded with a status code that falls out of the range of 2xx */
      console.log(err.response.data);
      console.log(err.response.status);
      console.log(err.response.headers);      
    } else if (err.request) {
      /* the request was made but no response was received */
      console.log(err.request);
      
      /* err.request is an XMLHttpRequest instance in the browser */
    } else {
      /* something happend(that triggered an Error) in setting up the request  */
      console.log(err.message);
    }
  })

```

在axios中，通过设置request中validateStatus来自定义http status code会报错的范围。


```javascript

axios.get('/user/123', {
  validateStatus: function (status) {
    // Reject only if the status code is greater than or equal to 500
    return status < 500;
  }
})

```

### request config

key|description|
--|--|
url|only required|
method|request method|
baseURL|will be prepended to url unless url is absolute|
headers|{''X-Requested-With': 'XMLHttpRequest'}|
params|url parameters, must be a `plain object` or a `URLSearchParams object`|
data|request body, only applicable for request methods 'PUT', 'POST', and 'PATCH', and it must be a one of the following types: `string`, `plain object`, `ArrayBuffer`, `ArrayBufferView`, `URLSearchParams`, `FormData(Browser only)`, `File(Browser only)`, `Blob(Browser only)`|
timeout|if the request takes longer than `timeout`, the request will be aborted|
withCredentials|indicates whether or not cross-site Access-Control requests should be made using credentials|
responseType|indicates encoding to use for decoding response, default utf8|
maxContentLength|defines the max size of the http response content in bytes allowed|
proxy|defines the hostname and port of the proxy server. this will set ad `Proxy-Authorization` header, overwriting any existing `Proxy-Authorization` custom headers you have set using `headers`|
onUploadProgress|function(progressEvent) {}, allows handling of progress events for uploads|
onDownloadProgress|function(progressEvent) {}, allows handling of progress events for downloads|
validateStatus|function(status){ return status>=200&&status<300; }, defines whether to resolve or reject the promise for a given http response status code.|
paramsSerializer|an optional function in charge of serializing `params`|
transformRequest|\[function (data, headers) { transform and return data }\], this is only applicable for put, post and patch method. last function in the array must return a `string` or an instance of `ArrayBuffer`, `FormData`. tips: we may modify the headers object|
transformResponse|\[function (data) { transform and return data }\], the return data will be passed to then/catch|
adapter|--|
auth|--|
xsrfCookieName|--|
cancelToken|--|

使用`axios.defaults`设置默认request config：

```javascript

axios.defaults.baseURL = 'https://api.example.com';

axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';

axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;

```

### response schema

key|description|
--|--|
response.data|{}|
response.status|200, http status code|
response.statusText|'OK', http status message|
response.headers|{}|
response.config|{}, the config that was provided to request|
request|{}, the request that generated this response, it is an XMLHttpRequest instance in the browser|


