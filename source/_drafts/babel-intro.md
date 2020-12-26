---
title: Babel 介紹及一些使用上整理
tags: [Babel]
categories: 
- [Babel]
---

## 前言

最近想深入了解 Webpack 的打包原理，看到網路上一篇自己做出一個  Webpack 的教學，就跟著實際 code 一遍。然而發現需要對 Babel 有一定程度的瞭解，於是先來熟悉一下 Babel

以下是閱讀完[官方文件](https://babeljs.io/)後的部分

babel 可以幹嘛？
- 轉換語法（這意謂著你可以寫自己的 JavaScript 框架，透過 babel 轉換成瀏覽器看得懂的語言）
- 支援那些你的目標使用瀏覽器不支援的功能（可以讓你使用最新潮的 JavaScript 語法）
- 原始碼轉換（codemods，這部分與更有效率地重構大型應用有關，詳細說明可以參考別人寫的[這篇](https://www.freshworks.com/company/codemods-the-new-age-saviors-for-js-developers-blog/)文章）

Babel 是一個將新版本 JavaScript 程式碼轉換成舊版本 JavaScript 程式碼的編譯器。

假設今天開發上我想使用 ES6 版本的功能，或是現在最新潮的 ES12，但瀏覽器卻只支援到 ES5，那麼就需要使用 Babel 將瀏覽器還尚未支援的語法轉換成瀏覽器看得懂的語法。

## Core Library

所有需要的 babel 相關模組都在 `@babel` 資料夾底下（從版本 7 之後，是 `@babel`，而不是 `babel`）

```shell shell
npm install --save-dev @babel/core
```
在 JavaScript 程式中使用：

```javascript javascript
const babel = require("@babel/core");

babel.transform("code", optionsObject);
```
針對一些終端用戶（我猜這裡意思是，不需要自己去寫 babel 規則的用戶，比方說一些前端框架已經幫你設定好 babel 的情況下），你也可以使用其他工具作為 `@babel/core` 的接口，即便需要針對 babel 去做一些設定，這些工具也提供設定的方法。



## CLI tool 命令列工具

你也可以在命令行使用 `@babel/cli`，安裝方法：

```shell shell
npm install --save-dev @babel/core @babel/cli

./node_modules/.bin/babel src --out-dir lib
```
這指令會將你存放在 `src` 資料夾內的檔案，經過轉換（如果你有設定轉換規則），產出到 `lib` 資料夾內。因為我們沒有給予任何轉換規則，產出的程式碼會跟給予的程式碼ㄧ樣，除了那些程式碼風格

你也可以透過 `--help` 去看這個 cli 命令有什麼功能，當然現在最重要的是 `--plugins` 跟 `--presets`

## Plugins & Presets 套件及預設

轉換程式碼都是透過套件，這些套件是一小段  JavaScript 的程式碼。這些程式碼給予 babel ㄧ些指示，告訴它該如何轉換程式碼。因此，你也可以寫自己的套件，去透過 babel 轉換你的 code。假設我們要將 ES6 以上的語法轉換成 ES5，透過官方套件 `@babel/plugin-transform-arrow-functions` 就可以做到：

```shell shell
npm install --save-dev @babel/plugin-transform-arrow-functions

./node_modules/.bin/babel src --out-dir lib --plugins=@babel/plugin-transform-arrow-functions
```

然後你的箭頭函式就會轉換成以下形式：

```javascript javascript
const fn = () => 1;

// converted to

var fn = function fn() {
  return 1;
};
```

這是個好的開始，但在 ES6 裡，我們還想轉換其他新的語法，除了一個一個將套件引入，我們還可以使用 preset，中文翻譯為「預設」，顧名思義，它是一個預先定義好的許多套件的集合。也可以說是一套由許多套件組成的規則。這邊例子使用的是一個很不錯的 preset，稱為 `env`

```shell shell
npm install --save-dev @babel/preset-env

./node_modules/.bin/babel src --out-dir lib --presets=@babel/env
```
不需要任何設定，preset 就會引入所有支援現代 JavaScript(ES6, ES7...)語法所需要的套件。但也可以在使用 preset 的同時，做一些設定。比起在終端機同時使用 cli 以及 preset 的設定，我們還有另一種設定方式：設定檔。

## Configuration 設定檔

> 有很多種針對你的需求去使用設定檔的方式。確保看過如何深入地去[設定 Babel](https://babeljs.io/docs/en/configuration) 來獲取更多資訊

現在我們創建一個包含以下內容名為 `babel.config.json` 的檔案（需要 v7.8.0 及以上版本）

```json json
{
"presets": [
  [
  "@babel/env",
    {
      "targets": {
        "edge": "17",
        "firefox": "60",
        "chrome": "67",
        "safari": "11.1"
        }
      }
    ]
  ]
}
```
現在 `env` 的 preset將會只轉換這些瀏覽器中不提供功能的轉換套件

## Polyfill(實現瀏覽器不支援的 api 功能)

> Babel 7.4.0 以上，不推薦使用此軟件包，而是直接引入 `core-js/stable`（實現 ECMAScript features 功能）以及 `regenerator-runtime/runtime`（轉換 generator 函式需要）
```javascript javascript
import "core-js/stable";
import "regenerator-runtime/runtime";
```
[@babel/polyfill](https://babeljs.io/docs/en/babel-polyfill) 模塊引入 `core-js`及一個客製化的 [regenerator runtime](https://github.com/facebook/regenerator/blob/master/packages/regenerator-runtime/runtime.js)來模擬一個完全的 ES6 以上環境。

這意謂著，你可以使用新的內建的像是 `Promise` or `WeakMap`,


## 額外補充

Compiling 通常包含三個階段：
1. parsing
2. transforming
3. printing

- 可以將新版本 JavaScript 轉換為舊版本
- 可以透過 ast 抽象語法樹解析 code，改變 node、產出新的 code
- 它是一個 transpiler !!，要說 compiler 也行，因為 transpiler 是 compiler 的 subset。想了解兩者之間差異，可以參考[Compiling vs Transpiling](https://stackoverflow.com/questions/44931479/compiling-vs-transpiling) 
