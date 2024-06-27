### 10. 기타 중요한 주제

#### 10.4. JSON 다루기 (JSON.parse, JSON.stringify)

JSON (JavaScript Object Notation)은 데이터를 저장하고 전송하는 데 사용되는 경량 데이터 형식입니다. JSON은 JavaScript 객체와 유사한 구조를 가지고 있어, JavaScript에서 쉽게 다룰 수 있습니다. JavaScript에서 JSON을 다루기 위해 `JSON.parse`와 `JSON.stringify` 메서드를 사용할 수 있습니다.

##### JSON.parse

`JSON.parse` 메서드는 JSON 문자열을 JavaScript 객체로 변환합니다.

###### 예제

```javascript
const jsonString = '{"name": "John", "age": 30, "city": "New York"}';
const obj = JSON.parse(jsonString);

console.log(obj.name); // "John"
console.log(obj.age);  // 30
console.log(obj.city); // "New York"
```

##### JSON.stringify

`JSON.stringify` 메서드는 JavaScript 객체를 JSON 문자열로 변환합니다.

###### 예제

```javascript
const obj = {
  name: "John",
  age: 30,
  city: "New York"
};

const jsonString = JSON.stringify(obj);

console.log(jsonString); // '{"name":"John","age":30,"city":"New York"}'
```

##### JSON의 주요 특징

1. **키-값 쌍의 구조**: JSON은 데이터를 키-값 쌍으로 저장합니다.
2. **데이터 타입**: 문자열, 숫자, 객체, 배열, 불리언, null을 지원합니다.
3. **문자열 형식**: JSON 데이터는 문자열 형식으로 저장되고 전송됩니다.

##### JSON 사용 예제

1. **객체 배열을 JSON 문자열로 변환**

   ```javascript
   const users = [
     { name: "John", age: 30 },
     { name: "Jane", age: 25 },
     { name: "Doe", age: 40 }
   ];

   const jsonString = JSON.stringify(users);
   console.log(jsonString); // '[{"name":"John","age":30},{"name":"Jane","age":25},{"name":"Doe","age":40}]'
   ```

2. **JSON 문자열을 객체 배열로 변환**

   ```javascript
   const jsonString = '[{"name":"John","age":30},{"name":"Jane","age":25},{"name":"Doe","age":40}]';
   const users = JSON.parse(jsonString);

   users.forEach(user => {
     console.log(`${user.name} is ${user.age} years old.`);
   });
   // John is 30 years old.
   // Jane is 25 years old.
   // Doe is 40 years old.
   ```

3. **네트워크 요청에서 JSON 데이터 처리**

   ```javascript
   async function fetchData(url) {
     try {
       const response = await fetch(url);
       if (!response.ok) {
         throw new Error('Network response was not ok');
       }
       const data = await response.json(); // JSON 데이터를 객체로 변환
       console.log(data);
     } catch (error) {
       console.error('Fetch error:', error);
     }
   }

   fetchData('https://jsonplaceholder.typicode.com/users');
   ```

4. **로컬 스토리지에 JSON 데이터 저장 및 불러오기**

   ```javascript
   const user = {
     name: "John",
     age: 30,
     city: "New York"
   };

   // 객체를 JSON 문자열로 변환하여 로컬 스토리지에 저장
   localStorage.setItem('user', JSON.stringify(user));

   // 로컬 스토리지에서 JSON 문자열을 불러와 객체로 변환
   const storedUser = JSON.parse(localStorage.getItem('user'));

   console.log(storedUser); // { name: "John", age: 30, city: "New York" }
   ```

##### 연습문제와 해답

1. **JSON 문자열을 객체로 변환하고, 객체의 속성을 출력하세요.**

   ```javascript
   const jsonString = '{"title": "JavaScript", "author": "John Doe", "year": 2023}';
   const book = JSON.parse(jsonString);

   console.log(`Title: ${book.title}`); // "JavaScript"
   console.log(`Author: ${book.author}`); // "John Doe"
   console.log(`Year: ${book.year}`); // 2023
   ```

2. **객체를 JSON 문자열로 변환하고, 문자열을 출력하세요.**

   ```javascript
   const movie = {
     title: "Inception",
     director: "Christopher Nolan",
     year: 2010
   };

   const jsonString = JSON.stringify(movie);

   console.log(jsonString); // '{"title":"Inception","director":"Christopher Nolan","year":2010}'
   ```

3. **JSON 데이터를 네트워크 요청으로 받아와서 처리하세요.**

   ```javascript
   async function getPosts() {
     try {
       const response = await fetch('https://jsonplaceholder.typicode.com/posts');
       const posts = await response.json();
       posts.forEach(post => {
         console.log(`${post.title}`);
       });
     } catch (error) {
       console.error('Error fetching posts:', error);
     }
   }

   getPosts();
   ```

4. **로컬 스토리지에 JSON 데이터를 저장하고, 불러와서 처리하세요.**

   ```javascript
   const settings = {
     theme: "dark",
     notifications: true,
     fontSize: 14
   };

   // JSON 문자열로 변환하여 로컬 스토리지에 저장
   localStorage.setItem('settings', JSON.stringify(settings));

   // 로컬 스토리지에서 JSON 문자열을 불러와 객체로 변환
   const storedSettings = JSON.parse(localStorage.getItem('settings'));

   console.log(storedSettings); // { theme: "dark", notifications: true, fontSize: 14 }
   ```
