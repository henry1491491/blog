---
title: Asset Folders
tags: [blog-tools]
categories: 
- [翻譯, Hexo, Basic Usage]
---

## 全域的資產資料夾

在 `source` 資料夾內，不屬於發佈文章類型的檔案稱為 Assets。假如你只是要放幾張照片在一個 Hexo 專案裡，那麼最簡單的方式就是將它們放入 `source/images` 資料夾。然後，你可以透過 `![](/images/image.jpg)` 像是這樣的方式存取到它們。

## 發佈資產資料夾

針對那些預期會經常提供照片或其他資料，或是偏好將他們的資產以「一篇發文對應一個資料夾」的形式存放的使用者們，Hexo 也提供一個更有組織的方式來管理這些資產。這涉及到更多，但資產處理可透過將 `_config.yml` 裡 `post_asset_folder` 的設定為 `true` 被非常輕易地啟動。

隨著啟用資產資料夾管理，Hexo 將會在每次你透過 `hexo new [layout] <title>` 指令創建新發文時，建立一個資料夾。這個資料夾名稱將會基於這個發文所產生與發文的 markdown 檔案一樣的名稱。將所有相關於這篇發文的資產放進去相對應的資料夾，且你能夠透過相對路徑的方式去引用它們，創建一個更加容易且便利的工作流。

## 針對相對路徑飲用的標記套件

透過一般的 markdown 語法及相對路徑來引用相片或是其他資產可能導致無法正確顯示在 archives 或是 index 頁面。在 Hexo 2，在社群裡有套件被發明用來解決這個問題。然而，僅有少數新的標記套件被添加到 Hexo 3 的發佈。以下在發文中引用你的資產的方式將會更簡單：

``` markdown
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```

舉例來說，在啟用資產資料夾的情況下，你放了一張叫做 `example.jpg` 的相片進入資產資料夾。假如你透過 `![](example.jpg)` 的 markdown 語法，使用相對路徑來引用它，這將不會在 index 頁面顯示。（然而用在發文本身是預期可以成功的）。

因此針對引用相片更為正確的用法將會是透過標記套件語法，而非 markdown。

```markdown
{% asset_img example.jpg This is an example image %}
{% asset_img "spaced asset.jpg" "spaced title" %}
```

這個方法將能同時顯示照片於 index 及 archive 頁面。

## 透過 markdown 嵌入相片

hexo-renderer-marked 3.1.0 介紹了一個不需使用 `asset_img` 標記套件的新設置。

啟用：
```markdown _config.yml
post_asset_folder: true
marked:
  prependRoot: true
  postAsset: true
```
一旦啟用，一張資產相片將會自動被解析到相對應的發文路徑。舉例來說：“image.jpg” 位於 “/2020/01/02/foo/image.jpg”, 意味著它是一個位在 “/2020/01/02/foo/“ 路徑發文的資產相片，`![](image.jpg)` 會被渲染為 `<img src="/2020/01/02/foo/image.jpg">`。