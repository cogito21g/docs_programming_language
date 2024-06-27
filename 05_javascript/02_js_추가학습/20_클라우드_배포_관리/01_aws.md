### 20. 클라우드 배포 및 관리

#### 20.1. AWS와 JavaScript

##### 20.1.1. AWS Lambda와 Serverless

AWS Lambda는 서버를 관리하지 않고 코드를 실행할 수 있는 서버리스 컴퓨팅 서비스입니다. Serverless Framework는 AWS Lambda를 쉽게 관리하고 배포할 수 있도록 도와줍니다.

###### AWS Lambda 기본 설정

1. **AWS CLI 설치 및 설정**

   ```bash
   npm install -g aws-cli
   aws configure
   ```

2. **Serverless Framework 설치**

   ```bash
   npm install -g serverless
   ```

3. **새 Serverless 프로젝트 생성**

   ```bash
   serverless create --template aws-nodejs --path my-service
   cd my-service
   ```

4. **Serverless 설정 파일 업데이트**

   `serverless.yml` 파일을 열고 필요한 설정을 추가합니다.

   ```yaml
   service: my-service
   provider:
     name: aws
     runtime: nodejs14.x
     region: us-east-1

   functions:
     hello:
       handler: handler.hello
       events:
         - http:
             path: hello
             method: get
   ```

5. **Lambda 함수 작성**

   `handler.js` 파일을 열고 Lambda 함수를 작성합니다.

   ```javascript
   module.exports.hello = async (event) => {
     return {
       statusCode: 200,
       body: JSON.stringify({
         message: 'Hello, world!',
       }),
     };
   };
   ```

6. **배포**

   ```bash
   serverless deploy
   ```

7. **배포된 함수 테스트**

   ```bash
   serverless invoke -f hello
   ```

##### 20.1.2. S3와 CloudFront를 이용한 배포

AWS S3와 CloudFront를 사용하여 정적 웹사이트를 배포할 수 있습니다.

###### S3 버킷 생성 및 설정

1. **S3 버킷 생성**

   AWS Management Console에서 S3 서비스를 선택하고, 새로운 버킷을 생성합니다.

2. **버킷 정책 설정**

   버킷을 공개적으로 접근할 수 있도록 설정합니다.

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::your-bucket-name/*"
       }
     ]
   }
   ```

3. **정적 웹사이트 호스팅 설정**

   S3 버킷의 속성에서 정적 웹사이트 호스팅을 활성화하고, 인덱스 문서와 오류 문서를 설정합니다.

###### CloudFront 설정

1. **CloudFront 배포 생성**

   AWS Management Console에서 CloudFront 서비스를 선택하고, 새로운 배포를 생성합니다.

2. **기본 설정**

   - 오리진 도메인: S3 버킷의 도메인 이름
   - 기본 캐시 동작 설정: 뷰어 프로토콜 정책을 `Redirect HTTP to HTTPS`로 설정

3. **배포 완료 후 도메인 이름 확인**

   CloudFront 배포가 완료되면, CloudFront 도메인 이름을 확인하여 웹사이트에 접근할 수 있습니다.

##### 연습문제와 해답

1. **AWS Lambda와 Serverless Framework를 사용하여 간단한 Lambda 함수를 배포하세요.**

   ```bash
   npm install -g aws-cli
   aws configure
   npm install -g serverless
   serverless create --template aws-nodejs --path my-service
   cd my-service
   ```

   ```yaml
   # serverless.yml
   service: my-service
   provider:
     name: aws
     runtime: nodejs14.x
     region: us-east-1

   functions:
     hello:
       handler: handler.hello
       events:
         - http:
             path: hello
             method: get
   ```

   ```javascript
   // handler.js
   module.exports.hello = async (event) => {
     return {
       statusCode: 200,
       body: JSON.stringify({
         message: 'Hello, world!',
       }),
     };
   };
   ```

   ```bash
   serverless deploy
   serverless invoke -f hello
   ```

2. **S3와 CloudFront를 사용하여 정적 웹사이트를 배포하세요.**

   - S3 버킷 생성 및 공개 설정
   - 정적 웹사이트 호스팅 활성화
   - CloudFront 배포 생성 및 도메인 설정
