### 9. Svelte

#### 9.2. 고급 Svelte

##### 9.2.2. SvelteKit 사용

SvelteKit은 Svelte 애플리케이션을 빌드하고 서버 사이드 렌더링(SSR)을 지원하는 프레임워크입니다. SvelteKit을 사용하면 Svelte 애플리케이션을 쉽게 구축하고 배포할 수 있습니다. 여기서는 SvelteKit을 설치하고 사용하는 방법을 설명하겠습니다.

###### SvelteKit 프로젝트 생성

1. **프로젝트 생성**

   SvelteKit 프로젝트를 생성하는 가장 간단한 방법은 `create-svelte` 명령어를 사용하는 것입니다.

   ```bash
   npm init svelte@next my-sveltekit-app
   cd my-sveltekit-app
   ```

2. **프로젝트 설정**

   설치 후 프로젝트 설정을 진행합니다. 설정 옵션에는 기본 템플릿, 타입스크립트 사용 여부 등을 선택할 수 있습니다.

3. **필수 패키지 설치**

   프로젝트 디렉토리로 이동하여 필요한 패키지를 설치합니다.

   ```bash
   npm install
   ```

4. **개발 서버 시작**

   개발 서버를 시작하여 SvelteKit 애플리케이션을 실행합니다.

   ```bash
   npm run dev -- --open
   ```

###### 기본 라우팅

1. **라우팅 기본 설정**

   SvelteKit에서는 파일 기반 라우팅을 지원합니다. `src/routes` 디렉토리 내의 파일이 자동으로 라우트가 됩니다.

   ```html
   <!-- src/routes/index.svelte -->
   <script>
     export let name = 'world';
   </script>

   <main>
     <h1>Hello {name}!</h1>
   </main>
   ```

2. **동적 라우팅**

   동적 라우트를 설정하여 URL 매개변수를 사용할 수 있습니다.

   ```html
   <!-- src/routes/[slug].svelte -->
   <script context="module">
     export async function load({ params }) {
       return {
         props: {
           slug: params.slug
         }
       };
     }
   </script>

   <script>
     export let slug;
   </script>

   <main>
     <h1>Slug: {slug}</h1>
   </main>
   ```

###### API 라우팅

1. **API 라우트 생성**

   SvelteKit을 사용하여 API 라우트를 생성할 수 있습니다.

   ```javascript
   // src/routes/api/hello.js
   export async function get() {
     return {
       status: 200,
       body: {
         message: 'Hello from SvelteKit API!'
       }
     };
   }
   ```

2. **API 호출**

   클라이언트 측에서 API를 호출합니다.

   ```html
   <!-- src/routes/index.svelte -->
   <script>
     let message = '';

     async function fetchMessage() {
       const res = await fetch('/api/hello');
       const data = await res.json();
       message = data.message;
     }

     fetchMessage();
   </script>

   <main>
     <h1>{message}</h1>
   </main>
   ```

###### 페이지 전환 및 레이아웃

1. **페이지 전환**

   SvelteKit은 내장된 `svelte:router`를 사용하여 페이지 전환을 관리합니다.

   ```html
   <!-- src/routes/index.svelte -->
   <script>
     import { goto } from '$app/navigation';
   </script>

   <button on:click={() => goto('/about')}>
     Go to About Page
   </button>
   ```

2. **레이아웃 설정**

   `src/routes/__layout.svelte` 파일을 사용하여 공통 레이아웃을 설정할 수 있습니다.

   ```html
   <!-- src/routes/__layout.svelte -->
   <script>
     export let segment;
   </script>

   <header>
     <h1>My SvelteKit App</h1>
   </header>

   <main>
     <slot></slot>
   </main>

   <footer>
     <p>&copy; 2024 My SvelteKit App</p>
   </footer>
   ```

##### 연습문제와 해답

1. **SvelteKit을 설치하고 기본 프로젝트를 생성하세요.**

   ```bash
   npm init svelte@next my-sveltekit-app
   cd my-sveltekit-app
   npm install
   npm run dev -- --open
   ```

2. **파일 기반 라우팅을 사용하여 새로운 페이지를 추가하고, 해당 페이지로 이동하는 버튼을 만드세요.**

   1. **src/routes/about.svelte**

      ```html
      <script>
        export let name = 'About Page';
      </script>

      <main>
        <h1>{name}</h1>
      </main>
      ```

   2. **src/routes/index.svelte**

      ```html
      <script>
        import { goto } from '$app/navigation';
      </script>

      <button on:click={() => goto('/about')}>
        Go to About Page
      </button>
      ```
