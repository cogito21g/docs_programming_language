### 9. Svelte

#### 9.1. Svelte 기초

##### 9.1.1. Svelte 설치와 설정

Svelte는 현대적인 웹 애플리케이션을 구축하기 위한 컴파일러로, 런타임에 프레임워크가 필요하지 않습니다. 여기서는 Svelte를 설치하고 설정하는 방법을 설명하겠습니다.

###### Svelte 프로젝트 생성

1. **프로젝트 생성**

   Svelte 프로젝트를 생성하는 가장 간단한 방법은 `degit`을 사용하는 것입니다. `degit`은 템플릿을 복제하는 도구입니다.

   ```bash
   npx degit sveltejs/template svelte-app
   cd svelte-app
   ```

2. **필수 패키지 설치**

   프로젝트 디렉토리로 이동하여 필요한 패키지를 설치합니다.

   ```bash
   npm install
   ```

3. **개발 서버 시작**

   개발 서버를 시작하여 Svelte 애플리케이션을 실행합니다.

   ```bash
   npm run dev
   ```

###### Svelte 기본 구조

1. **src/App.svelte**

   Svelte 애플리케이션의 기본 파일 구조는 다음과 같습니다.

   ```html
   <script>
     let count = 0;

     function increment() {
       count += 1;
     }
   </script>

   <style>
     h1 {
       color: #ff3e00;
     }
   </style>

   <main>
     <h1>Hello Svelte!</h1>
     <button on:click={increment}>
       Count: {count}
     </button>
   </main>
   ```

2. **src/main.js**

   Svelte 컴포넌트를 HTML 파일에 마운트합니다.

   ```javascript
   import App from './App.svelte';

   const app = new App({
     target: document.body,
     props: {
       name: 'world'
     }
   });

   export default app;
   ```

###### 기본 예제

1. **Svelte 기본 예제**

   아래 예제는 카운터 버튼을 클릭하여 숫자를 증가시키는 간단한 Svelte 애플리케이션입니다.

   ```html
   <script>
     let count = 0;

     function increment() {
       count += 1;
     }
   </script>

   <style>
     h1 {
       color: #ff3e00;
     }
   </style>

   <main>
     <h1>Hello Svelte!</h1>
     <button on:click={increment}>
       Count: {count}
     </button>
   </main>
   ```

##### 연습문제와 해답

1. **Svelte를 설치하고 간단한 "Hello, World!" 애플리케이션을 만들어보세요.**

   1. **프로젝트 생성**

      ```bash
      npx degit sveltejs/template svelte-app
      cd svelte-app
      npm install
      npm run dev
      ```

   2. **src/App.svelte**

      ```html
      <script>
        let message = 'Hello, World!';
      </script>

      <style>
        h1 {
          color: #ff3e00;
        }
      </style>

      <main>
        <h1>{message}</h1>
      </main>
      ```
