### 14. 보안

#### 14.2. CSRF (Cross-Site Request Forgery) 방지

CSRF(Cross-Site Request Forgery)는 사용자가 신뢰하는 웹 애플리케이션을 공격자가 조작하여 사용자 의지와는 상관없이 요청을 보내도록 하는 공격입니다. 이를 방지하기 위한 다양한 기법을 소개합니다.

##### CSRF 방지 기법

###### 1. CSRF 토큰 사용

CSRF 토큰은 폼이나 요청에 포함되어야 하는 고유한 값으로, 서버는 이 토큰을 검증하여 요청의 정당성을 확인합니다.

1. **서버에서 CSRF 토큰 생성**

   서버는 각 세션에 대해 고유한 CSRF 토큰을 생성하여 클라이언트에 전달합니다.

   ```python
   # Python Flask 예제
   from flask import Flask, session, request, render_template_string
   import os

   app = Flask(__name__)
   app.secret_key = os.urandom(24)

   @app.route('/')
   def index():
       session['csrf_token'] = os.urandom(24).hex()
       return render_template_string('''
           <form method="POST" action="/submit">
               <input type="hidden" name="csrf_token" value="{{ session.csrf_token }}">
               <input type="text" name="data">
               <input type="submit" value="Submit">
           </form>
       ''')

   @app.route('/submit', methods=['POST'])
   def submit():
       if request.form['csrf_token'] != session['csrf_token']:
           return "CSRF token mismatch", 400
       return "Data submitted successfully"
   ```

2. **클라이언트에서 CSRF 토큰 포함**

   클라이언트는 폼이나 AJAX 요청에 CSRF 토큰을 포함하여 서버로 전송합니다.

   ```html
   <form method="POST" action="/submit">
       <input type="hidden" name="csrf_token" value="{{ csrf_token }}">
       <input type="text" name="data">
       <input type="submit" value="Submit">
   </form>
   ```

###### 2. SameSite 쿠키 속성

SameSite 쿠키 속성을 사용하여 CSRF 공격을 방지할 수 있습니다. SameSite 속성은 쿠키가 어떤 컨텍스트에서 전송될 수 있는지를 지정합니다.

- **Strict**: 쿠키가 동일 사이트 요청에서만 전송됩니다.
- **Lax**: 쿠키가 동일 사이트 요청 및 일부 크로스 사이트 요청(GET 요청)에서 전송됩니다.
- **None**: 모든 요청에서 쿠키가 전송됩니다.

```http
Set-Cookie: sessionId=abc123; SameSite=Strict
```

###### 3. CORS 설정

CORS(Cross-Origin Resource Sharing) 정책을 적절히 설정하여 허용된 도메인에서만 요청을 수락하도록 합니다.

1. **CORS 헤더 설정**

   서버에서 특정 도메인만 허용하도록 CORS 헤더를 설정합니다.

   ```python
   from flask import Flask, request

   app = Flask(__name__)

   @app.after_request
   def apply_cors(response):
       response.headers['Access-Control-Allow-Origin'] = 'https://trusted-domain.com'
       response.headers['Access-Control-Allow-Methods'] = 'GET, POST, PUT, DELETE, OPTIONS'
       response.headers['Access-Control-Allow-Headers'] = 'Content-Type, Authorization'
       return response
   ```

###### 4. HTTP Referrer 헤더 검증

서버에서 요청의 HTTP Referrer 헤더를 검증하여 요청이 신뢰할 수 있는 출처에서 온 것인지 확인합니다.

1. **HTTP Referrer 헤더 검증**

   서버에서 Referrer 헤더를 확인하여 신뢰할 수 있는 도메인에서 온 요청인지 검증합니다.

   ```python
   @app.route('/submit', methods=['POST'])
   def submit():
       referrer = request.headers.get('Referer')
       if referrer and not referrer.startswith('https://trusted-domain.com'):
           return "Invalid referrer", 400
       return "Data submitted successfully"
   ```

##### 연습문제와 해답

1. **CSRF 토큰을 사용하여 폼 데이터를 보호하세요.**

   ```python
   from flask import Flask, session, request, render_template_string
   import os

   app = Flask(__name__)
   app.secret_key = os.urandom(24)

   @app.route('/')
   def index():
       session['csrf_token'] = os.urandom(24).hex()
       return render_template_string('''
           <form method="POST" action="/submit">
               <input type="hidden" name="csrf_token" value="{{ session.csrf_token }}">
               <input type="text" name="data">
               <input type="submit" value="Submit">
           </form>
       ''')

   @app.route('/submit', methods=['POST'])
   def submit():
       if request.form['csrf_token'] != session['csrf_token']:
           return "CSRF token mismatch", 400
       return "Data submitted successfully"
   ```

2. **SameSite 쿠키 속성을 사용하여 CSRF 공격을 방지하세요.**

   ```http
   Set-Cookie: sessionId=abc123; SameSite=Strict
   ```

3. **CORS 설정을 통해 허용된 도메인에서만 요청을 수락하세요.**

   ```python
   from flask import Flask, request

   app = Flask(__name__)

   @app.after_request
   def apply_cors(response):
       response.headers['Access-Control-Allow-Origin'] = 'https://trusted-domain.com'
       response.headers['Access-Control-Allow-Methods'] = 'GET, POST, PUT, DELETE, OPTIONS'
       response.headers['Access-Control-Allow-Headers'] = 'Content-Type, Authorization'
       return response
   ```

4. **HTTP Referrer 헤더를 검증하여 신뢰할 수 있는 도메인에서 온 요청인지 확인하세요.**

   ```python
   @app.route('/submit', methods=['POST'])
   def submit():
       referrer = request.headers.get('Referer')
       if referrer and not referrer.startswith('https://trusted-domain.com'):
           return "Invalid referrer", 400
       return "Data submitted successfully"
   ```
