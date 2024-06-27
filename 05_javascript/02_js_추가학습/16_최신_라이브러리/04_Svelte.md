### 16. 최신 JavaScript 프레임워크 및 라이브러리

#### 16.4. Svelte

Svelte는 컴파일 단계에서 프레임워크의 코드를 제거하여 더욱 빠르고 작은 애플리케이션을 만들 수 있도록 하는 JavaScript 프레임워크입니다. Svelte는 컴포넌트 기반의 프레임워크로, React나 Vue.js와 유사하지만, 컴파일 단계에서 UI를 최적화하는 방식이 다릅니다.

##### Svelte 설치 및 기본 설정

1. **Svelte 프로젝트 생성**

   ```bash
   npx degit sveltejs/template svelte-app
   cd svelte-app
   npm install
   npm run dev
   ```

2. **프로젝트 구조**

   ```plaintext
   svelte-app/
   ├── public/
   │   └── index.html
   ├── src/
   │   ├── App.svelte
   │   └── main.js
   ├── package.json
   └── rollup.config.js
   ```

##### 기본 컴포넌트 작성

1. **App.svelte**

   ```svelte
   <!-- src/App.svelte -->
   <script>
     let count = 0;

     function increment() {
       count += 1;
     }

     function decrement() {
       count -= 1;
     }
   </script>

   <style>
     h1 {
       color: steelblue;
     }
   </style>

   <main>
     <h1>Count: {count}</h1>
     <button on:click={increment}>Increment</button>
     <button on:click={decrement}>Decrement</button>
   </main>
   ```

2. **main.js**

   ```javascript
   // src/main.js
   import App from './App.svelte';

   const app = new App({
     target: document.body,
   });

   export default app;
   ```

##### Svelte의 주요 개념

###### 반응성 (Reactivity)

Svelte는 변수를 직접 변경할 때 반응성을 제공하여 UI를 업데이트합니다.

```svelte
<script>
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<button on:click={increment}>
  Count: {count}
</button>
```

###### Props와 이벤트

컴포넌트 간에 데이터를 전달하기 위해 Props를 사용하고, 이벤트를 통해 부모 컴포넌트와 통신할 수 있습니다.

1. **자식 컴포넌트 (Child.svelte)**

   ```svelte
   <script>
     export let count;
   </script>

   <p>Count in child: {count}</p>
   ```

2. **부모 컴포넌트 (App.svelte)**

   ```svelte
   <script>
     import Child from './Child.svelte';
     let count = 0;

     function increment() {
       count += 1;
     }
   </script>

   <button on:click={increment}>
     Count: {count}
   </button>
   <Child {count} />
   ```

###### 조건부 렌더링

조건부 렌더링은 `{#if ...}` 문을 사용하여 구현합니다.

```svelte
<script>
  let show = true;
</script>

<button on:click={() => (show = !show)}>
  {show ? 'Hide' : 'Show'} Message
</button>

{#if show}
  <p>The message is visible.</p>
{/if}
```

###### 반복 렌더링

반복 렌더링은 `{#each ...}` 문을 사용하여 구현합니다.

```svelte
<script>
  let items = ['Apple', 'Banana', 'Cherry'];
</script>

<ul>
  {#each items as item}
    <li>{item}</li>
  {/each}
</ul>
```

##### 연습문제와 해답

1. **간단한 카운터 컴포넌트를 작성하세요.**

   ```svelte
   <!-- src/App.svelte -->
   <script>
     let count = 0;

     function increment() {
       count += 1;
     }

     function decrement() {
       count -= 1;
     }
   </script>

   <main>
     <h1>Count: {count}</h1>
     <button on:click={increment}>Increment</button>
     <button on:click={decrement}>Decrement</button>
   </main>
   ```

2. **Props를 사용하여 자식 컴포넌트에 데이터를 전달하세요.**

   ```svelte
   <!-- src/Child.svelte -->
   <script>
     export let count;
   </script>

   <p>Count in child: {count}</p>
   ```

   ```svelte
   <!-- src/App.svelte -->
   <script>
     import Child from './Child.svelte';
     let count = 0;

     function increment() {
       count += 1;
     }
   </script>

   <button on:click={increment}>
     Count: {count}
   </button>
   <Child {count} />
   ```

3. **조건부 렌더링을 사용하여 메시지를 표시하고 숨기세요.**

   ```svelte
   <script>
     let show = true;
   </script>

   <button on:click={() => (show = !show)}>
     {show ? 'Hide' : 'Show'} Message
   </button>

   {#if show}
     <p>The message is visible.</p>
   {/if}
   ```

4. **반복 렌더링을 사용하여 리스트를 표시하세요.**

   ```svelte
   <script>
     let items = ['Apple', 'Banana', 'Cherry'];
   </script>

   <ul>
     {#each items as item}
       <li>{item}</li>
     {/each}
   </ul>
   ```
