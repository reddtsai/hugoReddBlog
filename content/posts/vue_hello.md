---
title: "初次見面 Vue"
date: 2020-05-08T14:36:28+08:00
description: 建立 Vue 開發環境
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Vue
categories:
- Vue
---

如果你跟我一樣習慣用 MVC 或是剛踏入前端的新手，需要一個 Front-end frameworks，請先參考下圖。
![Alt text](/images/frontend_frameworks_ranking.png)
![Alt text](/images/frontend_frameworks_overview.png)
假設你跟我一樣選擇 Vue.js，可以參考以下步驟來幫助你建立 Vue.js 的開發環境。

### Installation

以下是為 Mac 安裝 Vue 開發環境的步驟。
1. 安裝 Node.js
    ``` bash
    brew install node
    ```
2. 安裝 Vue.js
    ``` bash
    npm install vue
    ```
2. 安裝 Vue CLI
    ``` bash
    npm install -g @vue/cli
    ```

{{< alert theme="info" >}}
如果你不是要建立 Vue 專案，你也可以直接將 Vue.js 加入既有網頁中使用
```<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>```
{{< /alert >}}

### Creating Vue Project

環境準備就緒後，就可透過 Vue CLI 來建立一個 Vue Project。
``` bash
vue create hello-vue
```
接著會詢問專案設定，你可選用你慣用的功能項目。
![Alt text](/images/vue_create_option.png)
你也可以使用 Vue CLI UI 介面來建立專案。
``` bash
vue ui
```
![Alt text](/images/vue_cli_ui.png)
試著執行剛建好的專案 hello-vue
``` bash
cd hello-vue
npm run serve
```
可以看到 Vue 歡迎畫面(http://localhost:8080/)
![Alt text](/images/vue.png)

### Reference

[Vue.js](https://vuejs.org)