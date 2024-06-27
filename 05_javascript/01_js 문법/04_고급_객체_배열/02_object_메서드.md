### 4. 고급 객체와 배열 기능

#### 4.2. Object 메서드 (Object.assign, Object.entries, Object.values, Object.keys)

JavaScript에서는 객체를 다룰 때 유용하게 사용할 수 있는 다양한 `Object` 메서드를 제공합니다. 이러한 메서드를 사용하면 객체의 복사, 키-값 쌍의 추출, 값이나 키의 목록 생성 등을 쉽게 수행할 수 있습니다.

##### Object.assign

`Object.assign` 메서드는 하나 이상의 소스 객체로부터 타겟 객체로 속성을 복사합니다. 이를 통해 객체를 복사하거나 병합할 수 있습니다.

```javascript
const target = {a: 1, b: 2};
const source = {b: 3, c: 4};

const result = Object.assign(target, source);
console.log(result); // {a: 1, b: 3, c: 4}
console.log(target); // {a: 1, b: 3, c: 4} (target 객체도 변경됨)
```

##### Object.entries

`Object.entries` 메서드는 객체의 열거 가능한 속성 [key, value] 쌍의 배열을 반환합니다.

```javascript
const obj = {a: 1, b: 2, c: 3};
const entries = Object.entries(obj);
console.log(entries); // [['a', 1], ['b', 2], ['c', 3]]
```

##### Object.values

`Object.values` 메서드는 객체의 열거 가능한 속성 값들의 배열을 반환합니다.

```javascript
const obj = {a: 1, b: 2, c: 3};
const values = Object.values(obj);
console.log(values); // [1, 2, 3]
```

##### Object.keys

`Object.keys` 메서드는 객체의 열거 가능한 속성 이름(key)들의 배열을 반환합니다.

```javascript
const obj = {a: 1, b: 2, c: 3};
const keys = Object.keys(obj);
console.log(keys); // ['a', 'b', 'c']
```

#### 예제

1. `Object.assign`을 사용하여 객체를 병합하세요.
   ```javascript
   const target = {name: 'Alice'};
   const source1 = {age: 25};
   const source2 = {city: 'Wonderland'};

   const result = Object.assign(target, source1, source2);
   console.log(result); // {name: 'Alice', age: 25, city: 'Wonderland'}
   ```

2. `Object.entries`를 사용하여 객체의 키-값 쌍을 배열로 변환하세요.
   ```javascript
   const person = {name: 'Bob', age: 30};
   const entries = Object.entries(person);
   console.log(entries); // [['name', 'Bob'], ['age', 30]]
   ```

3. `Object.values`를 사용하여 객체의 값을 배열로 변환하세요.
   ```javascript
   const settings = {theme: 'dark', layout: 'grid', showSidebar: true};
   const values = Object.values(settings);
   console.log(values); // ['dark', 'grid', true]
   ```

4. `Object.keys`를 사용하여 객체의 키를 배열로 변환하세요.
   ```javascript
   const car = {make: 'Toyota', model: 'Corolla', year: 2021};
   const keys = Object.keys(car);
   console.log(keys); // ['make', 'model', 'year']
   ```

#### 연습문제와 해답

1. `Object.assign`을 사용하여 두 객체를 병합하고 결과를 출력하세요.
   ```javascript
   const obj1 = {a: 1, b: 2};
   const obj2 = {b: 3, c: 4};

   const merged = Object.assign({}, obj1, obj2);
   console.log(merged); // {a: 1, b: 3, c: 4}
   ```

2. `Object.entries`를 사용하여 객체의 키-값 쌍을 배열로 변환하고, 각 쌍을 콘솔에 출력하세요.
   ```javascript
   const fruit = {name: 'Apple', color: 'Red'};
   const entries = Object.entries(fruit);

   entries.forEach(([key, value]) => {
     console.log(`${key}: ${value}`);
   });
   // Output:
   // name: Apple
   // color: Red
   ```

3. `Object.values`를 사용하여 객체의 값을 배열로 변환하고, 각 값을 콘솔에 출력하세요.
   ```javascript
   const user = {username: 'charlie', email: 'charlie@example.com'};
   const values = Object.values(user);

   values.forEach(value => {
     console.log(value);
   });
   // Output:
   // charlie
   // charlie@example.com
   ```

4. `Object.keys`를 사용하여 객체의 키를 배열로 변환하고, 각 키를 콘솔에 출력하세요.
   ```javascript
   const book = {title: '1984', author: 'George Orwell', year: 1949};
   const keys = Object.keys(book);

   keys.forEach(key => {
     console.log(key);
   });
   // Output:
   // title
   // author
   // year
   ```
