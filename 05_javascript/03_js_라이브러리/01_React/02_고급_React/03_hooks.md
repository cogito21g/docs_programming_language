### 1. React

#### 1.2. 고급 React

##### 1.2.3. 리액트 훅 (Hooks)

React 훅(Hooks)은 함수형 컴포넌트에서 상태와 생명주기 기능을 사용할 수 있게 해주는 함수입니다. React 16.8에서 도입된 훅은 클래스형 컴포넌트의 대안으로, 함수형 컴포넌트를 더 강력하게 만듭니다.

###### 주요 훅

1. **useState**

   `useState`는 함수형 컴포넌트에서 상태를 추가할 수 있게 해줍니다.

   ```javascript
   import React, { useState } from 'react';

   const Counter = () => {
     const [count, setCount] = useState(0);

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>Click me</button>
       </div>
     );
   };

   export default Counter;
   ```

2. **useEffect**

   `useEffect`는 함수형 컴포넌트에서 부수 효과(side effects)를 수행할 수 있게 해줍니다. 이는 `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`를 합친 것과 유사합니다.

   ```javascript
   import React, { useState, useEffect } from 'react';

   const Example = () => {
     const [count, setCount] = useState(0);

     useEffect(() => {
       document.title = `You clicked ${count} times`;

       return () => {
         document.title = 'React App';
       };
     }, [count]);

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>Click me</button>
       </div>
     );
   };

   export default Example;
   ```

3. **useContext**

   `useContext`는 컨텍스트를 사용하여 전역 상태를 관리할 수 있게 해줍니다.

   ```javascript
   import React, { useContext } from 'react';
   import ThemeContext from './ThemeContext';

   const ThemedComponent = () => {
     const theme = useContext(ThemeContext);

     return <div>The current theme is {theme}</div>;
   };

   export default ThemedComponent;
   ```

4. **useReducer**

   `useReducer`는 상태 관리를 위해 `useState`보다 더 복잡한 로직을 사용해야 할 때 유용합니다. 이는 Redux와 유사한 패턴을 따릅니다.

   ```javascript
   import React, { useReducer } from 'react';

   const initialState = { count: 0 };

   const reducer = (state, action) => {
     switch (action.type) {
       case 'increment':
         return { count: state.count + 1 };
       case 'decrement':
         return { count: state.count - 1 };
       default:
         throw new Error();
     }
   };

   const Counter = () => {
     const [state, dispatch] = useReducer(reducer, initialState);

     return (
       <div>
         <p>Count: {state.count}</p>
         <button onClick={() => dispatch({ type: 'increment' })}>+</button>
         <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
       </div>
     );
   };

   export default Counter;
   ```

5. **useRef**

   `useRef`는 DOM 요소에 직접 접근하거나, 저장된 값을 유지할 때 유용합니다.

   ```javascript
   import React, { useRef } from 'react';

   const TextInput = () => {
     const inputRef = useRef(null);

     const focusInput = () => {
       inputRef.current.focus();
     };

     return (
       <div>
         <input ref={inputRef} type="text" />
         <button onClick={focusInput}>Focus the input</button>
       </div>
     );
   };

   export default TextInput;
   ```

6. **useMemo**

   `useMemo`는 값이 변경될 때까지 계산된 값을 캐싱합니다. 이는 성능 최적화에 유용합니다.

   ```javascript
   import React, { useState, useMemo } from 'react';

   const ExpensiveCalculation = ({ number }) => {
     const computeFactorial = (n) => {
       if (n === 0) {
         return 1;
       }
       return n * computeFactorial(n - 1);
     };

     const factorial = useMemo(() => computeFactorial(number), [number]);

     return <div>Factorial of {number} is {factorial}</div>;
   };

   export default ExpensiveCalculation;
   ```

7. **useCallback**

   `useCallback`은 함수가 변경되지 않도록 메모이제이션 합니다. 이는 함수를 props로 전달할 때 유용합니다.

   ```javascript
   import React, { useState, useCallback } from 'react';

   const Button = React.memo(({ onClick }) => {
     console.log('Button re-render');
     return <button onClick={onClick}>Click me</button>;
   });

   const Parent = () => {
     const [count, setCount] = useState(0);

     const increment = useCallback(() => {
       setCount((c) => c + 1);
     }, []);

     return (
       <div>
         <p>Count: {count}</p>
         <Button onClick={increment} />
       </div>
     );
   };

   export default Parent;
   ```

###### 예제

1. **useState와 useEffect를 사용한 예제**

   `src/components/Counter.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React, { useState, useEffect } from 'react';

   const Counter = () => {
     const [count, setCount] = useState(0);

     useEffect(() => {
       document.title = `You clicked ${count} times`;

       return () => {
         document.title = 'React App';
       };
     }, [count]);

     return (
       <div>
         <p>You clicked {count} times</p>
         <button onClick={() => setCount(count + 1)}>Click me</button>
       </div>
     );
   };

   export default Counter;
   ```

2. **useContext를 사용한 예제**

   `src/ThemeContext.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';

   const ThemeContext = React.createContext('light');

   export default ThemeContext;
   ```

   `src/components/ThemedComponent.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React, { useContext } from 'react';
   import ThemeContext from '../ThemeContext';

   const ThemedComponent = () => {
     const theme = useContext(ThemeContext);

     return <div>The current theme is {theme}</div>;
   };

   export default ThemedComponent;
   ```

3. **useReducer를 사용한 예제**

   `src/components/CounterWithReducer.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React, { useReducer } from 'react';

   const initialState = { count: 0 };

   const reducer = (state, action) => {
     switch (action.type) {
       case 'increment':
         return { count: state.count + 1 };
       case 'decrement':
         return { count: state.count - 1 };
       default:
         throw new Error();
     }
   };

   const CounterWithReducer = () => {
     const [state, dispatch] = useReducer(reducer, initialState);

     return (
       <div>
         <p>Count: {state.count}</p>
         <button onClick={() => dispatch({ type: 'increment' })}>+</button>
         <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
       </div>
     );
   };

   export default CounterWithReducer;
   ```

##### 연습문제와 해답

1. **useState를 사용하여 상태를 관리하는 카운터를 구현하세요. 버튼을 클릭할 때마다 카운트가 증가합니다.**

   ```javascript
   // src/components/Counter.js
   import React, { useState } from 'react';

   const Counter = () => {
     const [count, setCount] = useState(0);

     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={() => setCount(count + 1)}>Increment</button>
       </div>
     );
   };

   export default Counter;
   ```

2. **useEffect를 사용하여 컴포넌트가 마운트될 때와 언마운트될 때 콘솔에 로그를 출력하세요.**

   ```javascript
   // src/components/EffectLogger.js
   import React, { useEffect } from 'react';

   const EffectLogger = () => {
     useEffect(() => {
       console.log('Component mounted');

       return () => {
         console.log('Component will unmount');
       };
     }, []);

     return <div>Open console to see logs</div>;
   };

   export default EffectLogger;
   ```

3. **useReducer를 사용하여 상태를 관리하는 카운터를 구현하세요.

**

   ```javascript
   // src/components/ReducerCounter.js
   import React, { useReducer } from 'react';

   const initialState = { count: 0 };

   const reducer = (state, action) => {
     switch (action.type) {
       case 'increment':
         return { count: state.count + 1 };
       case 'decrement':
         return { count: state.count - 1 };
       default:
         throw new Error();
     }
   };

   const ReducerCounter = () => {
     const [state, dispatch] = useReducer(reducer, initialState);

     return (
       <div>
         <p>Count: {state.count}</p>
         <button onClick={() => dispatch({ type: 'increment' })}>+</button>
         <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
       </div>
     );
   };

   export default ReducerCounter;
   ```

4. **useContext를 사용하여 전역 상태를 관리하는 컴포넌트를 구현하세요.**

   ```javascript
   // src/ThemeContext.js
   import React from 'react';

   const ThemeContext = React.createContext('light');

   export default ThemeContext;
   ```

   ```javascript
   // src/components/ThemeComponent.js
   import React, { useContext } from 'react';
   import ThemeContext from '../ThemeContext';

   const ThemeComponent = () => {
     const theme = useContext(ThemeContext);

     return <div>The current theme is {theme}</div>;
   };

   export default ThemeComponent;
   ```
