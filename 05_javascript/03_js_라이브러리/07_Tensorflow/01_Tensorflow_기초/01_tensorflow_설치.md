### 7. TensorFlow.js

#### 7.1. TensorFlow.js 기초

##### 7.1.1. TensorFlow.js 설치와 설정

TensorFlow.js는 브라우저에서 머신러닝 모델을 훈련하고 실행할 수 있는 라이브러리입니다. 여기서는 TensorFlow.js를 설치하고 기본 설정하는 방법을 설명하겠습니다.

###### TensorFlow.js 설치

1. **CDN을 통한 설치**

   TensorFlow.js를 사용하는 가장 간단한 방법은 CDN을 통해 라이브러리를 포함하는 것입니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TensorFlow.js Basic Setup</title>
     <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
   </head>
   <body>
     <script>
       // TensorFlow.js 기본 코드
     </script>
   </body>
   </html>
   ```

2. **NPM을 통한 설치**

   NPM을 사용하여 프로젝트에 TensorFlow.js를 설치할 수 있습니다.

   ```bash
   npm install @tensorflow/tfjs
   ```

   설치 후, JavaScript 파일에서 TensorFlow.js를 import하여 사용할 수 있습니다.

   ```javascript
   import * as tf from '@tensorflow/tfjs';
   ```

###### TensorFlow.js 기본 설정

1. **Tensor 생성**

   TensorFlow.js에서 Tensor는 기본 데이터 구조입니다. Tensor를 생성하는 방법은 다음과 같습니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TensorFlow.js Tensor Example</title>
     <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
   </head>
   <body>
     <script>
       // Tensor 생성
       const tensor1 = tf.tensor([1, 2, 3, 4]);
       const tensor2 = tf.tensor([[1, 2], [3, 4]]);
       
       // Tensor 출력
       tensor1.print();
       tensor2.print();
     </script>
   </body>
   </html>
   ```

2. **Tensor 연산**

   TensorFlow.js에서는 다양한 연산을 수행할 수 있습니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TensorFlow.js Tensor Operations Example</title>
     <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
   </head>
   <body>
     <script>
       // Tensor 생성
       const tensor1 = tf.tensor([1, 2, 3, 4]);
       const tensor2 = tf.tensor([5, 6, 7, 8]);

       // Tensor 덧셈
       const sum = tensor1.add(tensor2);

       // Tensor 출력
       sum.print();
     </script>
   </body>
   </html>
   ```

###### 모델 생성 및 훈련

1. **기본 모델 생성**

   간단한 신경망 모델을 생성하는 방법입니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TensorFlow.js Model Example</title>
     <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
   </head>
   <body>
     <script>
       // 모델 생성
       const model = tf.sequential();
       model.add(tf.layers.dense({ units: 10, activation: 'relu', inputShape: [4] }));
       model.add(tf.layers.dense({ units: 3, activation: 'softmax' }));

       // 모델 컴파일
       model.compile({
         optimizer: 'adam',
         loss: 'categoricalCrossentropy',
         metrics: ['accuracy']
       });

       // 데이터 생성 (예시)
       const xs = tf.tensor2d([[0.1, 0.2, 0.3, 0.4], [0.5, 0.6, 0.7, 0.8]]);
       const ys = tf.tensor2d([[1, 0, 0], [0, 1, 0]]);

       // 모델 훈련
       model.fit(xs, ys, {
         epochs: 10
       }).then(() => {
         console.log('모델 훈련 완료');
       });
     </script>
   </body>
   </html>
   ```

2. **모델 예측**

   훈련된 모델을 사용하여 예측하는 방법입니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TensorFlow.js Model Prediction Example</title>
     <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
   </head>
   <body>
     <script>
       // 모델 생성
       const model = tf.sequential();
       model.add(tf.layers.dense({ units: 10, activation: 'relu', inputShape: [4] }));
       model.add(tf.layers.dense({ units: 3, activation: 'softmax' }));

       // 모델 컴파일
       model.compile({
         optimizer: 'adam',
         loss: 'categoricalCrossentropy',
         metrics: ['accuracy']
       });

       // 데이터 생성 (예시)
       const xs = tf.tensor2d([[0.1, 0.2, 0.3, 0.4], [0.5, 0.6, 0.7, 0.8]]);
       const ys = tf.tensor2d([[1, 0, 0], [0, 1, 0]]);

       // 모델 훈련
       model.fit(xs, ys, {
         epochs: 10
       }).then(() => {
         console.log('모델 훈련 완료');

         // 예측 데이터 생성
         const input = tf.tensor2d([[0.9, 1.0, 1.1, 1.2]]);
         
         // 모델 예측
         const output = model.predict(input);
         output.print();
       });
     </script>
   </body>
   </html>
   ```

##### 연습문제와 해답

1. **TensorFlow.js를 사용하여 1차원 텐서를 생성하고 출력하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TensorFlow.js 1D Tensor Example</title>
     <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
   </head>
   <body>
     <script>
       // 1차원 텐서 생성
       const tensor = tf.tensor([1, 2, 3, 4, 5]);

       // 텐서 출력
       tensor.print();
     </script>
   </body>
   </html>
   ```

2. **TensorFlow.js를 사용하여 두 텐서를 더하고 결과를 출력하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>TensorFlow.js Tensor Addition Example</title>
     <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
   </head>
   <body>
     <script>
       // 텐서 생성
       const tensor1 = tf.tensor([1, 2, 3, 4]);
       const tensor2 = tf.tensor([5, 6, 7, 8]);

       // 텐서 덧셈
       const sum = tensor1.add(tensor2);

       // 결과 출력
       sum.print();
     </script>
   </body>
   </html>
   ```
