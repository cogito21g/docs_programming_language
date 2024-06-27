### 5. D3.js

#### 5.1. D3.js 기초

##### 5.1.3. SVG 요소 생성

SVG(Scalable Vector Graphics)는 XML 기반의 벡터 그래픽을 표현하기 위한 형식입니다. D3.js를 사용하면 다양한 SVG 요소를 생성하여 데이터를 시각화할 수 있습니다. 여기서는 기본적인 SVG 요소인 `rect`, `circle`, `line`, `text` 등을 생성하는 방법을 다루겠습니다.

###### SVG 요소 생성

1. **사각형(rect) 생성**

   사각형 요소를 생성하고 속성을 설정합니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Rect Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('rect')
         .attr('x', 50)
         .attr('y', 50)
         .attr('width', 200)
         .attr('height', 100)
         .attr('fill', 'blue');
     </script>
   </body>
   </html>
   ```

2. **원(circle) 생성**

   원 요소를 생성하고 속성을 설정합니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Circle Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('circle')
         .attr('cx', 150)
         .attr('cy', 150)
         .attr('r', 50)
         .attr('fill', 'green');
     </script>
   </body>
   </html>
   ```

3. **선(line) 생성**

   선 요소를 생성하고 속성을 설정합니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Line Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('line')
         .attr('x1', 50)
         .attr('y1', 50)
         .attr('x2', 200)
         .attr('y2', 200)
         .attr('stroke', 'red')
         .attr('stroke-width', 2);
     </script>
   </body>
   </html>
   ```

4. **텍스트(text) 생성**

   텍스트 요소를 생성하고 속성을 설정합니다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Text Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('text')
         .attr('x', 100)
         .attr('y', 150)
         .attr('font-size', '24px')
         .attr('fill', 'black')
         .text('Hello, D3.js!');
     </script>
   </body>
   </html>
   ```

###### 예제

1. **사각형(rect) 생성 예제**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Rect Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('rect')
         .attr('x', 50)
         .attr('y', 50)
         .attr('width', 200)
         .attr('height', 100)
         .attr('fill', 'blue');
     </script>
   </body>
   </html>
   ```

2. **원(circle) 생성 예제**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Circle Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('circle')
         .attr('cx', 150)
         .attr('cy', 150)
         .attr('r', 50)
         .attr('fill', 'green');
     </script>
   </body>
   </html>
   ```

3. **선(line) 생성 예제**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Line Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('line')
         .attr('x1', 50)
         .attr('y1', 50)
         .attr('x2', 200)
         .attr('y2', 200)
         .attr('stroke', 'red')
         .attr('stroke-width', 2);
     </script>
   </body>
   </html>
   ```

4. **텍스트(text) 생성 예제**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Text Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('text')
         .attr('x', 100)
         .attr('y', 150)
         .attr('font-size', '24px')
         .attr('fill', 'black')
         .text('Hello, D3.js!');
     </script>
   </body>
   </html>
   ```

##### 연습문제와 해답

1. **D3.js를 사용하여 사각형(rect) 요소를 생성하고, 크기와 색상을 설정하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Rect Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .

attr('width', width)
         .attr('height', height);

       svg.append('rect')
         .attr('x', 50)
         .attr('y', 50)
         .attr('width', 200)
         .attr('height', 100)
         .attr('fill', 'blue');
     </script>
   </body>
   </html>
   ```

2. **D3.js를 사용하여 원(circle) 요소를 생성하고, 위치와 반지름을 설정하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Circle Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('circle')
         .attr('cx', 150)
         .attr('cy', 150)
         .attr('r', 50)
         .attr('fill', 'green');
     </script>
   </body>
   </html>
   ```

3. **D3.js를 사용하여 선(line) 요소를 생성하고, 시작점과 끝점을 설정하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Line Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('line')
         .attr('x1', 50)
         .attr('y1', 50)
         .attr('x2', 200)
         .attr('y2', 200)
         .attr('stroke', 'red')
         .attr('stroke-width', 2);
     </script>
   </body>
   </html>
   ```

4. **D3.js를 사용하여 텍스트(text) 요소를 생성하고, 위치와 내용을 설정하세요.**

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>D3.js Text Example</title>
     <script src="https://d3js.org/d3.v7.min.js"></script>
   </head>
   <body>
     <script>
       const width = 500;
       const height = 300;

       const svg = d3.select('body')
         .append('svg')
         .attr('width', width)
         .attr('height', height);

       svg.append('text')
         .attr('x', 100)
         .attr('y', 150)
         .attr('font-size', '24px')
         .attr('fill', 'black')
         .text('Hello, D3.js!');
     </script>
   </body>
   </html>
   ```
