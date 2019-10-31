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


### get/post request test

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
  .then(function (res) { /* handle success */ }
  .catch(funciton (err) { /* handle error */ })
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

### multiple concurrent requests 

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
paramsSerializer|an optional function in charge of serializing `params`|
transformRequest|\[function (data, headers) { transform and return data }\], this is only applicable for put, post and patch method. last function in the array must return a `string` or an instance of `ArrayBuffer`, `FormData`. tips: we may modify the headers object|
transformResponse|\[function (data) { transform and return data }\], the return data will be passed to then/catch|

### response schema

key|description|
--|--|
data|{}|
status|200, http status code|
statusText|'OK', http status message|
headers|{}|
config|{}, the config that was provided to request|
request|{}, the request that generated this response, it is an XMLHttpRequest instance the browser|


