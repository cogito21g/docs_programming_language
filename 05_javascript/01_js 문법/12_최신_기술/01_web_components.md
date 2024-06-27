### 12. 최신 웹 기술

#### 12.1. Web Components

Web Components는 재사용 가능한 커스텀 요소를 만들기 위한 기술로, HTML, CSS, JavaScript를 조합하여 독립적인 웹 컴포넌트를 작성할 수 있습니다. Web Components는 세 가지 주요 기술로 구성됩니다:

1. **Custom Elements**: 사용자 정의 HTML 요소를 만들고 사용합니다.
2. **Shadow DOM**: 캡슐화된 DOM 트리를 만들어 스타일과 스크립트를 격리합니다.
3. **HTML Templates**: 템플릿을 정의하고, 클론하여 사용할 수 있는 기능을 제공합니다.

##### 12.1.1. Custom Elements

Custom Elements는 새로운 HTML 요소를 정의하고, 기존 요소를 확장하여 사용자 정의 동작을 추가할 수 있습니다.

###### Custom Element 정의

Custom Element는 JavaScript 클래스를 사용하여 정의합니다. 이 클래스는 HTMLElement를 상속받아야 합니다.

```javascript
class MyElement extends HTMLElement {
  constructor() {
    super();
    // 요소 초기화
    this.innerHTML = '<p>Hello, World!</p>';
  }
}

// 사용자 정의 요소를 등록
customElements.define('my-element', MyElement);
```

이제 `my-element` 커스텀 요소를 HTML 문서에서 사용할 수 있습니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Custom Elements Example</title>
</head>
<body>
  <my-element></my-element>
  <script src="app.js"></script>
</body>
</html>
```

###### 라이프사이클 콜백

Custom Elements는 요소의 라이프사이클 동안 호출되는 콜백 메서드를 제공합니다:

- `connectedCallback()`: 요소가 DOM에 추가될 때 호출됩니다.
- `disconnectedCallback()`: 요소가 DOM에서 제거될 때 호출됩니다.
- `adoptedCallback()`: 요소가 새로운 문서로 이동될 때 호출됩니다.
- `attributeChangedCallback(name, oldValue, newValue)`: 요소의 속성이 추가, 제거 또는 변경될 때 호출됩니다.

예제:

```javascript
class MyElement extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = '<p>Hello, World!</p>';
  }

  connectedCallback() {
    console.log('Element added to the DOM');
  }

  disconnectedCallback() {
    console.log('Element removed from the DOM');
  }

  attributeChangedCallback(name, oldValue, newValue) {
    console.log(`Attribute: ${name} changed from ${oldValue} to ${newValue}`);
  }

  static get observedAttributes() {
    return ['data-attribute'];
  }
}

customElements.define('my-element', MyElement);
```

##### 연습문제와 해답

1. **간단한 Custom Element를 정의하고 HTML에서 사용하세요.**

   ```javascript
   class MyButton extends HTMLElement {
     constructor() {
       super();
       this.innerHTML = '<button>Click Me</button>';
     }
   }

   customElements.define('my-button', MyButton);
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Custom Button Example</title>
   </head>
   <body>
     <my-button></my-button>
     <script src="app.js"></script>
   </body>
   </html>
   ```

2. **Custom Element에서 라이프사이클 콜백을 구현하세요.**

   ```javascript
   class MyElement extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = '<p>Hello, World!</p>';
     }

     connectedCallback() {
       console.log('Element added to the DOM');
     }

     disconnectedCallback() {
       console.log('Element removed from the DOM');
     }

     attributeChangedCallback(name, oldValue, newValue) {
       console.log(`Attribute: ${name} changed from ${oldValue} to ${newValue}`);
     }

     static get observedAttributes() {
       return ['data-attribute'];
     }
   }

   customElements.define('my-element', MyElement);
   ```

3. **Custom Element에서 속성 변화를 감지하세요.**

   ```javascript
   class MyElement extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = '<p>Hello, World!</p>';
     }

     attributeChangedCallback(name, oldValue, newValue) {
       console.log(`Attribute: ${name} changed from ${oldValue} to ${newValue}`);
     }

     static get observedAttributes() {
       return ['data-attribute'];
     }
   }

   customElements.define('my-element', MyElement);

   // HTML
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Attribute Change Example</title>
   </head>
   <body>
     <my-element data-attribute="initial value"></my-element>
     <script>
       const myElement = document.querySelector('my-element');
       setTimeout(() => {
         myElement.setAttribute('data-attribute', 'new value');
       }, 2000);
     </script>
     <script src="app.js"></script>
   </body>
   </html>
   ```

---

### 12. 최신 웹 기술

#### 12.1. Web Components

##### 12.1.2. Shadow DOM

Shadow DOM은 DOM 트리의 일부를 캡슐화하여 외부 스타일과 스크립트의 영향을 받지 않도록 합니다. 이를 통해 컴포넌트의 내부 구현을 보호하고, 전역 네임스페이스 오염을 방지할 수 있습니다.

###### Shadow DOM 생성

Shadow DOM을 사용하려면 `attachShadow` 메서드를 사용하여 요소에 Shadow Root를 생성합니다. Shadow Root는 `mode` 옵션을 받아 `open` 또는 `closed`로 설정할 수 있습니다.

```javascript
class MyElement extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = '<p>Shadow DOM Content</p>';
  }
}

customElements.define('my-element', MyElement);
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Shadow DOM Example</title>
</head>
<body>
  <my-element></my-element>
  <script src="app.js"></script>
</body>
</html>
```

###### Shadow DOM 스타일링

Shadow DOM 내부의 스타일은 외부 스타일과 독립적으로 적용됩니다. Shadow DOM 내부에 `<style>` 태그를 사용하여 스타일을 정의할 수 있습니다.

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
      <p>Shadow DOM Content</p>
    `;
  }
}

customElements.define('my-element', MyElement);
```

###### Slot 요소

`<slot>` 요소를 사용하여 Shadow DOM 내부에 외부 콘텐츠를 삽입할 수 있습니다. 기본 슬롯과 이름 슬롯을 사용할 수 있습니다.

```javascript
class MyElement extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = `
      <style>
        ::slotted(p) {
          color: red;
        }
      </style>
      <slot></slot>
    `;
  }
}

customElements.define('my-element', MyElement);
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Slot Example</title>
</head>
<body>
  <my-element>
    <p>Light DOM Content</p>
  </my-element>
  <script src="app.js"></script>
</body>
</html>
```

###### 이름 슬롯

이름 슬롯을 사용하여 특정 콘텐츠를 지정된 슬롯에 삽입할 수 있습니다.

```javascript
class MyElement extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = `
      <style>
        ::slotted([slot="content"]) {
          color: green;
        }
      </style>
      <slot name="content"></slot>
    `;
  }
}

customElements.define('my-element', MyElement);
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Name Slot Example</title>
</head>
<body>
  <my-element>
    <p slot="content">Named Slot Content</p>
  </my-element>
  <script src="app.js"></script>
</body>
</html>
```

##### 연습문제와 해답

1. **Shadow DOM을 사용하여 외부 스타일과 독립된 콘텐츠를 생성하세요.**

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
         <p>Shadow DOM Content</p>
       `;
     }
   }

   customElements.define('my-element', MyElement);
   ```

2. **Slot 요소를 사용하여 외부 콘텐츠를 Shadow DOM 내부에 삽입하세요.**

   ```javascript
   class MyElement extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = `
         <style>
           ::slotted(p) {
             color: red;
           }
         </style>
         <slot></slot>
       `;
     }
   }

   customElements.define('my-element', MyElement);
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Slot Example</title>
   </head>
   <body>
     <my-element>
       <p>Light DOM Content</p>
     </my-element>
     <script src="app.js"></script>
   </body>
   </html>
   ```

3. **이름 슬롯을 사용하여 특정 콘텐츠를 지정된 슬롯에 삽입하세요.**

   ```javascript
   class MyElement extends HTMLElement {
     constructor() {
       super();
       this.attachShadow({ mode: 'open' });
       this.shadowRoot.innerHTML = `
         <style>
           ::slotted([slot="content"]) {
             color: green;
           }
         </style>
         <slot name="content"></slot>
       `;
     }
   }

   customElements.define('my-element', MyElement);
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Name Slot Example</title>
   </head>
   <body>
     <my-element>
       <p slot="content">Named Slot Content</p>
     </my-element>
     <script src="app.js"></script>
   </body>
   </html>
   ```

---

### 12. 최신 웹 기술

#### 12.1. Web Components

##### 12.1.3. HTML Templates

HTML Templates는 미리 정의된 HTML 구조를 템플릿으로 작성하고, 필요할 때 이 템플릿을 복제하여 DOM에 추가할 수 있는 기능을 제공합니다. 템플릿을 사용하면 반복적인 DOM 생성 작업을 쉽게 처리할 수 있습니다.

###### HTML Template 기본 구조

HTML 템플릿은 `<template>` 태그를 사용하여 정의합니다. `<template>` 태그 내부의 콘텐츠는 렌더링되지 않으며, JavaScript를 통해 DOM에 추가할 수 있습니다.

```html
<template id="my-template">
  <style>
    .my-element {
      color: blue;
    }
  </style>
  <div class="my-element">Hello, Template!</div>
</template>
```

###### 템플릿 사용

JavaScript를 사용하여 템플릿 콘텐츠를 복제하고, DOM에 추가합니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HTML Template Example</title>
</head>
<body>
  <template id="my-template">
    <style>
      .my-element {
        color: blue;
      }
    </style>
    <div class="my-element">Hello, Template!</div>
  </template>

  <div id="container"></div>

  <script>
    const template = document.getElementById('my-template');
    const container = document.getElementById('container');

    // 템플릿 콘텐츠 복제
    const clone = document.importNode(template.content, true);
    container.appendChild(clone);
  </script>
</body>
</html>
```

###### 반복적인 요소 생성

템플릿을 사용하여 반복적인 요소를 쉽게 생성할 수 있습니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HTML Template Example</title>
</head>
<body>
  <template id="item-template">
    <li class="item">Item</li>
  </template>

  <ul id="item-list"></ul>

  <script>
    const template = document.getElementById('item-template');
    const itemList = document.getElementById('item-list');

    for (let i = 1; i <= 5; i++) {
      const clone = document.importNode(template.content, true);
      clone.querySelector('.item').textContent = `Item ${i}`;
      itemList.appendChild(clone);
    }
  </script>
</body>
</html>
```

##### 연습문제와 해답

1. **HTML 템플릿을 정의하고, JavaScript를 사용하여 템플릿 콘텐츠를 DOM에 추가하세요.**

   ```html
   <template id="my-template">
     <style>
       .my-element {
         color: blue;
       }
     </style>
     <div class="my-element">Hello, Template!</div>
   </template>

   <div id="container"></div>

   <script>
     const template = document.getElementById('my-template');
     const container = document.getElementById('container');

     const clone = document.importNode(template.content, true);
     container.appendChild(clone);
   </script>
   ```

2. **템플릿을 사용하여 반복적인 목록 항목을 생성하세요.**

   ```html
   <template id="item-template">
     <li class="item">Item</li>
   </template>

   <ul id="item-list"></ul>

   <script>
     const template = document.getElementById('item-template');
     const itemList = document.getElementById('item-list');

     for (let i = 1; i <= 5; i++) {
       const clone = document.importNode(template.content, true);
       clone.querySelector('.item').textContent = `Item ${i}`;
       itemList.appendChild(clone);
     }
   </script>
   ```

3. **스타일이 포함된 템플릿을 사용하여 콘텐츠를 생성하고 스타일이 제대로 적용되도록 하세요.**

   ```html
   <template id="styled-template">
     <style>
       .styled-element {
         color: red;
       }
     </style>
     <div class="styled-element">Styled Template Content</div>
   </template>

   <div id="styled-container"></div>

   <script>
     const template = document.getElementById('styled-template');
     const container = document.getElementById('styled-container');

     const clone = document.importNode(template.content, true);
     container.appendChild(clone);
   </script>
   ```
