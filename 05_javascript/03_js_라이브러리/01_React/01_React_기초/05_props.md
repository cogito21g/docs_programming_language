### 1. React

#### 1.1. React 기초

##### 1.1.5. 컴포넌트 간의 데이터 전달 (Props)

React에서 컴포넌트 간의 데이터 전달은 props (properties)를 통해 이루어집니다. Props는 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하는 방법이며, 자식 컴포넌트는 전달받은 props를 읽기 전용으로 사용할 수 있습니다.

###### 기본적인 props 사용

1. **함수형 컴포넌트에서의 props 사용**

   ```javascript
   // src/components/Greeting.js
   import React from 'react';

   const Greeting = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };

   export default Greeting;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import Greeting from './components/Greeting';

   const App = () => {
     return (
       <div>
         <Greeting name="Alice" />
         <Greeting name="Bob" />
       </div>
     );
   };

   export default App;
   ```

2. **클래스형 컴포넌트에서의 props 사용**

   ```javascript
   // src/components/GreetingClass.js
   import React, { Component } from 'react';

   class GreetingClass extends Component {
     render() {
       return <h1>Hello, {this.props.name}!</h1>;
     }
   }

   export default GreetingClass;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import GreetingClass from './components/GreetingClass';

   const App = () => {
     return (
       <div>
         <GreetingClass name="Alice" />
         <GreetingClass name="Bob" />
       </div>
     );
   };

   export default App;
   ```

###### props의 기본값 설정

React 컴포넌트는 `defaultProps`를 사용하여 props의 기본값을 설정할 수 있습니다.

1. **함수형 컴포넌트에서의 기본값 설정**

   ```javascript
   // src/components/Greeting.js
   import React from 'react';

   const Greeting = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };

   Greeting.defaultProps = {
     name: 'Stranger',
   };

   export default Greeting;
   ```

2. **클래스형 컴포넌트에서의 기본값 설정**

   ```javascript
   // src/components/GreetingClass.js
   import React, { Component } from 'react';

   class GreetingClass extends Component {
     render() {
       return <h1>Hello, {this.props.name}!</h1>;
     }
   }

   GreetingClass.defaultProps = {
     name: 'Stranger',
   };

   export default GreetingClass;
   ```

###### props 타입 검증

React 컴포넌트는 `propTypes`를 사용하여 전달받은 props의 타입을 검증할 수 있습니다. 이는 코드의 가독성을 높이고 버그를 예방하는 데 유용합니다.

1. **함수형 컴포넌트에서의 props 타입 검증**

   ```javascript
   // src/components/Greeting.js
   import React from 'react';
   import PropTypes from 'prop-types';

   const Greeting = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };

   Greeting.propTypes = {
     name: PropTypes.string,
   };

   Greeting.defaultProps = {
     name: 'Stranger',
   };

   export default Greeting;
   ```

2. **클래스형 컴포넌트에서의 props 타입 검증**

   ```javascript
   // src/components/GreetingClass.js
   import React, { Component } from 'react';
   import PropTypes from 'prop-types';

   class GreetingClass extends Component {
     render() {
       return <h1>Hello, {this.props.name}!</h1>;
     }
   }

   GreetingClass.propTypes = {
     name: PropTypes.string,
   };

   GreetingClass.defaultProps = {
     name: 'Stranger',
   };

   export default GreetingClass;
   ```

###### props를 통해 콜백 함수 전달

부모 컴포넌트에서 자식 컴포넌트로 콜백 함수를 전달하여, 자식 컴포넌트에서 발생한 이벤트를 부모 컴포넌트에서 처리할 수 있습니다.

1. **콜백 함수 전달 예제**

   ```javascript
   // src/components/Button.js
   import React from 'react';

   const Button = (props) => {
     return <button onClick={props.onClick}>Click me</button>;
   };

   export default Button;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import Button from './components/Button';

   const App = () => {
     const handleClick = () => {
       alert('Button clicked!');
     };

     return (
       <div>
         <Button onClick={handleClick} />
       </div>
     );
   };

   export default App;
   ```

###### 예제

1. **기본적인 props 사용 예제**

   `src/components/Greeting.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';

   const Greeting = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };

   export default Greeting;
   ```

   `src/App.js` 파일을 수정하여 Greeting 컴포넌트를 사용합니다:

   ```javascript
   import React from 'react';
   import Greeting from './components/Greeting';

   const App = () => {
     return (
       <div>
         <Greeting name="Alice" />
         <Greeting name="Bob" />
       </div>
     );
   };

   export default App;
   ```

2. **props의 기본값 설정 및 타입 검증 예제**

   `src/components/Greeting.js` 파일을 수정하여 기본값과 타입 검증을 추가합니다:

   ```javascript
   import React from 'react';
   import PropTypes from 'prop-types';

   const Greeting = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };

   Greeting.propTypes = {
     name: PropTypes.string,
   };

   Greeting.defaultProps = {
     name: 'Stranger',
   };

   export default Greeting;
   ```

3. **콜백 함수 전달 예제**

   `src/components/Button.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';

   const Button = (props) => {
     return <button onClick={props.onClick}>Click me</button>;
   };

   export default Button;
   ```

   `src/App.js` 파일을 수정하여 Button 컴포넌트를 사용합니다:

   ```javascript
   import React from 'react';
   import Button from './components/Button';

   const App = () => {
     const handleClick = () => {
       alert('Button clicked!');
     };

     return (
       <div>
         <Button onClick={handleClick} />
       </div>
     );
   };

   export default App;
   ```

##### 연습문제와 해답

1. **함수형 컴포넌트에서 props를 사용하여 "Hello, [name]!" 메시지를 출력하세요.**

   ```javascript
   // src/components/HelloMessage.js
   import React from 'react';

   const HelloMessage = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };

   export default HelloMessage;
   ```

2. **클래스형 컴포넌트에서 props를 사용하여 "Hello, [name]!" 메시지를 출력하세요.**

   ```javascript
   // src/components/HelloMessageClass.js
   import React, { Component } from 'react';

   class HelloMessageClass extends Component {
     render() {
       return <h1>Hello, {this.props.name}!</h1>;
     }
   }

   export default HelloMessageClass;
   ```

3. **props의 기본값과 타입을 설정하여 "Hello, Stranger!" 메시지를 기본값으로 출력하고, name props가 문자열인지 검증하세요.**

   ```javascript
   // src/components/HelloMessage.js
   import React from 'react';
   import PropTypes from 'prop-types';

   const HelloMessage = (props) => {
     return <h1>Hello, {props.name}!</h1>;
   };

   HelloMessage.propTypes = {
     name: PropTypes.string,
   };

   HelloMessage.defaultProps = {
     name: 'Stranger',
   };

   export default HelloMessage;
   ```

4. **부모 컴포넌트에서 콜백 함수를 자식 컴포넌트로 전달하여, 버튼 클릭 시 메시지를 출력하세요.**

   ```javascript
   // src/components/ClickButton.js
   import React from 'react';

   const ClickButton = (props) => {
     return <button onClick={props.onClick}>Click me</button>;
   };

   export default ClickButton;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import ClickButton from './components/ClickButton';

   const App = () => {
     const showMessage = () => {
       alert('Button clicked!');
     };

     return (
       <div>
         <ClickButton onClick={showMessage} />
      

    </div>
        );
    };

    export default App;
    ```
