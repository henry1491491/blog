---
title: GitHub Pages
tags: [blog-tools]
categories: 
- [翻譯, Hexo, Deployment]
---

在這個教學，我們使用 [GitHub Actions](https://docs.github.com/en/actions) 來部署 Github Pages。這皆適用於公開及私人的程式碼倉庫。假如你偏好不要上傳 source 資料夾到 GitHub，則你可以跳到 [One-command deployment](https://hexo.io/docs/github-pages#One-command-deployment) 的章節。

1. 創建一個項目名為 **username.github.io**，username 就是你的 GitHub 使用者帳號。假如你已經上傳其他項目到這個倉庫，你可以重新命名它。

2.加入以下幾行到 `package.json`（假如你的檔案已經有了，可以跳過）

```json
{
  "scripts": {
    "build": "hexo generate"
  },
  "hexo": {
    "version": "5.0.0"
  },
  "dependencies": {
    "hexo": "^5.0.0",
    ...
  }
}
```
3. 將你的 Hexo 資料夾以 source 分支推至你的遠端倉庫， `public/` 資料夾預設不會（且不應該）被上傳，確定 `.gitignore` 檔案內有包含 `public/` 這行。該資料夾結構應該約略相似於[這個](https://github.com/hexojs/hexo-starter)專案，不包含 `.gitmodules` 這個檔案。

- 將你的 source 推到 GitHub 上：

``` bash
$ git push origin source
```

4. 加上 `.github/workflows/pages.yml` 這個檔案包含以下內容到你的專案

```yml .github/workflows/pages.yml
name: Pages

on:
  push:
    branches:
      - source  # default branch

jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 12.x
        uses: actions/setup-node@v1
        with:
          node-version: '12.x'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: master  # deploying branch
```
5. 一旦部署完成，產生出的頁面可以在你的倉庫的 master 分支中被找到

6. 在你的 GitHub 倉庫的設置，到 “GitHub Pages” 的區塊，將來源改為 master 分支。

7. 檢查 username.github.io 這個頁面


- 注意：假如你透過 CNAME 指定一個客製化的網域名，你必須將 CNAME 檔案添加到 `source/` 資料夾。


## Project page

假如你偏好在 GitHub 上有一個專案頁面：

1. 在 GitHub上，找到你的倉庫。到設定頁籤。改變倉庫名稱，因此你的部落格現在可以是 `username.github.io/repository` 這樣的名稱。repository 可以是任何名稱，像是 blog 或是 hexo。

2. 編輯你的 `_config.yml` 檔案，改變一下 root:value 為 `/<repository>/`（必須開始與結束都是 `/` 符號，不需要括號）。

3. 在 `.github/workflows/pages.yml` 改變以下幾行：

```yml .github/workflows/pages.yml
name: Pages

on:
  push:
    branches:
-      - source  # default branch
+      - master

jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
-          publish_branch: master  # deploying branch
+          publish_branch: gh-pages
```

4. 提交且推送到 master 分支。

- 將 master 分支推送到遠端：

``` bash
$ git push origin master
```

5. 一旦完成提交，產生的頁面可以在你的倉庫的 `gh-pages` 分支被找到

6. 在你的倉庫的設定，到 GitHub Pages 區塊，改變來源為 `gh-pages` 分支。

## 一條指令部署

以下指示適用於[one-command deployment](https://hexo.io/docs/one-command-deployment)頁面

1. 下載 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)
2. 在 `_config.yml` 加上以下設定（去除已經有的幾行）

``` yml
deploy:
  type: git
  repo: https://github.com/<username>/<project>
  # example, https://github.com/hexojs/hexojs.github.io
  branch: gh-pages
```
3. 執行 `hexo clean` 與 `hexo deploy`
4. 檢查 username.github.io 頁面

## 幾個有用的連結

[GitHub Pages](https://help.github.com/categories/github-pages-basics/)
[Travis CI Docs](https://docs.travis-ci.com/user/tutorial/)
[peaceiris/actions-gh-pages](https://github.com/marketplace/actions/github-pages-action)