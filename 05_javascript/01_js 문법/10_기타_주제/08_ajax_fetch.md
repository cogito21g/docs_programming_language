### 10. 기타 중요한 주제

#### 10.8. AJAX와 Fetch API

AJAX(Asynchronous JavaScript and XML)는 웹 페이지를 리로드하지 않고 서버와 비동기적으로 통신할 수 있게 하는 기술입니다. 전통적으로 `XMLHttpRequest` 객체를 사용하여 구현했지만, 최신 브라우저에서는 더 간편한 `Fetch API`를 사용할 수 있습니다. `Fetch API`는 더 직관적이고 사용하기 쉬운 인터페이스를 제공합니다.

##### AJAX (XMLHttpRequest)

1. **XMLHttpRequest 객체 생성**

   ```javascript
   const xhr = new XMLHttpRequest();
   ```

2. **요청 초기화**

   ```javascript
   xhr.open('GET', 'https://api.example.com/data', true);
   ```

3. **요청 전송**

   ```javascript
   xhr.send();
   ```

4. **응답 처리**

   ```javascript
   xhr.onreadystatechange = function () {
     if (xhr.readyState === XMLHttpRequest.DONE) {
       if (xhr.status === 200) {
         const data = JSON.parse(xhr.responseText);
         console.log(data);
       } else {
         console.error('Error:', xhr.statusText);
       }
     }
   };
   ```

###### 예제

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AJAX Example</title>
</head>
<body>
  <button id="loadData">Load Data</button>
  <div id="data"></div>
  <script>
    document.getElementById('loadData').addEventListener('click', function () {
      const xhr = new XMLHttpRequest();
      xhr.open('GET', 'https://jsonplaceholder.typicode.com/posts/1', true);
      xhr.send();

      xhr.onreadystatechange = function () {
        if (xhr.readyState === XMLHttpRequest.DONE) {
          if (xhr.status === 200) {
            const data = JSON.parse(xhr.responseText);
            document.getElementById('data').textContent = JSON.stringify(data);
          } else {
            console.error('Error:', xhr.statusText);
          }
        }
      };
    });
  </script>
</body>
</html>
```

##### Fetch API

1. **Fetch 요청**

   ```javascript
   fetch('https://api.example.com/data')
     .then(response => {
       if (!response.ok) {
         throw new Error('Network response was not ok ' + response.statusText);
       }
       return response.json();
     })
     .then(data => {
       console.log(data);
     })
     .catch(error => {
       console.error('There has been a problem with your fetch operation:', error);
     });
   ```

###### 예제

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fetch API Example</title>
</head>
<body>
  <button id="loadData">Load Data</button>
  <div id="data"></div>
  <script>
    document.getElementById('loadData').addEventListener('click', function () {
      fetch('https://jsonplaceholder.typicode.com/posts/1')
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok ' + response.statusText);
          }
          return response.json();
        })
        .then(data => {
          document.getElementById('data').textContent = JSON.stringify(data);
        })
        .catch(error => {
          console.error('There has been a problem with your fetch operation:', error);
        });
    });
  </script>
</body>
</html>
```

##### Fetch API의 주요 기능

1. **GET 요청**

   ```javascript
   fetch('https://api.example.com/data')
     .then(response => response.json())
     .then(data => console.log(data))
     .catch(error => console.error('Error:', error));
   ```

2. **POST 요청**

   ```javascript
   fetch('https://api.example.com/data', {
     method: 'POST',
     headers: {
       'Content-Type': 'application/json'
     },
     body: JSON.stringify({
       name: 'John Doe',
       age: 30
     })
   })
     .then(response => response.json())
     .then(data => console.log(data))
     .catch(error => console.error('Error:', error));
   ```

3. **요청 헤더 설정**

   ```javascript
   fetch('https://api.example.com/data', {
     method: 'GET',
     headers: {
       'Authorization': 'Bearer token',
       'Accept': 'application/json'
     }
   })
     .then(response => response.json())
     .then(data => console.log(data))
     .catch(error => console.error('Error:', error));
   ```

4. **비동기 함수와 함께 사용**

   ```javascript
   async function fetchData() {
     try {
       const response = await fetch('https://api.example.com/data');
       if (!response.ok) {
         throw new Error('Network response was not ok ' + response.statusText);
       }
       const data = await response.json();
       console.log(data);
     } catch (error) {
       console.error('There has been a problem with your fetch operation:', error);
     }
   }

   fetchData();
   ```

##### 연습문제와 해답

1. **AJAX를 사용하여 데이터 로드 및 출력**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>AJAX Practice</title>
   </head>
   <body>
     <button id="loadData">Load Data</button>
     <div id="data"></div>
     <script>
       document.getElementById('loadData').addEventListener('click', function () {
         const xhr = new XMLHttpRequest();
         xhr.open('GET', 'https://jsonplaceholder.typicode.com/posts/1', true);
         xhr.send();

         xhr.onreadystatechange = function () {
           if (xhr.readyState === XMLHttpRequest.DONE) {
             if (xhr.status === 200) {
               const data = JSON.parse(xhr.responseText);
               document.getElementById('data').textContent = JSON.stringify(data);
             } else {
               console.error('Error:', xhr.statusText);
             }
           }
         };
       });
     </script>
   </body>
   </html>
   ```

2. **Fetch API를 사용하여 데이터 로드 및 출력**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Fetch API Practice</title>
   </head>
   <body>
     <button id="loadData">Load Data</button>
     <div id="data"></div>
     <script>
       document.getElementById('loadData').addEventListener('click', function () {
         fetch('https://jsonplaceholder.typicode.com/posts/1')
           .then(response => {
             if (!response.ok) {
               throw new Error('Network response was not ok ' + response.statusText);
             }
             return response.json();
           })
           .then(data => {
             document.getElementById('data').textContent = JSON.stringify(data);
           })
           .catch(error => {
             console.error('There has been a problem with your fetch operation:', error);
           });
       });
     </script>
   </body>
   </html>
   ```

3. **POST 요청을 사용하여 데이터를 서버로 전송**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Fetch API POST Request</title>
   </head>
   <body>
     <button id="sendData">Send Data</button>
     <script>
       document.getElementById('sendData').addEventListener('click', function () {
         fetch('https://jsonplaceholder.typicode.com/posts', {
           method: 'POST',
           headers: {
             'Content-Type': 'application/json'
           },
           body: JSON.stringify({
             title: 'foo',
             body: 'bar',
             userId: 1
           })
         })
           .then(response => response.json())
           .then(data => console.log(data))
           .catch(error => console.error('Error:', error));
       });
     </script>
   </body>
   </html>
   ```
