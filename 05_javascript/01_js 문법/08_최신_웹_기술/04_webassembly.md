### 8. 최신 웹 기술

#### 8.4. WebAssembly

WebAssembly(약칭 wasm)는 고성능 웹 애플리케이션을 개발하기 위한 이진 명령어 형식입니다. WebAssembly는 다양한 프로그래밍 언어로 작성된 코드를 웹에서 실행할 수 있게 해줍니다. 이는 성능과 효율성을 높이고, 브라우저와 플랫폼 간의 일관성을 제공합니다.

##### WebAssembly의 주요 특징

1. **고성능**: 네이티브 코드에 근접한 성능을 제공합니다.
2. **언어 독립적**: 다양한 프로그래밍 언어를 지원합니다.
3. **이식성**: 모든 주요 브라우저와 플랫폼에서 작동합니다.
4. **보안**: 샌드박스 환경에서 실행됩니다.

##### WebAssembly 모듈 생성 및 사용

1. **C 코드를 WebAssembly로 컴파일하기**

   WebAssembly 모듈을 생성하기 위해 C 코드를 작성하고, 이를 WebAssembly로 컴파일합니다. 이를 위해 Emscripten이라는 도구를 사용할 수 있습니다.

   ```c
   // hello.c
   #include <stdio.h>

   int main() {
     printf("Hello, WebAssembly!\n");
     return 0;
   }
   ```

   Emscripten을 사용하여 C 코드를 WebAssembly로 컴파일합니다.

   ```bash
   emcc hello.c -o hello.html
   ```

2. **WebAssembly 모듈을 JavaScript에서 사용하기**

   WebAssembly 모듈을 JavaScript에서 로드하고, 사용하기 위해 WebAssembly API를 사용합니다.

   ```javascript
   // main.js
   const importObject = {
     env: {
       __memory_base: 0,
       __table_base: 0,
       memory: new WebAssembly.Memory({ initial: 256 }),
       table: new WebAssembly.Table({ initial: 0, element: 'anyfunc' })
     }
   };

   WebAssembly.instantiateStreaming(fetch('hello.wasm'), importObject)
     .then(result => {
       console.log(result.instance.exports._main());
     });
   ```

3. **HTML 파일에서 WebAssembly 모듈 로드하기**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebAssembly Example</title>
   </head>
   <body>
     <h1>WebAssembly Example</h1>
     <script src="main.js"></script>
   </body>
   </html>
   ```

##### Rust 코드를 WebAssembly로 컴파일하기

Rust는 WebAssembly로 컴파일할 수 있는 인기 있는 언어 중 하나입니다. Rust와 wasm-pack 도구를 사용하여 WebAssembly 모듈을 생성할 수 있습니다.

1. **Rust 설치 및 설정**

   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

   ```bash
   cargo install wasm-pack
   ```

2. **Rust 프로젝트 생성**

   ```bash
   cargo new hello-wasm --lib
   cd hello-wasm
   ```

3. **Rust 코드를 작성**

   ```rust
   // src/lib.rs
   #[no_mangle]
   pub extern "C" fn greet() {
       println!("Hello, WebAssembly!");
   }
   ```

4. **Rust 코드를 WebAssembly로 컴파일**

   ```bash
   wasm-pack build --target web
   ```

5. **JavaScript에서 WebAssembly 모듈 사용**

   ```javascript
   // main.js
   import * as wasm from './pkg/hello_wasm';

   wasm.greet();
   ```

6. **HTML 파일에서 WebAssembly 모듈 로드하기**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebAssembly with Rust</title>
   </head>
   <body>
     <h1>WebAssembly with Rust</h1>
     <script type="module" src="main.js"></script>
   </body>
   </html>
   ```

#### 예제

1. **C 코드를 WebAssembly로 컴파일하고, JavaScript에서 호출하는 예제**

   ```c
   // hello.c
   #include <stdio.h>

   void greet() {
     printf("Hello, WebAssembly!\n");
   }
   ```

   ```bash
   emcc hello.c -s EXPORTED_FUNCTIONS="['_greet']" -o hello.js
   ```

   ```javascript
   // main.js
   const importObject = {
     env: {
       __memory_base: 0,
       __table_base: 0,
       memory: new WebAssembly.Memory({ initial: 256 }),
       table: new WebAssembly.Table({ initial: 0, element: 'anyfunc' }),
       abort: console.log
     }
   };

   WebAssembly.instantiateStreaming(fetch('hello.wasm'), importObject)
     .then(result => {
       result.instance.exports._greet();
     });
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebAssembly Example</title>
   </head>
   <body>
     <h1>WebAssembly Example</h1>
     <script src="main.js"></script>
   </body>
   </html>
   ```

2. **Rust 코드를 WebAssembly로 컴파일하고, JavaScript에서 호출하는 예제**

   ```rust
   // src/lib.rs
   #[no_mangle]
   pub extern "C" fn greet() {
       println!("Hello, WebAssembly!");
   }
   ```

   ```bash
   wasm-pack build --target web
   ```

   ```javascript
   // main.js
   import * as wasm from './pkg/hello_wasm';

   wasm.greet();
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebAssembly with Rust</title>
   </head>
   <body>
     <h1>WebAssembly with Rust</h1>
     <script type="module" src="main.js"></script>
   </body>
   </html>
   ```

#### 연습문제와 해답

1. **C 코드를 WebAssembly로 컴파일하여 두 숫자를 더하는 함수를 작성하고, JavaScript에서 호출하세요.**

   ```c
   // add.c
   int add(int a, int b) {
     return a + b;
   }
   ```

   ```bash
   emcc add.c -s EXPORTED_FUNCTIONS="['_add']" -o add.js
   ```

   ```javascript
   // main.js
   const importObject = {
     env: {
       __memory_base: 0,
       __table_base: 0,
       memory: new WebAssembly.Memory({ initial: 256 }),
       table: new WebAssembly.Table({ initial: 0, element: 'anyfunc' }),
       abort: console.log
     }
   };

   WebAssembly.instantiateStreaming(fetch('add.wasm'), importObject)
     .then(result => {
       const add = result.instance.exports._add;
       console.log(add(5, 3)); // 8
     });
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebAssembly Add Example</title>
   </head>
   <body>
     <h1>WebAssembly Add Example</h1>
     <script src="main.js"></script>
   </body>
   </html>
   ```

2. **Rust 코드를 WebAssembly로 컴파일하여 두 문자열을 결합하는 함수를 작성하고, JavaScript에서 호출하세요.**

   ```rust
   // src/lib.rs
   #[no_mangle]
   pub extern "C" fn concatenate(a: &str, b: &str) -> String {
       format!("{}{}", a, b)
   }
   ```

   ```bash
   wasm-pack build --target web
   ```

   ```javascript
   // main.js
   import * as wasm from './pkg/hello_wasm';

   const result = wasm.concatenate("Hello, ", "WebAssembly!");
   console.log(result); // "Hello, WebAssembly!"
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>WebAssembly Concatenate Example</title>
   </head>
   <body>
     <h1>WebAssembly Concatenate Example</h1>
     <script type="module" src="main.js"></script>
   </body>
   </html>
   ```
   