### 18. 패키지 매니저 및 모듈 번들러

#### 18.4. Babel

Babel은 최신 JavaScript 문법을 지원하지 않는 구형 브라우저에서도 동작하도록 JavaScript 코드를 변환(트랜스파일)해주는 도구입니다. Babel을 사용하면 최신 ECMAScript 기능을 안전하게 사용할 수 있습니다. 이번 섹션에서는 Babel의 기본 개념과 설정 방법을 학습하겠습니다.

##### 18.4.1. Babel 설치 및 기본 설정

###### Babel 설치

Babel을 설치하려면 `@babel/core`, `@babel/preset-env`, 그리고 `babel-loader`를 설치해야 합니다.

```bash
npm install --save-dev @babel/core @babel/preset-env babel-loader
```

###### Babel 기본 설정

Babel 설정 파일인 `.babelrc`를 프로젝트 루트에 생성하고 기본 설정을 추가합니다.

```json
// .babelrc
{
  "presets": ["@babel/preset-env"]
}
```

##### 18.4.2. Babel과 Webpack 통합

Webpack을 사용하여 Babel을 통합하면 JavaScript 파일을 번들링하면서 동시에 최신 문법을 구형 브라우저에서도 동작하도록 변환할 수 있습니다.

###### Webpack 설정 파일 업데이트

`webpack.config.js` 파일을 수정하여 Babel 로더를 추가합니다.

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
    ],
  },
};
```

##### 18.4.3. Babel 플러그인

Babel 플러그인은 특정 JavaScript 기능을 트랜스파일하는데 도움을 줍니다. 예를 들어, 클래스 프로퍼티를 지원하는 플러그인을 추가할 수 있습니다.

###### 클래스 프로퍼티 플러그인 설치

```bash
npm install --save-dev @babel/plugin-proposal-class-properties
```

###### .babelrc 파일 업데이트

`.babelrc` 파일에 클래스 프로퍼티 플러그인을 추가합니다.

```json
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

##### 18.4.4. Babel 폴리필

폴리필은 특정 JavaScript 기능이 구형 브라우저에서 동작하지 않을 때, 해당 기능을 구현하여 추가하는 것입니다. Babel은 `@babel/polyfill`을 사용하여 폴리필을 제공합니다.

###### Babel 폴리필 설치

```bash
npm install --save @babel/polyfill
```

###### Webpack 엔트리 포인트 업데이트

Webpack의 엔트리 포인트에 Babel 폴리필을 추가합니다.

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: ['@babel/polyfill', './src/index.js'],
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
    ],
  },
};
```

##### 연습문제와 해답

1. **Babel을 설치하고 기본 설정을 추가하세요.**

   ```bash
   npm install --save-dev @babel/core @babel/preset-env babel-loader
   ```

   ```json
   // .babelrc
   {
     "presets": ["@babel/preset-env"]
   }
   ```

2. **Babel과 Webpack을 통합하여 최신 JavaScript 코드를 변환하세요.**

   ```bash
   npm install --save-dev webpack webpack-cli @babel/core @babel/preset-env babel-loader
   ```

   ```javascript
   // webpack.config.js
   const path = require('path');

   module.exports = {
     entry: './src/index.js',
     output: {
       filename: 'bundle.js',
       path: path.resolve(__dirname, 'dist'),
     },
     module: {
       rules: [
         {
           test: /\.js$/,
           exclude: /node_modules/,
           use: {
             loader: 'babel-loader',
           },
         },
       ],
     },
   };
   ```

3. **Babel 플러그인을 사용하여 클래스 프로퍼티를 지원하세요.**

   ```bash
   npm install --save-dev @babel/plugin-proposal-class-properties
   ```

   ```json
   // .babelrc
   {
     "presets": ["@babel/preset-env"],
     "plugins": ["@babel/plugin-proposal-class-properties"]
   }
   ```

4. **Babel 폴리필을 추가하여 구형 브라우저에서도 최신 JavaScript 기능이 동작하도록 하세요.**

   ```bash
   npm install --save @babel/polyfill
   ```

   ```javascript
   // webpack.config.js
   const path = require('path');

   module.exports = {
     entry: ['@babel/polyfill', './src/index.js'],
     output: {
       filename: 'bundle.js',
       path: path.resolve(__dirname, 'dist'),
     },
     module: {
       rules: [
         {
           test: /\.js$/,
           exclude: /node_modules/,
           use: {
             loader: 'babel-loader',
           },
         },
       ],
     },
   };
   ```
