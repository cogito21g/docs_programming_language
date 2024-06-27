### 6. Three.js

#### 6.1. Three.js 기초

##### 6.1.1. Three.js 설치와 설정

Three.js는 웹 브라우저에서 3D 그래픽을 쉽게 렌더링할 수 있게 해주는 JavaScript 라이브러리입니다. 여기서는 Three.js를 설치하고 기본 설정을 하는 방법을 설명합니다.

###### Three.js 설치

1. **CDN을 통한 설치**

   Three.js를 사용하는 가장 간단한 방법은 CDN을 통해 라이브러리를 포함하는 것입니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js Basic Setup</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
   </head>
   <body>
     <script>
       // Three.js 기본 설정 코드
     </script>
   </body>
   </html>
   ```

2. **NPM을 통한 설치**

   NPM을 사용하여 프로젝트에 Three.js를 설치할 수 있습니다.

   ```bash
   npm install three
   ```

   설치 후, JavaScript 파일에서 Three.js를 import하여 사용할 수 있습니다.

   ```javascript
   import * as THREE from 'three';
   ```

###### Three.js 기본 설정

1. **기본 장면(Scene) 설정**

   Three.js로 3D 장면을 설정하려면 씬(Scene), 카메라(Camera), 렌더러(Renderer)를 설정해야 합니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js Basic Setup</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
   </head>
   <body>
     <script>
       // 씬(Scene) 생성
       const scene = new THREE.Scene();

       // 카메라(Camera) 생성
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       camera.position.z = 5;

       // 렌더러(Renderer) 생성
       const renderer = new THREE.WebGLRenderer();
       renderer.setSize(window.innerWidth, window.innerHeight);
       document.body.appendChild(renderer.domElement);

       // 렌더링 함수
       function animate() {
         requestAnimationFrame(animate);
         renderer.render(scene, camera);
       }
       animate();
     </script>
   </body>
   </html>
   ```

2. **기본 도형 추가**

   기본적인 도형(큐브)을 씬에 추가하는 방법입니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js Basic Setup</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer();
       renderer.setSize(window.innerWidth, window.innerHeight);
       document.body.appendChild(renderer.domElement);

       // 큐브 생성
       const geometry = new THREE.BoxGeometry();
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       camera.position.z = 5;

       function animate() {
         requestAnimationFrame(animate);

         // 큐브 회전
         cube.rotation.x += 0.01;
         cube.rotation.y += 0.01;

         renderer.render(scene, camera);
       }
       animate();
     </script>
   </body>
   </html>
   ```

##### 연습문제와 해답

1. **Three.js를 사용하여 기본 씬, 카메라, 렌더러를 설정하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js Basic Setup</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer();
       renderer.setSize(window.innerWidth, window.innerHeight);
       document.body.appendChild(renderer.domElement);

       camera.position.z = 5;

       function animate() {
         requestAnimationFrame(animate);
         renderer.render(scene, camera);
       }
       animate();
     </script>
   </body>
   </html>
   ```

2. **Three.js를 사용하여 씬에 회전하는 큐브를 추가하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js Basic Setup</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer();
       renderer.setSize(window.innerWidth, window.innerHeight);
       document.body.appendChild(renderer.domElement);

       const geometry = new THREE.BoxGeometry();
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       camera.position.z = 5;

       function animate() {
         requestAnimationFrame(animate);
         cube.rotation.x += 0.01;
         cube.rotation.y += 0.01;
         renderer.render(scene, camera);
       }
       animate();
     </script>
   </body>
   </html>
   ```
