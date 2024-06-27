### 12. 최신 웹 기술

#### 12.4. WebAssembly

WebAssembly (Wasm)는 웹 애플리케이션에서 고성능 실행을 가능하게 하는 이진 형식의 언어입니다. WebAssembly는 주로 C, C++, Rust 등의 언어로 작성된 코드를 웹 브라우저에서 실행할 수 있도록 합니다. WebAssembly는 빠른 로드 시간과 높은 성능을 제공하며, JavaScript와 함께 사용할 수 있습니다.

##### 12.4.1. WebAssembly 개요

WebAssembly는 다음과 같은 특징을 가지고 있습니다:
- 이진 형식의 언어로, 빠른 파싱과 실행을 지원합니다.
- 다양한 언어로 작성된 코드를 웹 브라우저에서 실행할 수 있습니다.
- JavaScript와 상호 운용성이 뛰어납니다.
- 성능이 중요한 애플리케이션에 적합합니다.

##### 12.4.2. WebAssembly 모듈 작성

WebAssembly 모듈을 작성하려면 먼저 C, C++ 또는 Rust와 같은 언어로 코드를 작성한 후, 이를 WebAssembly로 컴파일해야 합니다. 이번 예제에서는 Emscripten을 사용하여 C 코드를 WebAssembly로 컴파일합니다.

###### Emscripten 설치

Emscripten을 설치하려면 다음 명령을 사용합니다:

```bash
git clone https://github.com/emscripten-core/emsdk.git
cd emsdk
./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh
```

###### C 코드 작성

간단한 C 코드를 작성합니다:

```c
// hello.c
#include <stdio.h>

int main() {
  printf("Hello, WebAssembly!\n");
  return 0;
}
```

###### WebAssembly로 컴파일

Emscripten을 사용하여 C 코드를 WebAssembly로 컴파일합니다:

```bash
emcc hello.c -o hello.html
```

이 명령은 `hello.html`, `hello.js`, `hello.wasm` 파일을 생성합니다.

##### 12.4.3. JavaScript와 WebAssembly 통합

생성된 WebAssembly 모듈을 JavaScript에서 로드하고 실행합니다.

###### WebAssembly 모듈 로드

HTML 파일에서 WebAssembly 모듈을 로드합니다:

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
  <script>
    fetch('hello.wasm').then(response =>
      response.arrayBuffer()
    ).then(bytes =>
      WebAssembly.instantiate(bytes, {})
    ).then(results => {
      console.log('WebAssembly module loaded:', results);
      // WebAssembly 모듈 실행
      const instance = results.instance;
      instance.exports._main();
    });
  </script>
</body>
</html>
```

##### 연습문제와 해답

1. **간단한 C 코드를 작성하고 WebAssembly로 컴파일하세요.**

   ```c
   // hello.c
   #include <stdio.h>

   int main() {
     printf("Hello, WebAssembly!\n");
     return 0;
   }
   ```

   ```bash
   emcc hello.c -o hello.html
   ```

2. **WebAssembly 모듈을 JavaScript에서 로드하고 실행하세요.**

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
     <script>
       fetch('hello.wasm').then(response =>
         response.arrayBuffer()
       ).then(bytes =>
         WebAssembly.instantiate(bytes, {})
       ).then(results => {
         console.log('WebAssembly module loaded:', results);
         const instance = results.instance;
         instance.exports._main();
       });
     </script>
   </body>
   </html>
   ```

3. **JavaScript에서 WebAssembly 함수를 호출하세요.**

   ```c
   // add.c
   int add(int a, int b) {
     return a + b;
   }
   ```

   ```bash
   emcc add.c -s EXPORTED_FUNCTIONS='["_add"]' -o add.wasm
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
     <script>
       fetch('add.wasm').then(response =>
         response.arrayBuffer()
       ).then(bytes =>
         WebAssembly.instantiate(bytes, {})
       ).then(results => {
         console.log('WebAssembly module loaded:', results);
         const instance = results.instance;
         const result = instance.exports._add(10, 20);
         console.log('Result of add(10, 20):', result);
       });
     </script>
   </body>
   </html>
   ```
