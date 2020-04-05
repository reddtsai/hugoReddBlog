---
title: "用 dotnet ef 工具，從 MySQL 自動生成 Model.cs"
date: 2020-04-04T15:32:00+08:00
description: 利用 dotnet-ef 實作 DB 反向工程
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- EF
categories:
- .Net
---

開發 .NET 應用程式在處理資料時，都會建立模型類別(Model)，特別像是 MVC 的專案，而當資料要儲存到資料庫時，資料庫需要產生對映 Schema 來儲存。這時千萬別馬上開始，先瞭解幾個專有名詞 code first、model first、database first，然後，再利用自動生成工具來減少你的工作，工具像是 PMC Tool - Migrations、POCO Entity ...。

不過這些工具，個人覺得在 mac 上，使用起來不是很順手，個人較習慣使用 CLI Tool 的方式，接下來用個實例說明反向工程，如何用 MySQL Schema 生成模型類別到專案中。
我所使用的環境：
- Mac
- Visual Studio Code
- dotnet core 3.0
- dotnet-ef
- Makefile

### Installing

請先建立好你的 dotnet project，然後確認已安裝：
1. CLI Tool
    安裝 dotnet-ef 工具，用來作模型移轉。
    ``` bash
    dotnet tool install --global dotnet-ef
    ```
2. Database Provider
    我所使用的是 MySQL，需安裝 MySql 資料庫提供者。
    ``` bash
    dotnet add package Pomelo.EntityFrameworkCore.MySql
    ```
    > [PROVIDER](https://docs.microsoft.com/zh-tw/ef/core/providers/?tabs=dotnet-core-cli)
3. EntityFrameworkCore.Design
    如果你是使用 .NET Core 2.1 以上的版本，.NET Core SDK 已包含 Design，可跳過這一步。
    ``` bash
    dotnet add package Microsoft.EntityFrameworkCore.Design
    ```

### Scaffold

當資料庫和工具準備完成，透過 dbcontext scaffold 指令來進行反向工程，它會讀取資料庫的資訊，像 tables, columns, constraints, and indexes ...，然後生成 DbContext.cs 和 Models.cs。
``` bash
dotnet ef dbcontext scaffold "server=127.0.0.1;port=3306;user=root;password=pwd;database=db" Pomelo.EntityFrameworkCore.MySql
```

### Script

最後，建立 makefile 腳本持續更新模型
``` Makefile
efScaffold:
    dotnet ef dbcontext scaffold "server=127.0.0.1;port=3306;user=root;password=pwd;database=db" \
        Pomelo.EntityFrameworkCore.MySql \
        -o Repositories/Entities \
        --context-dir Repositories -c LotteryContext \
        -p src -f
```

### Reference

[Side Project](https://github.com/reddtsai/dotnetLotteryAPI)

[Reverse Engineering](https://docs.microsoft.com/zh-tw/ef/core/miscellaneous/cli/dotnet)