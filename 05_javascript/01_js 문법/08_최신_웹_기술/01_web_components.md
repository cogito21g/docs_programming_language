### 8. 최신 웹 기술

#### 8.1. Web Components (Custom Elements, Shadow DOM, HTML Templates)

Web Components는 재사용 가능한 캡슐화된 HTML 요소를 만들기 위한 표준 기술입니다. Web Components는 Custom Elements, Shadow DOM, HTML Templates 세 가지 주요 기술로 구성됩니다.

##### 1. Custom Elements

Custom Elements는 새로운 HTML 요소를 정의하고, 브라우저가 이를 인식하도록 합니다.

###### 예제

```javascript
class MyElement extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = `
      <style>
        p {
          color: blue;
        }
      </style>
      <p>Hello, World!</p>
    `;
  }
}

customElements.define('my-element', MyElement);
```

```html
<my-element></my-element>
```

##### 2. Shadow DOM

Shadow DOM은 요소의 내부 DOM을 캡슐화하여 스타일과 구조가 외부와 격리되도록 합니다.

###### 예제

```javascript
class MyElement extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = `
      <style>
        p {
          color: red;
        }
      </style>
      <p>Shadow DOM Example</p>
    `;
  }
}

customElements.define('my-element', MyElement);
```

```html
<my-element></my-element>
```

##### 3. HTML Templates

HTML Templates는 재사용 가능한 HTML 구조를 정의할 수 있게 해줍니다. `<template>` 태그를 사용하여 템플릿을 정의합니다.

###### 예제

```html
<template id="my-template">
  <style>
    p {
      color: green;
    }
  </style>
  <p>Template Example</p>
</template>

<div id="container"></div>

<script>
  const template = document.getElementById('my-template');
  const clone = document.importNode(template.content, true);
  document.getElementById('container').appendChild(clone);
</script>
```

#### 예제

1. **Custom Elements를 사용하여 간단한 버튼 요소를 만드세요.**

   ```javascript
   class MyButton extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = `
         <style>
           button {
             background-color: #4CAF50;
             color: white;
             border: none;
             padding: 10px 20px;
             text-align: center;
             text-decoration: none;
             display: inline-block;
             font-size: 16px;
           }
         </style>
         <button><slot></slot></button>
       `;
     }
   }

   customElements.define('my-button', MyButton);
   ```

   ```html
   <my-button>Click Me</my-button>
   ```

2. **Shadow DOM을 사용하여 스타일이 캡슐화된 사용자 정의 요소를 만드세요.**

   ```javascript
   class ShadowElement extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = `
         <style>
           div {
             color: purple;
             border: 1px solid black;
             padding: 10px;
           }
         </style>
         <div>Shadow DOM Content</div>
       `;
     }
   }

   customElements.define('shadow-element', ShadowElement);
   ```

   ```html
   <shadow-element></shadow-element>
   ```

3. **HTML Templates를 사용하여 재사용 가능한 템플릿을 정의하고, 이를 여러 곳에 삽입하세요.**

   ```html
   <template id="item-template">
     <style>
       .item {
         color: blue;
         border: 1px solid gray;
         padding: 5px;
       }
     </style>
     <div class="item">Template Item</div>
   </template>

   <div id="list"></div>

   <script>
     const template = document.getElementById('item-template');
     const list = document.getElementById('list');

     for (let i = 0; i < 3; i++) {
       const clone = document.importNode(template.content, true);
       list.appendChild(clone);
     }
   </script>
   ```

#### 연습문제와 해답

1. **Custom Elements를 사용하여 'user-card'라는 요소를 정의하고, 사용자 정보를 표시하세요.**

   ```javascript
   class UserCard extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = `
         <style>
           .card {
             border: 1px solid #ccc;
             padding: 10px;
             border-radius: 5px;
             box-shadow: 2px 2px 12px #aaa;
           }
           .name {
             font-size: 1.2em;
             font-weight: bold;
           }
         </style>
         <div class="card">
           <div class="name">${this.getAttribute('name')}</div>
           <div class="email">${this.getAttribute('email')}</div>
         </div>
       `;
     }
   }

   customElements.define('user-card', UserCard);
   ```

   ```html
   <user-card name="John Doe" email="john.doe@example.com"></user-card>
   ```

2. **Shadow DOM을 사용하여 스타일이 격리된 'profile-card' 요소를 만드세요.**

   ```javascript
   class ProfileCard extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = `
         <style>
           .profile {
             border: 1px solid #000;
             padding: 15px;
             border-radius: 8px;
             background-color: #f9f9f9;
           }
           .name {
             color: #333;
             font-size: 1.5em;
           }
           .bio {
             color: #666;
           }
         </style>
         <div class="profile">
           <div class="name">${this.getAttribute('name')}</div>
           <div class="bio">${this.getAttribute('bio')}</div>
         </div>
       `;
     }
   }

   customElements.define('profile-card', ProfileCard);
   ```

   ```html
   <profile-card name="Jane Doe" bio="Web Developer and Designer"></profile-card>
   ```

3. **HTML Templates를 사용하여 'product-item' 템플릿을 정의하고, 이를 여러 번 사용하세요.**

   ```html
   <template id="product-template">
     <style>
       .product {
         border: 1px solid #ccc;
         padding: 10px;
         border-radius: 5px;
         margin: 5px 0;
       }
       .title {
         font-size: 1.2em;
         font-weight: bold;
       }
       .price {
         color: #b12704;
       }
     </style>
     <div class="product">
       <div class="title">Product Title</div>
       <div class="price">$0.00</div>
     </div>
   </template>

   <div id="product-list"></div>

   <script>
     const template = document.getElementById('product-template');
     const productList = document.getElementById('product-list');

     const products = [
       { title: 'Product 1', price: '$10.00' },
       { title: 'Product 2', price: '$20.00' },
       { title: 'Product 3', price: '$30.00' },
     ];

     products.forEach(product => {
       const clone = document.importNode(template.content, true);
       clone.querySelector('.title').textContent = product.title;
       clone.querySelector('.price').textContent = product.price;
       productList.appendChild(clone);
     });
   </script>
   ```
