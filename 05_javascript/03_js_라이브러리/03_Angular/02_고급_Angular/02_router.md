### 3. Angular

#### 3.2. 고급 Angular

##### 3.2.2. 라우터 (Router)

Angular 라우터는 단일 페이지 애플리케이션(SPA)에서 내비게이션을 관리하는 강력한 도구입니다. 라우터를 사용하면 URL을 통해 애플리케이션의 다양한 뷰를 로드하고, 뷰 간의 전환을 관리할 수 있습니다.

###### Angular 라우터 설정

1. **라우터 모듈 가져오기**

   라우터를 사용하려면 `RouterModule`을 가져와야 합니다.

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { RouterModule, Routes } from '@angular/router';
   import { AppComponent } from './app.component';
   import { HomeComponent } from './home/home.component';
   import { AboutComponent } from './about/about.component';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent }
   ];

   @NgModule({
     declarations: [
       AppComponent,
       HomeComponent,
       AboutComponent
     ],
     imports: [
       BrowserModule,
       RouterModule.forRoot(routes)
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

2. **라우팅 설정**

   라우팅을 설정하려면 `Routes` 배열을 정의하고, 각 경로에 대해 컴포넌트를 지정합니다.

   ```typescript
   // src/app/app.module.ts
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent }
   ];
   ```

3. **라우터 아웃렛**

   라우터 아웃렛은 컴포넌트를 렌더링할 위치를 정의합니다.

   ```html
   <!-- src/app/app.component.html -->
   <nav>
     <a routerLink="/">Home</a>
     <a routerLink="/about">About</a>
   </nav>
   <router-outlet></router-outlet>
   ```

4. **라우터 링크**

   `routerLink` 디렉티브를 사용하여 내비게이션 링크를 생성합니다.

   ```html
   <!-- src/app/app.component.html -->
   <nav>
     <a routerLink="/">Home</a>
     <a routerLink="/about">About</a>
   </nav>
   ```

###### 라우터의 고급 기능

1. **라우터 가드**

   라우터 가드는 라우트로의 접근을 제어합니다. 예를 들어, 인증된 사용자만 특정 라우트에 접근하도록 할 수 있습니다.

   ```typescript
   // src/app/auth.guard.ts
   import { Injectable } from '@angular/core';
   import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';

   @Injectable({
     providedIn: 'root'
   })
   export class AuthGuard implements CanActivate {

     constructor(private router: Router) {}

     canActivate(
       next: ActivatedRouteSnapshot,
       state: RouterStateSnapshot): boolean {
       const isAuthenticated = /* 인증 로직 */;
       if (isAuthenticated) {
         return true;
       } else {
         this.router.navigate(['/login']);
         return false;
       }
     }
   }
   ```

   ```typescript
   // src/app/app.module.ts
   import { AuthGuard } from './auth.guard';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent, canActivate: [AuthGuard] }
   ];
   ```

2. **라우터 파라미터**

   라우터 파라미터는 URL에 포함된 동적 값을 처리합니다.

   ```typescript
   // src/app/app.module.ts
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about/:id', component: AboutComponent }
   ];
   ```

   ```typescript
   // src/app/about/about.component.ts
   import { Component, OnInit } from '@angular/core';
   import { ActivatedRoute } from '@angular/router';

   @Component({
     selector: 'app-about',
     templateUrl: './about.component.html',
     styleUrls: ['./about.component.css']
   })
   export class AboutComponent implements OnInit {
     id: string;

     constructor(private route: ActivatedRoute) { }

     ngOnInit(): void {
       this.id = this.route.snapshot.paramMap.get('id');
     }
   }
   ```

   ```html
   <!-- src/app/about/about.component.html -->
   <p>About ID: {{ id }}</p>
   ```

3. **중첩 라우트**

   중첩 라우트는 자식 라우트를 정의하여 계층적 내비게이션을 구성합니다.

   ```typescript
   // src/app/app.module.ts
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent, children: [
       { path: 'details', component: DetailsComponent }
     ]}
   ];
   ```

   ```html
   <!-- src/app/about/about.component.html -->
   <p>About Component</p>
   <router-outlet></router-outlet>
   ```

###### 예제

1. **기본 라우팅 설정**

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { RouterModule, Routes } from '@angular/router';
   import { AppComponent } from './app.component';
   import { HomeComponent } from './home/home.component';
   import { AboutComponent } from './about/about.component';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent }
   ];

   @NgModule({
     declarations: [
       AppComponent,
       HomeComponent,
       AboutComponent
     ],
     imports: [
       BrowserModule,
       RouterModule.forRoot(routes)
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <nav>
     <a routerLink="/">Home</a>
     <a routerLink="/about">About</a>
   </nav>
   <router-outlet></router-outlet>
   ```

2. **라우터 가드 예제**

   ```typescript
   // src/app/auth.guard.ts
   import { Injectable } from '@angular/core';
   import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';

   @Injectable({
     providedIn: 'root'
   })
   export class AuthGuard implements CanActivate {

     constructor(private router: Router) {}

     canActivate(
       next: ActivatedRouteSnapshot,
       state: RouterStateSnapshot): boolean {
       const isAuthenticated = /* 인증 로직 */;
       if (isAuthenticated) {
         return true;
       } else {
         this.router.navigate(['/login']);
         return false;
       }
     }
   }
   ```

   ```typescript
   // src/app/app.module.ts
   import { AuthGuard } from './auth.guard';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent, canActivate: [AuthGuard] }
   ];
   ```

3. **라우터 파라미터 예제**

   ```typescript
   // src/app/app.module.ts
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about/:id', component: AboutComponent }
   ];
   ```

   ```typescript
   // src/app/about/about.component.ts
   import { Component, OnInit } from '@angular/core';
   import { ActivatedRoute } from '@angular/router';

   @Component({
     selector: 'app-about',
     templateUrl: './about.component.html',
     styleUrls: ['./about.component.css']
   })
   export class AboutComponent implements OnInit {
     id: string;

     constructor(private route: ActivatedRoute) { }

     ngOnInit(): void {
       this.id = this.route.snapshot.paramMap.get('id');
     }
   }
   ```

   ```html
   <!-- src/app/about/about.component.html -->
   <p>About ID: {{ id }}</p>
   ```

4. **중첩 라우트 예제**

   ```typescript
   // src/app/app.module.ts
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent, children: [
       { path: 'details', component: DetailsComponent }
     ]}
   ];
   ```

   ```html
   <!-- src/app/about/about.component.html -->
   <p>About Component</p>
   <router-outlet></router-outlet>
   ```

##### 연습문제와 해답

1. **기본 라우팅 설정을 작성하고, 홈과 어바웃 컴포넌트를 생성하여 라우팅을 설정하세요.**

   ```bash
   ng generate component home
   ng generate component about
   ```

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { RouterModule,

 Routes } from '@angular/router';
   import { AppComponent } from './app.component';
   import { HomeComponent } from './home/home.component';
   import { AboutComponent } from './about/about.component';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent }
   ];

   @NgModule({
     declarations: [
       AppComponent,
       HomeComponent,
       AboutComponent
     ],
     imports: [
       BrowserModule,
       RouterModule.forRoot(routes)
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <nav>
     <a routerLink="/">Home</a>
     <a routerLink="/about">About</a>
   </nav>
   <router-outlet></router-outlet>
   ```

2. **라우터 가드를 생성하고, 인증된 사용자만 어바웃 페이지에 접근할 수 있도록 설정하세요.**

   ```bash
   ng generate guard auth
   ```

   ```typescript
   // src/app/auth.guard.ts
   import { Injectable } from '@angular/core';
   import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';

   @Injectable({
     providedIn: 'root'
   })
   export class AuthGuard implements CanActivate {

     constructor(private router: Router) {}

     canActivate(
       next: ActivatedRouteSnapshot,
       state: RouterStateSnapshot): boolean {
       const isAuthenticated = /* 인증 로직 */;
       if (isAuthenticated) {
         return true;
       } else {
         this.router.navigate(['/login']);
         return false;
       }
     }
   }
   ```

   ```typescript
   // src/app/app.module.ts
   import { AuthGuard } from './auth.guard';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent, canActivate: [AuthGuard] }
   ];
   ```

3. **라우터 파라미터를 사용하여 어바웃 페이지에서 동적 ID를 출력하세요.**

   ```typescript
   // src/app/app.module.ts
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about/:id', component: AboutComponent }
   ];
   ```

   ```typescript
   // src/app/about/about.component.ts
   import { Component, OnInit } from '@angular/core';
   import { ActivatedRoute } from '@angular/router';

   @Component({
     selector: 'app-about',
     templateUrl: './about.component.html',
     styleUrls: ['./about.component.css']
   })
   export class AboutComponent implements OnInit {
     id: string;

     constructor(private route: ActivatedRoute) { }

     ngOnInit(): void {
       this.id = this.route.snapshot.paramMap.get('id');
     }
   }
   ```

   ```html
   <!-- src/app/about/about.component.html -->
   <p>About ID: {{ id }}</p>
   ```

4. **중첩 라우트를 사용하여 어바웃 페이지에 디테일 페이지를 추가하세요.**

   ```bash
   ng generate component details
   ```

   ```typescript
   // src/app/app.module.ts
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent, children: [
       { path: 'details', component: DetailsComponent }
     ]}
   ];
   ```

   ```html
   <!-- src/app/about/about.component.html -->
   <p>About Component</p>
   <router-outlet></router-outlet>
   ```
