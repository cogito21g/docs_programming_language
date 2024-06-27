### 4. Node.js

#### 4.2. 고급 Node.js

##### 4.2.1. Express.js 프레임워크

Express.js는 Node.js를 위한 빠르고 간단한 웹 프레임워크로, 웹 애플리케이션과 API를 쉽게 구축할 수 있도록 도와줍니다. 다음은 Express.js의 주요 기능과 사용 방법을 다룹니다.

###### Express.js 설치 및 기본 설정

1. **Express.js 설치**

   npm을 사용하여 Express.js를 설치합니다.

   ```bash
   npm install express
   ```

2. **기본 서버 설정**

   Express.js를 사용하여 기본 서버를 설정합니다.

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

###### 라우팅

1. **기본 라우팅**

   Express.js를 사용하여 다양한 경로에 대한 라우팅을 설정합니다.

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
     res.send('Home Page');
   });

   app.get('/about', (req, res) => {
     res.send('About Page');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

2. **URL 파라미터**

   URL 파라미터를 사용하여 동적인 경로를 처리할 수 있습니다.

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/user/:id', (req, res) => {
     const userId = req.params.id;
     res.send(`User ID: ${userId}`);
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

3. **쿼리 파라미터**

   쿼리 파라미터를 사용하여 추가 데이터를 전달할 수 있습니다.

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/search', (req, res) => {
     const query = req.query.q;
     res.send(`Search query: ${query}`);
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

###### 미들웨어

1. **미들웨어 사용**

   미들웨어는 요청과 응답 사이의 처리를 담당하는 함수입니다. Express.js에서는 다양한 내장 미들웨어와 커스텀 미들웨어를 사용할 수 있습니다.

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   // 내장 미들웨어 사용
   app.use(express.json());

   // 커스텀 미들웨어
   app.use((req, res, next) => {
     console.log(`${req.method} ${req.url}`);
     next();
   });

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

2. **에러 처리 미들웨어**

   에러 처리 미들웨어는 네 개의 인자를 받는 특별한 미들웨어 함수입니다.

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
     throw new Error('Something went wrong!');
   });

   // 에러 처리 미들웨어
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(500).send('Internal Server Error');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

###### 템플릿 엔진

1. **템플릿 엔진 설정**

   템플릿 엔진을 사용하여 동적 HTML 페이지를 렌더링할 수 있습니다. 예를 들어, Pug 템플릿 엔진을 사용할 수 있습니다.

   ```bash
   npm install pug
   ```

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.set('view engine', 'pug');
   app.set('views', './views');

   app.get('/', (req, res) => {
     res.render('index', { title: 'Home', message: 'Hello, World!' });
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

   ```pug
   // views/index.pug
   doctype html
   html
     head
       title= title
     body
       h1= message
   ```

###### 예제

1. **Express.js 설치 및 기본 서버 설정 예제**

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

2. **URL 파라미터 및 쿼리 파라미터 예제**

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/user/:id', (req, res) => {
     const userId = req.params.id;
     res.send(`User ID: ${userId}`);
   });

   app.get('/search', (req, res) => {
     const query = req.query.q;
     res.send(`Search query: ${query}`);
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

3. **미들웨어 사용 및 에러 처리 예제**

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.use(express.json());

   app.use((req, res, next) => {
     console.log(`${req.method} ${req.url}`);
     next();
   });

   app.get('/', (req, res) => {
     throw new Error('Something went wrong!');
   });

   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(500).send('Internal Server Error');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

4. **템플릿 엔진 설정 예제**

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.set('view engine', 'pug');
   app.set('views', './views');

   app.get('/', (req, res) => {
     res.render('index', { title: 'Home', message: 'Hello, World!' });
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

   ```pug
   // views/index.pug
   doctype html
   html
     head
       title= title
     body
       h1= message
   ```

##### 연습문제와 해답

1. **Express.js를 설치하고, "Hello, World!"를 반환하는 기본 서버를 설정하세요.**

   ```bash
   npm install express
   ```

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

2. **URL 파라미터와 쿼리 파라미터를 사용하여 데이터를 반환하는 라우트를 생성하세요.**

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.get('/user/:id', (req, res) => {
     const userId = req.params.id;
     res.send(`User ID: ${userId}`);
   });

   app.get('/search', (req, res) => {
     const query = req.query.q;
     res.send(`Search query: ${query}`);
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

3. **커스텀 미들웨어와 에러 처리 미들웨어를 사용하여 요청을 로깅하고 에러를 처리하세요.**

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.use(express.json());

   app.use((

req, res, next) => {
     console.log(`${req.method} ${req.url}`);
     next();
   });

   app.get('/', (req, res) => {
     throw new Error('Something went wrong!');
   });

   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(500).send('Internal Server Error');
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

4. **Pug 템플릿 엔진을 사용하여 동적 HTML 페이지를 렌더링하세요.**

   ```bash
   npm install pug
   ```

   ```javascript
   const express = require('express');
   const app = express();
   const PORT = process.env.PORT || 3000;

   app.set('view engine', 'pug');
   app.set('views', './views');

   app.get('/', (req, res) => {
     res.render('index', { title: 'Home', message: 'Hello, World!' });
   });

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

   ```pug
   // views/index.pug
   doctype html
   html
     head
       title= title
     body
       h1= message
   ```
