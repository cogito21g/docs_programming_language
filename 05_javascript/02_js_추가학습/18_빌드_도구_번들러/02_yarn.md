### 18. 패키지 매니저 및 모듈 번들러

#### 18.2. Yarn

Yarn은 Facebook에서 개발한 패키지 매니저로, NPM과 유사한 기능을 제공합니다. Yarn은 빠르고 안정적인 의존성 관리를 목표로 하며, 특히 대규모 프로젝트에서 유용합니다. 이번 섹션에서는 Yarn의 기본 사용법과 주요 명령어를 학습하겠습니다.

##### 18.2.1. Yarn 기본 명령어

###### Yarn 설치

Yarn을 설치하는 방법은 다음과 같습니다:

```bash
# npm을 통해 Yarn 설치
npm install -g yarn

# 또는 공식 웹사이트에서 설치
https://yarnpkg.com/getting-started/install
```

###### Yarn 초기화

Yarn 프로젝트를 초기화하여 `package.json` 파일을 생성합니다.

```bash
yarn init
```

###### 패키지 설치

Yarn을 사용하여 패키지를 설치합니다. 패키지는 `node_modules` 폴더에 저장되며, `package.json` 파일의 `dependencies` 항목에 추가됩니다.

```bash
# 특정 패키지 설치
yarn add <package-name>

# 예시: lodash 패키지 설치
yarn add lodash

# 전역 설치
yarn global add <package-name>
```

###### 패키지 제거

설치된 패키지를 제거합니다.

```bash
yarn remove <package-name>

# 예시: lodash 패키지 제거
yarn remove lodash
```

###### 개발 의존성 설치

개발 중에만 필요한 패키지를 설치할 때는 `--dev` 옵션을 사용합니다.

```bash
yarn add <package-name> --dev

# 예시: Jest 패키지 설치
yarn add jest --dev
```

###### 패키지 업데이트

설치된 패키지를 업데이트합니다.

```bash
yarn upgrade <package-name>

# 예시: lodash 패키지 업데이트
yarn upgrade lodash
```

###### 프로젝트 의존성 설치

프로젝트의 `package.json` 파일에 정의된 모든 의존성을 설치합니다. 보통 새로 클론한 프로젝트에서 사용합니다.

```bash
yarn install
```

##### 18.2.2. Yarn 스크립트

Yarn 스크립트를 사용하면 반복 작업을 자동화할 수 있습니다. `package.json` 파일의 `scripts` 항목에 스크립트를 정의할 수 있습니다.

###### Yarn 스크립트 정의

```json
{
  "name": "example-project",
  "version": "1.0.0",
  "scripts": {
    "start": "node app.js",
    "test": "jest",
    "build": "webpack --config webpack.config.js"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

###### Yarn 스크립트 실행

```bash
yarn run <script-name>

# 예시: test 스크립트 실행
yarn run test

# start 스크립트는 yarn start로 실행 가능
yarn start
```

##### 18.2.3. Yarn 패키지 관리

###### `package.json`

`package.json` 파일은 프로젝트의 메타데이터와 의존성을 관리하는 파일입니다. 주요 항목은 다음과 같습니다:

- **name**: 패키지 이름
- **version**: 패키지 버전
- **scripts**: Yarn 스크립트
- **dependencies**: 프로젝트의 의존성 패키지
- **devDependencies**: 개발 중에만 필요한 패키지

###### `yarn.lock`

`yarn.lock` 파일은 설치된 의존성의 정확한 버전을 기록하여, 프로젝트를 다른 환경에서 재현할 수 있도록 도와줍니다. 이 파일은 `yarn install` 명령어를 실행할 때 자동으로 생성되며, 의존성의 버전을 고정합니다.

##### 연습문제와 해답

1. **Yarn을 사용하여 새 프로젝트를 초기화하고, lodash 패키지를 설치하세요.**

   ```bash
   mkdir my-project
   cd my-project
   yarn init -y
   yarn add lodash
   ```

2. **Jest를 개발 의존성으로 설치하고, test 스크립트를 정의하세요.**

   ```bash
   yarn add jest --dev
   ```

   ```json
   {
     "name": "example-project",
     "version": "1.0.0",
     "scripts": {
       "test": "jest"
     },
     "dependencies": {},
     "devDependencies": {
       "jest": "^26.6.0"
     }
   }
   ```

   ```bash
   yarn run test
   ```

3. **Yarn 스크립트를 정의하여 Node.js 애플리케이션을 실행하세요.**

   ```json
   {
     "name": "example-project",
     "version": "1.0.0",
     "scripts": {
       "start": "node app.js"
     },
     "dependencies": {},
     "devDependencies": {}
   }
   ```

   ```bash
   yarn start
   ```
