---
title: "如何利用 Elastic 處理日誌"
date: 2019-07-11T18:57:13+08:00
toc: true
tags: ["elastic"]
---

<!--more-->
### Elastic Logs

當你面對數個虛擬機、資料庫、成百的服務、應用程式生成的日誌，你需要一個能處理這巨量負雜日誌的產品，這裡介紹 Elastic 這個產品來協助你，必免自己造輪子。將由下圖的內容來說明，如何使用 Elastic 處理日誌你的日誌。

![Alt text](/images/elastic.png)

#### 日誌收集

首先是如何收集匯整你的日誌？例如你有 IIS、NGINX、MySQL、APP 多種不同類型的日誌，你需要將它們收集起來存放，這裡會使用 FileBeat 和 Logstash 來收集。

<svg xmlns="http://www.w3.org/2000/svg" width="64px" height="64px"><image xlink:href="/images/beats.svg" /></svg> FileBeat 是匯整轉發日誌工具，可使用它內建模組或自訂格式來收集日誌，並將收集來的數據解析發送到 Logstash 或 Elasticsearch 儲存。

> 範例[Windows 如何安裝 Filebeat 收集 IIS Logs](https://reddtsai.github.io/posts/elk_iisfilebeat/)

<svg xmlns="http://www.w3.org/2000/svg" width="64px" height="64px"><image xlink:href="/images/logstash.svg" /></svg> Logstash 是接收轉發日誌中介服務，從過濾器接收不同形式的來源，透過建構結構來分析轉換成通用格式，再輸出到 Elasticsearch 或其它儲存設備。

#### 日誌存儲

接下來透過 Elasticsearch 來儲存收集來的資料。

<svg xmlns="http://www.w3.org/2000/svg" width="64px" height="64px"><image xlink:href="/images/elasticsearch.svg" /></svg> Elasticsearch 是一個分散式、RESTful 風格的搜尋和數據分析引擎，它是 Elastic 的核心，很像資料庫提供搜索和儲存。

> 範例[CentOS 7 如何安裝 Elasticsearch](https://reddtsai.github.io/posts/elk_centoselastic)

#### 日誌分析

最後，如何視覺化、分享這些數據，Elastc 提供 Kibana UI 來協助你分析。

<svg xmlns="http://www.w3.org/2000/svg" width="64px" height="64px"><image xlink:href="/images/kibana.svg" /></svg> Kibana 是個強大的 UI，透過網頁呈現，提供多種儀表皮和圖表來分享數據。

> 範例[CentOS 7 如何安裝 Kibana](https://reddtsai.github.io/posts/elk_centoskibana/)
