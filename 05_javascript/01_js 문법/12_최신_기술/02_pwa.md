### 12. 최신 웹 기술

#### 12.2. Progressive Web Apps (PWA)

Progressive Web Apps (PWA)는 웹 애플리케이션이 네이티브 애플리케이션과 같은 사용자 경험을 제공할 수 있도록 설계된 기술입니다. PWA는 웹 기술을 사용하여 오프라인에서 작동하고, 푸시 알림을 지원하며, 홈 화면에 설치할 수 있는 등의 기능을 제공합니다. PWA의 핵심 구성 요소는 Service Workers와 Web App Manifest입니다.

##### 12.2.1. Service Workers

Service Worker는 백그라운드에서 실행되는 스크립트로, 네트워크 요청을 가로채고 캐시를 관리하는 등의 작업을 수행합니다.

###### Service Worker 등록

Service Worker를 등록하려면 `navigator.serviceWorker.register` 메서드를 사용합니다.

```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js')
      .then((registration) => {
        console.log('ServiceWorker registration successful with scope: ', registration.scope);
      }, (error) => {
        console.log('ServiceWorker registration failed: ', error);
      });
  });
}
```

###### Service Worker 설치 및 활성화

Service Worker 스크립트에서 `install` 및 `activate` 이벤트를 처리합니다.

```javascript
self.addEventListener('install', (event) => {
  console.log('Service Worker installing.');
  // 캐시 프리로드 등의 작업 수행
});

self.addEventListener('activate', (event) => {
  console.log('Service Worker activating.');
  // 이전 캐시 정리 등의 작업 수행
});
```

##### 12.2.2. Web App Manifest

Web App Manifest는 웹 애플리케이션의 메타데이터를 포함하는 JSON 파일로, 애플리케이션의 이름, 아이콘, 시작 URL 등을 정의합니다.

###### manifest.json 예제

```json
{
  "name": "My Progressive Web App",
  "short_name": "MyPWA",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    {
      "src": "icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

###### HTML에서 Web App Manifest 참조

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Progressive Web App</title>
  <link rel="manifest" href="/manifest.json">
</head>
<body>
  <h1>Welcome to My Progressive Web App</h1>
  <script src="/app.js"></script>
</body>
</html>
```

##### 12.2.3. Caching Strategies

Service Worker는 다양한 캐싱 전략을 사용하여 애플리케이션의 성능을 최적화할 수 있습니다. 몇 가지 주요 캐싱 전략을 살펴보겠습니다.

###### Cache First

Cache First 전략은 네트워크 요청을 캐시에서 먼저 검색하고, 캐시에서 데이터를 찾지 못하면 네트워크에서 요청합니다.

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((response) => {
        return response || fetch(event.request);
      })
  );
});
```

###### Network First

Network First 전략은 네트워크에서 데이터를 먼저 요청하고, 네트워크 응답이 실패하면 캐시에서 데이터를 검색합니다.

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
      .then((response) => {
        return caches.open('my-cache').then((cache) => {
          cache.put(event.request, response.clone());
          return response;
        });
      })
      .catch(() => {
        return caches.match(event.request);
      })
  );
});
```

###### Cache Only

Cache Only 전략은 오직 캐시에서만 데이터를 검색합니다.

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
  );
});
```

###### Network Only

Network Only 전략은 오직 네트워크에서만 데이터를 요청합니다.

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    fetch(event.request)
  );
});
```

##### 연습문제와 해답

1. **Service Worker를 등록하고 설치 및 활성화 이벤트를 처리하세요.**

   ```javascript
   if ('serviceWorker' in navigator) {
     window.addEventListener('load', () => {
       navigator.serviceWorker.register('/service-worker.js')
         .then((registration) => {
           console.log('ServiceWorker registration successful with scope: ', registration.scope);
         }, (error) => {
           console.log('ServiceWorker registration failed: ', error);
         });
     });
   }

   // service-worker.js
   self.addEventListener('install', (event) => {
     console.log('Service Worker installing.');
   });

   self.addEventListener('activate', (event) => {
     console.log('Service Worker activating.');
   });
   ```

2. **Web App Manifest를 작성하고 HTML 파일에 참조하세요.**

   ```json
   // manifest.json
   {
     "name": "My Progressive Web App",
     "short_name": "MyPWA",
     "start_url": "/",
     "display": "standalone",
     "background_color": "#ffffff",
     "theme_color": "#000000",
     "icons": [
       {
         "src": "icon-192x192.png",
         "sizes": "192x192",
         "type": "image/png"
       },
       {
         "src": "icon-512x512.png",
         "sizes": "512x512",
         "type": "image/png"
       }
     ]
   }
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>My Progressive Web App</title>
     <link rel="manifest" href="/manifest.json">
   </head>
   <body>
     <h1>Welcome to My Progressive Web App</h1>
     <script src="/app.js"></script>
   </body>
   </html>
   ```

3. **Cache First 전략을 사용하여 네트워크 요청을 처리하세요.**

   ```javascript
   self.addEventListener('fetch', (event) => {
     event.respondWith(
       caches.match(event.request)
         .then((response) => {
           return response || fetch(event.request);
         })
     );
   });
   ```
