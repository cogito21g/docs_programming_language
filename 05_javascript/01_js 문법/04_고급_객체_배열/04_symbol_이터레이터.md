### 4. 고급 객체와 배열 기능

#### 4.4. Symbol과 이터레이터

##### Symbol

`Symbol`은 ES6에서 도입된 고유하고 변경 불가능한 기본 자료형입니다. 주로 객체의 속성 키로 사용되며, 충돌 없이 고유한 속성을 만들 수 있습니다.

```javascript
const sym1 = Symbol('description');
const sym2 = Symbol('description');

console.log(sym1 === sym2); // false (각 Symbol은 고유함)
```

##### 객체의 Symbol 속성

`Symbol`을 객체의 속성 키로 사용하여 숨겨진 속성을 만들 수 있습니다.

```javascript
const mySymbol = Symbol('mySymbol');
const obj = {
  [mySymbol]: 'hidden value',
  visible: 'visible value'
};

console.log(obj[mySymbol]); // "hidden value"
console.log(obj.visible); // "visible value"
```

##### Well-known Symbols

JavaScript는 여러 개의 내장 심볼을 제공합니다. 예를 들어, `Symbol.iterator`는 객체가 이터러블(iterable)임을 나타냅니다.

```javascript
const iterableObj = {
  [Symbol.iterator]() {
    let step = 0;
    const iterator = {
      next() {
        step++;
        if (step <= 5) {
          return { value: step, done: false };
        }
        return { value: undefined, done: true };
      }
    };
    return iterator;
  }
};

for (const value of iterableObj) {
  console.log(value); // 1 2 3 4 5
}
```

##### 이터레이터

이터레이터는 `next` 메서드를 가진 객체로, 이 메서드는 `{ value: 값, done: 불리언 }` 객체를 반환합니다. 이터러블은 `Symbol.iterator` 메서드를 구현한 객체입니다.

1. **이터레이터 직접 구현**

```javascript
const myIterator = {
  current: 0,
  next() {
    if (this.current < 3) {
      return { value: this.current++, done: false };
    }
    return { value: undefined, done: true };
  }
};

console.log(myIterator.next()); // { value: 0, done: false }
console.log(myIterator.next()); // { value: 1, done: false }
console.log(myIterator.next()); // { value: 2, done: false }
console.log(myIterator.next()); // { value: undefined, done: true }
```

2. **이터러블 구현**

```javascript
const myIterable = {
  [Symbol.iterator]() {
    let current = 0;
    return {
      next() {
        if (current < 3) {
          return { value: current++, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
};

for (const value of myIterable) {
  console.log(value); // 0 1 2
}
```

#### 예제

1. `Symbol`을 사용하여 객체의 고유한 속성을 만들어보세요.
   ```javascript
   const id = Symbol('id');
   const user = {
     name: 'Alice',
     [id]: 123
   };

   console.log(user.name); // "Alice"
   console.log(user[id]); // 123
   ```

2. 이터레이터를 직접 구현하여 `next` 메서드를 사용하는 예제를 작성하세요.
   ```javascript
   const iterator = {
     current: 1,
     last: 5,
     next() {
       if (this.current <= this.last) {
         return { value: this.current++, done: false };
       }
       return { value: undefined, done: true };
     }
   };

   console.log(iterator.next()); // { value: 1, done: false }
   console.log(iterator.next()); // { value: 2, done: false }
   console.log(iterator.next()); // { value: 3, done: false }
   console.log(iterator.next()); // { value: 4, done: false }
   console.log(iterator.next()); // { value: 5, done: false }
   console.log(iterator.next()); // { value: undefined, done: true }
   ```

3. 이터러블을 구현하여 `for...of` 문에서 사용할 수 있는 예제를 작성하세요.
   ```javascript
   const range = {
     from: 1,
     to: 5,
     [Symbol.iterator]() {
       let current = this.from;
       let last = this.to;
       return {
         next() {
           if (current <= last) {
             return { value: current++, done: false };
           }
           return { value: undefined, done: true };
         }
       };
     }
   };

   for (const num of range) {
     console.log(num); // 1 2 3 4 5
   }
   ```

#### 연습문제와 해답

1. `Symbol`을 사용하여 객체에 숨겨진 속성을 추가하고 이를 출력하는 코드를 작성하세요.
   ```javascript
   const secret = Symbol('secret');
   const obj = {
     name: 'Bob',
     [secret]: 'hidden'
   };

   console.log(obj.name); // "Bob"
   console.log(obj[secret]); // "hidden"
   ```

2. 이터레이터를 사용하여 1부터 3까지의 숫자를 생성하는 코드를 작성하세요.
   ```javascript
   const numberIterator = {
     current: 1,
     last: 3,
     next() {
       if (this.current <= this.last) {
         return { value: this.current++, done: false };
       }
       return { value: undefined, done: true };
     }
   };

   console.log(numberIterator.next()); // { value: 1, done: false }
   console.log(numberIterator.next()); // { value: 2, done: false }
   console.log(numberIterator.next()); // { value: 3, done: false }
   console.log(numberIterator.next()); // { value: undefined, done: true }
   ```

3. 이터러블을 구현하여 1부터 4까지의 숫자를 출력하는 코드를 작성하세요.
   ```javascript
   const numbers = {
     from: 1,
     to: 4,
     [Symbol.iterator]() {
       let current = this.from;
       let last = this.to;
       return {
         next() {
           if (current <= last) {
             return { value: current++, done: false };
           }
           return { value: undefined, done: true };
         }
       };
     }
   };

   for (const num of numbers) {
     console.log(num); // 1 2 3 4
   }
   ```
