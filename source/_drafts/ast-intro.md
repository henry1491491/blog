---
title: 什麼是 AST？為什麼需要它？
tags: [Babel]
categories: 
- [AST]
---

AST(abstract syntax tree)，抽象語法樹。是一種程式語言語法結構的樹狀表示方式

假設以下程式碼

```javascript javascript
const greet = 'Hello AST' 
```

它的 AST，就表示為（不是一定為 json，只是為了方便看）以下這種形式

```json json
{
  "type": "Program",
  "start": 0,
  "end": 27,
  "body": [
    {
      "type": "VariableDeclaration",
      "start": 0,
      "end": 25,
      "declarations": [
        {
          "type": "VariableDeclarator",
          "start": 6,
          "end": 25,
          "id": {
            "type": "Identifier",
            "start": 6,
            "end": 11,
            "name": "greet"
          },
          "init": {
            "type": "Literal",
            "start": 14,
            "end": 25,
            "value": "Hello AST",
            "raw": "'Hello AST'"
          }
        }
      ],
      "kind": "const"
    }
  ],
  "sourceType": "module"
}
```
每個 Node，都會有一個 type，表示這個節點的類型，這個節點可以是：一段程式碼（program）、一個宣告（VariableDeclaration）、標示符（Identifier）⋯⋯等等

為什麼我們要學這個？

因為可以操控 js

怎麼操控？

先讓我們了解一下 compiler。

一定會有 Input / Output

// Input
- Parse 
  - [astexplorer.net](https://astexplorer.net/)
  - 或是其他 parsing tools
- Traverse
// Output
- Manipulate
- Generate code