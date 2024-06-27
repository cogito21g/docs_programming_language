### 19. 빌드 도구와 작업 자동화

#### 19.1. Gulp

Gulp는 JavaScript로 작성된 빌드 도구로, 파일 기반 작업을 자동화하는 데 사용됩니다. Gulp는 스트림 기반으로 동작하여 파일을 읽고, 변환하고, 출력할 수 있습니다. 이번 섹션에서는 Gulp의 기본 개념과 설정 방법을 학습하겠습니다.

##### 19.1.1. Gulp 설치 및 기본 설정

###### Gulp 설치

Gulp CLI와 Gulp를 프로젝트에 설치합니다.

```bash
npm install --global gulp-cli
npm install --save-dev gulp
```

###### Gulp 기본 설정

프로젝트 루트에 `gulpfile.js` 파일을 생성하고, 기본 설정을 추가합니다.

```javascript
// gulpfile.js
const gulp = require('gulp');

gulp.task('default', () => {
  console.log('Gulp is running!');
});
```

Gulp를 실행하여 설정이 제대로 되었는지 확인합니다.

```bash
gulp
```

##### 19.1.2. Gulp를 사용한 작업 자동화

###### 기본 작업 정의

Gulp 작업은 파일을 읽고, 변환하고, 출력하는 작업을 정의할 수 있습니다. 예를 들어, SASS 파일을 CSS로 변환하는 작업을 정의해보겠습니다.

1. **필요한 플러그인 설치**

   ```bash
   npm install --save-dev gulp-sass
   ```

2. **작업 정의**

   ```javascript
   // gulpfile.js
   const gulp = require('gulp');
   const sass = require('gulp-sass')(require('sass'));

   gulp.task('styles', () => {
     return gulp.src('src/scss/**/*.scss')
       .pipe(sass().on('error', sass.logError))
       .pipe(gulp.dest('dist/css'));
   });

   gulp.task('default', gulp.series('styles'));
   ```

   `src/scss` 폴더의 모든 SASS 파일을 읽어와 `dist/css` 폴더에 CSS 파일로 출력하는 작업을 정의했습니다.

3. **작업 실행**

   ```bash
   gulp styles
   ```

###### 파일 변경 감지

파일 변경을 감지하여 자동으로 작업을 실행하도록 설정할 수 있습니다.

1. **감시 작업 정의**

   ```javascript
   // gulpfile.js
   const gulp = require('gulp');
   const sass = require('gulp-sass')(require('sass'));

   gulp.task('styles', () => {
     return gulp.src('src/scss/**/*.scss')
       .pipe(sass().on('error', sass.logError))
       .pipe(gulp.dest('dist/css'));
   });

   gulp.task('watch', () => {
     gulp.watch('src/scss/**/*.scss', gulp.series('styles'));
   });

   gulp.task('default', gulp.series('styles', 'watch'));
   ```

2. **감시 작업 실행**

   ```bash
   gulp
   ```

##### 19.1.3. Gulp를 사용한 추가 작업

Gulp를 사용하여 이미지 최적화, 파일 병합, 파일 압축 등의 작업을 자동화할 수 있습니다.

###### 이미지 최적화 작업

1. **플러그인 설치**

   ```bash
   npm install --save-dev gulp-imagemin
   ```

2. **작업 정의**

   ```javascript
   // gulpfile.js
   const gulp = require('gulp');
   const sass = require('gulp-sass')(require('sass'));
   const imagemin = require('gulp-imagemin');

   gulp.task('styles', () => {
     return gulp.src('src/scss/**/*.scss')
       .pipe(sass().on('error', sass.logError))
       .pipe(gulp.dest('dist/css'));
   });

   gulp.task('images', () => {
     return gulp.src('src/images/**/*')
       .pipe(imagemin())
       .pipe(gulp.dest('dist/images'));
   });

   gulp.task('watch', () => {
     gulp.watch('src/scss/**/*.scss', gulp.series('styles'));
     gulp.watch('src/images/**/*', gulp.series('images'));
   });

   gulp.task('default', gulp.series('styles', 'images', 'watch'));
   ```

###### 파일 병합 및 압축 작업

1. **플러그인 설치**

   ```bash
   npm install --save-dev gulp-concat gulp-uglify
   ```

2. **작업 정의**

   ```javascript
   // gulpfile.js
   const gulp = require('gulp');
   const sass = require('gulp-sass')(require('sass'));
   const imagemin = require('gulp-imagemin');
   const concat = require('gulp-concat');
   const uglify = require('gulp-uglify');

   gulp.task('styles', () => {
     return gulp.src('src/scss/**/*.scss')
       .pipe(sass().on('error', sass.logError))
       .pipe(gulp.dest('dist/css'));
   });

   gulp.task('images', () => {
     return gulp.src('src/images/**/*')
       .pipe(imagemin())
       .pipe(gulp.dest('dist/images'));
   });

   gulp.task('scripts', () => {
     return gulp.src('src/js/**/*.js')
       .pipe(concat('all.js'))
       .pipe(uglify())
       .pipe(gulp.dest('dist/js'));
   });

   gulp.task('watch', () => {
     gulp.watch('src/scss/**/*.scss', gulp.series('styles'));
     gulp.watch('src/images/**/*', gulp.series('images'));
     gulp.watch('src/js/**/*.js', gulp.series('scripts'));
   });

   gulp.task('default', gulp.series('styles', 'images', 'scripts', 'watch'));
   ```

##### 연습문제와 해답

1. **Gulp를 설치하고, 기본 작업을 정의하세요.**

   ```bash
   npm install --global gulp-cli
   npm install --save-dev gulp
   ```

   ```javascript
   // gulpfile.js
   const gulp = require('gulp');

   gulp.task('default', () => {
     console.log('Gulp is running!');
   });
   ```

   ```bash
   gulp
   ```

2. **SASS 파일을 CSS로 변환하는 Gulp 작업을 정의하세요.**

   ```bash
   npm install --save-dev gulp-sass
   ```

   ```javascript
   // gulpfile.js
   const gulp = require('gulp');
   const sass = require('gulp-sass')(require('sass'));

   gulp.task('styles', () => {
     return gulp.src('src/scss/**/*.scss')
       .pipe(sass().on('error', sass.logError))
       .pipe(gulp.dest('dist/css'));
   });

   gulp.task('default', gulp.series('styles'));
   ```

   ```bash
   gulp styles
   ```

3. **이미지 최적화 작업을 Gulp로 자동화하세요.**

   ```bash
   npm install --save-dev gulp-imagemin
   ```

   ```javascript
   // gulpfile.js
   const gulp = require('gulp');
   const imagemin = require('gulp-imagemin');

   gulp.task('images', () => {
     return gulp.src('src/images/**/*')
       .pipe(imagemin())
       .pipe(gulp.dest('dist/images'));
   });

   gulp.task('default', gulp.series('images'));
   ```

   ```bash
   gulp images
   ```
   