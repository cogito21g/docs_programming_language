### 17. 백엔드와의 통신

#### 17.3. WebSocket을 이용한 실시간 통신

WebSocket은 클라이언트와 서버 간의 양방향 통신을 지원하는 프로토콜입니다. WebSocket을 사용하면 서버와 클라이언트 간에 지속적인 연결을 유지하여 실시간 데이터를 주고받을 수 있습니다. 이번 섹션에서는 WebSocket의 기본 개념과 Node.js를 사용하여 WebSocket 서버와 클라이언트를 구현하는 방법을 학습하겠습니다.

##### 17.3.1. WebSocket 기본 개념

- **Connection**: WebSocket은 클라이언트와 서버 간의 지속적인 연결을 유지합니다.
- **Message**: 클라이언트와 서버는 메시지를 주고받습니다. 메시지는 텍스트 또는 바이너리 데이터로 전송될 수 있습니다.
- **Close**: 연결은 클라이언트 또는 서버에 의해 닫힐 수 있습니다.

##### 17.3.2. Node.js를 사용한 WebSocket 서버 구축

###### WebSocket 서버 설정

1. **WebSocket 설치**

   ```bash
   npm install ws
   ```

2. **WebSocket 서버 구현**

   ```javascript
   // server.js
   const WebSocket = require('ws');

   const wss = new WebSocket.Server({ port: 8080 });

   wss.on('connection', ws => {
     console.log('Client connected');

     ws.on('message', message => {
       console.log(`Received: ${message}`);
       ws.send(`Echo: ${message}`);
     });

     ws.on('close', () => {
       console.log('Client disconnected');
     });
   });

   console.log('WebSocket server is running on ws://localhost:8080');
   ```

##### 17.3.3. WebSocket 클라이언트 구현

###### 브라우저 기반 WebSocket 클라이언트

1. **HTML 파일 작성**

   ```html
   <!-- index.html -->
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebSocket Client</title>
   </head>
   <body>
     <h1>WebSocket Client</h1>
     <input type="text" id="messageInput" placeholder="Enter a message">
     <button onclick="sendMessage()">Send</button>
     <div id="messages"></div>

     <script>
       const ws = new WebSocket('ws://localhost:8080');

       ws.onopen = () => {
         console.log('Connected to server');
       };

       ws.onmessage = event => {
         const messagesDiv = document.getElementById('messages');
         const messageElement = document.createElement('p');
         messageElement.textContent = `Received: ${event.data}`;
         messagesDiv.appendChild(messageElement);
       };

       ws.onclose = () => {
         console.log('Disconnected from server');
       };

       function sendMessage() {
         const input = document.getElementById('messageInput');
         const message = input.value;
         ws.send(message);
         input.value = '';
       }
     </script>
   </body>
   </html>
   ```

###### Node.js 기반 WebSocket 클라이언트

1. **WebSocket 클라이언트 구현**

   ```javascript
   // client.js
   const WebSocket = require('ws');

   const ws = new WebSocket('ws://localhost:8080');

   ws.on('open', () => {
     console.log('Connected to server');
     ws.send('Hello, server!');
   });

   ws.on('message', message => {
     console.log(`Received: ${message}`);
   });

   ws.on('close', () => {
     console.log('Disconnected from server');
   });
   ```

##### 연습문제와 해답

1. **WebSocket 서버를 설정하고 클라이언트와 통신하세요.**

   ```javascript
   // server.js
   const WebSocket = require('ws');

   const wss = new WebSocket.Server({ port: 8080 });

   wss.on('connection', ws => {
     console.log('Client connected');

     ws.on('message', message => {
       console.log(`Received: ${message}`);
       ws.send(`Echo: ${message}`);
     });

     ws.on('close', () => {
       console.log('Client disconnected');
     });
   });

   console.log('WebSocket server is running on ws://localhost:8080');
   ```

2. **브라우저에서 WebSocket 클라이언트를 구현하여 서버와 메시지를 주고받으세요.**

   ```html
   <!-- index.html -->
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebSocket Client</title>
   </head>
   <body>
     <h1>WebSocket Client</h1>
     <input type="text" id="messageInput" placeholder="Enter a message">
     <button onclick="sendMessage()">Send</button>
     <div id="messages"></div>

     <script>
       const ws = new WebSocket('ws://localhost:8080');

       ws.onopen = () => {
         console.log('Connected to server');
       };

       ws.onmessage = event => {
         const messagesDiv = document.getElementById('messages');
         const messageElement = document.createElement('p');
         messageElement.textContent = `Received: ${event.data}`;
         messagesDiv.appendChild(messageElement);
       };

       ws.onclose = () => {
         console.log('Disconnected from server');
       };

       function sendMessage() {
         const input = document.getElementById('messageInput');
         const message = input.value;
         ws.send(message);
         input.value = '';
       }
     </script>
   </body>
   </html>
   ```

3. **Node.js에서 WebSocket 클라이언트를 구현하여 서버와 메시지를 주고받으세요.**

   ```javascript
   // client.js
   const WebSocket = require('ws');

   const ws = new WebSocket('ws://localhost:8080');

   ws.on('open', () => {
     console.log('Connected to server');
     ws.send('Hello, server!');
   });

   ws.on('message', message => {
     console.log(`Received: ${message}`);
   });

   ws.on('close', () => {
     console.log('Disconnected from server');
   });
   ```
