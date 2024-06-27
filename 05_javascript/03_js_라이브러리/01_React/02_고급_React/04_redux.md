### 1. React

#### 1.2. 고급 React

##### 1.2.4. 리덕스와 리액트 리덕스 (Redux & React-Redux)

Redux는 예측 가능한 상태 관리 라이브러리입니다. React-Redux는 React와 Redux를 함께 사용할 수 있게 해주는 바인딩 라이브러리입니다. 이 섹션에서는 Redux와 React-Redux의 기본 개념과 사용 방법을 다룹니다.

###### Redux의 기본 개념

1. **액션 (Action)**

   액션은 상태 변화를 설명하는 객체입니다. 액션은 `type` 속성을 가져야 하며, 추가 데이터가 필요한 경우 다른 속성을 가질 수 있습니다.

   ```javascript
   // src/actions.js
   export const increment = () => ({
     type: 'INCREMENT',
   });

   export const decrement = () => ({
     type: 'DECREMENT',
   });
   ```

2. **리듀서 (Reducer)**

   리듀서는 액션을 처리하여 새로운 상태를 반환하는 함수입니다. 이전 상태와 액션을 인수로 받아서 새로운 상태를 반환합니다.

   ```javascript
   // src/reducers.js
   const initialState = { count: 0 };

   const counterReducer = (state = initialState, action) => {
     switch (action.type) {
       case 'INCREMENT':
         return { count: state.count + 1 };
       case 'DECREMENT':
         return { count: state.count - 1 };
       default:
         return state;
     }
   };

   export default counterReducer;
   ```

3. **스토어 (Store)**

   스토어는 애플리케이션의 상태를 보관하고 관리합니다. 리덕스 스토어는 리듀서와 함께 생성됩니다.

   ```javascript
   // src/store.js
   import { createStore } from 'redux';
   import counterReducer from './reducers';

   const store = createStore(counterReducer);

   export default store;
   ```

###### React-Redux 사용

1. **프로바이더 (Provider)**

   React 애플리케이션에 Redux 스토어를 제공하려면 `Provider` 컴포넌트를 사용합니다.

   ```javascript
   // src/index.js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { Provider } from 'react-redux';
   import store from './store';
   import App from './App';

   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById('root')
   );
   ```

2. **connect 함수**

   `connect` 함수는 React 컴포넌트를 Redux 스토어에 연결합니다. 이는 컴포넌트가 Redux 상태와 디스패치 함수를 props로 받을 수 있게 해줍니다.

   ```javascript
   // src/components/Counter.js
   import React from 'react';
   import { connect } from 'react-redux';
   import { increment, decrement } from '../actions';

   const Counter = ({ count, increment, decrement }) => {
     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={increment}>Increment</button>
         <button onClick={decrement}>Decrement</button>
       </div>
     );
   };

   const mapStateToProps = (state) => ({
     count: state.count,
   });

   const mapDispatchToProps = {
     increment,
     decrement,
   };

   export default connect(mapStateToProps, mapDispatchToProps)(Counter);
   ```

###### 예제

1. **액션 정의**

   `src/actions.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   export const increment = () => ({
     type: 'INCREMENT',
   });

   export const decrement = () => ({
     type: 'DECREMENT',
   });
   ```

2. **리듀서 정의**

   `src/reducers.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   const initialState = { count: 0 };

   const counterReducer = (state = initialState, action) => {
     switch (action.type) {
       case 'INCREMENT':
         return { count: state.count + 1 };
       case 'DECREMENT':
         return { count: state.count - 1 };
       default:
         return state;
     }
   };

   export default counterReducer;
   ```

3. **스토어 생성**

   `src/store.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import { createStore } from 'redux';
   import counterReducer from './reducers';

   const store = createStore(counterReducer);

   export default store;
   ```

4. **Provider 설정**

   `src/index.js` 파일을 수정하여 `Provider`를 사용합니다:

   ```javascript
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { Provider } from 'react-redux';
   import store from './store';
   import App from './App';

   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById('root')
   );
   ```

5. **Counter 컴포넌트 정의**

   `src/components/Counter.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';
   import { connect } from 'react-redux';
   import { increment, decrement } from '../actions';

   const Counter = ({ count, increment, decrement }) => {
     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={increment}>Increment</button>
         <button onClick={decrement}>Decrement</button>
       </div>
     );
   };

   const mapStateToProps = (state) => ({
     count: state.count,
   });

   const mapDispatchToProps = {
     increment,
     decrement,
   };

   export default connect(mapStateToProps, mapDispatchToProps)(Counter);
   ```

6. **App 컴포넌트에서 Counter 사용**

   `src/App.js` 파일을 수정하여 Counter 컴포넌트를 사용합니다:

   ```javascript
   import React from 'react';
   import Counter from './components/Counter';

   const App = () => {
     return (
       <div>
         <h1>Redux Counter</h1>
         <Counter />
       </div>
     );
   };

   export default App;
   ```

##### 연습문제와 해답

1. **Redux 액션을 정의하고, 이를 사용하여 상태를 관리하는 컴포넌트를 구현하세요.**

   ```javascript
   // src/actions.js
   export const increment = () => ({
     type: 'INCREMENT',
   });

   export const decrement = () => ({
     type: 'DECREMENT',
   });
   ```

2. **리듀서를 정의하고, 이를 사용하여 스토어를 생성하세요.**

   ```javascript
   // src/reducers.js
   const initialState = { count: 0 };

   const counterReducer = (state = initialState, action) => {
     switch (action.type) {
       case 'INCREMENT':
         return { count: state.count + 1 };
       case 'DECREMENT':
         return { count: state.count - 1 };
       default:
         return state;
     }
   };

   export default counterReducer;
   ```

   ```javascript
   // src/store.js
   import { createStore } from 'redux';
   import counterReducer from './reducers';

   const store = createStore(counterReducer);

   export default store;
   ```

3. **React-Redux Provider를 설정하고, Redux 스토어를 애플리케이션에 제공하세요.**

   ```javascript
   // src/index.js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { Provider } from 'react-redux';
   import store from './store';
   import App from './App';

   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById('root')
   );
   ```

4. **connect 함수를 사용하여 Redux 상태와 디스패치 함수를 컴포넌트에 연결하세요.**

   ```javascript
   // src/components/Counter.js
   import React from 'react';
   import { connect } from 'react-redux';
   import { increment, decrement } from '../actions';

   const Counter = ({ count, increment, decrement }) => {
     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={increment}>Increment</button>
         <button onClick={decrement}>Decrement</button>
       </div>
     );
   };

   const mapStateToProps = (state) => ({
     count: state.count,
   });

   const mapDispatchToProps = {
     increment,
     decrement,
   };

   export default connect(mapStateToProps, mapDispatchToProps)(Counter);
   ```
