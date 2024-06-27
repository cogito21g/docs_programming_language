### 4. Node.js

#### 4.1. Node.js 기초

##### 4.1.5. HTTP 모듈로 서버 만들기

Node.js의 `http` 모듈을 사용하면 간단하게 HTTP 서버를 만들 수 있습니다. HTTP 서버는 웹 요청을 처리하고, 응답을 반환하는 역할을 합니다.

###### 기본 HTTP 서버 생성

1. **HTTP 모듈 가져오기**

   `http` 모듈을 가져와서 사용합니다.

   ```javascript
   const http = require('http');
   ```

2. **서버 생성**

   `http.createServer` 메서드를 사용하여 서버를 생성합니다. 이 메서드는 요청(request)과 응답(response) 객체를 인자로 받는 콜백 함수를 사용합니다.

   ```javascript
   const server = http.createServer((req, res) => {
     res.statusCode = 200;
     res.setHeader('Content-Type', 'text/plain');
     res.end('Hello, World!\n');
   });
   ```

3. **서버 리스닝**

   서버가 특정 포트에서 리스닝하도록 설정합니다.

   ```javascript
   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

4. **전체 코드 예제**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.statusCode = 200;
     res.setHeader('Content-Type', 'text/plain');
     res.end('Hello, World!\n');
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

###### 라우팅 처리

1. **URL과 메서드에 따라 다른 응답 반환**

   요청의 URL과 HTTP 메서드(GET, POST 등)에 따라 다른 응답을 반환할 수 있습니다.

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.setHeader('Content-Type', 'text/plain');
     
     if (req.method === 'GET' && req.url === '/') {
       res.statusCode = 200;
       res.end('Home Page\n');
     } else if (req.method === 'GET' && req.url === '/about') {
       res.statusCode = 200;
       res.end('About Page\n');
     } else {
       res.statusCode = 404;
       res.end('Not Found\n');
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

###### JSON 응답 처리

1. **JSON 응답 반환**

   응답 헤더를 `application/json`으로 설정하고, JSON 데이터를 반환할 수 있습니다.

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.setHeader('Content-Type', 'application/json');
     
     if (req.method === 'GET' && req.url === '/data') {
       res.statusCode = 200;
       const data = {
         message: 'Hello, World!',
         date: new Date()
       };
       res.end(JSON.stringify(data));
     } else {
       res.statusCode = 404;
       res.end(JSON.stringify({ error: 'Not Found' }));
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

###### POST 요청 처리

1. **POST 요청 데이터 읽기**

   `req.on('data')`와 `req.on('end')` 이벤트를 사용하여 POST 요청 데이터를 읽을 수 있습니다.

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     if (req.method === 'POST' && req.url === '/submit') {
       let body = '';

       req.on('data', chunk => {
         body += chunk.toString();
       });

       req.on('end', () => {
         res.setHeader('Content-Type', 'application/json');
         res.statusCode = 200;
         res.end(JSON.stringify({ receivedData: body }));
       });
     } else {
       res.statusCode = 404;
       res.end('Not Found\n');
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

###### 예제

1. **기본 HTTP 서버 생성 예제**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.statusCode = 200;
     res.setHeader('Content-Type', 'text/plain');
     res.end('Hello, World!\n');
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

2. **라우팅 처리 예제**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.setHeader('Content-Type', 'text/plain');
     
     if (req.method === 'GET' && req.url === '/') {
       res.statusCode = 200;
       res.end('Home Page\n');
     } else if (req.method === 'GET' && req.url === '/about') {
       res.statusCode = 200;
       res.end('About Page\n');
     } else {
       res.statusCode = 404;
       res.end('Not Found\n');
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

3. **JSON 응답 처리 예제**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.setHeader('Content-Type', 'application/json');
     
     if (req.method === 'GET' && req.url === '/data') {
       res.statusCode = 200;
       const data = {
         message: 'Hello, World!',
         date: new Date()
       };
       res.end(JSON.stringify(data));
     } else {
       res.statusCode = 404;
       res.end(JSON.stringify({ error: 'Not Found' }));
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

4. **POST 요청 처리 예제**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     if (req.method === 'POST' && req.url === '/submit') {
       let body = '';

       req.on('data', chunk => {
         body += chunk.toString();
       });

       req.on('end', () => {
         res.setHeader('Content-Type', 'application/json');
         res.statusCode = 200;
         res.end(JSON.stringify({ receivedData: body }));
       });
     } else {
       res.statusCode = 404;
       res.end('Not Found\n');
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

##### 연습문제와 해답

1. **기본 HTTP 서버를 생성하고, "Hello, World!" 메시지를 반환하세요.**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.statusCode = 200;
     res.setHeader('Content-Type', 'text/plain');
     res.end('Hello, World!\n');
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

2. **URL과 메서드에 따라 다른 응답을 반환하는 HTTP 서버를 생성하세요.**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.setHeader('Content-Type', 'text/plain');
     
     if (req.method === 'GET' && req.url === '/') {
       res.statusCode = 200;
       res.end('Home Page\n');
     } else if (req.method === 'GET' && req.url === '/about') {
       res.statusCode = 200;
       res.end('About Page\n');
     } else {
       res.statusCode = 404;
       res.end('Not Found\n');
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

3. **JSON 응답을 반환하는 HTTP 서버를 생성하세요.**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     res.setHeader('Content-Type', 'application/json');
     
     if (req.method === 'GET' && req.url === '/data') {
       res.statusCode = 200;
       const data = {
         message: 'Hello, World!',
         date: new Date()
       };
       res.end(JSON.stringify(data));
     } else {
       res.statusCode = 404;
       res.end(JSON.stringify({ error: 'Not Found' }));
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

4. **POST 요청을 처리하고, 받은 데이터를 JSON으로 반환하는 HTTP 서버를 생성하세요.**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
     if (req.method === 'POST' && req.url === '/submit') {
       let body = '';

       req.on('data', chunk => {
         body += chunk.toString();
       });

       req.on('end', () => {
         res.setHeader('Content-Type', 'application/json');
         res.statusCode = 200;
         res.end(JSON.stringify({ receivedData: body }));
       });
     } else {
       res.statusCode = 404;
       res.end('Not Found\n');
     }
   });

   const PORT = 3000;
   server.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```
