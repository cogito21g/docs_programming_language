### 19. 빌드 도구와 작업 자동화

#### 19.2. Rollup

Rollup은 JavaScript 모듈 번들러로, 주로 라이브러리와 같은 작은 규모의 프로젝트에서 사용됩니다. Rollup은 ES6 모듈을 기반으로 하여 더 작은 번들 파일을 생성할 수 있도록 최적화되어 있습니다. 이번 섹션에서는 Rollup의 기본 개념과 설정 방법을 학습하겠습니다.

##### 19.2.1. Rollup 설치 및 기본 설정

###### Rollup 설치

Rollup을 설치하려면 `rollup`과 `@rollup/plugin-node-resolve`, `@rollup/plugin-commonjs` 플러그인을 설치해야 합니다.

```bash
npm install --save-dev rollup @rollup/plugin-node-resolve @rollup/plugin-commonjs
```

###### Rollup 기본 설정

프로젝트 루트에 `rollup.config.js` 파일을 생성하고 기본 설정을 추가합니다.

```javascript
// rollup.config.js
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'iife', // 즉시 실행 함수 형식으로 번들링
    name: 'MyBundle',
  },
  plugins: [resolve(), commonjs()],
};
```

##### 19.2.2. Rollup 사용

###### Rollup 명령어 실행

Rollup을 사용하여 JavaScript 파일을 번들링합니다.

```bash
npx rollup -c
```

##### 19.2.3. Rollup 플러그인 사용

Rollup은 다양한 플러그인을 사용하여 기능을 확장할 수 있습니다. 예를 들어, Babel 플러그인을 사용하여 ES6 코드를 ES5 코드로 변환할 수 있습니다.

###### Babel 플러그인 설치

```bash
npm install --save-dev @rollup/plugin-babel @babel/core @babel/preset-env
```

###### Babel 설정 파일

프로젝트 루트에 `.babelrc` 파일을 생성하고 설정을 추가합니다.

```json
// .babelrc
{
  "presets": ["@babel/preset-env"]
}
```

###### Rollup 설정 파일 업데이트

`rollup.config.js` 파일을 수정하여 Babel 플러그인을 추가합니다.

```javascript
// rollup.config.js
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';
import babel from '@rollup/plugin-babel';

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'iife',
    name: 'MyBundle',
  },
  plugins: [
    resolve(),
    commonjs(),
    babel({ babelHelpers: 'bundled', exclude: 'node_modules/**' }),
  ],
};
```

##### 19.2.4. Rollup을 사용한 추가 작업

###### 파일 압축

파일을 압축하여 번들 크기를 줄일 수 있습니다. 이를 위해 `rollup-plugin-terser` 플러그인을 사용할 수 있습니다.

1. **플러그인 설치**

   ```bash
   npm install --save-dev rollup-plugin-terser
   ```

2. **설정 파일 업데이트**

   ```javascript
   // rollup.config.js
   import resolve from '@rollup/plugin-node-resolve';
   import commonjs from '@rollup/plugin-commonjs';
   import babel from '@rollup/plugin-babel';
   import { terser } from 'rollup-plugin-terser';

   export default {
     input: 'src/index.js',
     output: {
       file: 'dist/bundle.js',
       format: 'iife',
       name: 'MyBundle',
     },
     plugins: [
       resolve(),
       commonjs(),
       babel({ babelHelpers: 'bundled', exclude: 'node_modules/**' }),
       terser(), // 파일 압축 플러그인 추가
     ],
   };
   ```

##### 연습문제와 해답

1. **Rollup을 설치하고 기본 설정을 추가하세요.**

   ```bash
   npm install --save-dev rollup @rollup/plugin-node-resolve @rollup/plugin-commonjs
   ```

   ```javascript
   // rollup.config.js
   import resolve from '@rollup/plugin-node-resolve';
   import commonjs from '@rollup/plugin-commonjs';

   export default {
     input: 'src/index.js',
     output: {
       file: 'dist/bundle.js',
       format: 'iife',
       name: 'MyBundle',
     },
     plugins: [resolve(), commonjs()],
   };
   ```

2. **Babel 플러그인을 사용하여 ES6 코드를 번들링하세요.**

   ```bash
   npm install --save-dev @rollup/plugin-babel @babel/core @babel/preset-env
   ```

   ```json
   // .babelrc
   {
     "presets": ["@babel/preset-env"]
   }
   ```

   ```javascript
   // rollup.config.js
   import resolve from '@rollup/plugin-node-resolve';
   import commonjs from '@rollup/plugin-commonjs';
   import babel from '@rollup/plugin-babel';

   export default {
     input: 'src/index.js',
     output: {
       file: 'dist/bundle.js',
       format: 'iife',
       name: 'MyBundle',
     },
     plugins: [
       resolve(),
       commonjs(),
       babel({ babelHelpers: 'bundled', exclude: 'node_modules/**' }),
     ],
   };
   ```

3. **파일 압축을 위해 terser 플러그인을 추가하세요.**

   ```bash
   npm install --save-dev rollup-plugin-terser
   ```

   ```javascript
   // rollup.config.js
   import resolve from '@rollup/plugin-node-resolve';
   import commonjs from '@rollup/plugin-commonjs';
   import babel from '@rollup/plugin-babel';
   import { terser } from 'rollup-plugin-terser';

   export default {
     input: 'src/index.js',
     output: {
       file: 'dist/bundle.js',
       format: 'iife',
       name: 'MyBundle',
     },
     plugins: [
       resolve(),
       commonjs(),
       babel({ babelHelpers: 'bundled', exclude: 'node_modules/**' }),
       terser(),
     ],
   };
   ```
