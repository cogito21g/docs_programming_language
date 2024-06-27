### 2. Vue.js

#### 2.1. Vue.js 기초

##### 2.1.1. Vue 인스턴스

Vue.js는 뷰 모델과 Vue 인스턴스를 사용하여 DOM 요소와 데이터를 바인딩합니다. Vue 인스턴스는 Vue 애플리케이션의 핵심으로, 데이터, 메서드, 라이프사이클 훅 등을 포함합니다.

###### Vue 인스턴스 생성

Vue 인스턴스를 생성하려면 `new Vue()`를 사용합니다.

1. **기본 Vue 인스턴스**

   ```html
   <!-- index.html -->
   <!DOCTYPE html>
   <html>
     <head>
       <title>Vue.js App</title>
       <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
     </head>
     <body>
       <div id="app">
         {{ message }}
       </div>
       <script src="app.js"></script>
     </body>
   </html>
   ```

   ```javascript
   // app.js
   new Vue({
     el: '#app',
     data: {
       message: 'Hello, Vue.js!',
     },
   });
   ```

2. **Vue CLI를 사용한 프로젝트 설정**

   Vue CLI를 사용하면 더 복잡한 Vue 프로젝트를 쉽게 설정할 수 있습니다.

   ```bash
   npm install -g @vue/cli
   vue create my-vue-app
   cd my-vue-app
   npm run serve
   ```

###### Vue 인스턴스의 옵션

1. **data**

   Vue 인스턴스의 `data` 옵션은 Vue 인스턴스에서 사용할 데이터를 정의합니다.

   ```javascript
   new Vue({
     el: '#app',
     data: {
       message: 'Hello, Vue.js!',
     },
   });
   ```

2. **methods**

   Vue 인스턴스의 `methods` 옵션은 Vue 인스턴스에서 사용할 메서드를 정의합니다.

   ```javascript
   new Vue({
     el: '#app',
     data: {
       count: 0,
     },
     methods: {
       increment() {
         this.count++;
       },
     },
   });
   ```

3. **computed**

   Vue 인스턴스의 `computed` 옵션은 계산된 속성을 정의합니다. 계산된 속성은 종속된 데이터가 변경될 때 자동으로 다시 계산됩니다.

   ```javascript
   new Vue({
     el: '#app',
     data: {
       a: 1,
       b: 2,
     },
     computed: {
       sum() {
         return this.a + this.b;
       },
     },
   });
   ```

4. **watch**

   Vue 인스턴스의 `watch` 옵션은 데이터의 변화를 감지하고 이에 반응하는 핸들러를 정의합니다.

   ```javascript
   new Vue({
     el: '#app',
     data: {
       question: '',
       answer: 'I cannot give you an answer until you ask a question!',
     },
     watch: {
       question(newQuestion) {
         if (newQuestion.indexOf('?') > -1) {
           this.answer = 'Thinking...';
           setTimeout(() => {
             this.answer = 'I have an answer for you!';
           }, 2000);
         } else {
           this.answer = 'I cannot give you an answer until you ask a question!';
         }
       },
     },
   });
   ```

5. **라이프사이클 훅**

   Vue 인스턴스는 생성, 업데이트, 소멸 등 특정 시점에 호출되는 라이프사이클 훅을 제공합니다.

   ```javascript
   new Vue({
     el: '#app',
     data: {
       message: 'Hello, Vue.js!',
     },
     created() {
       console.log('Vue instance created!');
     },
     mounted() {
       console.log('Vue instance mounted!');
     },
     updated() {
       console.log('Vue instance updated!');
     },
     destroyed() {
       console.log('Vue instance destroyed!');
     },
   });
   ```

###### 예제

1. **기본 Vue 인스턴스**

   `index.html` 파일을 생성하고, 다음과 같이 작성합니다:

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>Vue.js App</title>
       <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
     </head>
     <body>
       <div id="app">
         {{ message }}
       </div>
       <script>
         new Vue({
           el: '#app',
           data: {
             message: 'Hello, Vue.js!',
           },
         });
       </script>
     </body>
   </html>
   ```

2. **Vue CLI를 사용한 프로젝트 설정**

   ```bash
   npm install -g @vue/cli
   vue create my-vue-app
   cd my-vue-app
   npm run serve
   ```

   `src/App.vue` 파일을 수정하여 기본 Vue 인스턴스를 설정합니다:

   ```html
   <template>
     <div id="app">
       {{ message }}
     </div>
   </template>

   <script>
   export default {
     data() {
       return {
         message: 'Hello, Vue.js!',
       };
     },
   };
   </script>

   <style>
   #app {
     font-family: Avenir, Helvetica, Arial, sans-serif;
     text-align: center;
     color: #2c3e50;
     margin-top: 60px;
   }
   </style>
   ```

##### 연습문제와 해답

1. **기본 Vue 인스턴스를 생성하여 "Hello, Vue.js!" 메시지를 출력하세요.**

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>Vue.js App</title>
       <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
     </head>
     <body>
       <div id="app">
         {{ message }}
       </div>
       <script>
         new Vue({
           el: '#app',
           data: {
             message: 'Hello, Vue.js!',
           },
         });
       </script>
     </body>
   </html>
   ```

2. **Vue 인스턴스의 `methods` 옵션을 사용하여 버튼 클릭 시 카운트를 증가시키는 기능을 구현하세요.**

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>Vue.js App</title>
       <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
     </head>
     <body>
       <div id="app">
         <p>{{ count }}</p>
         <button @click="increment">Increment</button>
       </div>
       <script>
         new Vue({
           el: '#app',
           data: {
             count: 0,
           },
           methods: {
             increment() {
               this.count++;
             },
           },
         });
       </script>
     </body>
   </html>
   ```

3. **Vue 인스턴스의 `computed` 옵션을 사용하여 두 숫자의 합을 계산하는 기능을 구현하세요.**

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <title>Vue.js App</title>
       <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
     </head>
     <body>
       <div id="app">
         <p>{{ sum }}</p>
       </div>
       <script>
         new Vue({
           el: '#app',
           data: {
             a: 1,
             b: 2,
           },
           computed: {
             sum() {
               return this.a + this.b;
             },
           },
         });
       </script>
     </body>
   </html>
   ```
