### 16. 최신 JavaScript 프레임워크 및 라이브러리

#### 16.2. Vue.js 심화

Vue.js는 프로그레시브 JavaScript 프레임워크로, 사용자 인터페이스를 만들기 위해 사용됩니다. Vue.js는 단순하고 유연한 API를 제공하여 컴포넌트 기반의 애플리케이션을 구축할 수 있습니다. 이번 섹션에서는 Vuex를 이용한 상태 관리와 Vue Router를 통한 라우팅을 다루겠습니다.

##### 16.2.1. Vuex를 이용한 상태 관리

Vuex는 Vue.js 애플리케이션의 상태 관리를 위한 중앙 저장소 역할을 합니다. Vuex는 애플리케이션의 상태를 예측 가능하게 관리할 수 있도록 도와줍니다.

###### Vuex 설치

```bash
npm install vuex
```

###### Vuex 설정

1. **Store 생성**

   ```javascript
   // store.js
   import Vue from 'vue';
   import Vuex from 'vuex';

   Vue.use(Vuex);

   export default new Vuex.Store({
     state: {
       count: 0
     },
     mutations: {
       increment(state) {
         state.count++;
       },
       decrement(state) {
         state.count--;
       }
     },
     actions: {
       increment({ commit }) {
         commit('increment');
       },
       decrement({ commit }) {
         commit('decrement');
       }
     },
     getters: {
       count: state => state.count
     }
   });
   ```

2. **Store 연결**

   ```javascript
   // main.js
   import Vue from 'vue';
   import App from './App.vue';
   import store from './store';

   new Vue({
     render: h => h(App),
     store
   }).$mount('#app');
   ```

3. **컴포넌트에서 Vuex 사용**

   ```javascript
   // App.vue
   <template>
     <div>
       <h1>Count: {{ count }}</h1>
       <button @click="increment">Increment</button>
       <button @click="decrement">Decrement</button>
     </div>
   </template>

   <script>
   import { mapState, mapActions } from 'vuex';

   export default {
     computed: {
       ...mapState(['count'])
     },
     methods: {
       ...mapActions(['increment', 'decrement'])
     }
   };
   </script>
   ```

##### 16.2.2. Vue Router

Vue Router는 Vue.js 애플리케이션에 라우팅 기능을 추가하는 공식 라우터입니다. Vue Router를 사용하면 여러 페이지를 가진 애플리케이션을 쉽게 만들 수 있습니다.

###### Vue Router 설치

```bash
npm install vue-router
```

###### Vue Router 설정

1. **Router 설정**

   ```javascript
   // router.js
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from './components/Home.vue';
   import About from './components/About.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home
       },
       {
         path: '/about',
         name: 'About',
         component: About
       }
     ]
   });
   ```

2. **Router 연결**

   ```javascript
   // main.js
   import Vue from 'vue';
   import App from './App.vue';
   import router from './router';
   import store from './store';

   new Vue({
     render: h => h(App),
     router,
     store
   }).$mount('#app');
   ```

3. **라우터 링크 사용**

   ```javascript
   // App.vue
   <template>
     <div id="app">
       <nav>
         <router-link to="/">Home</router-link>
         <router-link to="/about">About</router-link>
       </nav>
       <router-view/>
     </div>
   </template>

   <script>
   export default {
     name: 'App'
   };
   </script>
   ```

##### 연습문제와 해답

1. **Vuex를 사용하여 상태를 관리하는 간단한 카운터 애플리케이션을 작성하세요.**

   ```javascript
   // store.js
   import Vue from 'vue';
   import Vuex from 'vuex';

   Vue.use(Vuex);

   export default new Vuex.Store({
     state: {
       count: 0
     },
     mutations: {
       increment(state) {
         state.count++;
       },
       decrement(state) {
         state.count--;
       }
     },
     actions: {
       increment({ commit }) {
         commit('increment');
       },
       decrement({ commit }) {
         commit('decrement');
       }
     },
     getters: {
       count: state => state.count
     }
   });
   ```

   ```javascript
   // main.js
   import Vue from 'vue';
   import App from './App.vue';
   import store from './store';

   new Vue({
     render: h => h(App),
     store
   }).$mount('#app');
   ```

   ```javascript
   // App.vue
   <template>
     <div>
       <h1>Count: {{ count }}</h1>
       <button @click="increment">Increment</button>
       <button @click="decrement">Decrement</button>
     </div>
   </template>

   <script>
   import { mapState, mapActions } from 'vuex';

   export default {
     computed: {
       ...mapState(['count'])
     },
     methods: {
       ...mapActions(['increment', 'decrement'])
     }
   };
   </script>
   ```

2. **Vue Router를 사용하여 두 개의 페이지를 가진 간단한 애플리케이션을 작성하세요.**

   ```javascript
   // router.js
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from './components/Home.vue';
   import About from './components/About.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home
       },
       {
         path: '/about',
         name: 'About',
         component: About
       }
     ]
   });
   ```

   ```javascript
   // main.js
   import Vue from 'vue';
   import App from './App.vue';
   import router from './router';

   new Vue({
     render: h => h(App),
     router
   }).$mount('#app');
   ```

   ```javascript
   // App.vue
   <template>
     <div id="app">
       <nav>
         <router-link to="/">Home</router-link>
         <router-link to="/about">About</router-link>
       </nav>
       <router-view/>
     </div>
   </template>

   <script>
   export default {
     name: 'App'
   };
   </script>
   ```

   ```javascript
   // components/Home.vue
   <template>
     <div>
       <h1>Home Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'Home'
   };
   </script>
   ```

   ```javascript
   // components/About.vue
   <template>
     <div>
       <h1>About Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'About'
   };
   </script>
   ```
