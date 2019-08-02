---
title: "Elastic 簡介"
date: 2019-07-11T19:01:43+08:00
toc: true
tags: ["elastic"]
---

<!--more-->

* * * *

### 歷史

* * * *

Elastic 是由 Elasticsearch、Logstach 和 Kibana 這三個開源項目所組成，最初是由 Shay 編寫的 Elasticsearch 這個食譜搜尋引擎開始，後來與 Jordan 開發的 Logstach 和 Rashid 開發的 Kibana 共同合作。隨後推出一些商業服務並成立 Elastic 這個品牌。

![Alt text](/images/elk-stack.svg)

### Elasticsearch

* * * *

Elasticsearch 是個搜尋分析尋引擎，能夠集中儲存數據和分散運作，使用 RESTful 方式提供服務。

### Logstach

* * * *

Logstach 是個數據處理工具，將不同來源的資料轉化、解析或加密，然後將數據發送至指定的儲存體。

### Kibana

* * * *

Kibana 為 Elastic 的介面(UI)，可用來管理配置 Elastic，或透過 DSL、SQL 查詢語法，將 Elasticsearch 數據分享。/

> 文章參考來源  
> [Elastic 官網](https://www.elastic.co/cn/)

下篇文章會介紹[如何利用 Elastic 處理日誌](https://reddtsai.github.io/posts/elk_logging)
