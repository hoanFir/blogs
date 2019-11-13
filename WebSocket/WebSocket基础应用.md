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

```javascript

```
