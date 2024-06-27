### 3. Angular

#### 3.2. 고급 Angular

##### 3.2.4. Angular Material과 CDK

Angular Material은 Google Material Design 사양을 구현한 Angular용 UI 컴포넌트 라이브러리입니다. CDK(Component Dev Kit)는 Angular Material의 핵심 기능을 제공하여 재사용 가능한 사용자 인터페이스 구성 요소를 쉽게 만들 수 있게 해줍니다.

###### Angular Material 설치

1. **Angular Material 설치**

   Angular CLI를 사용하여 Angular Material을 설치합니다.

   ```bash
   ng add @angular/material
   ```

   이 명령어는 Angular Material, CDK 및 Angular Animations을 설치하고, 기본 테마를 적용합니다.

2. **테마 설정**

   Angular Material은 여러 가지 사전 정의된 테마를 제공합니다. 설치 중에 원하는 테마를 선택할 수 있으며, 이후에 `angular.json` 파일에서 테마를 변경할 수 있습니다.

   ```json
   // angular.json
   "styles": [
     "node_modules/@angular/material/prebuilt-themes/indigo-pink.css",
     "src/styles.css"
   ],
   ```

###### Angular Material 컴포넌트 사용

1. **모듈 가져오기**

   Angular Material의 컴포넌트를 사용하려면 해당 모듈을 가져와야 합니다.

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
   import { MatButtonModule } from '@angular/material/button';
   import { MatInputModule } from '@angular/material/input';
   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       BrowserAnimationsModule,
       MatButtonModule,
       MatInputModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

2. **버튼 컴포넌트**

   Material 버튼 컴포넌트를 사용하여 버튼을 생성합니다.

   ```html
   <!-- src/app/app.component.html -->
   <button mat-button>Click me!</button>
   ```

3. **입력 필드 컴포넌트**

   Material 입력 필드 컴포넌트를 사용하여 입력 필드를 생성합니다.

   ```html
   <!-- src/app/app.component.html -->
   <mat-form-field>
     <mat-label>Enter your name</mat-label>
     <input matInput>
   </mat-form-field>
   ```

###### CDK(Component Dev Kit)

1. **CDK 설치**

   Angular Material을 설치하면 CDK도 함께 설치됩니다.

2. **CDK 드래그 앤 드롭**

   CDK 드래그 앤 드롭 기능을 사용하여 요소를 드래그하고 드롭할 수 있습니다.

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
   import { DragDropModule } from '@angular/cdk/drag-drop';
   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       BrowserAnimationsModule,
       DragDropModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

3. **드래그 앤 드롭 사용 예제**

   ```html
   <!-- src/app/app.component.html -->
   <div cdkDrag>
     Drag me!
   </div>
   ```

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css']
   })
   export class AppComponent { }
   ```

###### 예제

1. **Angular Material 버튼과 입력 필드 사용 예제**

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
   import { MatButtonModule } from '@angular/material/button';
   import { MatInputModule } from '@angular/material/input';
   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       BrowserAnimationsModule,
       MatButtonModule,
       MatInputModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <button mat-button>Click me!</button>
   <mat-form-field>
     <mat-label>Enter your name</mat-label>
     <input matInput>
   </mat-form-field>
   ```

2. **CDK 드래그 앤 드롭 사용 예제**

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
   import { DragDropModule } from '@angular/cdk/drag-drop';
   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       BrowserAnimationsModule,
       DragDropModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <div cdkDrag>
     Drag me!
   </div>
   ```

   ```typescript
   // src/app/app.component.ts
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css']
   })
   export class AppComponent { }
   ```

##### 연습문제와 해답

1. **Angular Material을 설치하고, 기본 테마를 설정하세요.**

   ```bash
   ng add @angular/material
   ```

   ```json
   // angular.json
   "styles": [
     "node_modules/@angular/material/prebuilt-themes/indigo-pink.css",
     "src/styles.css"
   ],
   ```

2. **Angular Material 버튼과 입력 필드를 사용하여 폼을 작성하세요.**

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
   import { MatButtonModule } from '@angular/material/button';
   import { MatInputModule } from '@angular/material/input';
   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       BrowserAnimationsModule,
       MatButtonModule,
       MatInputModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <button mat-button>Click me!</button>
   <mat-form-field>
     <mat-label>Enter your name</mat-label>
     <input matInput>
   </mat-form-field>
   ```

3. **CDK를 사용하여 드래그 앤 드롭 기능을 구현하세요.**

   ```typescript
   // src/app/app.module.ts
   import { NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
   import { DragDropModule } from '@angular/cdk/drag-drop';
   import { AppComponent } from './app.component';

   @NgModule({
     declarations: [
       AppComponent
     ],
     imports: [
       BrowserModule,
       BrowserAnimationsModule,
       DragDropModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

   ```html
   <!-- src/app/app.component.html -->
   <div cdkDrag>
     Drag me!
   </div>
   ```
