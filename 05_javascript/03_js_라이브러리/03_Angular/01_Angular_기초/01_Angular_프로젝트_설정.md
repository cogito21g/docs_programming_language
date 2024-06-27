### 3. Angular

#### 3.1. Angular 기초

##### 3.1.1. Angular 프로젝트 설정

Angular는 Google에서 개발한 프레임워크로, SPA(Single Page Application) 개발에 사용됩니다. Angular CLI를 사용하면 쉽게 Angular 프로젝트를 설정할 수 있습니다.

###### Angular CLI 설치

1. **Node.js 설치**

   Angular CLI를 설치하기 전에 Node.js가 필요합니다. [Node.js 공식 웹사이트](https://nodejs.org)에서 Node.js를 다운로드하고 설치하세요.

2. **Angular CLI 설치**

   npm(Node Package Manager)을 사용하여 Angular CLI를 설치합니다.

   ```bash
   npm install -g @angular/cli
   ```

3. **설치 확인**

   Angular CLI가 정상적으로 설치되었는지 확인합니다.

   ```bash
   ng --version
   ```

###### Angular 프로젝트 생성

1. **새 프로젝트 생성**

   Angular CLI를 사용하여 새 프로젝트를 생성합니다.

   ```bash
   ng new my-angular-app
   ```

   - 프로젝트 이름을 입력하고 기본 옵션을 선택합니다.
   - Angular Routing 설정 여부를 묻는 질문에는 'Yes'를 선택합니다.
   - CSS 프리프로세서 선택에서는 'CSS'를 선택합니다.

2. **프로젝트 디렉토리로 이동**

   생성된 프로젝트 디렉토리로 이동합니다.

   ```bash
   cd my-angular-app
   ```

3. **프로젝트 실행**

   개발 서버를 시작하여 프로젝트를 실행합니다.

   ```bash
   ng serve
   ```

   - 브라우저에서 `http://localhost:4200`을 열어 Angular 애플리케이션이 실행되는지 확인합니다.

###### Angular 프로젝트 구조

1. **프로젝트 폴더 구조**

   Angular 프로젝트의 기본 폴더 구조는 다음과 같습니다:

   ```
   my-angular-app/
   ├── e2e/                   # End-to-End 테스트 폴더
   ├── node_modules/          # 설치된 npm 패키지 폴더
   ├── src/                   # 소스 코드 폴더
   │   ├── app/               # 애플리케이션 코드 폴더
   │   │   ├── app.component.css  # 루트 컴포넌트의 스타일
   │   │   ├── app.component.html # 루트 컴포넌트의 템플릿
   │   │   ├── app.component.ts   # 루트 컴포넌트의 클래스
   │   │   ├── app.module.ts      # 루트 모듈
   │   ├── assets/            # 애플리케이션에서 사용하는 자산
   │   ├── environments/      # 환경 설정 파일
   │   ├── index.html         # 메인 HTML 파일
   │   ├── main.ts            # 애플리케이션의 진입점
   │   ├── polyfills.ts       # 폴리필 설정 파일
   │   ├── styles.css         # 전역 스타일 파일
   │   └── test.ts            # 테스트 설정 파일
   ├── angular.json           # Angular CLI 설정 파일
   ├── package.json           # 프로젝트 메타데이터와 의존성
   ├── tsconfig.json          # TypeScript 설정 파일
   └── tslint.json            # TSLint 설정 파일
   ```

2. **주요 파일 설명**

   - `src/app/app.module.ts`: 애플리케이션의 루트 모듈을 정의합니다.
   - `src/app/app.component.ts`: 루트 컴포넌트 클래스를 정의합니다.
   - `src/app/app.component.html`: 루트 컴포넌트의 템플릿을 정의합니다.
   - `src/app/app.component.css`: 루트 컴포넌트의 스타일을 정의합니다.
   - `src/main.ts`: 애플리케이션의 진입점입니다.
   - `angular.json`: Angular CLI 설정 파일입니다.
   - `package.json`: 프로젝트 메타데이터와 의존성을 관리합니다.
   - `tsconfig.json`: TypeScript 설정 파일입니다.

###### Angular CLI 명령어

1. **컴포넌트 생성**

   Angular CLI를 사용하여 새로운 컴포넌트를 생성합니다.

   ```bash
   ng generate component component-name
   ```

2. **서비스 생성**

   Angular CLI를 사용하여 새로운 서비스를 생성합니다.

   ```bash
   ng generate service service-name
   ```

3. **모듈 생성**

   Angular CLI를 사용하여 새로운 모듈을 생성합니다.

   ```bash
   ng generate module module-name
   ```

###### 예제

1. **Angular CLI 설치**

   ```bash
   npm install -g @angular/cli
   ```

2. **새 프로젝트 생성**

   ```bash
   ng new my-angular-app
   cd my-angular-app
   ```

3. **프로젝트 실행**

   ```bash
   ng serve
   ```

4. **새 컴포넌트 생성**

   ```bash
   ng generate component my-new-component
   ```

5. **새 서비스 생성**

   ```bash
   ng generate service my-new-service
   ```

6. **새 모듈 생성**

   ```bash
   ng generate module my-new-module
   ```

##### 연습문제와 해답

1. **Angular CLI를 설치하고 새로운 프로젝트를 생성하세요.**

   ```bash
   npm install -g @angular/cli
   ng new my-angular-app
   cd my-angular-app
   ```

2. **생성된 프로젝트를 실행하세요.**

   ```bash
   ng serve
   ```

3. **새로운 컴포넌트를 생성하세요.**

   ```bash
   ng generate component my-new-component
   ```

4. **새로운 서비스를 생성하세요.**

   ```bash
   ng generate service my-new-service
   ```

5. **새로운 모듈을 생성하세요.**

   ```bash
   ng generate module my-new-module
   ```
