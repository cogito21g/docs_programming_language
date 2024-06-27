### 16. 최신 JavaScript 프레임워크 및 라이브러리

#### 16.3. Angular 심화

Angular는 Google에서 개발한 프레임워크로, 대규모 애플리케이션을 구축하는 데 적합합니다. Angular는 강력한 구조와 다양한 기능을 제공하여 복잡한 웹 애플리케이션을 쉽게 관리할 수 있습니다. 이번 섹션에서는 Angular Services, Dependency Injection, Angular Forms 등을 다루겠습니다.

##### 16.3.1. Angular Services

Services는 Angular에서 공통된 기능을 모듈화하여 여러 컴포넌트에서 재사용할 수 있도록 하는 객체입니다. 서비스는 주로 비즈니스 로직, 데이터 처리, HTTP 요청 등을 처리합니다.

###### 서비스 생성

1. **Angular CLI를 사용하여 서비스 생성**

   ```bash
   ng generate service data
   ```

2. **서비스 코드 작성**

   ```typescript
   // src/app/data.service.ts
   import { Injectable } from '@angular/core';

   @Injectable({
     providedIn: 'root',
   })
   export class DataService {
     private data: string[] = [];

     addData(item: string) {
       this.data.push(item);
     }

     getData(): string[] {
       return this.data;
     }
   }
   ```

3. **서비스 사용**

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';
   import { DataService } from './data.service';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     newItem = '';

     constructor(private dataService: DataService) {}

     addItem() {
       this.dataService.addData(this.newItem);
       this.newItem = '';
     }

     get items() {
       return this.dataService.getData();
     }
   }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <div>
     <input [(ngModel)]="newItem" placeholder="Add a new item">
     <button (click)="addItem()">Add</button>
   </div>
   <ul>
     <li *ngFor="let item of items">{{ item }}</li>
   </ul>
   ```

##### 16.3.2. Dependency Injection

Dependency Injection(DI)은 Angular의 핵심 개념으로, 객체 간의 의존성을 주입하여 객체를 관리하는 방법입니다. DI를 통해 코드의 결합도를 낮추고, 테스트 가능성을 높일 수 있습니다.

###### DI 예제

1. **서비스 생성**

   ```typescript
   // src/app/logger.service.ts
   import { Injectable } from '@angular/core';

   @Injectable({
     providedIn: 'root',
   })
   export class LoggerService {
     log(message: string) {
       console.log('LoggerService: ' + message);
     }
   }
   ```

2. **서비스 주입**

   ```typescript
   // src/app/data.service.ts
   import { Injectable } from '@angular/core';
   import { LoggerService } from './logger.service';

   @Injectable({
     providedIn: 'root',
   })
   export class DataService {
     private data: string[] = [];

     constructor(private logger: LoggerService) {}

     addData(item: string) {
       this.data.push(item);
       this.logger.log('Item added: ' + item);
     }

     getData(): string[] {
       return this.data;
     }
   }
   ```

3. **컴포넌트에서 서비스 사용**

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';
   import { DataService } from './data.service';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     newItem = '';

     constructor(private dataService: DataService) {}

     addItem() {
       this.dataService.addData(this.newItem);
       this.newItem = '';
     }

     get items() {
       return this.dataService.getData();
     }
   }
   ```

##### 16.3.3. Angular Forms

Angular Forms는 폼을 쉽게 작성하고, 검증하며, 처리할 수 있도록 도와줍니다. Angular Forms는 Template-driven Forms와 Reactive Forms 두 가지 방식으로 사용할 수 있습니다.

###### Template-driven Forms

Template-driven Forms는 Angular 템플릿에서 폼을 작성하고, Angular가 이를 자동으로 관리하는 방식입니다.

1. **폼 작성**

   ```html
   <!-- src/app/app.component.html -->
   <form #myForm="ngForm" (ngSubmit)="onSubmit(myForm)">
     <label for="name">Name:</label>
     <input type="text" id="name" name="name" ngModel required>
     <button type="submit" [disabled]="myForm.invalid">Submit</button>
   </form>
   ```

2. **폼 처리**

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     onSubmit(form) {
       console.log('Form submitted:', form.value);
     }
   }
   ```

###### Reactive Forms

Reactive Forms는 Angular에서 폼을 프로그래밍 방식으로 정의하고, 관리하는 방식입니다.

1. **Reactive Forms 모듈 추가**

   ```typescript
   // src/app/app.module.ts
   import { BrowserModule } from '@angular/platform-browser';
   import { NgModule } from '@angular/core';
   import { ReactiveFormsModule } from '@angular/forms';

   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       ReactiveFormsModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

2. **폼 작성**

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';
   import { FormBuilder, FormGroup, Validators } from '@angular/forms';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     myForm: FormGroup;

     constructor(private fb: FormBuilder) {
       this.myForm = this.fb.group({
         name: ['', Validators.required]
       });
     }

     onSubmit() {
       if (this.myForm.valid) {
         console.log('Form submitted:', this.myForm.value);
       }
     }
   }
   ```

3. **폼 템플릿**

   ```html
   <!-- src/app/app.component.html -->
   <form [formGroup]="myForm" (ngSubmit)="onSubmit()">
     <label for="name">Name:</label>
     <input type="text" id="name" formControlName="name">
     <div *ngIf="myForm.get('name').invalid && myForm.get('name').touched">
       Name is required
     </div>
     <button type="submit" [disabled]="myForm.invalid">Submit</button>
   </form>
   ```

##### 연습문제와 해답

1. **Angular Services를 사용하여 데이터를 관리하는 애플리케이션을 작성하세요.**

   ```typescript
   // src/app/data.service.ts
   import { Injectable } from '@angular/core';

   @Injectable({
     providedIn: 'root',
   })
   export class DataService {
     private data: string[] = [];

     addData(item: string) {
       this.data.push(item);
     }

     getData(): string[] {
       return this.data;
     }
   }
   ```

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';
   import { DataService } from './data.service';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     newItem = '';

     constructor(private dataService: DataService) {}

     addItem() {
       this.dataService.addData(this.newItem);
       this.newItem = '';
     }

     get items() {
       return this.dataService.getData();
     }
   }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <div>
     <input [(ngModel)]="newItem" placeholder="Add a new item">
     <button (click)="addItem()">Add</button>
   </div>
   <ul>
     <li *ngFor="let item of items">{{ item }}</li>
   </ul>
   ```

2. **Dependency Injection을 사용하여 Logger 서비스를 주입하세요.**

   ```typescript
   // src/app/logger.service.ts
   import { Injectable } from '@angular/core';

   @Injectable({
     providedIn: 'root',
   })
   export class LoggerService {
     log(message: string) {
       console.log('LoggerService: ' + message);
     }
   }
   ```

   ```typescript
   // src/app/data.service.ts
   import { Injectable } from '@angular/core';
   import { LoggerService } from './logger.service';

   @Injectable({
     providedIn: 'root',
   })
   export class DataService {
     private data: string[] = [];

     constructor

(private logger: LoggerService) {}

     addData(item: string) {
       this.data.push(item);
       this.logger.log('Item added: ' + item);
     }

     getData(): string[] {
       return this.data;
     }
   }
   ```

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';
   import { DataService } from './data.service';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     newItem = '';

     constructor(private dataService: DataService) {}

     addItem() {
       this.dataService.addData(this.newItem);
       this.newItem = '';
     }

     get items() {
       return this.dataService.getData();
     }
   }
   ```

3. **Template-driven Forms를 사용하여 폼 데이터를 관리하세요.**

   ```html
   <!-- src/app/app.component.html -->
   <form #myForm="ngForm" (ngSubmit)="onSubmit(myForm)">
     <label for="name">Name:</label>
     <input type="text" id="name" name="name" ngModel required>
     <button type="submit" [disabled]="myForm.invalid">Submit</button>
   </form>
   ```

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     onSubmit(form) {
       console.log('Form submitted:', form.value);
     }
   }
   ```

4. **Reactive Forms를 사용하여 폼 데이터를 관리하세요.**

   ```typescript
   // src/app/app.module.ts
   import { BrowserModule } from '@angular/platform-browser';
   import { NgModule } from '@angular/core';
   import { ReactiveFormsModule } from '@angular/forms';

   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       ReactiveFormsModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';
   import { FormBuilder, FormGroup, Validators } from '@angular/forms';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css'],
   })
   export class AppComponent {
     myForm: FormGroup;

     constructor(private fb: FormBuilder) {
       this.myForm = this.fb.group({
         name: ['', Validators.required]
       });
     }

     onSubmit() {
       if (this.myForm.valid) {
         console.log('Form submitted:', this.myForm.value);
       }
     }
   }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <form [formGroup]="myForm" (ngSubmit)="onSubmit()">
     <label for="name">Name:</label>
     <input type="text" id="name" formControlName="name">
     <div *ngIf="myForm.get('name').invalid && myForm.get('name').touched">
       Name is required
     </div>
     <button type="submit" [disabled]="myForm.invalid">Submit</button>
   </form>
   ```

