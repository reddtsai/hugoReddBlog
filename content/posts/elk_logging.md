---
title: "如何利用 Elastic 處理日誌"
date: 2019-07-11T18:57:13+08:00
toc: true
tags: ["ELK"]
---

<!--more-->
## Elastic Logs

當你面對數個虛擬機、資料庫、成百的服務、應用程式生成的日誌，你需要一個能處理這巨量負雜日誌的產品，這裡介紹 Elastic 這個產品來協助你，必免自己造輪子。將由下圖的內容來說明，如何使用 Elastic 處理日誌你的日誌。

![Alt text](/images/elastic.png)

### 日誌收集

首先是如何收集匯整你的日誌？例如你有 IIS、NGINX、MySQL、APP 多種不同類型的日誌，你需要將它們收集起來存放。

![Alt text](/images/beats.svg) FileBeat

![Alt text](/images/logstash.svg) Logstash

### 日誌分析

### 日誌監測
