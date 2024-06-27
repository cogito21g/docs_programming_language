### 14. 보안

#### 14.1. XSS (Cross-Site Scripting) 방지

XSS(Cross-Site Scripting)는 웹 애플리케이션에서 사용자 입력을 통해 악성 스크립트를 주입하는 공격입니다. XSS는 사용자의 세션 하이재킹, 악성 코드 실행, 피싱 공격 등 다양한 보안 문제를 일으킬 수 있습니다. XSS 방지를 위해 다음과 같은 방법을 사용할 수 있습니다.

##### XSS의 종류

1. **반사형 XSS(Reflected XSS)**: 공격자가 만든 링크를 클릭할 때 발생하는 XSS.
2. **저장형 XSS(Stored XSS)**: 악성 스크립트가 데이터베이스에 저장되어 여러 사용자에게 실행되는 XSS.
3. **DOM 기반 XSS(DOM-based XSS)**: 클라이언트 측 코드에서 DOM을 직접 조작할 때 발생하는 XSS.

##### XSS 방지 방법

###### 1. 입력 검증(Input Validation)

모든 사용자 입력을 철저히 검증하여 예상치 못한 값이 입력되지 않도록 합니다. 이를 위해 정규 표현식과 화이트리스트를 사용할 수 있습니다.

```javascript
function sanitizeInput(input) {
  const regex = /^[a-zA-Z0-9 ]*$/; // 알파벳, 숫자, 공백만 허용
  if (!regex.test(input)) {
    throw new Error("Invalid input");
  }
  return input;
}
```

###### 2. 출력 인코딩(Output Encoding)

사용자 입력을 HTML, JavaScript, URL 등 다양한 컨텍스트에 출력할 때는 적절한 인코딩을 적용합니다.

```javascript
function escapeHTML(str) {
  return str.replace(/&/g, "&amp;")
            .replace(/</g, "&lt;")
            .replace(/>/g, "&gt;")
            .replace(/"/g, "&quot;")
            .replace(/'/g, "&#39;");
}
```

###### 3. 보안 헤더 설정(Security Headers)

HTTP 응답에 보안 헤더를 설정하여 XSS 공격을 방지할 수 있습니다.

- **Content Security Policy (CSP)**: 스크립트 소스를 제한하여 XSS를 방지합니다.

  ```http
  Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted-cdn.com;
  ```

- **X-XSS-Protection**: 브라우저에서 XSS 필터를 활성화합니다.

  ```http
  X-XSS-Protection: 1; mode=block
  ```

###### 4. 프레임워크 보안 기능 활용

많은 웹 프레임워크가 XSS 방지를 위한 보안 기능을 내장하고 있습니다. 이러한 기능을 활용하여 보안을 강화할 수 있습니다.

- **React**: JSX에서 자동으로 HTML 이스케이프를 적용합니다.
- **Angular**: 데이터 바인딩 시 자동으로 XSS 방지를 위한 인코딩을 수행합니다.

##### 연습문제와 해답

1. **사용자 입력을 검증하여 예상치 못한 값이 입력되지 않도록 하세요.**

   ```javascript
   function sanitizeInput(input) {
     const regex = /^[a-zA-Z0-9 ]*$/; // 알파벳, 숫자, 공백만 허용
     if (!regex.test(input)) {
       throw new Error("Invalid input");
     }
     return input;
   }

   try {
     console.log(sanitizeInput("validInput123")); // validInput123
     console.log(sanitizeInput("<script>alert('XSS')</script>")); // Error: Invalid input
   } catch (error) {
     console.error(error.message);
   }
   ```

2. **출력 시 사용자 입력을 HTML 이스케이프 처리하세요.**

   ```javascript
   function escapeHTML(str) {
     return str.replace(/&/g, "&amp;")
               .replace(/</g, "&lt;")
               .replace(/>/g, "&gt;")
               .replace(/"/g, "&quot;")
               .replace(/'/g, "&#39;");
   }

   const userInput = "<script>alert('XSS')</script>";
   const safeOutput = escapeHTML(userInput);
   console.log(safeOutput); // &lt;script&gt;alert(&#39;XSS&#39;)&lt;/script&gt;
   ```

3. **Content Security Policy (CSP) 헤더를 설정하여 스크립트 소스를 제한하세요.**

   ```http
   Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted-cdn.com;
   ```

4. **React를 사용하여 XSS 공격을 방지하세요.**

   ```javascript
   import React from 'react';

   function App() {
     const userInput = "<script>alert('XSS')</script>";
     return (
       <div>
         <h1>User Input</h1>
         <div>{userInput}</div> {/* React는 자동으로 이스케이프 처리 */}
       </div>
     );
   }

   export default App;
   ```
