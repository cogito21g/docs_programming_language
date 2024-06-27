### 7. 고급 주제

#### 7.5. JavaScript 엔진 내부 동작 (V8, SpiderMonkey)

JavaScript 엔진은 JavaScript 코드를 해석하고 실행하는 역할을 합니다. 대표적인 JavaScript 엔진으로는 Google의 V8 엔진과 Mozilla의 SpiderMonkey 엔진이 있습니다. 이 엔진들은 JavaScript 코드를 고성능으로 실행하기 위해 다양한 최적화 기법을 사용합니다.

##### V8 엔진

V8 엔진은 Google에서 개발한 JavaScript 엔진으로, Chrome 브라우저와 Node.js에서 사용됩니다. V8 엔진은 JavaScript 코드를 바이트코드로 컴파일하고, JIT(Just-In-Time) 컴파일러를 사용하여 성능을 최적화합니다.

###### V8 엔진의 주요 구성 요소

1. **파서(Parser)**: JavaScript 코드를 분석하여 AST(Abstract Syntax Tree)로 변환합니다.
2. **인터프리터(Ignition)**: AST를 바이트코드로 변환하고, 이를 실행합니다.
3. **컴파일러(Turbofan)**: 바이트코드를 최적화된 머신 코드로 컴파일합니다.

###### V8 엔진의 동작 과정

1. JavaScript 코드는 파서에 의해 AST로 변환됩니다.
2. AST는 인터프리터에 의해 바이트코드로 변환됩니다.
3. 바이트코드는 인터프리터에 의해 실행됩니다.
4. 실행 중인 코드의 성능을 분석하여 자주 실행되는 코드(핫 코드)는 JIT 컴파일러에 의해 최적화된 머신 코드로 컴파일됩니다.

##### SpiderMonkey 엔진

SpiderMonkey 엔진은 Mozilla에서 개발한 JavaScript 엔진으로, Firefox 브라우저에서 사용됩니다. SpiderMonkey 엔진은 V8 엔진과 유사한 방식으로 동작하지만, 자체적인 최적화 기법을 사용합니다.

###### SpiderMonkey 엔진의 주요 구성 요소

1. **파서(Parser)**: JavaScript 코드를 분석하여 AST로 변환합니다.
2. **인터프리터**: AST를 바이트코드로 변환하고, 이를 실행합니다.
3. **컴파일러(Baseline, IonMonkey)**: 바이트코드를 최적화된 머신 코드로 컴파일합니다.

###### SpiderMonkey 엔진의 동작 과정

1. JavaScript 코드는 파서에 의해 AST로 변환됩니다.
2. AST는 인터프리터에 의해 바이트코드로 변환됩니다.
3. 바이트코드는 인터프리터에 의해 실행됩니다.
4. Baseline 컴파일러는 바이트코드를 기본적인 최적화로 컴파일합니다.
5. 자주 실행되는 코드(핫 코드)는 IonMonkey 컴파일러에 의해 고도로 최적화된 머신 코드로 컴파일됩니다.

#### 예제

1. JavaScript 코드의 AST 생성 예제

   ```javascript
   const esprima = require('esprima');

   const code = 'function add(a, b) { return a + b; }';
   const ast = esprima.parseScript(code);
   console.log(JSON.stringify(ast, null, 2));
   ```

2. JavaScript 코드의 바이트코드 변환 예제

   ```javascript
   const code = 'function add(a, b) { return a + b; }';
   const script = new Function(code);
   console.log(script.toString());
   ```

3. JavaScript 코드의 최적화 예제

   ```javascript
   function fibonacci(n) {
     if (n <= 1) return n;
     return fibonacci(n - 1) + fibonacci(n - 2);
   }

   console.time('fibonacci');
   console.log(fibonacci(35));
   console.timeEnd('fibonacci');
   ```

#### 연습문제와 해답

1. JavaScript 코드의 AST를 생성하고 출력하세요.
   ```javascript
   const esprima = require('esprima');

   const code = 'const x = 42;';
   const ast = esprima.parseScript(code);
   console.log(JSON.stringify(ast, null, 2));
   ```

2. JavaScript 코드의 바이트코드 변환을 시뮬레이션하는 예제를 작성하세요.
   ```javascript
   const code = 'function multiply(a, b) { return a * b; }';
   const script = new Function(code);
   console.log(script.toString());
   ```

3. 재귀 함수를 사용하여 JavaScript 코드의 최적화를 시뮬레이션하는 예제를 작성하세요.
   ```javascript
   function factorial(n) {
     if (n <= 1) return 1;
     return n * factorial(n - 1);
   }

   console.time('factorial');
   console.log(factorial(10)); // 3628800
   console.timeEnd('factorial');
   ```

4. V8 엔진과 SpiderMonkey 엔진의 주요 차이점을 설명하세요.
   ```markdown
   - **V8 엔진**:
     - Google에서 개발한 JavaScript 엔진.
     - Chrome 브라우저와 Node.js에서 사용됨.
     - Ignition 인터프리터와 Turbofan JIT 컴파일러 사용.
     - 성능 최적화를 위해 핫 코드를 머신 코드로 컴파일.

   - **SpiderMonkey 엔진**:
     - Mozilla에서 개발한 JavaScript 엔진.
     - Firefox 브라우저에서 사용됨.
     - Baseline 컴파일러와 IonMonkey JIT 컴파일러 사용.
     - 바이트코드를 기본적으로 최적화한 후, 핫 코드를 고도로 최적화된 머신 코드로 컴파일.
   ```
