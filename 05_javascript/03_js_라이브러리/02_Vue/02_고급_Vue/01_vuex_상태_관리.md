### 2. Vue.js

#### 2.2. 고급 Vue.js

##### 2.2.1. Vuex 상태 관리

Vuex는 Vue.js 애플리케이션을 위한 상태 관리 패턴 + 라이브러리입니다. Vuex는 애플리케이션의 모든 컴포넌트에 대해 중앙 집중식 저장소 역할을 하며, 예측 가능한 방식으로 상태를 관리합니다.

###### Vuex 설치 및 설정

1. **Vuex 설치**

   Vuex를 설치합니다.

   ```bash
   npm install vuex --save
   ```

2. **Vuex 설정**

   Vuex 스토어를 설정합니다.

   ```javascript
   // src/store.js
   import Vue from 'vue';
   import Vuex from 'vuex';

   Vue.use(Vuex);

   export default new Vuex.Store({
     state: {
       count: 0,
     },
     mutations: {
       increment(state) {
         state.count++;
       },
       decrement(state) {
         state.count--;
       },
     },
     actions: {
       increment({ commit }) {
         commit('increment');
       },
       decrement({ commit }) {
         commit('decrement');
       },
     },
     getters: {
       count: (state) => state.count,
     },
   });
   ```

3. **Vue 인스턴스에 Vuex 스토어 연결**

   Vue 인스턴스에 Vuex 스토어를 연결합니다.

   ```javascript
   // src/main.js
   import Vue from 'vue';
   import App from './App.vue';
   import store from './store';

   new Vue({
     store,
     render: (h) => h(App),
   }).$mount('#app');
   ```

###### Vuex의 주요 개념

1. **State**

   `state`는 애플리케이션의 상태를 저장하는 객체입니다.

   ```javascript
   // src/store.js
   export default new Vuex.Store({
     state: {
       count: 0,
     },
   });
   ```

2. **Mutations**

   `mutations`는 상태를 변경하는 메서드입니다. 동기적이어야 합니다.

   ```javascript
   // src/store.js
   export default new Vuex.Store({
     state: {
       count: 0,
     },
     mutations: {
       increment(state) {
         state.count++;
       },
       decrement(state) {
         state.count--;
       },
     },
   });
   ```

3. **Actions**

   `actions`는 비동기 작업을 포함할 수 있는 메서드입니다. `mutations`를 커밋하여 상태를 변경합니다.

   ```javascript
   // src/store.js
   export default new Vuex.Store({
     state: {
       count: 0,
     },
     mutations: {
       increment(state) {
         state.count++;
       },
       decrement(state) {
         state.count--;
       },
     },
     actions: {
       increment({ commit }) {
         commit('increment');
       },
       decrement({ commit }) {
         commit('decrement');
       },
     },
   });
   ```

4. **Getters**

   `getters`는 상태에서 파생된 상태를 계산하는 메서드입니다.

   ```javascript
   // src/store.js
   export default new Vuex.Store({
     state: {
       count: 0,
     },
     getters: {
       count: (state) => state.count,
     },
   });
   ```

###### Vuex 사용 예제

1. **컴포넌트에서 상태 접근 및 변경**

   ```html
   <!-- src/components/Counter.vue -->
   <template>
     <div>
       <p>{{ count }}</p>
       <button @click="increment">Increment</button>
       <button @click="decrement">Decrement</button>
     </div>
   </template>

   <script>
   import { mapState, mapActions } from 'vuex';

   export default {
     computed: {
       ...mapState(['count']),
     },
     methods: {
       ...mapActions(['increment', 'decrement']),
     },
   };
   </script>
   ```

2. **Vue 인스턴스에 Vuex 스토어 연결**

   ```javascript
   // src/main.js
   import Vue from 'vue';
   import App from './App.vue';
   import store from './store';

   new Vue({
     store,
     render: (h) => h(App),
   }).$mount('#app');
   ```

3. **애플리케이션 실행**

   Vue CLI를 사용하여 애플리케이션을 실행합니다.

   ```bash
   npm run serve
   ```

###### 예제

1. **Vuex 설치 및 설정**

   ```bash
   npm install vuex --save
   ```

   `src/store.js` 파일을 생성하고 다음과 같이 작성합니다:

   ```javascript
   import Vue from 'vue';
   import Vuex from 'vuex';

   Vue.use(Vuex);

   export default new Vuex.Store({
     state: {
       count: 0,
     },
     mutations: {
       increment(state) {
         state.count++;
       },
       decrement(state) {
         state.count--;
       },
     },
     actions: {
       increment({ commit }) {
         commit('increment');
       },
       decrement({ commit }) {
         commit('decrement');
       },
     },
     getters: {
       count: (state) => state.count,
     },
   });
   ```

2. **Vue 인스턴스에 Vuex 스토어 연결**

   `src/main.js` 파일을 수정하여 Vuex 스토어를 연결합니다:

   ```javascript
   import Vue from 'vue';
   import App from './App.vue';
   import store from './store';

   new Vue({
     store,
     render: (h) => h(App),
   }).$mount('#app');
   ```

3. **컴포넌트에서 상태 접근 및 변경**

   `src/components/Counter.vue` 파일을 생성하고 다음과 같이 작성합니다:

   ```html
   <template>
     <div>
       <p>{{ count }}</p>
       <button @click="increment">Increment</button>
       <button @click="decrement">Decrement</button>
     </div>
   </template>

   <script>
   import { mapState, mapActions } from 'vuex';

   export default {
     computed: {
       ...mapState(['count']),
     },
     methods: {
       ...mapActions(['increment', 'decrement']),
     },
   };
   </script>
   ```

4. **App.vue에서 Counter 컴포넌트 사용**

   `src/App.vue` 파일을 수정하여 Counter 컴포넌트를 사용합니다:

   ```html
   <template>
     <div id="app">
       <Counter />
     </div>
   </template>

   <script>
   import Counter from './components/Counter.vue';

   export default {
     components: {
       Counter,
     },
   };
   </script>
   ```

##### 연습문제와 해답

1. **Vuex 스토어를 설정하여 상태를 관리하는 카운터를 구현하세요.**

   ```bash
   npm install vuex --save
   ```

   ```javascript
   // src/store.js
   import Vue from 'vue';
   import Vuex from 'vuex';

   Vue.use(Vuex);

   export default new Vuex.Store({
     state: {
       count: 0,
     },
     mutations: {
       increment(state) {
         state.count++;
       },
       decrement(state) {
         state.count--;
       },
     },
     actions: {
       increment({ commit }) {
         commit('increment');
       },
       decrement({ commit }) {
         commit('decrement');
       },
     },
     getters: {
       count: (state) => state.count,
     },
   });
   ```

2. **컴포넌트에서 Vuex 상태에 접근하고, 상태를 변경하는 메서드를 구현하세요.**

   ```html
   <!-- src/components/Counter.vue -->
   <template>
     <div>
       <p>{{ count }}</p>
       <button @click="increment">Increment</button>
       <button @click="decrement">Decrement</button>
     </div>
   </template>

   <script>
   import { mapState, mapActions } from 'vuex';

   export default {
     computed: {
       ...mapState(['count']),
     },
     methods: {
       ...mapActions(['increment', 'decrement']),
     },
   };
   </script>
   ```

3. **Vue 인스턴스에 Vuex 스토어를 연결하고, Counter 컴포넌트를 사용하세요.**

   ```javascript
   // src/main.js
   import Vue from 'vue';
   import App from './App.vue';
   import store from './store';

   new Vue({
     store,
     render: (h) => h(App),
   }).$mount('#app');
   ```

   ```html
   <!-- src/App.vue -->
   <template>
     <div id="app">
       <Counter />
     </div>
   </template>

   <script>
   import Counter from './components/Counter.vue';

   export default {
     components: {
       Counter,
     },
   };
   </script>
   ```
