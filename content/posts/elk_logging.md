---
title: "如何利用 Elastic 處理日誌"
date: 2019-07-11T18:57:13+08:00
toc: true
tags: ["elastic"]
---

<!--more-->
## Elastic Logs

當你面對數個虛擬機、資料庫、成百的服務、應用程式生成的日誌，你需要一個能處理這巨量負雜日誌的產品，這裡介紹 Elastic 這個產品來協助你，必免自己造輪子。將由下圖的內容來說明，如何使用 Elastic 處理日誌你的日誌。

![Alt text](/images/elastic.png)

### 日誌收集

首先是如何收集匯整你的日誌？例如你有 IIS、NGINX、MySQL、APP 多種不同類型的日誌，你需要將它們收集起來存放。

<svg xmlns="http://www.w3.org/2000/svg" width="32px" height="32px"><image xlink:href="/images/beats.svg" /></svg> FileBeat 是匯整轉發日誌工具，可使用它內建模組或自訂格式來收集日誌，並將收集來的數據解析發送到 Logstash 或 Elasticsearch 儲存。

[範例 - Filebeat 收集 IIS Logs]:https://reddtsai.github.io/posts/elk_iisfilebeat/

<svg xmlns="http://www.w3.org/2000/svg" width="32px" height="32px"><image xlink:href="/images/logstash.svg" /></svg> Logstash 是接收轉發日誌服務，接收

### 日誌分析

### 日誌監測
