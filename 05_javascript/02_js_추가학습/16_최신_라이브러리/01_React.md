### 16. 최신 JavaScript 프레임워크 및 라이브러리

#### 16.1. React 심화

React는 Facebook에서 개발한 JavaScript 라이브러리로, UI를 구성하는 컴포넌트를 작성할 수 있습니다. 이번 섹션에서는 Redux, Context API, React Hooks 등 React의 고급 기능을 다루겠습니다.

##### 16.1.1. Redux를 이용한 상태 관리

Redux는 애플리케이션의 상태를 중앙에서 관리할 수 있도록 도와주는 라이브러리입니다. Redux를 사용하면 상태를 예측 가능하게 관리할 수 있습니다.

###### Redux 설치

```bash
npm install redux react-redux
```

###### 기본 개념

1. **Action**: 상태 변화를 일으키는 객체입니다. `type` 속성을 가지며, 상태 변경에 필요한 데이터를 포함할 수 있습니다.
2. **Reducer**: Action을 처리하여 새로운 상태를 반환하는 함수입니다.
3. **Store**: 상태를 저장하는 객체로, 애플리케이션 전체에서 하나만 존재합니다.

###### Redux 설정

1. **Action 작성**

   ```javascript
   // actions.js
   export const increment = () => ({
     type: 'INCREMENT'
   });

   export const decrement = () => ({
     type: 'DECREMENT'
   });
   ```

2. **Reducer 작성**

   ```javascript
   // reducer.js
   const initialState = {
     count: 0
   };

   const counter = (state = initialState, action) => {
     switch (action.type) {
       case 'INCREMENT':
         return {
           ...state,
           count: state.count + 1
         };
       case 'DECREMENT':
         return {
           ...state,
           count: state.count - 1
         };
       default:
         return state;
     }
   };

   export default counter;
   ```

3. **Store 생성**

   ```javascript
   // store.js
   import { createStore } from 'redux';
   import counter from './reducer';

   const store = createStore(counter);

   export default store;
   ```

4. **React와 Redux 연결**

   ```javascript
   // index.js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { Provider } from 'react-redux';
   import App from './App';
   import store from './store';

   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById('root')
   );
   ```

5. **컴포넌트에서 Redux 사용**

   ```javascript
   // App.js
   import React from 'react';
   import { useSelector, useDispatch } from 'react-redux';
   import { increment, decrement } from './actions';

   function App() {
     const count = useSelector(state => state.count);
     const dispatch = useDispatch();

     return (
       <div>
         <h1>Count: {count}</h1>
         <button onClick={() => dispatch(increment())}>Increment</button>
         <button onClick={() => dispatch(decrement())}>Decrement</button>
       </div>
     );
   }

   export default App;
   ```

##### 16.1.2. Context API

Context API는 컴포넌트 트리 전체에 걸쳐 데이터를 공유할 수 있는 방법을 제공합니다. 이를 통해 Prop Drilling을 방지할 수 있습니다.

###### Context 생성

1. **Context 생성**

   ```javascript
   // ThemeContext.js
   import React, { createContext, useState } from 'react';

   const ThemeContext = createContext();

   const ThemeProvider = ({ children }) => {
     const [theme, setTheme] = useState('light');

     const toggleTheme = () => {
       setTheme(prevTheme => (prevTheme === 'light' ? 'dark' : 'light'));
     };

     return (
       <ThemeContext.Provider value={{ theme, toggleTheme }}>
         {children}
       </ThemeContext.Provider>
     );
   };

   export { ThemeContext, ThemeProvider };
   ```

2. **Context 사용**

   ```javascript
   // App.js
   import React from 'react';
   import { ThemeProvider, ThemeContext } from './ThemeContext';

   function App() {
     return (
       <ThemeProvider>
         <Header />
         <Content />
       </ThemeProvider>
     );
   }

   function Header() {
     const { theme, toggleTheme } = React.useContext(ThemeContext);

     return (
       <header className={theme}>
         <h1>Theme is {theme}</h1>
         <button onClick={toggleTheme}>Toggle Theme</button>
       </header>
     );
   }

   function Content() {
     const { theme } = React.useContext(ThemeContext);

     return (
       <div className={theme}>
         <p>This is the content section.</p>
       </div>
     );
   }

   export default App;
   ```

##### 16.1.3. React Hooks

React Hooks는 함수형 컴포넌트에서 상태와 라이프사이클 메서드를 사용할 수 있게 해주는 기능입니다.

###### 주요 Hooks

1. **useState**: 상태 변수를 선언하고, 이를 업데이트할 수 있는 함수를 반환합니다.

   ```javascript
   import React, { useState } from 'react';

   function Counter() {
     const [count, setCount] = useState(0);

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>Click me</button>
       </div>
     );
   }

   export default Counter;
   ```

2. **useEffect**: 사이드 이펙트를 수행할 수 있는 Hook으로, 컴포넌트가 마운트, 업데이트, 언마운트될 때 실행됩니다.

   ```javascript
   import React, { useEffect } from 'react';

   function Example() {
     useEffect(() => {
       console.log('Component mounted or updated');

       return () => {
         console.log('Component will unmount');
       };
     });

     return <div>Hello, World!</div>;
   }

   export default Example;
   ```

3. **useContext**: Context API와 함께 사용하여 Context의 값을 가져올 수 있습니다.

   ```javascript
   import React, { useContext } from 'react';
   import { ThemeContext } from './ThemeContext';

   function ThemedComponent() {
     const { theme } = useContext(ThemeContext);

     return <div className={theme}>This is a themed component</div>;
   }

   export default ThemedComponent;
   ```

##### 연습문제와 해답

1. **Redux를 사용하여 상태를 관리하는 간단한 카운터 애플리케이션을 작성하세요.**

   ```javascript
   // actions.js
   export const increment = () => ({
     type: 'INCREMENT'
   });

   export const decrement = () => ({
     type: 'DECREMENT'
   });
   ```

   ```javascript
   // reducer.js
   const initialState = {
     count: 0
   };

   const counter = (state = initialState, action) => {
     switch (action.type) {
       case 'INCREMENT':
         return {
           ...state,
           count: state.count + 1
         };
       case 'DECREMENT':
         return {
           ...state,
           count: state.count - 1
         };
       default:
         return state;
     }
   };

   export default counter;
   ```

   ```javascript
   // store.js
   import { createStore } from 'redux';
   import counter from './reducer';

   const store = createStore(counter);

   export default store;
   ```

   ```javascript
   // index.js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { Provider } from 'react-redux';
   import App from './App';
   import store from './store';

   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById('root')
   );
   ```

   ```javascript
   // App.js
   import React from 'react';
   import { useSelector, useDispatch } from 'react-redux';
   import { increment, decrement } from './actions';

   function App() {
     const count = useSelector(state => state.count);
     const dispatch = useDispatch();

     return (
       <div>
         <h1>Count: {count}</h1>
         <button onClick={() => dispatch(increment())}>Increment</button>
         <button onClick={() => dispatch(decrement())}>Decrement</button>
       </div>
     );
   }

   export default App;
   ```

2. **Context API를 사용하여 테마를 전환하는 애플리케이션을 작성하세요.**

   ```javascript
   // ThemeContext.js
   import React, { createContext, useState } from 'react';

   const ThemeContext = createContext();

   const ThemeProvider = ({ children }) => {
     const [theme, setTheme] = useState('light');

     const toggleTheme = () => {
       setTheme(prevTheme => (prevTheme === 'light' ? 'dark' : 'light'));
     };

     return (
       <ThemeContext.Provider value={{ theme, toggleTheme }}>
         {children}
       </ThemeContext.Provider>
     );
   };

   export { ThemeContext, ThemeProvider };
   ```

   ```javascript
   // App.js
   import React from'react';
   import { ThemeProvider, ThemeContext } from './ThemeContext';

   function App() {
     return (
       <ThemeProvider>
         <Header />
         <Content />
       </ThemeProvider>
     );
   }

   function Header() {
     const { theme, toggleTheme } = React.useContext(ThemeContext);

     return (
       <header className={theme}>
         <h1>Theme is {theme}</h1>
         <button onClick={toggleTheme}>Toggle Theme</button>
       </header>
     );
   }

   function Content() {
     const { theme } = React.useContext(ThemeContext);

     return (
       <div className={theme}>
         <p>This is the content section.</p>
       </div>
     );
   }

   export default App;
   ```

3. **React Hooks를 사용하여 상태와 효과를 관리하는 간단한 컴포넌트를 작성하세요.**

   ```javascript
   import React, { useState, useEffect } from 'react';

   function Counter() {
     const [count, setCount] = useState(0);

     useEffect(() => {
       console.log(`You clicked ${count} times`);
     }, [count]);

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>Click me</button>
       </div>
     );
   }

   export default Counter;
   ```
