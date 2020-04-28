---
title: "API 性能測試"
date: 2020-04-23T17:14:58+08:00
description: 使用 wrk 測試 API 的吞吐量
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Load
categories:
- Test
---

介紹一個輕量級性能測試工具 wrk 給後端開發者，它可協助你瞭解 1.為什麼服務掛了?? 2.需不需要 Load Balance?? 3.API 的 QPS 多少?? 等等 ...，來看看怎麼作。

環境
- Mac
- Nginx (Debian 9, Google 雲端平台)
- API (.NET Core 3.0, Google 雲端平台)
- SQL Server (Google 雲端平台)

### Install

首先安裝 wrk，因為個人是使用 Mac 就透過 Homebrew 來安裝：
``` bash
brew install wrk
```

### Test

準備好要測試的 API，使用 wrk 進行壓測：
GET http://35.234.53.61/api/lotto649/109000035，它會回傳一段 Json
``` bash
wrk -t8 -c500 -d30s  --latency http://35.234.53.61/api/lotto649/109000035
```
{{< alert theme="info" >}}
-c, --connections：跟伺服器建立並保持的TCP連接數量  
-d, --duration   ：壓測時間          
-t, --threads    ：開啟多少個線程進行壓測                                                 
-s, --script     ：Lua腳本路徑      
-H, --header     ：添加 HTTP header
    --latency    ：列印延遲統計信息  
    --timeout    ：timeout     
-v, --version    ：wrk的詳細版本信息
{{< /alert >}}
測試報告：
``` bash
Running 30s test @ http://35.234.53.61/api/lotto649/109000035
  8 threads and 500 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   708.36ms  223.55ms   1.80s    83.41%
    Req/Sec    57.29     49.79   346.00     82.95%
  Latency Distribution
     50%  707.45ms
     75%  819.04ms
     90%  878.19ms
     99%    1.70s 
  10240 requests in 30.06s, 3.12MB read
  Socket errors: connect 251, read 0, write 0, timeout 0
Requests/sec:    340.64
Transfer/sec:    106.45KB
```
報告有幾個重點：
- 平均每秒處理(Requests/sec)
- 平均每秒流量(Transfer/sec)
- 發生錯誤數(Socket errors)

### lua

wrk 有提供自定義腳本，可用來客製測試方法和報告。例如需使用 POST 方法測試 API：
post.lua
``` text
wrk.method = "POST"
wrk.body   = "{ "id": 1, "name": "redd" }"
wrk.headers["Content-Type"] = "application/json"
``` 

### Reference

[wrk](https://github.com/wg/wrk)