### 19. 빌드 도구와 작업 자동화

#### 19.3. Parcel

Parcel은 구성 파일 없이도 빠르게 작동하는 모듈 번들러입니다. Parcel은 자동으로 종속성을 탐지하고, JavaScript, CSS, HTML, 이미지, 파일 등을 번들링합니다. 이번 섹션에서는 Parcel의 기본 개념과 설정 방법을 학습하겠습니다.

##### 19.3.1. Parcel 설치 및 기본 설정

###### Parcel 설치

Parcel을 설치하려면 `parcel-bundler`를 프로젝트에 설치합니다.

```bash
npm install --save-dev parcel-bundler
```

###### 기본 프로젝트 구조

Parcel은 기본적으로 HTML 파일을 엔트리 포인트로 사용합니다. 예시 프로젝트 구조는 다음과 같습니다.

```plaintext
my-project/
├── src/
│   ├── index.html
│   ├── index.js
│   └── style.css
└── package.json
```

###### 기본 HTML 파일

`src/index.html` 파일을 생성하고, 기본 설정을 추가합니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Parcel Example</title>
  <link rel="stylesheet" href="./style.css">
</head>
<body>
  <h1>Hello, Parcel!</h1>
  <script src="./index.js"></script>
</body>
</html>
```

###### 기본 JavaScript 파일

`src/index.js` 파일을 생성하고, 간단한 JavaScript 코드를 추가합니다.

```javascript
console.log('Hello, Parcel!');
```

###### 기본 CSS 파일

`src/style.css` 파일을 생성하고, 간단한 스타일을 추가합니다.

```css
body {
  font-family: Arial, sans-serif;
}

h1 {
  color: steelblue;
}
```

##### 19.3.2. Parcel 실행

###### Parcel 개발 서버 실행

Parcel을 사용하여 개발 서버를 실행하면, 파일이 변경될 때마다 자동으로 새로 고침됩니다.

1. **package.json에 스크립트 추가**

   ```json
   {
     "name": "my-project",
     "version": "1.0.0",
     "scripts": {
       "start": "parcel src/index.html"
     },
     "devDependencies": {
       "parcel-bundler": "^1.12.4"
     }
   }
   ```

2. **개발 서버 실행**

   ```bash
   npm start
   ```

###### 번들링된 파일 생성

Parcel을 사용하여 번들링된 파일을 생성합니다.

```bash
npx parcel build src/index.html
```

##### 19.3.3. Parcel의 추가 기능

###### Babel 통합

Parcel은 Babel을 기본적으로 지원합니다. Babel 설정 파일을 추가하여 최신 JavaScript 기능을 사용할 수 있습니다.

1. **Babel 설정 파일 생성**

   프로젝트 루트에 `.babelrc` 파일을 생성하고, 설정을 추가합니다.

   ```json
   {
     "presets": ["@babel/preset-env"]
   }
   ```

2. **Babel 프리셋 설치**

   ```bash
   npm install --save-dev @babel/preset-env
   ```

###### PostCSS 통합

Parcel은 PostCSS를 기본적으로 지원합니다. PostCSS 설정 파일을 추가하여 CSS를 처리할 수 있습니다.

1. **PostCSS 설정 파일 생성**

   프로젝트 루트에 `postcss.config.js` 파일을 생성하고, 설정을 추가합니다.

   ```javascript
   // postcss.config.js
   module.exports = {
     plugins: {
       autoprefixer: {}
     }
   };
   ```

2. **Autoprefixer 설치**

   ```bash
   npm install --save-dev autoprefixer
   ```

##### 연습문제와 해답

1. **Parcel을 설치하고 기본 설정을 추가하세요.**

   ```bash
   npm install --save-dev parcel-bundler
   ```

   ```plaintext
   my-project/
   ├── src/
   │   ├── index.html
   │   ├── index.js
   │   └── style.css
   └── package.json
   ```

2. **Parcel을 사용하여 개발 서버를 실행하세요.**

   ```json
   {
     "name": "my-project",
     "version": "1.0.0",
     "scripts": {
       "start": "parcel src/index.html"
     },
     "devDependencies": {
       "parcel-bundler": "^1.12.4"
     }
   }
   ```

   ```bash
   npm start
   ```

3. **Parcel과 Babel을 통합하여 최신 JavaScript 기능을 사용하세요.**

   ```json
   {
     "presets": ["@babel/preset-env"]
   }
   ```

   ```bash
   npm install --save-dev @babel/preset-env
   ```

4. **Parcel과 PostCSS를 통합하여 CSS를 처리하세요.**

   ```javascript
   // postcss.config.js
   module.exports = {
     plugins: {
       autoprefixer: {}
     }
   };
   ```

   ```bash
   npm install --save-dev autoprefixer
   ```
