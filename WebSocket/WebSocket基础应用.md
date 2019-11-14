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

---

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
let heartbeatInterval = null;
let heartbeatType = 'client_heartbeat';
let online = true;
let disableCare = false;
let checkRange = 0;
let statusTimer = null;
let statusType = 'cfg.customCare';

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

//export default channel
const channel = ({
  socketUrl,
  onReady = () => {},
  onError = () => {},
  onMessage = () => {},
}) => {
  console.log(socketUrl); 
  // ws://product.company.com/channelMessage?venderId=1573090357357&cluster=PRODUCTUAT&aid=5A29EBDE0DB049D9BB426C6C2A0E6582
  
  
  //正在连接
  if(webSocket && webSocket.readyState === webSocket.CONNECTING) {
    console.log('retrying...');
    
    socketRetry -= 1;
    return;
  }
  // 处于连接状态
  if(webSocket && webSocket.readyState === webSocket.OPEN) {
    return;
  }
  
  
  // 先关闭通道，清除相关状态
  close();
  
  
  // 创建WebSocket
  webSocket = new WebSocket(socketUrl);
  
  
  const sayHelloMessage = messageWrapper(sayHelloBase, 'customer');
  
  webSocket.onopen = () => {
    socketRetry = 0;
    
    if(connectedSuccess === false) {
      webSocket.send(JSON.stringiy(sayHelloMessage));
    }
    
    //隔一段时间发送心跳, 保持连接
    heartbeat();
    
    //执行外部回调
    onReady();
  };
  
  webSocket.onerror = (e) => {
    //执行外部回调
    onError(e.message);
    
    if(calledClose === true) {
      calledClose = false;
      return;
    }
    
    if(socketRetry === 5) {
      onError('连接次数超过5次, 不再尝试重新连接.');
      return;
    }
    
    //尝试重连次数不超过5次就持续尝试创建连接
    setTimeout(() => {
      socketRetry += 1;
      
      webSocket = new WebSocket(socketUrl);
    }, socketRetry * 1000);
  };
  
  webSocket.onmessage = (event) => {
    let message = event.data;
    
    if(typeof message === 'string') {
      message = JSON.parse(message);
    }
    
    if(message.upid === sayHelloMessage.id) {
      connectedSuccss = true;
    }
    
    if(message.type === 'event_message' || message.type === 'chat_message') {
      if(!message.body) return;
      
      if(!disableCare) {
        if(message.body.type === 'transfer_artificial' || message.body.type === 'switch_waiter') {
          window.clearTimeout(statusTimer);
          disableCare = true;
        } else {
          //检测状态
          checkStatus();
        }
      }
    }
    
    //执行外部回调
    onMessage(message);
  };
  
  window.addEventListener('online', async () => {
    online = true;
    
    webSocket = new WebSocket(socketUrl);
  })
  
  window.addEventListener('offline', async () => {
    online = false;
    
    if(webSocket) {
      webSocket.close();
    };
  });
  
  return {
    close, // 关闭通道
    send, // 发送消息
    evaluate, // 发送评价
    readAck, // 发送已读回执
    getQuickEntry, // 获取快捷入口
    setHeartbeatType, // 修改heartbeat类型
    setCheckRange, // 修改检查状态的频率
  };

}

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
  window.clearInterval(heartbeatInterval);
  
  heartbeatInterval = setInterval(() => {
    if(webSocket && webSocket.readyState === WebSocket.OPEN) {
      webSocket.send(JSON.stringify(messageWrapper({
        type: heartbeatType, //"client_heartbeat"
      }, 'customer')));
    }
  }, 60000);
}

function close() {
    calledClose = true;
    
    try {
        if (webSocket) {
            webSocket.close();
        }
    } catch (e) {
        // do nothing
    } finally {
        webSocket = null;
        window.clearInterval(heartbeatInterval);
        window.clearTimeout(statusTimer);
    }
}

function checkStatus() {
  if(!checkRange) return false;
  
  window.clearTimeout(statusTimer);
  statusTimer = setTimeout(() => {
    webSocket.send(JSON.stringify(messageWrapper({
      type: 'event_message',
      body: {
        action: {
          code: statusType
        }
        type: 'config',
      },
    }, 'customer')));
    disableCare = true;
  }, checkRange);
}

function send(message) {
  if(!online) {
    return false;
  }
  
  if(typeof message === 'string') {
    return false;
  }
  
  const { id } = message;
  
  if(!id) {
    return false;
  }
  
  if(webSocket.readyState === WebSocket.OPEN) {
    webSocket.send(JSON.stringify(message));
    return true;
  }
  
  return false;
}

function evaluate() {}

async function getQuickEntry() {
    return send(messageWrapper(quickEntryBase, 'customer'));
}

function setHeartbeatType(type) {
    heartbeatType = type;
    
    webSocket.send(JSON.stringify(messageWrapper({
        type: heartbeatType,
    }, 'customer')));
}

function setCheckRange(second) {
    window.clearTimeout(statusTimer);
    checkRange = second;
}

export default channel;

```

