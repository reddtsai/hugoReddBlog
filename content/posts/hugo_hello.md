---
title: 建立免費的個人博客 By HUGO
date: 2020-03-11T13:38:34+08:00
description: 透過HUGO建立Blog
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Hugo
categories:
- Hugo
---

身為一位軟體工程師怎麼可以沒有 Blog，再加上工程師最愛挖洞給自己跳，所以一定要自己架Blog😭😭😭。這裡跟大家介紹個靜態網站生成引擎 HUGO，接著透過 HUGO 來建立一個美美又免費的Blog。

<!--more-->

### About Hugo

[HUGO](https://gohugo.io) 是由 go 所編寫的靜態網站生成器，最初由 Steve Francia 創立，以開源專案方式持續更新。Hugo 提供將資料檔案、i18n 包、組態、布局模板、靜態檔案，以及用 Markdown 編寫的內容，生成一個完整的靜態網站。

### Create a Hugo site

以下說明你可能需要一點程式基楚，這裡會使用 mac os 來作說明。(個人是使用蘋果筆電😅😅)

#### 1.安裝 Hugo

使用 homebrew 安裝 Hugo，在終端機中輸入
``` bash
brew install hugo
```
驗証安裝成功，確認 hugo 版本。
``` bash
hugo version
```

#### 2.產生新網站

透過 new 命令來產生一個新站台，在終端機中操作
```
cd YourWorkSpace
hugo new site myBlog
```

#### 3.美化網站

加入外觀主題到剛建立好的網站，在這挑選一個[外觀主題](https://themes.gohugo.io)。
我用 [Ananke](https://themes.gohugo.io/gohugo-theme-ananke/) 來作說明，在終端機中操作
``` bash
cd myBlog
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```
設定主題
``` bash
echo 'theme = "ananke"' >> config.toml
```

#### 4.加入文章

使用 new 命令來產生一篇文章，在終端機中操作
``` bash
hugo new posts/my-first-post.md
```
你可以用 markdown 編輯內容。

#### 5.啟動網站

啟動你的 Blog
``` bash
hugo server -D
```
打開瀏覽器 http://localhost:1313/

#### 參考來源

謝謝 [HUGO](https://gohugo.io)