### 8. Bootstrap

#### 8.1. Bootstrap 기초

##### 8.1.1. Bootstrap 설치와 설정

Bootstrap은 웹 개발을 위한 오픈 소스 프론트엔드 프레임워크로, 반응형 웹 디자인을 쉽게 구현할 수 있도록 도와줍니다. 여기서는 Bootstrap을 설치하고 설정하는 방법을 설명하겠습니다.

###### Bootstrap 설치

1. **CDN을 통한 설치**

   Bootstrap을 사용하는 가장 간단한 방법은 CDN을 통해 라이브러리를 포함하는 것입니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Bootstrap Setup Example</title>
     <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
     <!-- Bootstrap 코드 -->
     <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
     <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js"></script>
     <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
   </body>
   </html>
   ```

2. **NPM을 통한 설치**

   NPM을 사용하여 프로젝트에 Bootstrap을 설치할 수 있습니다.

   ```bash
   npm install bootstrap
   ```

   설치 후, JavaScript 파일에서 Bootstrap을 import하여 사용할 수 있습니다.

   ```javascript
   import 'bootstrap/dist/css/bootstrap.min.css';
   import 'bootstrap/dist/js/bootstrap.bundle.min.js';
   ```

###### 기본 예제

Bootstrap을 사용하여 간단한 레이아웃을 만드는 예제입니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bootstrap Basic Example</title>
  <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
  <div class="container">
    <div class="row">
      <div class="col">
        <h1 class="text-center">Hello, Bootstrap!</h1>
        <p>This is a simple Bootstrap example.</p>
      </div>
    </div>
  </div>
  <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js"></script>
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</body>
</html>
```

##### 연습문제와 해답

1. **CDN을 통해 Bootstrap을 포함하고 간단한 버튼을 추가하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Bootstrap Button Example</title>
     <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
   </head>
   <body>
     <div class="container">
       <div class="row">
         <div class="col">
           <h1 class="text-center">Bootstrap Button Example</h1>
           <button type="button" class="btn btn-primary">Click Me!</button>
         </div>
       </div>
     </div>
     <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
     <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js"></script>
     <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
   </body>
   </html>
   ```

2. **NPM을 통해 Bootstrap을 설치하고 간단한 네비게이션 바를 추가하세요.**

   1. **Bootstrap 설치**

      ```bash
      npm install bootstrap
      ```

   2. **HTML 파일 작성**

      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Bootstrap Navbar Example</title>
        <link href="node_modules/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet">
      </head>
      <body>
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
          <a class="navbar-brand" href="#">Navbar</a>
          <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
              <li class="nav-item active">
                <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Features</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Pricing</a>
              </li>
            </ul>
          </div>
        </nav>
        <script src="node_modules/jquery/dist/jquery.slim.min.js"></script>
        <script src="node_modules/@popperjs/core/dist/umd/popper.min.js"></script>
        <script src="node_modules/bootstrap/dist/js/bootstrap.min.js"></script>
      </body>
      </html>
      ```
