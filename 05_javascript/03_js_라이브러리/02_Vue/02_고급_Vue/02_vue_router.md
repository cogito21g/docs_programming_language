### 2. Vue.js

#### 2.2. 고급 Vue.js

##### 2.2.2. Vue 라우터 (Vue Router)

Vue Router는 Vue.js 애플리케이션에서 클라이언트 사이드 라우팅을 구현하는 데 사용되는 공식 라우팅 라이브러리입니다. 이를 통해 SPA(Single Page Application)에서 다양한 페이지 간의 탐색을 간편하게 구현할 수 있습니다.

###### Vue Router 설치 및 설정

1. **Vue Router 설치**

   Vue Router를 설치합니다.

   ```bash
   npm install vue-router
   ```

2. **기본 설정**

   Vue Router를 설정하고 라우트를 정의합니다.

   ```javascript
   // src/router/index.js
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';
   import Contact from '@/components/Contact.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home,
       },
       {
         path: '/about',
         name: 'About',
         component: About,
       },
       {
         path: '/contact',
         name: 'Contact',
         component: Contact,
       },
     ],
   });
   ```

3. **Vue 인스턴스에 라우터 연결**

   Vue 인스턴스에 라우터를 연결합니다.

   ```javascript
   // src/main.js
   import Vue from 'vue';
   import App from './App.vue';
   import router from './router';

   new Vue({
     router,
     render: (h) => h(App),
   }).$mount('#app');
   ```

###### 라우트 설정

1. **라우트 컴포넌트 생성**

   라우트 컴포넌트를 생성합니다.

   ```html
   <!-- src/components/Home.vue -->
   <template>
     <div>
       <h1>Home Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'Home',
   };
   </script>
   ```

   ```html
   <!-- src/components/About.vue -->
   <template>
     <div>
       <h1>About Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'About',
   };
   </script>
   ```

   ```html
   <!-- src/components/Contact.vue -->
   <template>
     <div>
       <h1>Contact Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'Contact',
   };
   </script>
   ```

2. **라우터 링크 설정**

   `router-link` 컴포넌트를 사용하여 페이지 간 탐색을 설정합니다.

   ```html
   <!-- src/App.vue -->
   <template>
     <div id="app">
       <nav>
         <router-link to="/">Home</router-link>
         <router-link to="/about">About</router-link>
         <router-link to="/contact">Contact</router-link>
       </nav>
       <router-view></router-view>
     </div>
   </template>

   <script>
   export default {
     name: 'App',
   };
   </script>
   ```

###### 동적 라우트와 URL 파라미터

1. **동적 라우트 설정**

   URL 파라미터를 사용하여 동적 라우트를 설정합니다.

   ```javascript
   // src/router/index.js
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';
   import Contact from '@/components/Contact.vue';
   import UserProfile from '@/components/UserProfile.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home,
       },
       {
         path: '/about',
         name: 'About',
         component: About,
       },
       {
         path: '/contact',
         name: 'Contact',
         component: Contact,
       },
       {
         path: '/user/:id',
         name: 'UserProfile',
         component: UserProfile,
       },
     ],
   });
   ```

2. **URL 파라미터 사용**

   URL 파라미터를 사용하여 컴포넌트에서 동적으로 데이터를 가져옵니다.

   ```html
   <!-- src/components/UserProfile.vue -->
   <template>
     <div>
       <h1>User Profile for User {{ $route.params.id }}</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'UserProfile',
   };
   </script>
   ```

###### 중첩 라우트

1. **중첩 라우트 설정**

   중첩된 라우트를 설정합니다.

   ```javascript
   // src/router/index.js
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';
   import Contact from '@/components/Contact.vue';
   import UserProfile from '@/components/UserProfile.vue';
   import UserPosts from '@/components/UserPosts.vue';
   import UserSettings from '@/components/UserSettings.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home,
       },
       {
         path: '/about',
         name: 'About',
         component: About,
       },
       {
         path: '/contact',
         name: 'Contact',
         component: Contact,
       },
       {
         path: '/user/:id',
         name: 'UserProfile',
         component: UserProfile,
         children: [
           {
             path: 'posts',
             component: UserPosts,
           },
           {
             path: 'settings',
             component: UserSettings,
           },
         ],
       },
     ],
   });
   ```

2. **중첩된 라우트 컴포넌트**

   중첩된 라우트에서 사용될 컴포넌트를 생성합니다.

   ```html
   <!-- src/components/UserPosts.vue -->
   <template>
     <div>
       <h2>User Posts</h2>
     </div>
   </template>

   <script>
   export default {
     name: 'UserPosts',
   };
   </script>
   ```

   ```html
   <!-- src/components/UserSettings.vue -->
   <template>
     <div>
       <h2>User Settings</h2>
     </div>
   </template>

   <script>
   export default {
     name: 'UserSettings',
   };
   </script>
   ```

   ```html
   <!-- src/components/UserProfile.vue -->
   <template>
     <div>
       <h1>User Profile for User {{ $route.params.id }}</h1>
       <router-link :to="{ name: 'UserPosts', params: { id: $route.params.id } }">Posts</router-link>
       <router-link :to="{ name: 'UserSettings', params: { id: $route.params.id } }">Settings</router-link>
       <router-view></router-view>
     </div>
   </template>

   <script>
   export default {
     name: 'UserProfile',
   };
   </script>
   ```

###### 예제

1. **Vue Router 설치 및 설정**

   ```bash
   npm install vue-router
   ```

   `src/router/index.js` 파일을 생성하고 다음과 같이 작성합니다:

   ```javascript
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';
   import Contact from '@/components/Contact.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home,
       },
       {
         path: '/about',
         name: 'About',
         component: About,
       },
       {
         path: '/contact',
         name: 'Contact',
         component: Contact,
       },
     ],
   });
   ```

2. **Vue 인스턴스에 라우터 연결**

   `src/main.js` 파일을 수정하여 Vue Router를 연결합니다:

   ```javascript
   import Vue from 'vue';
   import App from './App.vue';
   import router from './router';

   new Vue({
     router,
     render: (h) => h(App),
   }).$mount('#app');
   ```

3. **라우트 컴포넌트 생성**

   `src/components/Home.vue`, `src/components/About.vue`, `src/components/Contact.vue` 파일을 각각 생성하고 다음과 같이 작성합니다:

   ```html
   <!-- src/components/Home.vue -->
   <template>
     <div>
       <h1>Home Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'Home',
   };
   </script>
   ```

   ```html
   <!-- src/components/About.vue -->
   <template>
     <div>
       <h1>About Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'About',
   };
   </script>
   ```

   ```html
   <!-- src

/components/Contact.vue -->
   <template>
     <div>
       <h1>Contact Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'Contact',
   };
   </script>
   ```

4. **라우터 링크 설정**

   `src/App.vue` 파일을 수정하여 라우터 링크를 설정합니다:

   ```html
   <template>
     <div id="app">
       <nav>
         <router-link to="/">Home</router-link>
         <router-link to="/about">About</router-link>
         <router-link to="/contact">Contact</router-link>
       </nav>
       <router-view></router-view>
     </div>
   </template>

   <script>
   export default {
     name: 'App',
   };
   </script>
   ```

5. **동적 라우트 및 URL 파라미터 설정**

   `src/router/index.js` 파일을 수정하여 동적 라우트 및 URL 파라미터를 추가합니다:

   ```javascript
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';
   import Contact from '@/components/Contact.vue';
   import UserProfile from '@/components/UserProfile.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home,
       },
       {
         path: '/about',
         name: 'About',
         component: About,
       },
       {
         path: '/contact',
         name: 'Contact',
         component: Contact,
       },
       {
         path: '/user/:id',
         name: 'UserProfile',
         component: UserProfile,
       },
     ],
   });
   ```

   `src/components/UserProfile.vue` 파일을 생성하고 다음과 같이 작성합니다:

   ```html
   <template>
     <div>
       <h1>User Profile for User {{ $route.params.id }}</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'UserProfile',
   };
   </script>
   ```

###### 연습문제와 해답

1. **Vue Router를 설치하고, Home, About, Contact 페이지를 설정하세요.**

   ```bash
   npm install vue-router
   ```

   ```javascript
   // src/router/index.js
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';
   import Contact from '@/components/Contact.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home,
       },
       {
         path: '/about',
         name: 'About',
         component: About,
       },
       {
         path: '/contact',
         name: 'Contact',
         component: Contact,
       },
     ],
   });
   ```

   ```javascript
   // src/main.js
   import Vue from 'vue';
   import App from './App.vue';
   import router from './router';

   new Vue({
     router,
     render: (h) => h(App),
   }).$mount('#app');
   ```

2. **라우트 컴포넌트를 생성하고, 라우터 링크를 설정하세요.**

   ```html
   <!-- src/components/Home.vue -->
   <template>
     <div>
       <h1>Home Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'Home',
   };
   </script>
   ```

   ```html
   <!-- src/components/About.vue -->
   <template>
     <div>
       <h1>About Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'About',
   };
   </script>
   ```

   ```html
   <!-- src/components/Contact.vue -->
   <template>
     <div>
       <h1>Contact Page</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'Contact',
   };
   </script>
   ```

   ```html
   <!-- src/App.vue -->
   <template>
     <div id="app">
       <nav>
         <router-link to="/">Home</router-link>
         <router-link to="/about">About</router-link>
         <router-link to="/contact">Contact</router-link>
       </nav>
       <router-view></router-view>
     </div>
   </template>

   <script>
   export default {
     name: 'App',
   };
   </script>
   ```

3. **동적 라우트 및 URL 파라미터를 설정하고, UserProfile 컴포넌트를 생성하세요.**

   ```javascript
   // src/router/index.js
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';
   import Contact from '@/components/Contact.vue';
   import UserProfile from '@/components/UserProfile.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history',
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home,
       },
       {
         path: '/about',
         name: 'About',
         component: About,
       },
       {
         path: '/contact',
         name: 'Contact',
         component: Contact,
       },
       {
         path: '/user/:id',
         name: 'UserProfile',
         component: UserProfile,
       },
     ],
   });
   ```

   ```html
   <!-- src/components/UserProfile.vue -->
   <template>
     <div>
       <h1>User Profile for User {{ $route.params.id }}</h1>
     </div>
   </template>

   <script>
   export default {
     name: 'UserProfile',
   };
   </script>
   ```
