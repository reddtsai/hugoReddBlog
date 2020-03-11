---
title: "如何使用 Kibana 分享日誌"
date: 2019-08-02T11:49:01+08:00
description: 介紹 Kibana 個人應用實例
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Kibana
categories:
- Elastic
---

<!--more-->

 Elastic 提供一個強大的 UI 介面 Kibana，透過它能更有效的分享與呈現 Elasticsearch 中的數據，接下來會以本身使用的案例，來介紹如何使用 Kibana。

> 版本
>
> * Kibana 6
>
> 安裝請參考下一篇文章[CentOS 7 如何安裝 Kibana](https://reddtsai.github.io/posts/elk_centoskibana/)

* * * *

#### Discover Page

數據探索功能，使用 Elasticsearch Index、Time Range 和 Query DSL 過濾搜索，顯示數據計數和內䆟

![Alt text](/images/kibana_discover.PNG)

> 注意
>
> 如果遇到 no cached mapping for this field
>
> 試著在 Management，Refresh field list

* * * *

#### Dashboard Page

儀表版功能，Kibana 提供多樣視覺化圖表，儀表版可幫助你收集管理這些圖表

![Alt text](/images/kibana_dashboard.PNG)

* * * *

#### Logs Page

數據功能，顯示匯入的數據

![Alt text](/images/kibana_logs.PNG)

* * * *

#### Query DSL

Elastic 提供了一套查詢語法 DSL (Domain Specific Language)，可透過 RESTful 或 Kibana 的介面來使用，這邊介紹幾個範例

* Match

    例如查詢 IIS 中，有那些 Request 回應 500

        GET /_search
        {
            "query": {
                "match": {
                    "iis.access.response_code": {
                        "query": 500,
                        "type": "phrase"
                    }
                }
            }
        }

* Wildcard

    例如查詢 IIS 中，有那些 Request url like */app/

        GET /_search
        {
            "query": {
                "wildcard": {
                    "iis.access.url": "*/app"
                }
            }
        }

* Prefix

    例如查詢 IIS 中，url 以 /en/app 開頭的 Request 有那些

        GET /_search
        {
            "query": {
                "prefix": {
                    "iis.access.url": "/en/app"
                }
            }
        }

* * * *

#### 參考

[Elastic 官網](https://www.elastic.co/cn/)
