### 11. 실전 예제와 프로젝트

#### 11.3. SPA (Single Page Application) 기초

Single Page Application (SPA)은 한 페이지로 구성된 웹 애플리케이션으로, 사용자가 페이지를 이동할 때 전체 페이지를 새로 고침하지 않고도 새로운 콘텐츠를 동적으로 로드하고 표시할 수 있습니다. 이를 통해 더 빠르고 매끄러운 사용자 경험을 제공합니다. 이번 섹션에서는 간단한 SPA를 만드는 방법을 학습하겠습니다.

##### 프로젝트 구조

```
spa-app/
├── index.html
├── style.css
└── app.js
```

##### 1. HTML 작성

`index.html` 파일을 작성하여 기본적인 구조를 만듭니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SPA Example</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <nav>
      <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
    <div id="content"></div>
  </div>
  <script src="app.js"></script>
</body>
</html>
```

##### 2. CSS 작성

`style.css` 파일을 작성하여 기본적인 스타일을 적용합니다.

```css
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.container {
  background: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  width: 300px;
}

nav ul {
  list-style: none;
  padding: 0;
  display: flex;
  justify-content: space-around;
}

nav a {
  text-decoration: none;
  color: #007bff;
}

nav a:hover {
  text-decoration: underline;
}

#content {
  margin-top: 20px;
  text-align: center;
}
```

##### 3. JavaScript 작성

`app.js` 파일을 작성하여 라우팅 기능을 구현합니다.

```javascript
document.addEventListener('DOMContentLoaded', () => {
  const content = document.getElementById('content');

  function navigate() {
    const hash = window.location.hash || '#home';
    switch (hash) {
      case '#home':
        content.innerHTML = '<h2>Home</h2><p>Welcome to the Home page!</p>';
        break;
      case '#about':
        content.innerHTML = '<h2>About</h2><p>About us page.</p>';
        break;
      case '#contact':
        content.innerHTML = '<h2>Contact</h2><p>Contact us page.</p>';
        break;
      default:
        content.innerHTML = '<h2>Home</h2><p>Welcome to the Home page!</p>';
        break;
    }
  }

  window.addEventListener('hashchange', navigate);
  navigate(); // 초기 로드 시 호출
});
```

##### 연습문제와 해답

1. **SPA를 구현하기 위해 필요한 HTML 구조를 작성하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>SPA Example</title>
     <link rel="stylesheet" href="style.css">
   </head>
   <body>
     <div class="container">
       <nav>
         <ul>
           <li><a href="#home">Home</a></li>
           <li><a href="#about">About</a></li>
           <li><a href="#contact">Contact</a></li>
         </ul>
       </nav>
       <div id="content"></div>
     </div>
     <script src="app.js"></script>
   </body>
   </html>
   ```

2. **라우팅 기능을 구현하기 위한 JavaScript 코드를 작성하세요.**

   ```javascript
   document.addEventListener('DOMContentLoaded', () => {
     const content = document.getElementById('content');

     function navigate() {
       const hash = window.location.hash || '#home';
       switch (hash) {
         case '#home':
           content.innerHTML = '<h2>Home</h2><p>Welcome to the Home page!</p>';
           break;
         case '#about':
           content.innerHTML = '<h2>About</h2><p>About us page.</p>';
           break;
         case '#contact':
           content.innerHTML = '<h2>Contact</h2><p>Contact us page.</p>';
           break;
         default:
           content.innerHTML = '<h2>Home</h2><p>Welcome to the Home page!</p>';
           break;
       }
     }

     window.addEventListener('hashchange', navigate);
     navigate(); // 초기 로드 시 호출
   });
   ```

3. **스타일을 적용하여 SPA의 기본 레이아웃을 만드세요.**

   ```css
   body {
     font-family: Arial, sans-serif;
     background-color: #f4f4f4;
     display: flex;
     justify-content: center;
     align-items: center;
     height: 100vh;
     margin: 0;
   }

   .container {
     background: white;
     padding: 20px;
     border-radius: 5px;
     box-shadow: 0 0 10px rgba(0,0,0,0.1);
     width: 300px;
   }

   nav ul {
     list-style: none;
     padding: 0;
     display: flex;
     justify-content: space-around;
   }

   nav a {
     text-decoration: none;
     color: #007bff;
   }

   nav a:hover {
     text-decoration: underline;
   }

   #content {
     margin-top: 20px;
     text-align: center;
   }
   ```
