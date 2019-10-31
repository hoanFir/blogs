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

//basic use

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

```javascript

function getUserAccount() { return axios.get('/user/123') }

function getUserPermissions() { return axios.get('/user/123/permissions') }

axios.all([getUserAccount()， getUserPermissions()])
  .then(axios.spread(funciton (acct, perms) {
    //all requests are now complete
  }))

```
