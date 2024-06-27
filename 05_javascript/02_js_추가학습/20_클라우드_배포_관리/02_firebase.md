### 20. 클라우드 배포 및 관리

#### 20.2. Firebase와 JavaScript

##### 20.2.1. Firebase Authentication

Firebase Authentication은 사용자가 앱에 쉽게 로그인할 수 있도록 다양한 인증 방식을 제공합니다. 이 섹션에서는 Firebase Authentication을 사용하여 이메일 및 비밀번호로 사용자를 인증하는 방법을 학습하겠습니다.

###### Firebase 프로젝트 생성

1. **Firebase 콘솔에 로그인**
   [Firebase 콘솔](https://console.firebase.google.com/)에 로그인합니다.

2. **새 프로젝트 생성**
   새 프로젝트를 생성하고 프로젝트 이름을 지정합니다.

3. **Authentication 활성화**
   Firebase 콘솔에서 프로젝트를 선택한 후, 'Authentication' 섹션으로 이동하여 이메일/비밀번호 인증을 활성화합니다.

###### Firebase 초기화

1. **Firebase SDK 설치**

   ```bash
   npm install firebase
   ```

2. **Firebase 초기화**

   `src/firebase.js` 파일을 생성하고 Firebase 초기화 코드를 추가합니다.

   ```javascript
   // src/firebase.js
   import firebase from 'firebase/app';
   import 'firebase/auth';

   const firebaseConfig = {
     apiKey: 'YOUR_API_KEY',
     authDomain: 'YOUR_PROJECT_ID.firebaseapp.com',
     projectId: 'YOUR_PROJECT_ID',
     storageBucket: 'YOUR_PROJECT_ID.appspot.com',
     messagingSenderId: 'YOUR_MESSAGING_SENDER_ID',
     appId: 'YOUR_APP_ID',
   };

   firebase.initializeApp(firebaseConfig);

   export const auth = firebase.auth();
   ```

###### 이메일/비밀번호로 사용자 인증

1. **사용자 등록**

   `src/register.js` 파일을 생성하고 사용자 등록 기능을 추가합니다.

   ```javascript
   // src/register.js
   import { auth } from './firebase';

   const register = (email, password) => {
     auth.createUserWithEmailAndPassword(email, password)
       .then(userCredential => {
         console.log('User registered:', userCredential.user);
       })
       .catch(error => {
         console.error('Error registering user:', error);
       });
   };

   export default register;
   ```

2. **사용자 로그인**

   `src/login.js` 파일을 생성하고 사용자 로그인 기능을 추가합니다.

   ```javascript
   // src/login.js
   import { auth } from './firebase';

   const login = (email, password) => {
     auth.signInWithEmailAndPassword(email, password)
       .then(userCredential => {
         console.log('User logged in:', userCredential.user);
       })
       .catch(error => {
         console.error('Error logging in user:', error);
       });
   };

   export default login;
   ```

##### 20.2.2. Firestore와 실시간 데이터베이스

Firestore는 Firebase에서 제공하는 NoSQL 클라우드 데이터베이스로, 실시간 데이터 동기화를 지원합니다. 이번 섹션에서는 Firestore를 사용하여 데이터를 저장하고 실시간으로 동기화하는 방법을 학습하겠습니다.

###### Firestore 설정

1. **Firestore 활성화**
   Firebase 콘솔에서 프로젝트를 선택한 후, 'Firestore Database' 섹션으로 이동하여 Firestore를 활성화합니다.

2. **Firestore 초기화**

   `src/firebase.js` 파일을 업데이트하여 Firestore를 초기화합니다.

   ```javascript
   // src/firebase.js
   import firebase from 'firebase/app';
   import 'firebase/auth';
   import 'firebase/firestore';

   const firebaseConfig = {
     apiKey: 'YOUR_API_KEY',
     authDomain: 'YOUR_PROJECT_ID.firebaseapp.com',
     projectId: 'YOUR_PROJECT_ID',
     storageBucket: 'YOUR_PROJECT_ID.appspot.com',
     messagingSenderId: 'YOUR_MESSAGING_SENDER_ID',
     appId: 'YOUR_APP_ID',
   };

   firebase.initializeApp(firebaseConfig);

   export const auth = firebase.auth();
   export const firestore = firebase.firestore();
   ```

###### 데이터 저장 및 읽기

1. **데이터 저장**

   `src/saveData.js` 파일을 생성하고 데이터를 Firestore에 저장하는 기능을 추가합니다.

   ```javascript
   // src/saveData.js
   import { firestore } from './firebase';

   const saveData = (collection, data) => {
     firestore.collection(collection).add(data)
       .then(docRef => {
         console.log('Document written with ID:', docRef.id);
       })
       .catch(error => {
         console.error('Error adding document:', error);
       });
   };

   export default saveData;
   ```

2. **데이터 읽기**

   `src/getData.js` 파일을 생성하고 Firestore에서 데이터를 읽는 기능을 추가합니다.

   ```javascript
   // src/getData.js
   import { firestore } from './firebase';

   const getData = (collection) => {
     firestore.collection(collection).get()
       .then(querySnapshot => {
         querySnapshot.forEach(doc => {
           console.log(`${doc.id} =>`, doc.data());
         });
       })
       .catch(error => {
         console.error('Error getting documents:', error);
       });
   };

   export default getData;
   ```

###### 실시간 데이터 동기화

1. **실시간 데이터 구독**

   `src/subscribeData.js` 파일을 생성하고 Firestore에서 실시간 데이터 동기화를 설정합니다.

   ```javascript
   // src/subscribeData.js
   import { firestore } from './firebase';

   const subscribeData = (collection, callback) => {
     return firestore.collection(collection).onSnapshot(snapshot => {
       const data = [];
       snapshot.forEach(doc => data.push({ id: doc.id, ...doc.data() }));
       callback(data);
     });
   };

   export default subscribeData;
   ```

##### 20.2.3. Firebase Hosting

Firebase Hosting은 웹 애플리케이션을 배포할 수 있는 빠르고 안전한 호스팅 서비스입니다. 이번 섹션에서는 Firebase Hosting을 사용하여 웹 애플리케이션을 배포하는 방법을 학습하겠습니다.

###### Firebase Hosting 설정

1. **Firebase CLI 설치**

   ```bash
   npm install -g firebase-tools
   ```

2. **Firebase 프로젝트 초기화**

   ```bash
   firebase init
   ```

   - **Hosting** 옵션을 선택합니다.
   - 기존 프로젝트를 선택하거나 새 프로젝트를 생성합니다.
   - `public` 디렉토리를 `build`로 설정합니다.
   - 싱글 페이지 애플리케이션일 경우, `y`를 선택하여 `index.html`을 재작성합니다.

3. **Firebase 프로젝트 배포**

   ```bash
   npm run build
   firebase deploy
   ```

##### 연습문제와 해답

1. **Firebase Authentication을 사용하여 이메일/비밀번호로 사용자를 등록하고 로그인하세요.**

   ```javascript
   // src/register.js
   import { auth } from './firebase';

   const register = (email, password) => {
     auth.createUserWithEmailAndPassword(email, password)
       .then(userCredential => {
         console.log('User registered:', userCredential.user);
       })
       .catch(error => {
         console.error('Error registering user:', error);
       });
   };

   export default register;
   ```

   ```javascript
   // src/login.js
   import { auth } from './firebase';

   const login = (email, password) => {
     auth.signInWithEmailAndPassword(email, password)
       .then(userCredential => {
         console.log('User logged in:', userCredential.user);
       })
       .catch(error => {
         console.error('Error logging in user:', error);
       });
   };

   export default login;
   ```

2. **Firestore를 사용하여 데이터를 저장하고 읽으세요.**

   ```javascript
   // src/saveData.js
   import { firestore } from './firebase';

   const saveData = (collection, data) => {
     firestore.collection(collection).add(data)
       .then(docRef => {
         console.log('Document written with ID:', docRef.id);
       })
       .catch(error => {
         console.error('Error adding document:', error);
       });
   };

   export default saveData;
   ```

   ```javascript
   // src/getData.js
   import { firestore } from './firebase';

   const getData = (collection) => {
     firestore.collection(collection).get()
       .then(querySnapshot => {
         querySnapshot.forEach(doc => {
           console.log(`${doc.id} =>`, doc.data());
         });
       })
       .catch(error => {
         console.error('Error getting documents:', error);
       });
   };

   export default getData;
   ```

3. **Firebase Hosting을 사용하여 웹 애플리케이션을 배포하세요.**

   ```bash
   npm install -g firebase-tools
   firebase init
   npm run build
   firebase deploy
   ```
