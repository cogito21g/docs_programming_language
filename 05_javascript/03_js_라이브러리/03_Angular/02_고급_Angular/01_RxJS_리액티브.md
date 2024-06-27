### 3. Angular

#### 3.2. 고급 Angular

##### 3.2.1. RxJS와 리액티브 프로그래밍

RxJS(Reactive Extensions for JavaScript)는 리액티브 프로그래밍을 위한 라이브러리로, Angular에서 비동기 데이터 스트림을 관리하고 처리하는 데 사용됩니다. RxJS는 Observable, Observer, Subject, Operator 등을 사용하여 복잡한 비동기 작업을 단순화합니다.

###### Observable

1. **Observable 생성**

   Observable은 데이터 스트림을 나타내며, `Observable.create` 또는 `new Observable`을 사용하여 생성할 수 있습니다.

   ```typescript
   import { Observable } from 'rxjs';

   const observable = new Observable(observer => {
     observer.next('Hello');
     observer.next('World');
     observer.complete();
   });
   ```

2. **Observable 구독**

   Observable은 `subscribe` 메서드를 사용하여 구독할 수 있습니다.

   ```typescript
   observable.subscribe({
     next(value) {
       console.log(value);
     },
     complete() {
       console.log('Complete');
     },
   });
   ```

###### Operator

1. **map**

   `map` 연산자는 Observable의 각 값을 변환합니다.

   ```typescript
   import { of } from 'rxjs';
   import { map } from 'rxjs/operators';

   const source$ = of(1, 2, 3);
   const result$ = source$.pipe(
     map(value => value * 2)
   );

   result$.subscribe(value => console.log(value));
   ```

2. **filter**

   `filter` 연산자는 Observable의 값을 필터링합니다.

   ```typescript
   import { of } from 'rxjs';
   import { filter } from 'rxjs/operators';

   const source$ = of(1, 2, 3, 4, 5);
   const result$ = source$.pipe(
     filter(value => value % 2 === 0)
   );

   result$.subscribe(value => console.log(value));
   ```

3. **combineLatest**

   `combineLatest` 연산자는 여러 Observable의 최신 값을 결합합니다.

   ```typescript
   import { of, combineLatest } from 'rxjs';

   const obs1$ = of(1, 2, 3);
   const obs2$ = of('A', 'B', 'C');
   const result$ = combineLatest([obs1$, obs2$]);

   result$.subscribe(([val1, val2]) => {
     console.log(val1, val2);
   });
   ```

###### Subject

1. **Subject 생성 및 사용**

   Subject는 Observable과 Observer를 모두 구현하는 특별한 유형의 Observable입니다.

   ```typescript
   import { Subject } from 'rxjs';

   const subject = new Subject<number>();

   subject.subscribe(value => console.log('Observer 1:', value));
   subject.subscribe(value => console.log('Observer 2:', value));

   subject.next(1);
   subject.next(2);
   ```

###### Angular에서 RxJS 사용

1. **HTTP 요청**

   Angular의 `HttpClient`는 RxJS Observable을 반환합니다.

   ```typescript
   // src/app/app.module.ts
   import { BrowserModule } from '@angular/platform-browser';
   import { NgModule } from '@angular/core';
   import { HttpClientModule } from '@angular/common/http';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       HttpClientModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```typescript
   // src/app/my-service.service.ts
   import { Injectable } from '@angular/core';
   import { HttpClient } from '@angular/common/http';
   import { Observable } from 'rxjs';

   @Injectable({
     providedIn: 'root'
   })
   export class MyService {
     private apiUrl = 'https://jsonplaceholder.typicode.com/posts';

     constructor(private http: HttpClient) { }

     getPosts(): Observable<any[]> {
       return this.http.get<any[]>(this.apiUrl);
     }
   }
   ```

   ```typescript
   // src/app/my-component/my-component.component.ts
   import { Component, OnInit } from '@angular/core';
   import { MyService } from '../my-service.service';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {
     posts: any[] = [];

     constructor(private myService: MyService) { }

     ngOnInit(): void {
       this.myService.getPosts().subscribe(data => {
         this.posts = data;
       });
     }
   }
   ```

   ```html
   <!-- src/app/my-component/my-component.component.html -->
   <ul>
     <li *ngFor="let post of posts">{{ post.title }}</li>
   </ul>
   ```

###### 예제

1. **Observable 생성 및 구독 예제**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { Observable } from 'rxjs';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {

     ngOnInit(): void {
       const observable = new Observable(observer => {
         observer.next('Hello');
         observer.next('World');
         observer.complete();
       });

       observable.subscribe({
         next(value) {
           console.log(value);
         },
         complete() {
           console.log('Complete');
         },
       });
     }
   }
   ```

2. **Operator 사용 예제**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { of } from 'rxjs';
   import { map, filter } from 'rxjs/operators';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {

     ngOnInit(): void {
       const source$ = of(1, 2, 3, 4, 5);
       const result$ = source$.pipe(
         filter(value => value % 2 === 0),
         map(value => value * 2)
       );

       result$.subscribe(value => console.log(value));
     }
   }
   ```

3. **Subject 사용 예제**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { Subject } from 'rxjs';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {
     private subject = new Subject<number>();

     ngOnInit(): void {
       this.subject.subscribe(value => console.log('Observer 1:', value));
       this.subject.subscribe(value => console.log('Observer 2:', value));

       this.subject.next(1);
       this.subject.next(2);
     }
   }
   ```

##### 연습문제와 해답

1. **Observable을 생성하고 값을 출력하세요.**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { Observable } from 'rxjs';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {

     ngOnInit(): void {
       const observable = new Observable(observer => {
         observer.next('Hello');
         observer.next('World');
         observer.complete();
       });

       observable.subscribe({
         next(value) {
           console.log(value);
         },
         complete() {
           console.log('Complete');
         },
       });
     }
   }
   ```

2. **`map` 연산자를 사용하여 Observable의 값을 변환하세요.**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { of } from 'rxjs';
   import { map } from 'rxjs/operators';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {

     ngOnInit(): void {
       const source$ = of(1, 2, 3);
       const result$ = source$.pipe(
         map(value => value * 2)
       );

       result$.subscribe(value => console.log(value));
     }
   }
   ```

3. **`filter` 연산자를 사용하여 Observable의 값을 필터링하세요.**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { of } from 'rxjs';
   import { filter } from 'rxjs/operators';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {

     ngOnInit(): void {
       const source$ = of(1, 2, 3, 4, 5);
       const result$ = source$.pipe(
         filter(value => value % 2 === 0)
       );

       result$.subscribe(value => console.log(value));
     }
   }
   ```

4. **`Subject`를 생성하고 두 개의 구독자가 값을 출력하도록 하세요.**

   ```typescript
   import { Component, OnInit } from

 '@angular/core';
   import { Subject } from 'rxjs';

   @Component({
     selector: 'app-my-component',
     templateUrl: './my-component.component.html',
     styleUrls: ['./my-component.component.css']
   })
   export class MyComponent implements OnInit {
     private subject = new Subject<number>();

     ngOnInit(): void {
       this.subject.subscribe(value => console.log('Observer 1:', value));
       this.subject.subscribe(value => console.log('Observer 2:', value));

       this.subject.next(1);
       this.subject.next(2);
     }
   }
   ```
