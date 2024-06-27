### 11. 실전 예제와 프로젝트

#### 11.4. 프레임워크 사용 (React, Vue.js 등)

JavaScript 프레임워크와 라이브러리를 사용하면 복잡한 웹 애플리케이션을 더 쉽게 개발할 수 있습니다. 이번 섹션에서는 React와 Vue.js를 사용하여 간단한 애플리케이션을 만드는 방법을 학습하겠습니다.

##### React

React는 Facebook에서 개발한 사용자 인터페이스 라이브러리로, 컴포넌트 기반 아키텍처를 사용하여 UI를 구성합니다.

###### 프로젝트 설정

1. **Create React App 사용하여 프로젝트 생성**

   ```bash
   npx create-react-app my-react-app
   cd my-react-app
   npm start
   ```

2. **기본 구조**

   프로젝트 폴더 구조는 다음과 같습니다.

   ```
   my-react-app/
   ├── public/
   ├── src/
   │   ├── App.css
   │   ├── App.js
   │   ├── App.test.js
   │   ├── index.css
   │   ├── index.js
   │   ├── logo.svg
   │   └── serviceWorker.js
   ├── .gitignore
   ├── package.json
   ├── README.md
   └── yarn.lock
   ```

###### 간단한 React 컴포넌트 작성

1. **App.js 수정**

   ```javascript
   import React, { useState } from 'react';
   import './App.css';

   function App() {
     const [tasks, setTasks] = useState([]);
     const [newTask, setNewTask] = useState('');

     const addTask = () => {
       setTasks([...tasks, newTask]);
       setNewTask('');
     };

     return (
       <div className="App">
         <h1>React To-Do List</h1>
         <input
           type="text"
           value={newTask}
           onChange={(e) => setNewTask(e.target.value)}
         />
         <button onClick={addTask}>Add Task</button>
         <ul>
           {tasks.map((task, index) => (
             <li key={index}>{task}</li>
           ))}
         </ul>
       </div>
     );
   }

   export default App;
   ```

2. **스타일 추가 (App.css)**

   ```css
   .App {
     text-align: center;
     font-family: Arial, sans-serif;
   }

   input {
     padding: 10px;
     margin-right: 10px;
     border: 1px solid #ddd;
     border-radius: 5px;
   }

   button {
     padding: 10px;
     border: none;
     background-color: #28a745;
     color: white;
     border-radius: 5px;
     cursor: pointer;
   }

   button:hover {
     background-color: #218838;
   }

   ul {
     list-style-type: none;
     padding: 0;
   }

   li {
     padding: 10px;
     border-bottom: 1px solid #ddd;
   }
   ```

##### Vue.js

Vue.js는 Evan You가 개발한 프레임워크로, 직관적이고 간단한 API를 제공합니다.

###### 프로젝트 설정

1. **Vue CLI 사용하여 프로젝트 생성**

   ```bash
   npm install -g @vue/cli
   vue create my-vue-app
   cd my-vue-app
   npm run serve
   ```

2. **기본 구조**

   프로젝트 폴더 구조는 다음과 같습니다.

   ```
   my-vue-app/
   ├── public/
   ├── src/
   │   ├── assets/
   │   ├── components/
   │   ├── App.vue
   │   ├── main.js
   ├── .gitignore
   ├── babel.config.js
   ├── package.json
   ├── README.md
   └── yarn.lock
   ```

###### 간단한 Vue 컴포넌트 작성

1. **App.vue 수정**

   ```html
   <template>
     <div id="app">
       <h1>Vue To-Do List</h1>
       <input v-model="newTask" type="text" placeholder="Enter a new task" />
       <button @click="addTask">Add Task</button>
       <ul>
         <li v-for="(task, index) in tasks" :key="index">{{ task }}</li>
       </ul>
     </div>
   </template>

   <script>
   export default {
     data() {
       return {
         tasks: [],
         newTask: ''
       };
     },
     methods: {
       addTask() {
         if (this.newTask) {
           this.tasks.push(this.newTask);
           this.newTask = '';
         }
       }
     }
   };
   </script>

   <style>
   #app {
     text-align: center;
     font-family: Arial, sans-serif;
   }

   input {
     padding: 10px;
     margin-right: 10px;
     border: 1px solid #ddd;
     border-radius: 5px;
   }

   button {
     padding: 10px;
     border: none;
     background-color: #28a745;
     color: white;
     border-radius: 5px;
     cursor: pointer;
   }

   button:hover {
     background-color: #218838;
   }

   ul {
     list-style-type: none;
     padding: 0;
   }

   li {
     padding: 10px;
     border-bottom: 1px solid #ddd;
   }
   </style>
   ```

##### 연습문제와 해답

1. **React로 만든 To-Do 리스트 애플리케이션에서 항목을 추가하는 기능을 구현하세요.**

   ```javascript
   const addTask = () => {
     setTasks([...tasks, newTask]);
     setNewTask('');
   };
   ```

2. **Vue.js로 만든 To-Do 리스트 애플리케이션에서 항목을 추가하는 기능을 구현하세요.**

   ```javascript
   addTask() {
     if (this.newTask) {
       this.tasks.push(this.newTask);
       this.newTask = '';
     }
   }
   ```
