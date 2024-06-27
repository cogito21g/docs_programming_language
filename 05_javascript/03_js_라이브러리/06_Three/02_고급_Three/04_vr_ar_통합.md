### 6. Three.js

#### 6.2. 고급 Three.js

##### 6.2.4. VR과 AR 통합

Three.js를 사용하여 가상 현실(VR)과 증강 현실(AR)을 통합할 수 있습니다. WebXR API를 사용하여 VR과 AR 환경을 설정하고 Three.js와 통합하여 3D 콘텐츠를 렌더링하는 방법을 설명하겠습니다.

###### VR 설정

1. **VR 설정**

   WebXR API를 사용하여 VR 환경을 설정합니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js VR Example</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/VRButton.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer({ antialias: true });
       renderer.setSize(window.innerWidth, window.innerHeight);
       document.body.appendChild(renderer.domElement);

       document.body.appendChild(VRButton.createButton(renderer));
       renderer.xr.enabled = true;

       const geometry = new THREE.BoxGeometry();
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       camera.position.z = 5;

       function animate() {
         renderer.setAnimationLoop(() => {
           cube.rotation.x += 0.01;
           cube.rotation.y += 0.01;
           renderer.render(scene, camera);
         });
       }
       animate();
     </script>
   </body>
   </html>
   ```

###### AR 설정

2. **AR 설정**

   WebXR API를 사용하여 AR 환경을 설정합니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js AR Example</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/ARButton.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
       renderer.setSize(window.innerWidth, window.innerHeight);
       renderer.xr.enabled = true;
       document.body.appendChild(renderer.domElement);

       document.body.appendChild(ARButton.createButton(renderer));

       const geometry = new THREE.BoxGeometry(0.1, 0.1, 0.1);
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       function animate() {
         renderer.setAnimationLoop(() => {
           cube.rotation.x += 0.01;
           cube.rotation.y += 0.01;
           renderer.render(scene, camera);
         });
       }
       animate();
     </script>
   </body>
   </html>
   ```

###### 전체 예제

1. **VR 통합 전체 예제**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js VR Example</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/VRButton.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer({ antialias: true });
       renderer.setSize(window.innerWidth, window.innerHeight);
       document.body.appendChild(renderer.domElement);

       document.body.appendChild(VRButton.createButton(renderer));
       renderer.xr.enabled = true;

       const geometry = new THREE.BoxGeometry();
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       camera.position.z = 5;

       function animate() {
         renderer.setAnimationLoop(() => {
           cube.rotation.x += 0.01;
           cube.rotation.y += 0.01;
           renderer.render(scene, camera);
         });
       }
       animate();
     </script>
   </body>
   </html>
   ```

2. **AR 통합 전체 예제**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js AR Example</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/ARButton.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
       renderer.setSize(window.innerWidth, window.innerHeight);
       renderer.xr.enabled = true;
       document.body.appendChild(renderer.domElement);

       document.body.appendChild(ARButton.createButton(renderer));

       const geometry = new THREE.BoxGeometry(0.1, 0.1, 0.1);
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       function animate() {
         renderer.setAnimationLoop(() => {
           cube.rotation.x += 0.01;
           cube.rotation.y += 0.01;
           renderer.render(scene, camera);
         });
       }
       animate();
     </script>
   </body>
   </html>
   ```

##### 연습문제와 해답

1. **Three.js를 사용하여 VR 환경을 설정하고 씬에 회전하는 큐브를 추가하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js VR Example</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/VRButton.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer({ antialias: true });
       renderer.setSize(window.innerWidth, window.innerHeight);
       document.body.appendChild(renderer.domElement);

       document.body.appendChild(VRButton.createButton(renderer));
       renderer.xr.enabled = true;

       const geometry = new THREE.BoxGeometry();
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       camera.position.z = 5;

       function animate() {
         renderer.setAnimationLoop(() => {
           cube.rotation.x += 0.01;
           cube.rotation.y += 0.01;
           renderer.render(scene, camera);
         });
       }
       animate();
     </script>
   </body>
   </html>
   ```

2. **Three.js를 사용하여 AR 환경을 설정하고 씬에 회전하는 큐브를 추가하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Three.js AR Example</title>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
     <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/ARButton.js"></script>
   </head>
   <body>
     <script>
       const scene = new THREE.Scene();
       const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
       const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
       renderer.setSize(window.innerWidth, window.innerHeight);
       renderer.xr.enabled = true;
       document.body.appendChild(renderer.domElement);

       document.body.appendChild(ARButton.createButton(renderer));

       const geometry = new THREE.BoxGeometry(0.1, 0.1, 0.1);
       const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
       const cube = new THREE.Mesh(geometry, material);
       scene.add(cube);

       function animate() {
         renderer.setAnimationLoop(() => {
           cube.rotation.x += 0.01;
           cube.rotation.y += 0.01;
           renderer.render(scene, camera);
         });
       }
       animate();
     </script>
   </body>
   </html>
   ```
