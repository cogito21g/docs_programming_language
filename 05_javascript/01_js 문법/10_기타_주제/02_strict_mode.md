### 10. 기타 중요한 주제

#### 10.2. Strict Mode

Strict Mode는 JavaScript에서 더 엄격한 문법을 적용하여 잠재적인 오류를 사전에 방지하고, 안전한 코드를 작성할 수 있도록 도와줍니다. Strict Mode는 ES5부터 도입되었으며, 전역 또는 함수 단위로 적용할 수 있습니다.

##### Strict Mode 활성화

Strict Mode를 활성화하려면 `"use strict";` 지시어를 사용합니다. 이를 스크립트의 최상단 또는 함수의 최상단에 추가합니다.

###### 전역 Strict Mode

```javascript
"use strict";

function test() {
  // Strict Mode가 적용된 코드
}
```

###### 함수 단위 Strict Mode

```javascript
function test() {
  "use strict";
  // Strict Mode가 적용된 코드
}
```

##### Strict Mode에서의 주요 변경 사항

1. **암시적 전역 변수 금지**

   변수를 선언하지 않고 사용하면 오류가 발생합니다.

   ```javascript
   "use strict";

   function test() {
     x = 10; // ReferenceError: x is not defined
   }

   test();
   ```

2. **읽기 전용 속성에 값 할당 금지**

   읽기 전용 속성에 값을 할당하면 오류가 발생합니다.

   ```javascript
   "use strict";

   const obj = {};
   Object.defineProperty(obj, 'x', { value: 42, writable: false });

   obj.x = 9; // TypeError: Cannot assign to read-only property 'x'
   ```

3. **중복된 매개변수 이름 금지**

   중복된 매개변수 이름을 사용하면 오류가 발생합니다.

   ```javascript
   "use strict";

   function sum(a, a, c) { // SyntaxError: Duplicate parameter name not allowed in this context
     return a + a + c;
   }
   ```

4. **`with` 문 금지**

   `with` 문을 사용하면 오류가 발생합니다.

   ```javascript
   "use strict";

   const obj = { x: 10 };
   with (obj) { // SyntaxError: Strict mode code may not include a with statement
     console.log(x);
   }
   ```

5. **`eval` 및 `arguments` 키워드 제한**

   `eval` 및 `arguments` 키워드를 변수로 사용하거나, 이들의 속성을 수정할 수 없습니다.

   ```javascript
   "use strict";

   const eval = 17; // SyntaxError: Unexpected eval or arguments in strict mode
   const arguments = 42; // SyntaxError: Unexpected eval or arguments in strict mode

   function test() {
     eval = 42; // SyntaxError: Assignment to eval or arguments is not allowed in strict mode
   }
   ```

##### 예제

1. **암시적 전역 변수 금지**

   ```javascript
   "use strict";

   function example() {
     message = "Hello, world!"; // ReferenceError: message is not defined
   }

   example();
   ```

2. **읽기 전용 속성에 값 할당 금지**

   ```javascript
   "use strict";

   const obj = {};
   Object.defineProperty(obj, 'x', { value: 10, writable: false });

   obj.x = 20; // TypeError: Cannot assign to read-only property 'x' of object '#<Object>'
   ```

3. **중복된 매개변수 이름 금지**

   ```javascript
   "use strict";

   function greet(name, name) { // SyntaxError: Duplicate parameter name not allowed in this context
     return "Hello " + name;
   }
   ```

4. **`with` 문 금지**

   ```javascript
   "use strict";

   const obj = { a: 1, b: 2 };

   with (obj) { // SyntaxError: Strict mode code may not include a with statement
     console.log(a + b);
   }
   ```

5. **`eval` 및 `arguments` 키워드 제한**

   ```javascript
   "use strict";

   function test() {
     const eval = 10; // SyntaxError: Unexpected eval or arguments in strict mode
     const arguments = 20; // SyntaxError: Unexpected eval or arguments in strict mode
   }
   ```

#### 연습문제와 해답

1. **Strict Mode를 사용하여 암시적 전역 변수를 방지하세요.**

   ```javascript
   "use strict";

   function calculate() {
     result = 2 + 3; // ReferenceError: result is not defined
     return result;
   }

   console.log(calculate());
   ```

2. **Strict Mode를 사용하여 읽기 전용 속성에 값을 할당할 때 오류가 발생하도록 하세요.**

   ```javascript
   "use strict";

   const person = {};
   Object.defineProperty(person, 'name', { value: 'John', writable: false });

   person.name = 'Doe'; // TypeError: Cannot assign to read-only property 'name' of object '#<Object>'
   ```

3. **Strict Mode를 사용하여 중복된 매개변수 이름을 방지하세요.**

   ```javascript
   "use strict";

   function add(a, b, a) { // SyntaxError: Duplicate parameter name not allowed in this context
     return a + b;
   }
   ```

4. **Strict Mode를 사용하여 `with` 문의 사용을 방지하세요.**

   ```javascript
   "use strict";

   const car = { make: 'Toyota', model: 'Camry' };

   with (car) { // SyntaxError: Strict mode code may not include a with statement
     console.log(make + ' ' + model);
   }
   ```

5. **Strict Mode를 사용하여 `eval` 및 `arguments` 키워드의 잘못된 사용을 방지하세요.**

   ```javascript
   "use strict";

   function process() {
     const eval = 10; // SyntaxError: Unexpected eval or arguments in strict mode
     const arguments = 20; // SyntaxError: Unexpected eval or arguments in strict mode
   }
   ```
