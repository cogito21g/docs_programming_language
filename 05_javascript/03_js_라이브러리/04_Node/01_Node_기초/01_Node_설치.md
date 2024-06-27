### 4. Node.js

#### 4.1. Node.js 기초

##### 4.1.1. Node.js 설치와 설정

Node.js는 서버 측에서 JavaScript를 실행할 수 있는 환경입니다. 다음은 Node.js를 설치하고 기본 설정을 하는 방법입니다.

###### Node.js 설치

1. **공식 웹사이트에서 설치**

   Node.js의 공식 웹사이트에서 최신 LTS(Long Term Support) 버전을 다운로드하고 설치합니다.

   - [Node.js 다운로드](https://nodejs.org/)

2. **버전 확인**

   Node.js와 npm(Node Package Manager)이 제대로 설치되었는지 확인합니다.

   ```bash
   node -v
   npm -v
   ```

3. **nvm(Node Version Manager) 사용**

   nvm을 사용하면 여러 버전의 Node.js를 쉽게 관리할 수 있습니다.

   - nvm 설치 (Mac/Linux)

     ```bash
     curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
     ```

   - nvm 설치 (Windows)

     - [nvm-windows 다운로드](https://github.com/coreybutler/nvm-windows/releases)

   - nvm을 사용하여 Node.js 설치

     ```bash
     nvm install node
     nvm use node
     ```

###### Node.js 프로젝트 설정

1. **프로젝트 디렉토리 생성**

   새로운 프로젝트를 위한 디렉토리를 생성하고 이동합니다.

   ```bash
   mkdir my-node-app
   cd my-node-app
   ```

2. **package.json 생성**

   npm init 명령어를 사용하여 package.json 파일을 생성합니다. 이 파일은 프로젝트의 메타데이터를 포함하며, 의존성 관리를 돕습니다.

   ```bash
   npm init -y
   ```

   `package.json` 파일이 생성됩니다.

   ```json
   {
     "name": "my-node-app",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "author": "",
     "license": "ISC"
   }
   ```

3. **의존성 설치**

   필요한 의존성을 설치합니다. 예를 들어, Express.js를 설치합니다.

   ```bash
   npm install express
   ```

4. **기본 서버 설정**

   `index.js` 파일을 생성하고, 기본 Express 서버를 설정합니다.

   ```javascript
   // index.js
   const express = require('express');
   const app = express();

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   const PORT = process.env.PORT || 3000;
   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

5. **서버 실행**

   Node.js 서버를 실행합니다.

   ```bash
   node index.js
   ```

   브라우저에서 `http://localhost:3000`을 열어 "Hello, World!" 메시지를 확인합니다.

###### 예제

1. **Node.js 설치 및 버전 확인 예제**

   ```bash
   node -v
   npm -v
   ```

2. **nvm을 사용하여 Node.js 설치 예제**

   ```bash
   nvm install node
   nvm use node
   ```

3. **프로젝트 디렉토리 생성 및 package.json 파일 생성 예제**

   ```bash
   mkdir my-node-app
   cd my-node-app
   npm init -y
   ```

4. **Express.js 설치 및 기본 서버 설정 예제**

   ```bash
   npm install express
   ```

   ```javascript
   // index.js
   const express = require('express');
   const app = express();

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   const PORT = process.env.PORT || 3000;
   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

5. **서버 실행 예제**

   ```bash
   node index.js
   ```

##### 연습문제와 해답

1. **Node.js를 설치하고, 버전을 확인하세요.**

   ```bash
   node -v
   npm -v
   ```

2. **nvm을 사용하여 Node.js의 최신 버전을 설치하고, 사용할 버전을 설정하세요.**

   ```bash
   nvm install node
   nvm use node
   ```

3. **새로운 Node.js 프로젝트를 생성하고, package.json 파일을 생성하세요.**

   ```bash
   mkdir my-node-app
   cd my-node-app
   npm init -y
   ```

4. **Express.js를 설치하고, "Hello, World!"를 출력하는 기본 서버를 설정하세요.**

   ```bash
   npm install express
   ```

   ```javascript
   // index.js
   const express = require('express');
   const app = express();

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   const PORT = process.env.PORT || 3000;
   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

5. **Node.js 서버를 실행하고, 브라우저에서 "Hello, World!" 메시지를 확인하세요.**

   ```bash
   node index.js
   ```
