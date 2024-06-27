### 10. 기타 중요한 주제

#### 10.9. CORS와 보안

CORS(Cross-Origin Resource Sharing)는 웹 페이지가 다른 도메인에서 자원을 요청할 때 발생하는 보안 문제를 해결하기 위한 메커니즘입니다. 기본적으로 웹 브라우저는 동일 출처 정책(Same-Origin Policy)을 통해 다른 도메인에서 자원을 요청하는 것을 제한합니다. CORS는 이러한 제한을 완화하여 특정 조건에서 다른 도메인에 자원을 요청할 수 있도록 허용합니다.

##### CORS 기본 개념

1. **동일 출처 정책(Same-Origin Policy)**: 웹 페이지가 자신의 도메인 외부의 리소스에 접근하는 것을 제한합니다.
2. **출처(Origin)**: 도메인, 프로토콜, 포트를 포함하는 URL의 일부입니다.
3. **CORS 헤더**: 서버가 응답할 때 설정하여 브라우저가 요청을 허용할지 결정합니다.

##### CORS 헤더

서버는 CORS를 설정하기 위해 응답 헤더에 특정 헤더를 추가합니다.

1. **Access-Control-Allow-Origin**: 요청을 허용할 출처를 지정합니다.

   ```http
   Access-Control-Allow-Origin: https://example.com
   ```

2. **Access-Control-Allow-Methods**: 요청을 허용할 HTTP 메서드를 지정합니다.

   ```http
   Access-Control-Allow-Methods: GET, POST, PUT, DELETE
   ```

3. **Access-Control-Allow-Headers**: 요청을 허용할 헤더를 지정합니다.

   ```http
   Access-Control-Allow-Headers: Content-Type, Authorization
   ```

4. **Access-Control-Allow-Credentials**: 자격 증명(Credentials, 쿠키 등)을 포함한 요청을 허용합니다.

   ```http
   Access-Control-Allow-Credentials: true
   ```

##### 예제

1. **간단한 CORS 요청**

   ```javascript
   fetch('https://api.example.com/data', {
     method: 'GET',
     headers: {
       'Content-Type': 'application/json'
     }
   })
   .then(response => response.json())
   .then(data => console.log(data))
   .catch(error => console.error('Error:', error));
   ```

   서버 응답 헤더:

   ```http
   HTTP/1.1 200 OK
   Access-Control-Allow-Origin: *
   Content-Type: application/json
   ```

2. **인증 정보를 포함한 CORS 요청**

   ```javascript
   fetch('https://api.example.com/data', {
     method: 'GET',
     credentials: 'include', // 자격 증명 포함
     headers: {
       'Content-Type': 'application/json'
     }
   })
   .then(response => response.json())
   .then(data => console.log(data))
   .catch(error => console.error('Error:', error));
   ```

   서버 응답 헤더:

   ```http
   HTTP/1.1 200 OK
   Access-Control-Allow-Origin: https://example.com
   Access-Control-Allow-Credentials: true
   Content-Type: application/json
   ```

##### 프리플라이트 요청 (Preflight Request)

특정 조건을 만족하는 CORS 요청은 실제 요청 전에 프리플라이트 요청을 보냅니다. 프리플라이트 요청은 HTTP OPTIONS 메서드를 사용하여 서버에 실제 요청이 허용되는지 확인합니다.

###### 조건

- HTTP 메서드가 `GET`, `POST`, `HEAD` 외의 메서드인 경우 (`PUT`, `DELETE` 등)
- 사용자 정의 헤더가 포함된 경우 (`Content-Type`, `Authorization` 등)
- 자격 증명 포함 요청인 경우

###### 예제

프리플라이트 요청:

```http
OPTIONS /data HTTP/1.1
Host: api.example.com
Origin: https://example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type
```

서버 응답:

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: Content-Type
Access-Control-Allow-Credentials: true
Content-Type: application/json
```

##### 보안 고려사항

1. **CORS 설정을 신중하게 적용**: `Access-Control-Allow-Origin` 헤더에 와일드카드(`*`)를 사용하면 모든 도메인에서 접근이 가능해져 보안에 취약할 수 있습니다. 특정 도메인만 허용하도록 설정합니다.

2. **민감한 데이터 보호**: `Access-Control-Allow-Credentials`를 `true`로 설정하면 자격 증명을 포함한 요청을 허용합니다. 이를 사용할 때는 반드시 `Access-Control-Allow-Origin`에 특정 도메인을 설정해야 합니다.

3. **HTTPS 사용**: 민감한 데이터를 주고받을 때는 HTTPS를 사용하여 데이터 전송을 암호화합니다.

##### 연습문제와 해답

1. **CORS 설정을 포함한 서버 응답 헤더 작성**

   ```http
   HTTP/1.1 200 OK
   Access-Control-Allow-Origin: https://example.com
   Access-Control-Allow-Methods: GET, POST
   Access-Control-Allow-Headers: Content-Type
   Access-Control-Allow-Credentials: true
   Content-Type: application/json
   ```

2. **JavaScript를 사용하여 CORS 요청 보내기**

   ```javascript
   fetch('https://api.example.com/data', {
     method: 'GET',
     headers: {
       'Content-Type': 'application/json'
     }
   })
   .then(response => response.json())
   .then(data => console.log(data))
   .catch(error => console.error('Error:', error));
   ```

3. **프리플라이트 요청을 포함한 CORS 요청 보내기**

   ```javascript
   fetch('https://api.example.com/data', {
     method: 'POST',
     headers: {
       'Content-Type': 'application/json',
       'Authorization': 'Bearer token'
     },
     body: JSON.stringify({ name: 'John Doe' })
   })
   .then(response => response.json())
   .then(data => console.log(data))
   .catch(error => console.error('Error:', error));
   ```
