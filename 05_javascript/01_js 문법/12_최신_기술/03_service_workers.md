### 12. 최신 웹 기술

#### 12.3. Service Workers와 캐싱 전략

Service Workers는 웹 애플리케이션이 오프라인에서도 작동할 수 있게 하고, 네트워크 요청을 가로채고 캐싱하는 등의 기능을 제공합니다. 이번 섹션에서는 Service Worker를 설정하고 다양한 캐싱 전략을 사용하는 방법을 학습하겠습니다.

##### 12.3.1. Service Worker 등록

Service Worker를 사용하려면 먼저 Service Worker를 등록해야 합니다. 이를 위해 `navigator.serviceWorker.register` 메서드를 사용합니다.

###### 예제

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

##### 12.3.2. Fetch 이벤트 처리

Service Worker는 `fetch` 이벤트를 가로채서 네트워크 요청을 처리할 수 있습니다. 이를 통해 요청을 캐시하거나, 네트워크 요청이 실패할 경우 캐시된 응답을 제공할 수 있습니다.

###### 예제

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

##### 12.3.3. 캐싱 전략

Service Worker는 다양한 캐싱 전략을 사용하여 네트워크 요청을 최적화할 수 있습니다. 몇 가지 주요 캐싱 전략을 살펴보겠습니다.

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

###### Stale-While-Revalidate

Stale-While-Revalidate 전략은 캐시된 응답을 즉시 반환하고, 네트워크 요청을 통해 캐시를 업데이트합니다.

```javascript
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      const fetchPromise = fetch(event.request).then((networkResponse) => {
        caches.open('my-cache').then((cache) => {
          cache.put(event.request, networkResponse.clone());
        });
        return networkResponse;
      });
      return response || fetchPromise;
    })
  );
});
```

##### 연습문제와 해답

1. **Service Worker를 등록하고, 네트워크 요청을 Cache First 전략으로 처리하세요.**

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
   self.addEventListener('fetch', (event) => {
     event.respondWith(
       caches.match(event.request)
         .then((response) => {
           return response || fetch(event.request);
         })
     );
   });
   ```

2. **Network First 전략을 사용하여 네트워크 요청을 처리하세요.**

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

3. **Stale-While-Revalidate 전략을 사용하여 네트워크 요청을 처리하세요.**

   ```javascript
   self.addEventListener('fetch', (event) => {
     event.respondWith(
       caches.match(event.request).then((response) => {
         const fetchPromise = fetch(event.request).then((networkResponse) => {
           caches.open('my-cache').then((cache) => {
             cache.put(event.request, networkResponse.clone());
           });
           return networkResponse;
         });
         return response || fetchPromise;
       })
     );
   });
   ```
