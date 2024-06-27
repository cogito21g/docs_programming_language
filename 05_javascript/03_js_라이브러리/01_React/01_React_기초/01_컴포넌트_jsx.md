### 1. React

#### 1.1. React 기초

##### 1.1.1. 컴포넌트와 JSX

React는 UI를 구축하기 위한 JavaScript 라이브러리로, 컴포넌트를 사용하여 재사용 가능한 UI 요소를 만들고 관리합니다. JSX는 JavaScript를 확장한 문법으로, XML과 유사한 문법을 사용하여 React 컴포넌트를 작성할 수 있습니다.

###### 컴포넌트

컴포넌트는 두 가지 방식으로 정의할 수 있습니다: 함수형 컴포넌트와 클래스형 컴포넌트입니다.

1. **함수형 컴포넌트**

   함수형 컴포넌트는 단순히 JavaScript 함수로 정의됩니다. React 16.8 이후로, 리액트 훅(Hooks)을 사용하여 함수형 컴포넌트에서도 상태를 관리할 수 있습니다.

   ```javascript
   // src/components/Hello.js
   import React from 'react';

   const Hello = () => {
     return <h1>Hello, World!</h1>;
   };

   export default Hello;
   ```

2. **클래스형 컴포넌트**

   클래스형 컴포넌트는 ES6 클래스 문법을 사용하여 정의됩니다. React.Component를 상속받아야 하며, render 메서드를 통해 UI를 반환합니다.

   ```javascript
   // src/components/HelloClass.js
   import React, { Component } from 'react';

   class HelloClass extends Component {
     render() {
       return <h1>Hello, World!</h1>;
     }
   }

   export default HelloClass;
   ```

###### JSX

JSX는 JavaScript와 XML을 결합한 문법으로, React 컴포넌트를 정의할 때 매우 편리합니다. JSX는 Babel을 사용하여 순수 JavaScript로 변환됩니다.

1. **JSX 사용 예시**

   ```javascript
   // src/components/JSXExample.js
   import React from 'react';

   const JSXExample = () => {
     const name = 'React';
     return <h1>Hello, {name}!</h1>;
   };

   export default JSXExample;
   ```

2. **JSX 변환**

   JSX 코드는 Babel을 사용하여 JavaScript로 변환됩니다. 예를 들어, 다음 JSX 코드:

   ```jsx
   const element = <h1>Hello, World!</h1>;
   ```

   는 다음과 같은 JavaScript 코드로 변환됩니다:

   ```javascript
   const element = React.createElement('h1', null, 'Hello, World!');
   ```

###### 예제

1. **프로젝트 초기화 및 설정**

   ```bash
   npx create-react-app my-app
   cd my-app
   npm start
   ```

2. **컴포넌트 생성 및 사용**

   `src/components/Hello.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';

   const Hello = () => {
     return <h1>Hello, World!</h1>;
   };

   export default Hello;
   ```

   `src/App.js` 파일을 수정하여 Hello 컴포넌트를 사용합니다:

   ```javascript
   import React from 'react';
   import Hello from './components/Hello';

   const App = () => {
     return (
       <div className="App">
         <Hello />
       </div>
     );
   };

   export default App;
   ```

3. **JSX 예제**

   `src/components/JSXExample.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';

   const JSXExample = () => {
     const name = 'React';
     return <h1>Hello, {name}!</h1>;
   };

   export default JSXExample;
   ```

   `src/App.js` 파일을 수정하여 JSXExample 컴포넌트를 사용합니다:

   ```javascript
   import React from 'react';
   import JSXExample from './components/JSXExample';

   const App = () => {
     return (
       <div className="App">
         <JSXExample />
       </div>
     );
   };

   export default App;
   ```

### 연습문제

1. **함수형 컴포넌트를 생성하고, "Hello, React!"를 반환하는 컴포넌트를 작성하세요.**

   ```javascript
   // src/components/HelloReact.js
   import React from 'react';

   const HelloReact = () => {
     return <h1>Hello, React!</h1>;
   };

   export default HelloReact;
   ```

2. **클래스형 컴포넌트를 생성하고, "Welcome to React!"를 반환하는 컴포넌트를 작성하세요.**

   ```javascript
   // src/components/WelcomeReact.js
   import React, { Component } from 'react';

   class WelcomeReact extends Component {
     render() {
       return <h1>Welcome to React!</h1>;
     }
   }

   export default WelcomeReact;
   ```

3. **JSX를 사용하여 변수를 포함하는 문자열을 렌더링하는 컴포넌트를 작성하세요.**

   ```javascript
   // src/components/JSXVariable.js
   import React from 'react';

   const JSXVariable = () => {
     const framework = 'React';
     return <h1>Welcome to {framework}!</h1>;
   };

   export default JSXVariable;
   ```
