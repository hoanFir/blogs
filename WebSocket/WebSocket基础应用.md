🐾 WebSocket基础应用

🕘 2019.10.14 由 hoanfirst 编辑

WebSocket API：

let webSocket = new WebSocket(socketUrl);

- webSocket.readyState === webSocket.CONNECTING || webSocket.OPEN

- webSocket.onopen = () => {}

- webSocket.send(JSON.stringify(message))

- webSocket.onerror = () => {}

- webSocket.onmessage = () => {}

- webSocket.close()

- ...

### Examples 1

```javascript

const webSocket = new WebSocket('ws://localhost:8080');

webSocket.addEventListener('open', (event) => {
  webSocket.send('hello');
});

webSocket.addEventListener('message', (event) => {
  console.log('message from server ', event.data);
});

```

### Examples 2

ws.js

```javascript

let webSocket = null;
let socketRetry = 0;

const channel = ({
  socketUrl,
}) => {
  console.log(socketUrl); 
  //ws://product.company.com/channelMessage?venderId=1573090357357&cluster=PRODUCTUAT&aid=5A29EBDE0DB049D9BB426C6C2A0E6582
  
  if(webSocket && webSocket.readyState === webSocket.CONNECTING) {
    console.log('retrying...');
    socketRetry -= 1;
    return;
  }
  
  if(webSocket && webSocket.readyState === webSocket.OPEN) {
    return;
  }
}

export default channel;

```
