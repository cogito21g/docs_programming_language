### 10. 기타 중요한 주제

#### 10.5. DOM 조작 (기초)

DOM(Document Object Model)은 웹 페이지의 구조화된 표현으로, JavaScript를 사용하여 웹 페이지의 콘텐츠와 구조를 동적으로 조작할 수 있게 합니다. DOM을 조작하는 기본적인 방법을 학습하여 웹 페이지의 요소를 추가, 제거, 수정하는 방법을 익힐 수 있습니다.

##### DOM 요소 선택

DOM 요소를 선택하는 다양한 방법이 있습니다.

1. **getElementById**

   ```javascript
   const element = document.getElementById('myElement');
   ```

2. **getElementsByClassName**

   ```javascript
   const elements = document.getElementsByClassName('myClass');
   ```

3. **getElementsByTagName**

   ```javascript
   const elements = document.getElementsByTagName('div');
   ```

4. **querySelector**

   ```javascript
   const element = document.querySelector('.myClass');
   ```

5. **querySelectorAll**

   ```javascript
   const elements = document.querySelectorAll('.myClass');
   ```

##### DOM 요소 생성 및 추가

DOM 요소를 동적으로 생성하고, 기존 요소에 추가할 수 있습니다.

1. **createElement**

   ```javascript
   const newElement = document.createElement('div');
   newElement.textContent = 'Hello, World!';
   ```

2. **appendChild**

   ```javascript
   const parentElement = document.getElementById('parent');
   parentElement.appendChild(newElement);
   ```

3. **insertBefore**

   ```javascript
   const referenceElement = document.getElementById('reference');
   parentElement.insertBefore(newElement, referenceElement);
   ```

##### DOM 요소 수정

기존의 DOM 요소를 수정할 수 있습니다.

1. **textContent**

   ```javascript
   const element = document.getElementById('myElement');
   element.textContent = 'New Content';
   ```

2. **innerHTML**

   ```javascript
   const element = document.getElementById('myElement');
   element.innerHTML = '<span>New Content</span>';
   ```

3. **setAttribute**

   ```javascript
   const element = document.getElementById('myElement');
   element.setAttribute('class', 'newClass');
   ```

##### DOM 요소 제거

DOM 요소를 제거할 수 있습니다.

1. **removeChild**

   ```javascript
   const parentElement = document.getElementById('parent');
   const childElement = document.getElementById('child');
   parentElement.removeChild(childElement);
   ```

2. **remove**

   ```javascript
   const element = document.getElementById('myElement');
   element.remove();
   ```

##### 예제

1. **DOM 요소 생성 및 추가**

   ```javascript
   // 새로운 <div> 요소 생성
   const newDiv = document.createElement('div');
   newDiv.textContent = 'Hello, World!';

   // <body> 요소에 새로운 <div> 요소 추가
   document.body.appendChild(newDiv);
   ```

2. **DOM 요소 선택 및 수정**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>DOM Manipulation Example</title>
   </head>
   <body>
     <div id="content">Old Content</div>
     <script>
       const contentDiv = document.getElementById('content');
       contentDiv.textContent = 'New Content';
     </script>
   </body>
   </html>
   ```

3. **DOM 요소 제거**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>DOM Manipulation Example</title>
   </head>
   <body>
     <div id="parent">
       <div id="child">Child Element</div>
     </div>
     <script>
       const parentDiv = document.getElementById('parent');
       const childDiv = document.getElementById('child');
       parentDiv.removeChild(childDiv);
     </script>
   </body>
   </html>
   ```

##### 연습문제와 해답

1. **DOM 요소를 생성하여 특정 부모 요소에 추가하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>DOM Manipulation Practice</title>
   </head>
   <body>
     <div id="parent"></div>
     <script>
       const parentDiv = document.getElementById('parent');
       const newDiv = document.createElement('div');
       newDiv.textContent = 'This is a new div';
       parentDiv.appendChild(newDiv);
     </script>
   </body>
   </html>
   ```

2. **특정 클래스명을 가진 모든 요소의 텍스트 내용을 변경하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>DOM Manipulation Practice</title>
   </head>
   <body>
     <div class="myClass">Old Text 1</div>
     <div class="myClass">Old Text 2</div>
     <script>
       const elements = document.querySelectorAll('.myClass');
       elements.forEach(element => {
         element.textContent = 'New Text';
       });
     </script>
   </body>
   </html>
   ```

3. **DOM 요소의 속성을 설정하고, 변경된 속성을 확인하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>DOM Manipulation Practice</title>
   </head>
   <body>
     <img id="myImage" src="old_image.jpg" alt="Old Image">
     <script>
       const image = document.getElementById('myImage');
       image.setAttribute('src', 'new_image.jpg');
       image.setAttribute('alt', 'New Image');
       console.log(image.getAttribute('src')); // "new_image.jpg"
       console.log(image.getAttribute('alt')); // "New Image"
     </script>
   </body>
   </html>
   ```

4. **DOM 요소를 삭제하고, 삭제된 요소가 DOM에서 사라졌는지 확인하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>DOM Manipulation Practice</title>
   </head>
   <body>
     <div id="container">
       <p id="paragraph">This is a paragraph.</p>
     </div>
     <script>
       const paragraph = document.getElementById('paragraph');
       paragraph.remove();
       console.log(document.getElementById('paragraph')); // null
     </script>
   </body>
   </html>
   ```
