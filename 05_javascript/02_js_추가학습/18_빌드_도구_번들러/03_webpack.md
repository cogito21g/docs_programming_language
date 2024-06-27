### 18. 패키지 매니저 및 모듈 번들러

#### 18.3. Webpack

Webpack은 모듈 번들러로, JavaScript 애플리케이션을 모듈 단위로 관리하고, 하나의 파일로 번들링하여 최적화된 자바스크립트 파일을 생성해줍니다. Webpack은 다양한 플러그인과 로더를 통해 JavaScript 외에도 CSS, 이미지 등의 자산을 처리할 수 있습니다. 이번 섹션에서는 Webpack의 기본 개념과 설정 방법을 학습하겠습니다.

##### 18.3.1. Webpack 기본 개념

- **Entry**: Webpack이 애플리케이션을 번들링하기 시작하는 시작점입니다.
- **Output**: 번들링된 파일이 저장되는 위치를 정의합니다.
- **Loaders**: 파일을 모듈로 변환하는 데 사용됩니다. 예: Babel 로더를 사용하여 ES6 코드를 ES5 코드로 변환.
- **Plugins**: 번들링 과정에서 다양한 작업을 수행할 수 있도록 도와줍니다. 예: HTML 파일 생성, CSS 파일 추출 등.

##### 18.3.2. Webpack 설치 및 기본 설정

###### Webpack 설치

Webpack과 Webpack CLI를 설치합니다.

```bash
npm install --save-dev webpack webpack-cli
```

###### Webpack 설정 파일

Webpack 설정 파일(`webpack.config.js`)을 생성하고 기본 설정을 추가합니다.

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.js', // 엔트리 포인트
  output: {
    filename: 'bundle.js', // 출력 파일명
    path: path.resolve(__dirname, 'dist'), // 출력 경로
  },
  module: {
    rules: [
      {
        test: /\.js$/, // .js 파일에 대해
        exclude: /node_modules/, // node_modules 제외
        use: {
          loader: 'babel-loader', // Babel 로더 사용
        },
      },
      {
        test: /\.css$/, // .css 파일에 대해
        use: ['style-loader', 'css-loader'], // CSS 로더 사용
      },
    ],
  },
  plugins: [],
};
```

###### Babel 설정

Babel을 사용하여 최신 JavaScript 코드를 구형 브라우저에서도 동작하도록 변환합니다.

1. **Babel 설치**

   ```bash
   npm install --save-dev babel-loader @babel/core @babel/preset-env
   ```

2. **Babel 설정 파일**

   `.babelrc` 파일을 생성하고 설정을 추가합니다.

   ```json
   {
     "presets": ["@babel/preset-env"]
   }
   ```

##### 18.3.3. Webpack Dev Server 설정

Webpack Dev Server를 사용하면 개발 중에 애플리케이션을 자동으로 새로고침하고, 빠르게 개발할 수 있습니다.

1. **Webpack Dev Server 설치**

   ```bash
   npm install --save-dev webpack-dev-server
   ```

2. **Webpack 설정 파일 업데이트**

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
         {
           test: /\.css$/,
           use: ['style-loader', 'css-loader'],
         },
       ],
     },
     devServer: {
       contentBase: path.join(__dirname, 'dist'),
       compress: true,
       port: 9000,
     },
     plugins: [],
   };
   ```

3. **package.json에 스크립트 추가**

   ```json
   {
     "scripts": {
       "start": "webpack serve --open"
     }
   }
   ```

4. **Webpack Dev Server 실행**

   ```bash
   npm start
   ```

##### 18.3.4. 플러그인 사용

Webpack 플러그인을 사용하면 번들링 과정에서 다양한 작업을 수행할 수 있습니다. 예를 들어, HTML 파일을 자동으로 생성하는 `html-webpack-plugin`을 사용할 수 있습니다.

1. **플러그인 설치**

   ```bash
   npm install --save-dev html-webpack-plugin
   ```

2. **Webpack 설정 파일 업데이트**

   ```javascript
   // webpack.config.js
   const path = require('path');
   const HtmlWebpackPlugin = require('html-webpack-plugin');

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
         {
           test: /\.css$/,
           use: ['style-loader', 'css-loader'],
         },
       ],
     },
     plugins: [
       new HtmlWebpackPlugin({
         template: './src/index.html', // 템플릿 파일
       }),
     ],
     devServer: {
       contentBase: path.join(__dirname, 'dist'),
       compress: true,
       port: 9000,
     },
   };
   ```

##### 연습문제와 해답

1. **Webpack과 Babel을 사용하여 ES6 코드를 번들링하고 변환하세요.**

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

   ```json
   // .babelrc
   {
     "presets": ["@babel/preset-env"]
   }
   ```

   ```bash
   npm install --save-dev webpack webpack-cli babel-loader @babel/core @babel/preset-env
   ```

2. **Webpack Dev Server를 설정하여 개발 환경을 구축하세요.**

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
         {
           test: /\.css$/,
           use: ['style-loader', 'css-loader'],
         },
       ],
     },
     devServer: {
       contentBase: path.join(__dirname, 'dist'),
       compress: true,
       port: 9000,
     },
   };
   ```

   ```bash
   npm install --save-dev webpack-dev-server
   npm start
   ```

3. **HTML Webpack Plugin을 사용하여 HTML 파일을 자동으로 생성하세요.**

   ```javascript
   // webpack.config.js
   const path = require('path');
   const HtmlWebpackPlugin = require('html-webpack-plugin');

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
         {
           test: /\.css$/,
           use: ['style-loader', 'css-loader'],
         },
       ],
     },
     plugins: [
       new HtmlWebpackPlugin({
         template: './src/index.html',
       }),
     ],
     devServer: {
       contentBase: path.join(__dirname, 'dist'),
       compress: true,
       port: 9000,
     },
   };
   ```

   ```bash
   npm install --save-dev html-webpack-plugin
   ```
