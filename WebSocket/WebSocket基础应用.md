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
let connectedSuccess = false;
let calledClose = false;

const sayHelloBase = {
  type: 'event_message',
  body: {
    action: {
      code: 'code',
    },
    chatinfo: {
      mt: 'mt',
    },
    type: 'config',
  },
};

function messageWrapper(msg, type, to) => {
  const message = msg;
  
  if(type === 'waiter') {
    message.from.app = 'waiter';
    
    message.to.app = 'customer';
  } else if(type === 'customer') {
    message.from.app = 'customer';
    message.from.pin = 'hongzhenpeng';
    
    message.to.app = 'waiter';
    
    message.body && (message.body.chatinfo = {
      ...message.body.chatinfo,
      venderId: 1573090357357,
    })
    
    message.type = message.type || 'chat_message';
  } else if(type === 'artificial') {
    message.from.app = 'customer';
    message.from.pin = 'hongzhenpeng';
    
    message.to.app = 'waiter.artificial';
    
    message.body && (message.body.chatinfo = {
      ...message.body.chatinfo,
      venderId: 1573090357357,
    })
  }
  
  if(/artificial/.test(message.to.app)) {
    message.body.chatinfo.toMan = true;
  }
  
  return message;
}

function heartbeat = () => {
  
}

function onReady = () => {
  
}

function onError = (msg) => {
  console.log('Err: ', msg);
}

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
  close();


  webSocket = new WebSocket(socketUrl);
  
  const sayHelloMessage = messageWrapper(sayHelloBase, 'customer');
  
  webSocket.onopen = () => {
    socketRetry = 0;
    
    if(connectedSuccess === false) {
      webSocket.send(JSON.stringiy(sayHelloMessage));
    }
    
    heartbeat();
    
    onReady();
  };
  
  webSocket.onerror = (e) => {
    onError(e.message);
    
    if(calledClose === true) {
      calledClose = false;
      return;
    }
    
    if(socketRetry === 5) {
      onError('连接次数超过5次, 不再尝试重新连接.');
      return;
    }
    
    setTimeout(() => {
      socketRetry += 1;
      
      webSocket = new WebSocket(socketUrl);
    }, socketRetry * 1000);
  };
  
  webSocket.onmessage = (event) => {
  
  }
  
  window.addEventListener('online', async () => {
    online = true;
    
    webSocket = new WebSocket(socketUrl);
  })
  
  window.addEventListener('online', async () => {
    online = false;
    
    if(webSocket) {
      webSocket.close();
    }
  });

}

export default channel;

```
