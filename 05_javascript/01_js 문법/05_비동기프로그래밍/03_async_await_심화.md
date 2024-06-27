### 5. 비동기 프로그래밍

#### 5.3. async/await 심화

`async`와 `await`는 비동기 코드를 작성하는 데 매우 유용합니다. 이 섹션에서는 `async`와 `await`를 사용하여 복잡한 비동기 작업을 처리하는 방법과 몇 가지 고급 사용법을 다루겠습니다.

##### 여러 개의 비동기 작업을 순차적으로 처리

여러 개의 비동기 작업을 순차적으로 처리할 때는 `await`를 사용하여 각 작업이 완료될 때까지 기다립니다.

```javascript
async function fetchDataSequentially() {
  const data1 = await fetchData1();
  console.log(data1); // "Data 1"

  const data2 = await fetchData2();
  console.log(data2); // "Data 2"

  const data3 = await fetchData3();
  console.log(data3); // "Data 3"
}

fetchDataSequentially();
```

##### 여러 개의 비동기 작업을 병렬로 처리

여러 개의 비동기 작업을 병렬로 처리할 때는 `Promise.all`을 사용하여 모든 작업이 완료될 때까지 기다립니다.

```javascript
async function fetchDataInParallel() {
  const [data1, data2, data3] = await Promise.all([fetchData1(), fetchData2(), fetchData3()]);
  console.log(data1); // "Data 1"
  console.log(data2); // "Data 2"
  console.log(data3); // "Data 3"
}

fetchDataInParallel();
```

##### 오류 처리

`try-catch` 문을 사용하여 `async` 함수 내에서 발생하는 오류를 처리할 수 있습니다.

```javascript
async function fetchDataWithErrorHandling() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (error) {
    console.error("Error fetching data:", error);
  }
}

fetchDataWithErrorHandling();
```

##### 중첩된 async/await

중첩된 `async` 함수 호출에서 비동기 작업을 처리할 때도 `await`를 사용하여 순차적으로 작업을 처리할 수 있습니다.

```javascript
async function fetchData1() {
  return new Promise((resolve) => setTimeout(resolve, 1000, "Data 1"));
}

async function fetchData2() {
  return new Promise((resolve) => setTimeout(resolve, 2000, "Data 2"));
}

async function fetchDataWithNestedAsync() {
  const data1 = await fetchData1();
  console.log(data1); // "Data 1"

  const data2 = await fetchData2();
  console.log(data2); // "Data 2"
}

fetchDataWithNestedAsync();
```

#### 예제

1. `async`와 `await`를 사용하여 두 개의 비동기 작업을 순차적으로 처리하는 코드를 작성하세요.
   ```javascript
   async function fetchSequentialData() {
     const data1 = await new Promise((resolve) => setTimeout(resolve, 1000, 'First data'));
     console.log(data1); // "First data"
   
     const data2 = await new Promise((resolve) => setTimeout(resolve, 2000, 'Second data'));
     console.log(data2); // "Second data"
   }
   
   fetchSequentialData();
   ```

2. `Promise.all`과 `async/await`를 사용하여 세 개의 비동기 작업을 병렬로 처리하는 코드를 작성하세요.
   ```javascript
   async function fetchParallelData() {
     const [data1, data2, data3] = await Promise.all([
       new Promise((resolve) => setTimeout(resolve, 1000, 'Data 1')),
       new Promise((resolve) => setTimeout(resolve, 2000, 'Data 2')),
       new Promise((resolve) => setTimeout(resolve, 3000, 'Data 3'))
     ]);
   
     console.log(data1); // "Data 1"
     console.log(data2); // "Data 2"
     console.log(data3); // "Data 3"
   }
   
   fetchParallelData();
   ```

3. `try-catch`를 사용하여 `async` 함수에서 발생하는 오류를 처리하는 코드를 작성하세요.
   ```javascript
   async function fetchDataWithTryCatch() {
     try {
       const data = await new Promise((_, reject) => setTimeout(reject, 1000, 'Fetch error'));
       console.log(data);
     } catch (error) {
       console.error('Error:', error); // "Error: Fetch error"
     }
   }
   
   fetchDataWithTryCatch();
   ```

4. 중첩된 `async/await`를 사용하여 비동기 작업을 순차적으로 처리하는 코드를 작성하세요.
   ```javascript
   async function fetchData1() {
     return new Promise((resolve) => setTimeout(resolve, 1000, 'First data'));
   }
   
   async function fetchData2() {
     return new Promise((resolve) => setTimeout(resolve, 2000, 'Second data'));
   }
   
   async function fetchNestedAsyncData() {
     const data1 = await fetchData1();
     console.log(data1); // "First data"
   
     const data2 = await fetchData2();
     console.log(data2); // "Second data"
   }
   
   fetchNestedAsyncData();
   ```

#### 연습문제와 해답

1. `async`와 `await`를 사용하여 두 개의 비동기 작업을 순차적으로 처리하고, 각 결과를 출력하세요.
   ```javascript
   async function fetchDataSequentially() {
     const result1 = await new Promise((resolve) => setTimeout(resolve, 1000, 'Result 1'));
     console.log(result1); // "Result 1"
   
     const result2 = await new Promise((resolve) => setTimeout(resolve, 2000, 'Result 2'));
     console.log(result2); // "Result 2"
   }
   
   fetchDataSequentially();
   ```

2. `Promise.all`을 사용하여 세 개의 비동기 작업을 병렬로 처리하고, 모든 작업이 완료된 후 결과를 배열로 출력하세요.
   ```javascript
   async function fetchDataInParallel() {
     const results = await Promise.all([
       new Promise((resolve) => setTimeout(resolve, 1000, 'Result 1')),
       new Promise((resolve) => setTimeout(resolve, 2000, 'Result 2')),
       new Promise((resolve) => setTimeout(resolve, 3000, 'Result 3'))
     ]);
   
     console.log(results); // ["Result 1", "Result 2", "Result 3"]
   }
   
   fetchDataInParallel();
   ```

3. `try-catch`를 사용하여 `async` 함수에서 발생한 오류를 처리하고, 오류 메시지를 출력하세요.
   ```javascript
   async function fetchDataWithErrorHandling() {
     try {
       const data = await new Promise((_, reject) => setTimeout(reject, 1000, 'Error occurred'));
       console.log(data);
     } catch (error) {
       console.error('Caught error:', error); // "Caught error: Error occurred"
     }
   }
   
   fetchDataWithErrorHandling();
   ```

4. 중첩된 `async/await`를 사용하여 세 개의 비동기 작업을 순차적으로 처리하고, 각 결과를 출력하세요.
   ```javascript
   async function fetchData1() {
     return new Promise((resolve) => setTimeout(resolve, 1000, 'First data'));
   }
   
   async function fetchData2() {
     return new Promise((resolve) => setTimeout(resolve, 2000, 'Second data'));
   }
   
   async function fetchData3() {
     return new Promise((resolve) => setTimeout(resolve, 3000, 'Third data'));
   }
   
   async function fetchSequentialData() {
     const data1 = await fetchData1();
     console.log(data1); // "First data"
   
     const data2 = await fetchData2();
     console.log(data2); // "Second data"
   
     const data3 = await fetchData3();
     console.log(data3); // "Third data"
   }
   
   fetchSequentialData();
   ```
