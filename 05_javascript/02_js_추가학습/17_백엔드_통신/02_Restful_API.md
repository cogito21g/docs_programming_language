### 17. 백엔드와의 통신

#### 17.2. RESTful API 디자인

RESTful API는 Representational State Transfer(REST) 아키텍처 스타일을 따르는 웹 서비스입니다. RESTful API는 HTTP 메서드를 사용하여 리소스의 상태를 조작하고, URL을 통해 리소스를 식별합니다. 이번 섹션에서는 RESTful API의 기본 개념, 디자인 원칙, 그리고 Express.js를 사용하여 RESTful API를 구축하는 방법을 학습하겠습니다.

##### 17.2.1. RESTful API 기본 개념

###### HTTP 메서드

RESTful API는 다음과 같은 HTTP 메서드를 사용하여 리소스를 조작합니다:

- **GET**: 리소스를 조회합니다.
- **POST**: 새로운 리소스를 생성합니다.
- **PUT**: 기존 리소스를 업데이트합니다.
- **DELETE**: 리소스를 삭제합니다.
- **PATCH**: 리소스의 일부를 업데이트합니다.

###### 리소스

리소스는 API에서 관리하는 데이터를 의미합니다. 리소스는 URL을 통해 고유하게 식별됩니다.

###### 엔드포인트

엔드포인트는 리소스에 접근할 수 있는 URL 경로입니다. 엔드포인트는 일반적으로 명사로 표현되며, 복수형을 사용합니다.

예: `/users`, `/posts`, `/comments`

##### 17.2.2. RESTful API 디자인 원칙

###### 1. 명사 사용

엔드포인트는 리소스를 나타내는 명사를 사용하여 명명합니다.

```http
GET /users
POST /users
GET /users/{id}
PUT /users/{id}
DELETE /users/{id}
```

###### 2. HTTP 메서드 사용

리소스의 상태를 조작하기 위해 적절한 HTTP 메서드를 사용합니다.

###### 3. 상태 코드 사용

HTTP 상태 코드를 사용하여 클라이언트에게 요청의 결과를 명확히 전달합니다.

- **200 OK**: 요청이 성공적으로 처리되었습니다.
- **201 Created**: 새로운 리소스가 성공적으로 생성되었습니다.
- **204 No Content**: 요청이 성공적으로 처리되었으나, 응답 본문이 없습니다.
- **400 Bad Request**: 클라이언트의 요청이 잘못되었습니다.
- **404 Not Found**: 요청한 리소스를 찾을 수 없습니다.
- **500 Internal Server Error**: 서버에서 에러가 발생했습니다.

###### 4. 필터링, 정렬, 페이징

리소스를 조회할 때 필터링, 정렬, 페이징을 지원합니다.

```http
GET /users?age=25&sort=name&page=2&limit=10
```

##### 17.2.3. Express.js를 사용한 RESTful API 구축

Express.js는 Node.js를 위한 간단하고 유연한 웹 프레임워크입니다. Express.js를 사용하여 RESTful API를 구축할 수 있습니다.

###### Express.js 설치

```bash
npm install express
```

###### RESTful API 예제

1. **기본 설정**

   ```javascript
   // server.js
   const express = require('express');
   const app = express();
   const port = 3000;

   app.use(express.json());

   app.listen(port, () => {
     console.log(`Server is running on http://localhost:${port}`);
   });
   ```

2. **리소스 정의**

   ```javascript
   // server.js
   const express = require('express');
   const app = express();
   const port = 3000;

   app.use(express.json());

   let users = [
     { id: 1, name: 'John Doe', age: 30 },
     { id: 2, name: 'Jane Doe', age: 25 },
   ];

   // GET /users
   app.get('/users', (req, res) => {
     res.status(200).json(users);
   });

   // GET /users/:id
   app.get('/users/:id', (req, res) => {
     const user = users.find(u => u.id === parseInt(req.params.id));
     if (!user) return res.status(404).send('User not found');
     res.status(200).json(user);
   });

   // POST /users
   app.post('/users', (req, res) => {
     const newUser = {
       id: users.length + 1,
       name: req.body.name,
       age: req.body.age,
     };
     users.push(newUser);
     res.status(201).json(newUser);
   });

   // PUT /users/:id
   app.put('/users/:id', (req, res) => {
     const user = users.find(u => u.id === parseInt(req.params.id));
     if (!user) return res.status(404).send('User not found');

     user.name = req.body.name;
     user.age = req.body.age;
     res.status(200).json(user);
   });

   // DELETE /users/:id
   app.delete('/users/:id', (req, res) => {
     const userIndex = users.findIndex(u => u.id === parseInt(req.params.id));
     if (userIndex === -1) return res.status(404).send('User not found');

     users.splice(userIndex, 1);
     res.status(204).send();
   });

   app.listen(port, () => {
     console.log(`Server is running on http://localhost:${port}`);
   });
   ```

##### 연습문제와 해답

1. **Express.js를 사용하여 간단한 RESTful API를 작성하세요.**

   ```javascript
   // server.js
   const express = require('express');
   const app = express();
   const port = 3000;

   app.use(express.json());

   let items = [
     { id: 1, name: 'Item 1', description: 'This is item 1' },
     { id: 2, name: 'Item 2', description: 'This is item 2' },
   ];

   // GET /items
   app.get('/items', (req, res) => {
     res.status(200).json(items);
   });

   // GET /items/:id
   app.get('/items/:id', (req, res) => {
     const item = items.find(i => i.id === parseInt(req.params.id));
     if (!item) return res.status(404).send('Item not found');
     res.status(200).json(item);
   });

   // POST /items
   app.post('/items', (req, res) => {
     const newItem = {
       id: items.length + 1,
       name: req.body.name,
       description: req.body.description,
     };
     items.push(newItem);
     res.status(201).json(newItem);
   });

   // PUT /items/:id
   app.put('/items/:id', (req, res) => {
     const item = items.find(i => i.id === parseInt(req.params.id));
     if (!item) return res.status(404).send('Item not found');

     item.name = req.body.name;
     item.description = req.body.description;
     res.status(200).json(item);
   });

   // DELETE /items/:id
   app.delete('/items/:id', (req, res) => {
     const itemIndex = items.findIndex(i => i.id === parseInt(req.params.id));
     if (itemIndex === -1) return res.status(404).send('Item not found');

     items.splice(itemIndex, 1);
     res.status(204).send();
   });

   app.listen(port, () => {
     console.log(`Server is running on http://localhost:${port}`);
   });
   ```
