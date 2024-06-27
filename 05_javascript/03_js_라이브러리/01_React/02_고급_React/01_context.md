### 1. React

#### 1.2. 고급 React

##### 1.2.1. 컨텍스트 API

컨텍스트(Context) API는 리액트 컴포넌트 트리 전체에 전역 데이터를 제공할 수 있게 해줍니다. 이를 통해 깊이 중첩된 컴포넌트들에게 props를 일일이 전달할 필요 없이 데이터를 공유할 수 있습니다.

###### 컨텍스트 생성 및 제공

1. **컨텍스트 생성**

   컨텍스트를 생성하려면 `React.createContext`를 사용합니다.

   ```javascript
   // src/contexts/ThemeContext.js
   import React from 'react';

   const ThemeContext = React.createContext('light');

   export default ThemeContext;
   ```

2. **컨텍스트 제공**

   컨텍스트 제공자는 `Provider` 컴포넌트를 사용하여 데이터를 하위 컴포넌트에 제공할 수 있습니다.

   ```javascript
   // src/App.js
   import React from 'react';
   import ThemeContext from './contexts/ThemeContext';
   import ThemedComponent from './components/ThemedComponent';

   const App = () => {
     return (
       <ThemeContext.Provider value="dark">
         <ThemedComponent />
       </ThemeContext.Provider>
     );
   };

   export default App;
   ```

3. **컨텍스트 소비**

   컨텍스트 소비자는 `Consumer` 컴포넌트를 사용하거나 클래스형 컴포넌트에서는 `contextType`을 사용하여 컨텍스트에 접근할 수 있습니다.

   ```javascript
   // src/components/ThemedComponent.js
   import React from 'react';
   import ThemeContext from '../contexts/ThemeContext';

   const ThemedComponent = () => {
     return (
       <ThemeContext.Consumer>
         {value => <div>The theme is {value}</div>}
       </ThemeContext.Consumer>
     );
   };

   export default ThemedComponent;
   ```

   ```javascript
   // src/components/ThemedClassComponent.js
   import React, { Component } from 'react';
   import ThemeContext from '../contexts/ThemeContext';

   class ThemedClassComponent extends Component {
     static contextType = ThemeContext;

     render() {
       return <div>The theme is {this.context}</div>;
     }
   }

   export default ThemedClassComponent;
   ```

###### useContext 훅

함수형 컴포넌트에서 컨텍스트를 사용하려면 `useContext` 훅을 사용합니다.

```javascript
// src/components/ThemedComponentHook.js
import React, { useContext } from 'react';
import ThemeContext from '../contexts/ThemeContext';

const ThemedComponentHook = () => {
  const theme = useContext(ThemeContext);
  return <div>The theme is {theme}</div>;
};

export default ThemedComponentHook;
```

###### 다중 컨텍스트

여러 개의 컨텍스트를 동시에 사용할 수도 있습니다.

```javascript
// src/contexts/LanguageContext.js
import React from 'react';

const LanguageContext = React.createContext('en');

export default LanguageContext;
```

```javascript
// src/App.js
import React from 'react';
import ThemeContext from './contexts/ThemeContext';
import LanguageContext from './contexts/LanguageContext';
import MultiContextComponent from './components/MultiContextComponent';

const App = () => {
  return (
    <ThemeContext.Provider value="dark">
      <LanguageContext.Provider value="fr">
        <MultiContextComponent />
      </LanguageContext.Provider>
    </ThemeContext.Provider>
  );
};

export default App;
```

```javascript
// src/components/MultiContextComponent.js
import React from 'react';
import ThemeContext from '../contexts/ThemeContext';
import LanguageContext from '../contexts/LanguageContext';

const MultiContextComponent = () => {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <LanguageContext.Consumer>
          {language => (
            <div>
              The theme is {theme} and the language is {language}
            </div>
          )}
        </LanguageContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
};

export default MultiContextComponent;
```

###### 예제

1. **컨텍스트 생성 및 사용 예제**

   `src/contexts/ThemeContext.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';

   const ThemeContext = React.createContext('light');

   export default ThemeContext;
   ```

   `src/components/ThemedComponent.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';
   import ThemeContext from '../contexts/ThemeContext';

   const ThemedComponent = () => {
     return (
       <ThemeContext.Consumer>
         {value => <div>The theme is {value}</div>}
       </ThemeContext.Consumer>
     );
   };

   export default ThemedComponent;
   ```

   `src/App.js` 파일을 수정하여 ThemedComponent를 사용합니다:

   ```javascript
   import React from 'react';
   import ThemeContext from './contexts/ThemeContext';
   import ThemedComponent from './components/ThemedComponent';

   const App = () => {
     return (
       <ThemeContext.Provider value="dark">
         <ThemedComponent />
       </ThemeContext.Provider>
     );
   };

   export default App;
   ```

2. **useContext 훅을 사용한 예제**

   `src/components/ThemedComponentHook.js` 파일을 생성하고, 다음과 같이 작성합니다:

   ```javascript
   import React, { useContext } from 'react';
   import ThemeContext from '../contexts/ThemeContext';

   const ThemedComponentHook = () => {
     const theme = useContext(ThemeContext);
     return <div>The theme is {theme}</div>;
   };

   export default ThemedComponentHook;
   ```

##### 연습문제와 해답

1. **컨텍스트를 사용하여 사용자의 언어 설정을 제공하고, 이를 소비하는 컴포넌트를 작성하세요.**

   ```javascript
   // src/contexts/LanguageContext.js
   import React from 'react';

   const LanguageContext = React.createContext('en');

   export default LanguageContext;
   ```

   ```javascript
   // src/components/LanguageComponent.js
   import React from 'react';
   import LanguageContext from '../contexts/LanguageContext';

   const LanguageComponent = () => {
     return (
       <LanguageContext.Consumer>
         {language => <div>The language is {language}</div>}
       </LanguageContext.Consumer>
     );
   };

   export default LanguageComponent;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import LanguageContext from './contexts/LanguageContext';
   import LanguageComponent from './components/LanguageComponent';

   const App = () => {
     return (
       <LanguageContext.Provider value="fr">
         <LanguageComponent />
       </LanguageContext.Provider>
     );
   };

   export default App;
   ```

2. **useContext 훅을 사용하여 사용자의 테마 설정을 제공하고, 이를 소비하는 컴포넌트를 작성하세요.**

   ```javascript
   // src/components/ThemeComponentHook.js
   import React, { useContext } from 'react';
   import ThemeContext from '../contexts/ThemeContext';

   const ThemeComponentHook = () => {
     const theme = useContext(ThemeContext);
     return <div>The theme is {theme}</div>;
   };

   export default ThemeComponentHook;
   ```

3. **다중 컨텍스트를 사용하여 사용자의 테마와 언어 설정을 동시에 제공하고, 이를 소비하는 컴포넌트를 작성하세요.**

   ```javascript
   // src/contexts/ThemeContext.js
   import React from 'react';

   const ThemeContext = React.createContext('light');

   export default ThemeContext;
   ```

   ```javascript
   // src/contexts/LanguageContext.js
   import React from 'react';

   const LanguageContext = React.createContext('en');

   export default LanguageContext;
   ```

   ```javascript
   // src/components/MultiContextComponent.js
   import React from 'react';
   import ThemeContext from '../contexts/ThemeContext';
   import LanguageContext from '../contexts/LanguageContext';

   const MultiContextComponent = () => {
     return (
       <ThemeContext.Consumer>
         {theme => (
           <LanguageContext.Consumer>
             {language => (
               <div>
                 The theme is {theme} and the language is {language}
               </div>
             )}
           </LanguageContext.Consumer>
         )}
       </ThemeContext.Consumer>
     );
   };

   export default MultiContextComponent;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import ThemeContext from './contexts/ThemeContext';
   import LanguageContext from './contexts/LanguageContext';
   import MultiContextComponent from './components/MultiContextComponent';

   const App = () => {
     return (
       <ThemeContext.Provider value="dark">
         <LanguageContext.Provider value="fr">
           <MultiContextComponent />
         </LanguageContext.Provider>
       </ThemeContext.Provider>
     );
   };

   export default App;
   ```
