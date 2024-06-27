### 4. 고급 객체와 배열 기능

#### 4.5. Set과 Map

ES6에서는 새로운 데이터 구조인 `Set`과 `Map`이 도입되었습니다. `Set`은 중복 없는 값들의 집합을 저장하고, `Map`은 키-값 쌍의 집합을 저장합니다. 각각 고유한 특징과 메서드를 제공합니다.

##### Set

`Set`은 중복되지 않는 값들의 집합입니다. 값의 순서와 삽입 순서를 유지합니다.

```javascript
const set = new Set();

set.add(1);
set.add(2);
set.add(2); // 중복된 값은 추가되지 않음
set.add(3);

console.log(set); // Set { 1, 2, 3 }
```

###### 주요 메서드와 속성

- `add(value)`: 값을 추가합니다.
- `delete(value)`: 값을 제거합니다.
- `has(value)`: 값이 있는지 확인합니다.
- `clear()`: 모든 값을 제거합니다.
- `size`: 집합의 크기를 반환합니다.

```javascript
const set = new Set([1, 2, 3, 4, 5]);

console.log(set.has(3)); // true
set.delete(3);
console.log(set.has(3)); // false

console.log(set.size); // 4

set.clear();
console.log(set.size); // 0
```

###### Set 순회

`Set`은 `for...of` 문, `forEach` 메서드, `entries()`, `keys()`, `values()` 메서드를 사용해 순회할 수 있습니다.

```javascript
const set = new Set(['apple', 'banana', 'cherry']);

for (const value of set) {
  console.log(value);
}
// Output: apple banana cherry

set.forEach(value => {
  console.log(value);
});
// Output: apple banana cherry
```

##### Map

`Map`은 키-값 쌍의 집합을 저장합니다. 객체와 달리 `Map`은 키의 타입에 제한이 없습니다.

```javascript
const map = new Map();

map.set('name', 'Alice');
map.set('age', 25);
map.set('isStudent', true);

console.log(map); // Map { 'name' => 'Alice', 'age' => 25, 'isStudent' => true }
```

###### 주요 메서드와 속성

- `set(key, value)`: 키-값 쌍을 추가합니다.
- `get(key)`: 키에 해당하는 값을 반환합니다.
- `delete(key)`: 키에 해당하는 값을 제거합니다.
- `has(key)`: 키가 있는지 확인합니다.
- `clear()`: 모든 키-값 쌍을 제거합니다.
- `size`: Map의 크기를 반환합니다.

```javascript
const map = new Map([['name', 'Alice'], ['age', 25]]);

console.log(map.get('name')); // "Alice"
console.log(map.has('age')); // true

map.delete('age');
console.log(map.has('age')); // false

console.log(map.size); // 1

map.clear();
console.log(map.size); // 0
```

###### Map 순회

`Map`은 `for...of` 문, `forEach` 메서드, `entries()`, `keys()`, `values()` 메서드를 사용해 순회할 수 있습니다.

```javascript
const map = new Map([['name', 'Alice'], ['age', 25]]);

for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
}
// Output: name: Alice age: 25

map.forEach((value, key) => {
  console.log(`${key}: ${value}`);
});
// Output: name: Alice age: 25
```

#### 예제

1. `Set`을 사용하여 중복 없는 유일한 값들을 저장하고 순회하세요.
   ```javascript
   const uniqueNumbers = new Set([1, 2, 3, 3, 4, 4, 5]);
   
   uniqueNumbers.forEach(number => {
     console.log(number);
   });
   // Output: 1 2 3 4 5
   ```

2. `Map`을 사용하여 키-값 쌍을 저장하고 순회하세요.
   ```javascript
   const user = new Map();
   user.set('name', 'Bob');
   user.set('age', 30);
   user.set('email', 'bob@example.com');
   
   user.forEach((value, key) => {
     console.log(`${key}: ${value}`);
   });
   // Output: name: Bob age: 30 email: bob@example.com
   ```

3. `Set`에서 값을 추가하고 삭제하는 예제를 작성하세요.
   ```javascript
   const fruits = new Set();
   fruits.add('apple');
   fruits.add('banana');
   fruits.add('cherry');
   
   console.log(fruits.size); // 3
   
   fruits.delete('banana');
   console.log(fruits.size); // 2
   console.log(fruits.has('banana')); // false
   ```

4. `Map`에서 키를 기반으로 값을 추가하고 삭제하는 예제를 작성하세요.
   ```javascript
   const capitals = new Map();
   capitals.set('France', 'Paris');
   capitals.set('Germany', 'Berlin');
   capitals.set('Italy', 'Rome');
   
   console.log(capitals.get('Germany')); // "Berlin"
   
   capitals.delete('Germany');
   console.log(capitals.has('Germany')); // false
   ```

#### 연습문제와 해답

1. `Set`을 사용하여 중복된 문자열을 제거하고, 결과를 출력하는 코드를 작성하세요.
   ```javascript
   const words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'date'];
   const uniqueWords = new Set(words);
   
   uniqueWords.forEach(word => {
     console.log(word);
   });
   // Output: apple banana cherry date
   ```

2. `Map`을 사용하여 학생의 이름과 점수를 저장하고, 점수가 80 이상인 학생만 출력하세요.
   ```javascript
   const students = new Map();
   students.set('Alice', 85);
   students.set('Bob', 75);
   students.set('Charlie', 90);
   
   students.forEach((score, name) => {
     if (score >= 80) {
       console.log(`${name}: ${score}`);
     }
   });
   // Output: Alice: 85 Charlie: 90
   ```

3. `Set`을 사용하여 숫자 배열에서 중복을 제거한 후, 배열로 변환하여 출력하세요.
   ```javascript
   const numbers = [1, 2, 3, 4, 3, 2, 1];
   const uniqueNumbers = Array.from(new Set(numbers));
   console.log(uniqueNumbers); // [1, 2, 3, 4]
   ```

4. `Map`을 사용하여 단어의 빈도를 계산하는 코드를 작성하세요.
   ```javascript
   const text = 'hello world hello everyone';
   const words = text.split(' ');
   const wordCount = new Map();
   
   words.forEach(word => {
     wordCount.set(word, (wordCount.get(word) || 0) + 1);
   });
   
   wordCount.forEach((count, word) => {
     console.log(`${word}: ${count}`);
   });
   // Output: hello: 2 world: 1 everyone: 1
   ```
