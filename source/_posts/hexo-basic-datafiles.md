---
title: Data Files
tags: [blog-tools]
categories: 
- [翻譯, Hexo, Basic Usage]
---

有時你可能需要在模板內使用某些沒有辦法直接在你的發文內取得的資料，或者你想要在其它地方複用。在那樣的情況下，Hexo 3 介紹了一種新的資料資料夾。這個功能在 `source/_data` 資料夾讀取 YAML 或 JSON 檔案，這樣你可以在你的網站裡使用它們。

舉裡來說，在 `source/_data` 下新增一個 `menu.yml` 檔案。

```yml menu.yml
Home: /
Gallery: /gallery/
Archives: /archives/
```

然後你可以在模板使用它們

``` 
<% for (var link in site.data.menu) { %>
  <a href="<%= site.data.menu[link] %>"> <%= link %> </a>
<% } %>
```

渲然成這樣：

``` html
<a href="/"> Home </a>
<a href="/gallery/"> Gallery </a>
<a href="/archives/"> Archives </a>
```