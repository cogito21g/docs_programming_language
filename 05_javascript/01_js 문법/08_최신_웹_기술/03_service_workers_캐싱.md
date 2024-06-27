### 8. 최신 웹 기술

#### 8.3. Service Workers와 캐싱 전략

서비스 워커(Service Workers)는 웹 애플리케이션의 백그라운드에서 실행되는 스크립트로, 네트워크 요청을 가로채고, 캐싱을 관리하며, 오프라인 기능을 제공합니다. 적절한 캐싱 전략을 사용하면 웹 애플리케이션의 성능과 사용자 경험을 크게 향상시킬 수 있습니다.

##### 서비스 워커 설정

서비스 워커를 설정하려면 서비스 워커 파일을 작성하고, 이를 애플리케이션에 등록해야 합니다.

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

기본적인 서비스 워커 파일은 설치, 활성화, fetch 이벤트를 처리합니다.

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

##### 캐싱 전략

1. **Cache First**

   캐시된 자원을 우선적으로 사용하고, 캐시에 없으면 네트워크에서 자원을 가져옵니다. 이를 통해 빠른 응답을 제공합니다.

   ```javascript
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

2. **Network First**

   네트워크에서 자원을 우선적으로 가져오고, 네트워크에 실패하면 캐시된 자원을 사용합니다. 이를 통해 최신 데이터를 제공합니다.

   ```javascript
   self.addEventListener('fetch', event => {
     event.respondWith(
       fetch(event.request)
         .then(response => {
           if (!response || response.status !== 200 || response.type !== 'basic') {
             return response;
           }
           const responseToCache = response.clone();
           caches.open(CACHE_NAME)
             .then(cache => {
               cache.put(event.request, responseToCache);
             });
           return response;
         })
         .catch(() => caches.match(event.request))
     );
   });
   ```

3. **Cache Only**

   오직 캐시된 자원만을 사용합니다. 이는 오프라인 전용 애플리케이션에 적합합니다.

   ```javascript
   self.addEventListener('fetch', event => {
     event.respondWith(
       caches.match(event.request)
     );
   });
   ```

4. **Network Only**

   오직 네트워크에서 자원을 가져옵니다. 이는 실시간 데이터가 필요한 애플리케이션에 적합합니다.

   ```javascript
   self.addEventListener('fetch', event => {
     event.respondWith(
       fetch(event.request)
     );
   });
   ```

5. **Stale-While-Revalidate**

   캐시된 자원을 즉시 반환하고, 네트워크에서 자원을 가져와 캐시를 업데이트합니다. 이를 통해 빠른 응답과 최신 데이터를 동시에 제공합니다.

   ```javascript
   self.addEventListener('fetch', event => {
     event.respondWith(
       caches.match(event.request)
         .then(response => {
           const fetchPromise = fetch(event.request).then(networkResponse => {
             caches.open(CACHE_NAME).then(cache => {
               cache.put(event.request, networkResponse.clone());
             });
             return networkResponse;
           });
           return response || fetchPromise;
         })
     );
   });
   ```

#### 예제

1. **Cache First 전략을 사용하여 정적 자원을 캐싱하세요.**

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'static-cache-v1';
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

2. **Network First 전략을 사용하여 API 데이터를 캐싱하세요.**

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'api-cache-v1';

   self.addEventListener('install', event => {
     event.waitUntil(
       caches.open(CACHE_NAME)
     );
   });

   self.addEventListener('fetch', event => {
     if (event.request.url.includes('/api/')) {
       event.respondWith(
         fetch(event.request)
           .then(response => {
             if (!response || response.status !== 200 || response.type !== 'basic') {
               return response;
             }
             const responseToCache = response.clone();
             caches.open(CACHE_NAME)
               .then(cache => {
                 cache.put(event.request, responseToCache);
               });
             return response;
           })
           .catch(() => caches.match(event.request))
       );
     }
   });
   ```

3. **Stale-While-Revalidate 전략을 사용하여 정적 자원을 캐싱하세요.**

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'sw-cache-v1';
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
           const fetchPromise = fetch(event.request).then(networkResponse => {
             caches.open(CACHE_NAME).then(cache => {
               cache.put(event.request, networkResponse.clone());
             });
             return networkResponse;
           });
           return response || fetchPromise;
         })
     );
   });
   ```

#### 연습문제와 해답

1. **Cache First 전략을 사용하여 이미지 파일을 캐싱하세요.**

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'image-cache-v1';
   const urlsToCache = [
     '/images/logo.png',
     '/images/banner.jpg'
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
     if (event.request.url.includes('/images/')) {
       event.respondWith(
         caches.match(event.request)
           .then(response => {
             if (response) {
               return response;
             }
             return fetch(event.request);
           })
       );
     }
   });
   ```

2. **Network First 전략을 사용하여 사용자 데이터를 캐싱하세요.**

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'user-data-cache-v1';

   self.addEventListener('install', event => {
     event.waitUntil(
       caches.open(CACHE_NAME)
     );
   });

   self.addEventListener('fetch', event => {
     if (event.request.url.includes('/users/')) {
       event.respondWith(
         fetch(event.request)
           .then(response => {
             if (!response || response.status !== 200 || response.type !== 'basic') {
               return response;
             }
             const responseToCache = response.clone();
             caches.open(CACHE_NAME)
               .then(cache => {
                 cache.put(event.request, responseToCache);
               });
             return response;
           })
           .catch(() => caches.match(event.request))
       );
     }
   });
   ```

3. **Cache Only 전략을 사용하여 오프라인 상태에서 정적 파일을 제공하세요.**

   ```javascript
   // service-worker.js
   const CACHE_NAME = 'offline-cache-v1';
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
     );
   });
   ```
