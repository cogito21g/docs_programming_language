### 10. 기타 중요한 주제

#### 10.3. Date와 Time

JavaScript에서는 `Date` 객체를 사용하여 날짜와 시간을 처리할 수 있습니다. `Date` 객체는 날짜와 시간을 조작하고, 포맷팅하며, 비교하는 다양한 메서드를 제공합니다.

##### Date 객체 생성

`Date` 객체는 여러 가지 방법으로 생성할 수 있습니다.

1. **현재 날짜와 시간**

   ```javascript
   const now = new Date();
   console.log(now); // 현재 날짜와 시간
   ```

2. **특정 날짜와 시간**

   ```javascript
   const specificDate = new Date('2023-06-24T12:00:00');
   console.log(specificDate);
   ```

3. **연도, 월, 일, 시간, 분, 초, 밀리초로 생성**

   ```javascript
   const anotherDate = new Date(2023, 5, 24, 12, 0, 0, 0); // 월은 0부터 시작 (0 = 1월)
   console.log(anotherDate);
   ```

4. **타임스탬프로 생성 (1970-01-01T00:00:00Z 이후의 밀리초)**

   ```javascript
   const timestampDate = new Date(1624531200000);
   console.log(timestampDate);
   ```

##### 주요 메서드

1. **날짜와 시간 가져오기**

   ```javascript
   const date = new Date();
   console.log(date.getFullYear()); // 연도
   console.log(date.getMonth()); // 월 (0 = 1월)
   console.log(date.getDate()); // 일
   console.log(date.getDay()); // 요일 (0 = 일요일)
   console.log(date.getHours()); // 시간
   console.log(date.getMinutes()); // 분
   console.log(date.getSeconds()); // 초
   console.log(date.getMilliseconds()); // 밀리초
   console.log(date.getTime()); // 타임스탬프 (밀리초)
   ```

2. **날짜와 시간 설정하기**

   ```javascript
   const date = new Date();
   date.setFullYear(2022);
   date.setMonth(11); // 12월
   date.setDate(25); // 25일
   date.setHours(10);
   date.setMinutes(30);
   date.setSeconds(15);
   console.log(date);
   ```

3. **날짜와 시간 포맷팅**

   ```javascript
   const date = new Date();
   console.log(date.toDateString()); // "Wed Jun 24 2023"
   console.log(date.toTimeString()); // "12:00:00 GMT+0000 (UTC)"
   console.log(date.toISOString()); // "2023-06-24T12:00:00.000Z"
   console.log(date.toLocaleDateString()); // "6/24/2023" (로컬 형식에 따라 다름)
   console.log(date.toLocaleTimeString()); // "12:00:00 PM" (로컬 형식에 따라 다름)
   console.log(date.toLocaleString()); // "6/24/2023, 12:00:00 PM" (로컬 형식에 따라 다름)
   ```

4. **날짜 비교**

   ```javascript
   const date1 = new Date('2023-06-24');
   const date2 = new Date('2023-07-01');
   console.log(date1 > date2); // false
   console.log(date1 < date2); // true
   console.log(date1.getTime() === date2.getTime()); // false
   ```

##### 예제

1. **현재 날짜와 시간 출력**

   ```javascript
   const now = new Date();
   console.log(`현재 날짜와 시간: ${now}`);
   ```

2. **특정 날짜를 설정하고 포맷팅**

   ```javascript
   const birthday = new Date(1990, 6, 15);
   console.log(`생일: ${birthday.toDateString()}`);
   ```

3. **두 날짜 사이의 차이 계산**

   ```javascript
   const startDate = new Date('2023-06-01');
   const endDate = new Date('2023-06-24');
   const diffTime = Math.abs(endDate - startDate);
   const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
   console.log(`두 날짜 사이의 차이: ${diffDays}일`);
   ```

4. **시간 추가 및 빼기**

   ```javascript
   const date = new Date();
   console.log(`현재 시간: ${date}`);

   date.setHours(date.getHours() + 3); // 3시간 추가
   console.log(`3시간 후: ${date}`);

   date.setDate(date.getDate() - 7); // 7일 빼기
   console.log(`7일 전: ${date}`);
   ```

##### 연습문제와 해답

1. **특정 날짜와 시간으로 `Date` 객체를 생성하고, 연도와 월을 출력하세요.**

   ```javascript
   const date = new Date('2023-12-25T10:30:00');
   console.log(`연도: ${date.getFullYear()}`); // 2023
   console.log(`월: ${date.getMonth() + 1}`); // 12 (월은 0부터 시작)
   ```

2. **현재 날짜로부터 5일 후의 날짜를 계산하여 출력하세요.**

   ```javascript
   const today = new Date();
   today.setDate(today.getDate() + 5);
   console.log(`5일 후의 날짜: ${today.toDateString()}`);
   ```

3. **두 날짜를 비교하여 더 이른 날짜를 출력하세요.**

   ```javascript
   const date1 = new Date('2023-06-01');
   const date2 = new Date('2023-07-01');
   const earlierDate = date1 < date2 ? date1 : date2;
   console.log(`더 이른 날짜: ${earlierDate.toDateString()}`);
   ```
