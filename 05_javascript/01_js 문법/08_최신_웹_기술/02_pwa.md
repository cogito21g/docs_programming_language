### 8. 최신 웹 기술

#### 8.2. Progressive Web Apps (PWA)

Progressive Web Apps (PWA)는 웹과 네이티브 애플리케이션의 장점을 결합한 애플리케이션입니다. PWA는 네트워크 상태에 상관없이 작동하며, 푸시 알림을 보내고, 홈 화면에 추가할 수 있습니다. PWA는 성능, 접근성, 사용자 경험을 향상시킵니다.

##### PWA의 주요 기능

1. **오프라인 지원**: 네트워크가 없을 때도 작동.
2. **푸시 알림**: 사용자에게 실시간 알림을 전송.
3. **홈 화면 추가**: 웹 앱을 설치하여 네이티브 앱처럼 사용.

##### PWA 구성 요소

1. **서비스 워커 (Service Worker)**
2. **웹 앱 매니페스트 (Web App Manifest)**
3. **HTTPS**

##### 1. 서비스 워커

서비스 워커는 백그라운드에서 실행되는 스크립트로, 네트워크 요청을 가로채고, 캐싱을 제어하며, 오프라인 기능을 제공합니다.

###### 서비스 워커 등록

```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then(registration => {
      console.log('ServiceWorker registration successful with scope: ', registration.scope);
    })
    .catch(error => {
      console.log('ServiceWorker registration failed: ', error);
    });
}
```

###### 서비스 워커 파일 (service-worker.js)

```javascript
const CACHE_NAME = 'my-cache-v1';
const urlsToCache = [
  '/',
  '/styles.css',
  '/script.js',
  '/index.html'
];

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => {
        return cache.addAll(urlsToCache);
      })
  );
});

self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(response => {
        if (response) {
          return response;
        }
        return fetch(event.request);
      })
  );
});
```

##### 2. 웹 앱 매니페스트

웹 앱 매니페스트는 PWA의 메타데이터를 포함하는 JSON 파일입니다. 이는 애플리케이션이 설치될 때 어떻게 보일지 정의합니다.

###### manifest.json

```json
{
  "name": "My Progressive Web App",
  "short_name": "MyPWA",
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    {
      "src": "/images/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/images/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

###### HTML에 매니페스트 포함

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="manifest" href="/manifest.json">
  <title>My PWA</title>
</head>
<body>
  <h1>Welcome to My PWA</h1>
</body>
</html>
```

##### 3. HTTPS

PWA는 HTTPS를 통해 제공되어야 합니다. 이는 보안 및 데이터 무결성을 보장하기 위해 필요합니다. HTTPS를 설정하는 방법은 웹 서버마다 다릅니다. 예를 들어, Nginx 설정은 다음과 같습니다.

###### Nginx 설정 예제

```nginx
server {
  listen 80;
  server_name example.com;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl;
  server_name example.com;

  ssl_certificate /path/to/ssl/certificate.crt;
  ssl_certificate_key /path/to/ssl/certificate.key;

  location / {
    root /path/to/your/app;
    index index.html;
  }
}
```

#### 예제

1. **서비스 워커를 등록하고, 기본적인 캐싱을 설정하세요.**

   ```javascript
   // main.js
   if ('serviceWorker' in navigator) {
     navigator.serviceWorker.register('/service-worker.js')
       .then(registration => {
         console.log('ServiceWorker registration successful with scope: ', registration.scope);
       })
       .catch(error => {
         console.log('ServiceWorker registration failed: ', error);
       });
   }
   ```

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'my-cache-v1';
   const urlsToCache = [
     '/',
     '/styles.css',
     '/script.js',
     '/index.html'
   ];

   self.addEventListener('install', event => {
     event.waitUntil(
       caches.open(CACHE_NAME)
         .then(cache => {
           return cache.addAll(urlsToCache);
         })
     );
   });

   self.addEventListener('fetch', event => {
     event.respondWith(
       caches.match(event.request)
         .then(response => {
           if (response) {
             return response;
           }
           return fetch(event.request);
         })
     );
   });
   ```

2. **웹 앱 매니페스트를 작성하고 HTML 파일에 포함하세요.**

   ```json
   // manifest.json
   {
     "name": "Sample PWA",
     "short_name": "SamplePWA",
     "start_url": "/index.html",
     "display": "standalone",
     "background_color": "#ffffff",
     "theme_color": "#0000ff",
     "icons": [
       {
         "src": "/images/icon-192x192.png",
         "sizes": "192x192",
         "type": "image/png"
       },
       {
         "src": "/images/icon-512x512.png",
         "sizes": "512x512",
         "type": "image/png"
       }
     ]
   }
   ```

   ```html
   <!-- index.html -->
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <link rel="manifest" href="/manifest.json">
     <title>Sample PWA</title>
   </head>
   <body>
     <h1>Welcome to Sample PWA</h1>
   </body>
   </html>
   ```

3. **PWA가 오프라인 상태에서도 동작하도록 설정하세요.**

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'my-cache-v1';
   const urlsToCache = [
     '/',
     '/styles.css',
     '/script.js',
     '/index.html'
   ];

   self.addEventListener('install', event => {
     event.waitUntil(
       caches.open(CACHE_NAME)
         .then(cache => {
           return cache.addAll(urlsToCache);
         })
     );
   });

   self.addEventListener('fetch', event => {
     event.respondWith(
       caches.match(event.request)
         .then(response => {
           if (response) {
             return response;
           }
           return fetch(event.request).then(
             response => {
               if (!response || response.status !== 200 || response.type !== 'basic') {
                 return response;
               }

               const responseToCache = response.clone();

               caches.open(CACHE_NAME)
                 .then(cache => {
                   cache.put(event.request, responseToCache);
                 });

               return response;
             }
           );
         })
     );
   });
   ```

#### 연습문제와 해답

1. **서비스 워커를 사용하여 네트워크 요청을 캐싱하고 오프라인 상태에서도 작동하도록 만드세요.**

   ```javascript
   // main.js
   if ('serviceWorker' in navigator) {
     navigator.serviceWorker.register('/service-worker.js')
       .then(registration => {
         console.log('ServiceWorker registration successful with scope: ', registration.scope);
       })
       .catch(error => {
         console.log('ServiceWorker registration failed: ', error);
       });
   }
   ```

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'my-cache-v1';
   const urlsToCache = [
     '/',
     '/styles.css',
     '/script.js',
     '/index.html'
   ];

   self.addEventListener('install', event => {
     event.waitUntil(
       caches.open(CACHE_NAME)
         .then(cache => {
           return cache.addAll(urlsToCache);
         })
     );
   });

   self.addEventListener('fetch', event => {
     event.respondWith(
       caches.match(event.request)
         .then(response => {
           if (response) {
             return response;
           }
           return fetch(event.request).then(
             response => {
               if (!response || response.status !== 200 || response.type !== 'basic') {
                 return response;
               }

               const responseToCache = response.clone();

               caches.open(CACHE_NAME)
                 .then(cache => {
                   cache.put(event.request, responseToCache);
                 });

               return response;
             }
           );
         })
     );
   });
   ```

2. **웹 앱 매니페스트를 작성하고 이를 HTML 파일에 포함하여 PWA로 설정하세요.**

   ```json
  

 // manifest.json
   {
     "name": "My PWA",
     "short_name": "PWA",
     "start_url": "/index.html",
     "display": "standalone",
     "background_color": "#ffffff",
     "theme_color": "#007bff",
     "icons": [
       {
         "src": "/images/icon-192x192.png",
         "sizes": "192x192",
         "type": "image/png"
       },
       {
         "src": "/images/icon-512x512.png",
         "sizes": "512x512",
         "type": "image/png"
       }
     ]
   }
   ```

   ```html
   <!-- index.html -->
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <link rel="manifest" href="/manifest.json">
     <title>My PWA</title>
   </head>
   <body>
     <h1>Welcome to My PWA</h1>
   </body>
   </html>
   ```

3. **서비스 워커와 웹 앱 매니페스트를 설정하여 PWA를 완성하고, 이를 테스트하세요.**

   ```javascript
   // main.js
   if ('serviceWorker' in navigator) {
     navigator.serviceWorker.register('/service-worker.js')
       .then(registration => {
         console.log('ServiceWorker registration successful with scope: ', registration.scope);
       })
       .catch(error => {
         console.log('ServiceWorker registration failed: ', error);
       });
   }
   ```

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'my-cache-v1';
   const urlsToCache = [
     '/',
     '/styles.css',
     '/script.js',
     '/index.html'
   ];

   self.addEventListener('install', event => {
     event.waitUntil(
       caches.open(CACHE_NAME)
         .then(cache => {
           return cache.addAll(urlsToCache);
         })
     );
   });

   self.addEventListener('fetch', event => {
     event.respondWith(
       caches.match(event.request)
         .then(response => {
           if (response) {
             return response;
           }
           return fetch(event.request).then(
             response => {
               if (!response || response.status !== 200 || response.type !== 'basic') {
                 return response;
               }

               const responseToCache = response.clone();

               caches.open(CACHE_NAME)
                 .then(cache => {
                   cache.put(event.request, responseToCache);
                 });

               return response;
             }
           );
         })
     );
   });
   ```

   ```json
   // manifest.json
   {
     "name": "Complete PWA",
     "short_name": "PWA",
     "start_url": "/index.html",
     "display": "standalone",
     "background_color": "#ffffff",
     "theme_color": "#007bff",
     "icons": [
       {
         "src": "/images/icon-192x192.png",
         "sizes": "192x192",
         "type": "image/png"
       },
       {
         "src": "/images/icon-512x512.png",
         "sizes": "512x512",
         "type": "image/png"
       }
     ]
   }
   ```

   ```html
   <!-- index.html -->
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <link rel="manifest" href="/manifest.json">
     <title>Complete PWA</title>
   </head>
   <body>
     <h1>Welcome to Complete PWA</h1>
   </body>
   </html>
   ```
