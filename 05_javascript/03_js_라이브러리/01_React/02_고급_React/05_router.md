### 1. React

#### 1.2. 고급 React

##### 1.2.5. 리액트 라우터 (React Router)

React Router는 React 애플리케이션에서 클라이언트 사이드 라우팅을 구현하는 데 사용되는 표준 라이브러리입니다. 이를 통해 SPA(Single Page Application)에서 다양한 페이지 간의 탐색을 간편하게 구현할 수 있습니다.

###### React Router 기본 설정

1. **설치**

   React Router를 설치합니다.

   ```bash
   npm install react-router-dom
   ```

2. **기본 설정**

   React Router의 `BrowserRouter`, `Route`, `Switch` 컴포넌트를 사용하여 라우팅을 설정합니다.

   ```javascript
   // src/App.js
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><a href="/">Home</a></li>
               <li><a href="/about">About</a></li>
               <li><a href="/contact">Contact</a></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

3. **기본 컴포넌트 생성**

   `Home`, `About`, `Contact` 컴포넌트를 생성합니다.

   ```javascript
   // src/components/Home.js
   import React from 'react';

   const Home = () => {
     return <h2>Home Page</h2>;
   };

   export default Home;
   ```

   ```javascript
   // src/components/About.js
   import React from 'react';

   const About = () => {
     return <h2>About Page</h2>;
   };

   export default About;
   ```

   ```javascript
   // src/components/Contact.js
   import React from 'react';

   const Contact = () => {
     return <h2>Contact Page</h2>;
   };

   export default Contact;
   ```

###### 동적 라우트와 URL 파라미터

1. **동적 라우트 설정**

   URL 파라미터를 사용하여 동적 라우트를 설정합니다.

   ```javascript
   // src/App.js
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';
   import UserProfile from './components/UserProfile';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><a href="/">Home</a></li>
               <li><a href="/about">About</a></li>
               <li><a href="/contact">Contact</a></li>
               <li><a href="/user/1">User 1</a></li>
               <li><a href="/user/2">User 2</a></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
             <Route path="/user/:id" component={UserProfile} />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

2. **URL 파라미터 사용**

   URL 파라미터를 사용하여 컴포넌트에서 동적으로 데이터를 가져옵니다.

   ```javascript
   // src/components/UserProfile.js
   import React from 'react';
   import { useParams } from 'react-router-dom';

   const UserProfile = () => {
     const { id } = useParams();
     return <h2>User Profile for User {id}</h2>;
   };

   export default UserProfile;
   ```

###### 링크와 내비게이션

1. **Link 컴포넌트**

   `Link` 컴포넌트를 사용하여 페이지 간 탐색을 구현합니다.

   ```javascript
   // src/App.js
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';
   import UserProfile from './components/UserProfile';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><Link to="/">Home</Link></li>
               <li><Link to="/about">About</Link></li>
               <li><Link to="/contact">Contact</Link></li>
               <li><Link to="/user/1">User 1</Link></li>
               <li><Link to="/user/2">User 2</Link></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
             <Route path="/user/:id" component={UserProfile} />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

###### 리다이렉트

1. **Redirect 컴포넌트**

   `Redirect` 컴포넌트를 사용하여 특정 경로에서 다른 경로로 리다이렉트할 수 있습니다.

   ```javascript
   // src/App.js
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Redirect } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><a href="/">Home</a></li>
               <li><a href="/about">About</a></li>
               <li><a href="/contact">Contact</a></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
             <Redirect from="/old-contact" to="/contact" />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

###### 예제

1. **기본 라우팅 예제**

   `src/components/Home.js`, `src/components/About.js`, `src/components/Contact.js` 파일을 각각 생성하고 다음과 같이 작성합니다:

   ```javascript
   // src/components/Home.js
   import React from 'react';

   const Home = () => {
     return <h2>Home Page</h2>;
   };

   export default Home;
   ```

   ```javascript
   // src/components/About.js
   import React from 'react';

   const About = () => {
     return <h2>About Page</h2>;
   };

   export default About;
   ```

   ```javascript
   // src/components/Contact.js
   import React from 'react';

   const Contact = () => {
     return <h2>Contact Page</h2>;
   };

   export default Contact;
   ```

   `src/App.js` 파일을 수정하여 기본 라우팅을 설정합니다:

   ```javascript
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><Link to="/">Home</Link></li>
               <li><Link to="/about">About</Link></li>
               <li><Link to="/contact">Contact</Link></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

2. **동적 라우트와 URL 파라미터 예제**

   `src/components/UserProfile.js` 파일을 생성하고 다음과 같이 작성합니다:

   ```javascript
   import React from 'react';
   import { useParams } from 'react-router-dom';

   const UserProfile = () => {
     const { id } = useParams();
     return <h2>User Profile for User {id}</h2>;
   };

   export default UserProfile;
   ```

   `src/App.js` 파일을 수정하여 동적 라우트를 설정합니다:

   ```javascript
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Link

 } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';
   import UserProfile from './components/UserProfile';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><Link to="/">Home</Link></li>
               <li><Link to="/about">About</Link></li>
               <li><Link to="/contact">Contact</Link></li>
               <li><Link to="/user/1">User 1</Link></li>
               <li><Link to="/user/2">User 2</Link></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
             <Route path="/user/:id" component={UserProfile} />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

3. **리다이렉트 예제**

   `src/App.js` 파일을 수정하여 리다이렉트를 설정합니다:

   ```javascript
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Link, Redirect } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><Link to="/">Home</Link></li>
               <li><Link to="/about">About</Link></li>
               <li><Link to="/contact">Contact</Link></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
             <Redirect from="/old-contact" to="/contact" />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

##### 연습문제와 해답

1. **기본적인 라우팅을 설정하여 Home, About, Contact 페이지를 렌더링하세요.**

   ```javascript
   // src/components/Home.js
   import React from 'react';

   const Home = () => {
     return <h2>Home Page</h2>;
   };

   export default Home;
   ```

   ```javascript
   // src/components/About.js
   import React from 'react';

   const About = () => {
     return <h2>About Page</h2>;
   };

   export default About;
   ```

   ```javascript
   // src/components/Contact.js
   import React from 'react';

   const Contact = () => {
     return <h2>Contact Page</h2>;
   };

   export default Contact;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><Link to="/">Home</Link></li>
               <li><Link to="/about">About</Link></li>
               <li><Link to="/contact">Contact</Link></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

2. **동적 라우트와 URL 파라미터를 사용하여 User Profile 페이지를 렌더링하세요.**

   ```javascript
   // src/components/UserProfile.js
   import React from 'react';
   import { useParams } from 'react-router-dom';

   const UserProfile = () => {
     const { id } = useParams();
     return <h2>User Profile for User {id}</h2>;
   };

   export default UserProfile;
   ```

   ```javascript
   // src/App.js
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';
   import UserProfile from './components/UserProfile';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><Link to="/">Home</Link></li>
               <li><Link to="/about">About</Link></li>
               <li><Link to="/contact">Contact</Link></li>
               <li><Link to="/user/1">User 1</Link></li>
               <li><Link to="/user/2">User 2</Link></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
             <Route path="/user/:id" component={UserProfile} />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```

3. **리다이렉트를 설정하여 "/old-contact" 경로에서 "/contact" 경로로 리다이렉트하세요.**

   ```javascript
   // src/App.js
   import React from 'react';
   import { BrowserRouter as Router, Route, Switch, Link, Redirect } from 'react-router-dom';
   import Home from './components/Home';
   import About from './components/About';
   import Contact from './components/Contact';

   const App = () => {
     return (
       <Router>
         <div>
           <nav>
             <ul>
               <li><Link to="/">Home</Link></li>
               <li><Link to="/about">About</Link></li>
               <li><Link to="/contact">Contact</Link></li>
             </ul>
           </nav>
           <Switch>
             <Route exact path="/" component={Home} />
             <Route path="/about" component={About} />
             <Route path="/contact" component={Contact} />
             <Redirect from="/old-contact" to="/contact" />
           </Switch>
         </div>
       </Router>
     );
   };

   export default App;
   ```
