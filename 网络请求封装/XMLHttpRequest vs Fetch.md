

XMLHttpRequest: ajax、axios

Fetch: fetch. The kind of functionality was previously achieved using XMLHttpRequest.

### XMLHttpRequest

Use XMLHttpRequest(XHR) objects to interact with servers.

Why using XHR?

- With XHR, you can retrieve data from a URL without having to do a full page refresh. This enables a Web page to update just part of a page without disrupting what the user is doing.



### Using Fetch

Why using fetch?

- Fetch provides a better alternative that can be easily used by other technoligies sucs as Service Workers.

- Fetch provides a single logical place to defined other Http-related concepts such as CORS and extensions to HTTP.

what differs from ajax?

- fetch()返回的Promise在响应码为404或500时不被转化为reject，除了出现网络故障或请求被阻止时才会。其他情况都被标记为resolve。

- fetch()如果要从服务器端发送或接收任何cookies时，需要设置 credentials，因为默认是不会带上cookies。

how using fetch？

基础使用

```javascript

//using async+await
const response = await fetch('xxx'); //return a Response object
if(!response.ok) { //an accurate check that the fetch was successful
  throw New Error("network response was not ok");
}
const myJson = await response.json(); //it is just an http response, not the actual JSON, to extract the JSON body content from the response, we use the json().该方法在Body mixin中定义，被Request object和Response object都实现了
console.log(JSON.stringify(myJson));

//using promise
fetch('xxx')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  })
```

发送带凭据的请求

```javascript

//在同源或跨域源都携带凭证
fetch('xxx', {
  credentials: 'include'
})

//只在同源情况下
fetch('xxx', {
  credentials: 'same-origin'
}) 

//确保不携带凭据
fetch('xxx', {
  credentials: 'omit'
}) 

```

发送JSON数据

```javascript

const url = 'xxx'
const data = { username: 'example' }

try {
  const response = await fetch(url, {
    method: 'POST',
    body: JSON.stringify(data),
    headers: {
      'Content-Type': 'application/json'
    }
  })
  const json = response.json();
  console.log('Success:', JSON.stringify(json))
} catch (error) {
  console.log('Error', error)
}
```

上传一个文件或多个文件

```javascript

//upload a file
const formData = new FormData();
const fileField = document.querySelect('input[type="file"]');

formData.append('username', 'abc');
formData.append('avatar', fileField.files[0]);

try {
  const response = await fetch(url, {
    method: 'PUT',
    body: formData
  })
  const json = response.json();
  console.log('Success:', JSON.stringify(json))
} catch (error) {
  console.log('Error', error)
}

//upload multiple files
const formData = new FormData();
const fileField = document.querySelect('input[type="file"][multiple]');

formData.append('title', 'my photos');
for(let i = 0; i<fileField.files.length; i++) {
  formData.append('photos', fileField.files[i]); 
}


try {
  const response = await fetch(url, {
    method: 'PUT',
    body: formData
  })
  const json = response.json();
  console.log('Success:', JSON.stringify(json))
} catch (error) {
  console.log('Error', error)
}

```

自定义请求对象

```javascript

//using the Request constructor to create a request object
const myHeaders = new Headers();
const myInit = {
  method: 'GET',
  headers: myHeaders,
  mode: 'cors',
  cache: 'default'
};
const myRequest = new Request('xxx', myInit);
const response = await fetch(myRequest);
const myBlob = await response.blob();
const objectUrl = URL.createObjectURL(myBlob);
myImage.src = objectURL;

```

拷贝自定义请求对象

```javascript

const myRequestCopy = new Request(myRequest, myInit);

```

注意，拷贝Request对象是很有用的。因为request and response bodies 规定只能被使用一次。
