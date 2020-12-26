---
title: 使用 Web Component 做一個 Uber eats 卡片元件
tags: [web-component]
categories: 
- [web-component, Basic Usage]
---

## 起源？
## 好處？壞處？
## 基本使用方式？

創建 web-component 主要有三種方式：
1. Custom Elements
2. Shadow DOM
3. HTML Templates

### Custom Elements
```javascript javascript
class RestaurantCard extends HTMLElement {
  constructor() {
    super()
    //
  }
}

window.customElements.define('restaurant-card' RestaurantCard)
```
```html html
<html>
  <body>
    <restaurant-card></restaurant-card>
  </body>
</html>
```

但目前這樣，畫面上應該是一片空白，我們並沒有給予這個元件任何元素！所以最簡單方式，就是直接在 super() 方法底下加上：
```javascript javascript
class RestaurantCard extends HTMLElement {
  constructor() {
    super()
    this.innerHTML = `
    <style>
    .restaurant-card {
      color: red;
    }
    </style>
    <div class="restaurant-card">Hello John Doe.</div>
    `
  }
}
```

然後我們要知道在創建這個元件的 class 裡面提供了哪些可以使用的 methods，這些方法在我們深入研究後就會碰到。
- constructor
- connectedCallback
- disconnectedCallback
- attributedChangedCallback(attributeName, oldValue, newValue)

## Shadow DOM && HTML Templates
好處：可以在一個 HTML tag 下裝上一個 shadow DOM，這個 DOM 的 style 可以是 scoped style，也就是不會受全局影響。

做法即是：

```javascript javascript
const template = document.createElement('template')
template.innerHTML = `
  <style>
    h3 {
      color: coral;
    }
  </style>
  <div class="my-custom-web-component">
    <h3>${this}</h3>
  </div>
`
class RestaurantCard extends HTMLElement {
  constructor() {
    super()
    this.attachShadow({mode:'open'})
    // 為什麼要複製一份？？
    this.shadowRoot.appendChild(template.content.cloneNode(true))
  }
}
```

注意：上面的 template 字串模板裡面的 this 是取用不到的，因為超出 class 之外。所以我們要在 class 裡做些設定

```javascript javascript
class RestaurantCard extends HTMLElement {
  constructor() {
    super()
    this.attachShadow({mode:'open'})
    this.shadowRoot.appendChild(template.content.cloneNode(true))
    // 加上這行，我們就可以在 custom tag 上加上 name 屬性，這樣一來，就會把 h3 變成 name
    this.shadowRoot.querySelector('h3').innerText = this.getAttribute('name')
  }
}
```
因此，html 這邊要這樣改：

```html html
<html>
  <body>
    <my-custom-web-component name="John Doe"></my-custom-web-component>
  </body>
</html>
```
