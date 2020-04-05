---
title: "在雲上使用 MySQL"
date: 2020-03-17T15:25:22+08:00
description: Cloud SQl for MySQL on Google Cloud Platform
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- MySQL
categories:
- GCP
- Database
---

不論公司 project 或個人 side project，都會遇到資料儲存的需求，這時可能就是
- 求 DBA
- 自己安裝
- 向 Infra 申請
- 開始學 SQL
- Google

### Why Cloud SQl

我為什麼選擇 Cloud SQl？原因只有一個，就是快
1. 安裝快
2. 上手快
3. 擴展快
4. 運維快

這裡會用 GCP 提供的 Cloud SQl 來說明，當然也可以選擇 AWS、Azure、阿里雲的產品。

### Install MySQL

安裝快，只需幾分鐘就能建立一台 DB。開始安裝前你需要先有：
1. 啟用 [Google Cloud](https://cloud.google.com/gcp/getting-started) (你需要一組 Google 帳戶和一張信用卡)
2. 安裝 cloud SDK
``` bash
brew cask install google-cloud-sdk
```
安裝 DB 只需開啟終端機，執行下段命令。
``` bash
gcloud sql instances create [INSTANCE_NAME] --tier=[MACHINE_TYPE] --region=[REGION]
```

> - INSTANCE_NAME：命名你的 Database instance。
> - MACHINE_TYPE：Database 規格。[Price](https://cloud.google.com/sql/pricing#2nd-gen-pricing)
> - REGION：Instance 存放位置。[Region](https://cloud.google.com/sql/docs/mysql/instance-settings#region-values)
>  
>   [Creating instances 說明](https://cloud.google.com/sql/docs/mysql/create-instance)

另外，你不習慣冷冰冰的命令，GCP 也提供 [UI](https://console.cloud.google.com) 介面讓你設定。

![Alt text](/images/createMysqlInstance.png)

### Connection

上手快，你可快速連至 cloud database，官方提供多種方式及各程式語言的完整範例說明，減少你摸索的時間。
這裡介紹透過 cloud sql proxy 快速連到剛建立的 DB。首先，安裝 cloud sql proxy
``` bash
gcloud components install cloud_sql_proxy
./cloud_sql_proxy -instances=<INSTANCE_CONNECTION_NAME>=tcp:3306
```
如下圖所示，你可以找到 INSTANCE_CONNECTION_NAME

![Alt text](/images/mysqlInstance.png)
再來使用 MySQL workbench 來作個本機連線測試，如果你還沒安裝，請先透過以下命令來安裝。
``` bash
brew cask install mysqlworkbench
```

![Alt text](/images/workbench.png)

### Replication

擴展快、運維快，當你初步認識 SQL 後，再進階就是效能跟穏定，最長聽到的就是 HA(high availability)，而在 cloud sql 你也只需要作簡單的設定來逹成 HA，讓你的 DB 擁有備援。

![Alt text](/images/mysqlInstanceHA.png)

另一個例子，在 cloud sql 新增一個讀取位置，分散流量來提升效能。

![Alt text](/images/mysqlInstanceRead1.png)

![Alt text](/images/mysqlInstanceRead2.png)

### Reference

本次實作的費用
![Alt text](/images/mysqlInstancePrice.png)

[cloud sql for mysql](https://cloud.google.com/sql/docs/mysql)