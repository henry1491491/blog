---
title: Writing
tags: [blog-tools]
categories: 
- [翻譯, Hexo, Basic Usage]
---

你可以使用以下指令來創建或發佈一個頁面：

``` bash
$ hexo new [layout] <title>
```
`post` 在是預設的佈局（layout），但是你可以提供自己的。透過編輯檔案 `_config.yml` 裡的 `default_layout`，你可以改變預設佈局。

### 佈局

在 Hexo 裡有三種預設佈局：`post`、`page`以及`draft`。透過其中任一種方式創建一個檔案將會儲存在不同路徑。新創建的發佈會被存入 `source/_posts` 這個資料夾。

禁用佈局，參考[這篇](https://hexo.io/docs/front-matter#Layout)

### 檔名

Hexo 預設會用發佈的標題作為檔名。你可以在 `_config.yml` 去修改 `new_post_name` 的設定，去更換預設檔名，例如：`:year-:month-:day-:title.md` 將會在檔名前加上發佈創建日期作為前綴。你也可以使用以下佔位符

```
:title 
:year
:month
:i_month 個位數，不會補 0
:day
:i_day 個位數，不會補 0
```

## 草稿

在這之前我們提到過一個在 Hexo 裡一個特別的佈局：草稿。所有在這個佈局裡的發佈都會被存入 `source/_drafts` 這個資料夾。你可以透過 `publish` 指令去移動草稿到 `source/_posts` 這個資料夾。它跟 `new` 指令有相似的作用。

```bash
$ hexo publish [layout] <title>
```

草稿預設不會顯示。你可以在執行 hexo 命令時加上 `--draft` 的設定，或是啟用在 `_config.yml` 的 `render_drafts` 設定去渲染草稿 

## 腳手架

當你要創建發佈時，Hexo 將會基於在 `scaffolds` 檔案夾裡相關的檔案去建立檔案。舉例來說：

``` bash
$ hexo new photo "My Gallery"
```

當你下這個指令，Hexo 會試著在 `scaffolds` 資料夾找出 `photo.md` 且基於它來建立發佈。以下佔位符是可以使用的腳手架。

```
layout 
title
date
```

## 支援的各式

Hexo 支援任何格式撰寫文章，只要相關的渲染套件有被安裝。

舉裡來說，Hexo 預設有 `hexo-renderer-marked` 及 `hexo-renderer-ejs`。所以你可以透過 `markdown` 或是 `ejs` 語法來撰寫你的文章。假如你安裝了 `hexo-renderer-pug`，你將能夠透過 `pug` 模板語法來撰寫你的文章。

你可以重新命名你的文章且轉換檔名至 `.ejs`，然後 Hexo 會使用 `hexo-renderer-ejs` 來渲染那個檔案，其他格式同樣也行。
