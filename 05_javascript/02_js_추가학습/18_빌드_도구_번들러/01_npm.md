### 18. 패키지 매니저 및 모듈 번들러

#### 18.1. NPM (Node Package Manager)

NPM은 JavaScript 패키지를 관리하기 위한 기본 도구로, Node.js와 함께 설치됩니다. NPM을 사용하면 다양한 패키지를 설치하고 관리할 수 있습니다. 이번 섹션에서는 NPM의 기본 사용법과 중요한 명령어에 대해 학습하겠습니다.

##### 18.1.1. NPM 기본 명령어

###### NPM 초기화

NPM 프로젝트를 초기화하여 `package.json` 파일을 생성합니다.

```bash
npm init
```

###### 패키지 설치

NPM을 사용하여 패키지를 설치합니다. 패키지는 `node_modules` 폴더에 저장되며, `package.json` 파일의 `dependencies` 항목에 추가됩니다.

```bash
# 특정 패키지 설치
npm install <package-name>

# 예시: lodash 패키지 설치
npm install lodash

# 전역 설치
npm install -g <package-name>
```

###### 패키지 제거

설치된 패키지를 제거합니다.

```bash
npm uninstall <package-name>

# 예시: lodash 패키지 제거
npm uninstall lodash
```

###### 개발 의존성 설치

개발 중에만 필요한 패키지를 설치할 때는 `--save-dev` 옵션을 사용합니다.

```bash
npm install <package-name> --save-dev

# 예시: Jest 패키지 설치
npm install jest --save-dev
```

###### 패키지 업데이트

설치된 패키지를 업데이트합니다.

```bash
npm update <package-name>

# 예시: lodash 패키지 업데이트
npm update lodash
```

###### 프로젝트 의존성 설치

프로젝트의 `package.json` 파일에 정의된 모든 의존성을 설치합니다. 보통 새로 클론한 프로젝트에서 사용합니다.

```bash
npm install
```

##### 18.1.2. NPM 스크립트

NPM 스크립트를 사용하면 반복 작업을 자동화할 수 있습니다. `package.json` 파일의 `scripts` 항목에 스크립트를 정의할 수 있습니다.

###### NPM 스크립트 정의

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

###### NPM 스크립트 실행

```bash
npm run <script-name>

# 예시: test 스크립트 실행
npm run test

# start 스크립트는 npm start로 실행 가능
npm start
```

##### 18.1.3. NPM 패키지 관리

###### `package.json`

`package.json` 파일은 프로젝트의 메타데이터와 의존성을 관리하는 파일입니다. 주요 항목은 다음과 같습니다:

- **name**: 패키지 이름
- **version**: 패키지 버전
- **scripts**: NPM 스크립트
- **dependencies**: 프로젝트의 의존성 패키지
- **devDependencies**: 개발 중에만 필요한 패키지

###### `package-lock.json`

`package-lock.json` 파일은 설치된 의존성의 정확한 버전을 기록하여, 프로젝트를 다른 환경에서 재현할 수 있도록 도와줍니다.

##### 연습문제와 해답

1. **NPM을 사용하여 새 프로젝트를 초기화하고, lodash 패키지를 설치하세요.**

   ```bash
   mkdir my-project
   cd my-project
   npm init -y
   npm install lodash
   ```

2. **Jest를 개발 의존성으로 설치하고, test 스크립트를 정의하세요.**

   ```bash
   npm install jest --save-dev
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
   npm run test
   ```

3. **NPM 스크립트를 정의하여 Node.js 애플리케이션을 실행하세요.**

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
   npm start
   ```
