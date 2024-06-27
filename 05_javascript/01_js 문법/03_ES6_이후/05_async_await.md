### 3. ES6 이후의 주요 기능

#### 3.5. async/await

`async`와 `await`는 ES8(ECMAScript 2017)에서 도입된 비동기 코드를 보다 쉽게 작성할 수 있게 하는 문법입니다. `async` 함수는 항상 프라미스를 반환하며, `await` 키워드는 프라미스가 해결될 때까지 기다립니다. 이를 통해 비동기 코드를 마치 동기 코드처럼 작성할 수 있습니다.

##### async 함수

`async` 키워드를 함수 선언 앞에 붙여 사용합니다. `async` 함수는 자동으로 프라미스를 반환하며, 반환 값은 `resolve`된 값이 됩니다.

```javascript
async function fetchData() {
  return "Data fetched";
}

fetchData().then(data => console.log(data)); // "Data fetched"
```

##### await

`await` 키워드는 `async` 함수 내에서만 사용할 수 있으며, 프라미스가 해결될 때까지 함수 실행을 일시 중지합니다.

```javascript
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("Data fetched");
    }, 2000);
  });
}

async function getData() {
  const data = await fetchData();
  console.log(data); // "Data fetched" (after 2 seconds)
}

getData();
```

##### try-catch 문과 async/await

`async` 함수 내에서 `await`를 사용할 때 발생할 수 있는 오류를 처리하려면 `try-catch` 문을 사용할 수 있습니다.

```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("Failed to fetch data");
    }, 2000);
  });
}

async function getData() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (error) {
    console.error("Error:", error);
  }
}

getData();
```

#### 예제

1. `async`와 `await`를 사용하여 비동기 데이터를 가져오는 함수를 작성하세요.
   ```javascript
   function fetchData() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data fetched");
       }, 1000);
     });
   }

   async function getData() {
     const data = await fetchData();
     console.log(data); // "Data fetched" (after 1 second)
   }

   getData();
   ```

2. 여러 비동기 작업을 순차적으로 처리하는 예제를 작성하세요.
   ```javascript
   function fetchData1() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 1 fetched");
       }, 1000);
     });
   }

   function fetchData2() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 2 fetched");
       }, 2000);
     });
   }

   async function getData() {
     const data1 = await fetchData1();
     console.log(data1); // "Data 1 fetched" (after 1 second)

     const data2 = await fetchData2();
     console.log(data2); // "Data 2 fetched" (after 3 seconds)
   }

   getData();
   ```

3. `try-catch`를 사용하여 비동기 함수에서 발생한 오류를 처리하는 예제를 작성하세요.
   ```javascript
   function fetchData() {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         reject("Failed to fetch data");
       }, 1000);
     });
   }

   async function getData() {
     try {
       const data = await fetchData();
       console.log(data);
     } catch (error) {
       console.error("Error:", error); // "Error: Failed to fetch data" (after 1 second)
     }
   }

   getData();
   ```

4. 여러 비동기 작업을 병렬로 처리하는 예제를 작성하세요.
   ```javascript
   function fetchData1() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 1 fetched");
       }, 1000);
     });
   }

   function fetchData2() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 2 fetched");
       }, 2000);
     });
   }

   async function getData() {
     const [data1, data2] = await Promise.all([fetchData1(), fetchData2()]);
     console.log(data1); // "Data 1 fetched" (after 2 seconds)
     console.log(data2); // "Data 2 fetched" (after 2 seconds)
   }

   getData();
   ```

#### 연습문제와 해답

1. `async`와 `await`를 사용하여 1초 후에 "Hello, World!"를 출력하는 함수를 작성하세요.
   ```javascript
   function fetchMessage() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Hello, World!");
       }, 1000);
     });
   }

   async function displayMessage() {
     const message = await fetchMessage();
     console.log(message); // "Hello, World!" (after 1 second)
   }

   displayMessage();
   ```

2. 비동기 함수 내에서 두 개의 데이터를 순차적으로 가져와서 출력하는 코드를 작성하세요.
   ```javascript
   function fetchData1() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 1");
       }, 1000);
     });
   }

   function fetchData2() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 2");
       }, 2000);
     });
   }

   async function displayData() {
     const data1 = await fetchData1();
     console.log(data1); // "Data 1" (after 1 second)

     const data2 = await fetchData2();
     console.log(data2); // "Data 2" (after 3 seconds)
   }

   displayData();
   ```

3. `try-catch`를 사용하여 비동기 함수에서 발생한 오류를 처리하는 코드를 작성하세요.
   ```javascript
   function fetchData() {
     return new Promise((resolve, reject) => {
       setTimeout(() => {
         reject("Error fetching data");
       }, 1000);
     });
   }

   async function getData() {
     try {
       const data = await fetchData();
       console.log(data);
     } catch (error) {
       console.error("Caught an error:", error); // "Caught an error: Error fetching data" (after 1 second)
     }
   }

   getData();
   ```

4. 여러 비동기 작업을 병렬로 처리하고 결과를 배열로 반환하는 코드를 작성하세요.
   ```javascript
   function fetchData1() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 1");
       }, 1000);
     });
   }

   function fetchData2() {
     return new Promise((resolve) => {
       setTimeout(() => {
         resolve("Data 2");
       }, 2000);
     });
   }

   async function getAllData() {
     const results = await Promise.all([fetchData1(), fetchData2()]);
     console.log(results); // ["Data 1", "Data 2"] (after 2 seconds)
   }

   getAllData();
   ```
