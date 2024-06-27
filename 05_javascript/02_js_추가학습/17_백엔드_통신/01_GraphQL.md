### 17. 백엔드와의 통신

#### 17.1. GraphQL

GraphQL은 Facebook에서 개발한 쿼리 언어로, 클라이언트가 필요한 데이터를 정확하게 요청할 수 있도록 설계되었습니다. GraphQL은 단일 엔드포인트를 통해 다양한 쿼리를 처리할 수 있어 REST API보다 유연하게 데이터를 제공할 수 있습니다. 이번 섹션에서는 GraphQL의 기본 개념과 Apollo Client를 사용한 클라이언트 구현, 그리고 GraphQL 서버 구축을 다루겠습니다.

##### 17.1.1. GraphQL 기본 개념

###### GraphQL 스키마

GraphQL 스키마는 API의 데이터 구조를 정의합니다. 스키마는 타입(type), 쿼리(query), 변형(mutation)으로 구성됩니다.

```graphql
type Query {
  hello: String
  user(id: ID!): User
}

type User {
  id: ID!
  name: String
  age: Int
}

type Mutation {
  createUser(name: String!, age: Int!): User
}
```

###### GraphQL 쿼리

GraphQL 쿼리는 클라이언트가 요청할 데이터를 명시적으로 정의합니다.

```graphql
query {
  user(id: "1") {
    id
    name
    age
  }
}
```

###### GraphQL 변형

GraphQL 변형은 데이터를 변경하는 요청을 처리합니다.

```graphql
mutation {
  createUser(name: "John Doe", age: 30) {
    id
    name
    age
  }
}
```

##### 17.1.2. Apollo Client

Apollo Client는 GraphQL 클라이언트를 쉽게 구성하고 사용할 수 있도록 도와주는 라이브러리입니다.

###### Apollo Client 설치

```bash
npm install @apollo/client graphql
```

###### Apollo Client 설정

1. **ApolloProvider 설정**

   ```javascript
   // src/index.js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { ApolloProvider, InMemoryCache, ApolloClient } from '@apollo/client';
   import App from './App';

   const client = new ApolloClient({
     uri: 'http://localhost:4000/graphql',
     cache: new InMemoryCache(),
   });

   ReactDOM.render(
     <ApolloProvider client={client}>
       <App />
     </ApolloProvider>,
     document.getElementById('root')
   );
   ```

2. **GraphQL 쿼리 실행**

   ```javascript
   // src/App.js
   import React from 'react';
   import { useQuery, gql } from '@apollo/client';

   const GET_USER = gql`
     query GetUser($id: ID!) {
       user(id: $id) {
         id
         name
         age
       }
     }
   `;

   function App() {
     const { loading, error, data } = useQuery(GET_USER, {
       variables: { id: '1' },
     });

     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error: {error.message}</p>;

     return (
       <div>
         <h1>{data.user.name}</h1>
         <p>Age: {data.user.age}</p>
       </div>
     );
   }

   export default App;
   ```

##### 17.1.3. GraphQL 서버 구축

GraphQL 서버는 Apollo Server를 사용하여 쉽게 구축할 수 있습니다.

###### Apollo Server 설치

```bash
npm install apollo-server graphql
```

###### GraphQL 스키마 및 리졸버 정의

1. **스키마 정의**

   ```javascript
   // src/schema.js
   const { gql } = require('apollo-server');

   const typeDefs = gql`
     type Query {
       hello: String
       user(id: ID!): User
     }

     type User {
       id: ID!
       name: String
       age: Int
     }

     type Mutation {
       createUser(name: String!, age: Int!): User
     }
   `;

   module.exports = typeDefs;
   ```

2. **리졸버 정의**

   ```javascript
   // src/resolvers.js
   const users = [];

   const resolvers = {
     Query: {
       hello: () => 'Hello, world!',
       user: (_, { id }) => users.find(user => user.id === id),
     },
     Mutation: {
       createUser: (_, { name, age }) => {
         const user = { id: `${users.length + 1}`, name, age };
         users.push(user);
         return user;
       },
     },
   };

   module.exports = resolvers;
   ```

3. **서버 설정**

   ```javascript
   // src/index.js
   const { ApolloServer } = require('apollo-server');
   const typeDefs = require('./schema');
   const resolvers = require('./resolvers');

   const server = new ApolloServer({ typeDefs, resolvers });

   server.listen().then(({ url }) => {
     console.log(`🚀  Server ready at ${url}`);
   });
   ```

##### 연습문제와 해답

1. **Apollo Client를 사용하여 사용자 데이터를 가져오는 React 애플리케이션을 작성하세요.**

   ```javascript
   // src/index.js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { ApolloProvider, InMemoryCache, ApolloClient } from '@apollo/client';
   import App from './App';

   const client = new ApolloClient({
     uri: 'http://localhost:4000/graphql',
     cache: new InMemoryCache(),
   });

   ReactDOM.render(
     <ApolloProvider client={client}>
       <App />
     </ApolloProvider>,
     document.getElementById('root')
   );
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import { useQuery, gql } from '@apollo/client';

   const GET_USER = gql`
     query GetUser($id: ID!) {
       user(id: $id) {
         id
         name
         age
       }
     }
   `;

   function App() {
     const { loading, error, data } = useQuery(GET_USER, {
       variables: { id: '1' },
     });

     if (loading) return <p>Loading...</p>;
     if (error) return <p>Error: {error.message}</p>;

     return (
       <div>
         <h1>{data.user.name}</h1>
         <p>Age: {data.user.age}</p>
       </div>
     );
   }

   export default App;
   ```

2. **GraphQL 서버를 구축하고 사용자 데이터를 관리하세요.**

   ```javascript
   // src/schema.js
   const { gql } = require('apollo-server');

   const typeDefs = gql`
     type Query {
       hello: String
       user(id: ID!): User
     }

     type User {
       id: ID!
       name: String
       age: Int
     }

     type Mutation {
       createUser(name: String!, age: Int!): User
     }
   `;

   module.exports = typeDefs;
   ```

   ```javascript
   // src/resolvers.js
   const users = [];

   const resolvers = {
     Query: {
       hello: () => 'Hello, world!',
       user: (_, { id }) => users.find(user => user.id === id),
     },
     Mutation: {
       createUser: (_, { name, age }) => {
         const user = { id: `${users.length + 1}`, name, age };
         users.push(user);
         return user;
       },
     },
   };

   module.exports = resolvers;
   ```

   ```javascript
   // src/index.js
   const { ApolloServer } = require('apollo-server');
   const typeDefs = require('./schema');
   const resolvers = require('./resolvers');

   const server = new ApolloServer({ typeDefs, resolvers });

   server.listen().then(({ url }) => {
     console.log(`🚀  Server ready at ${url}`);
   });
   ```
