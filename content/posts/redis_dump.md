---
title: "Redis 資料匯出匯入"
date: 2020-08-02T16:58:13+08:00
description: export/import redis data
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Redis
categories:
- Database
---

### Install

安裝 redis client
``` bash
brew install redis
redis-cli --version
```
安裝 redis dump tool
``` bash
npm install redis-dump -g
redis-dump --help
```

{{< alert theme="info" >}}
OS：Mac
Package Manager：npm, Homebrew
{{< /alert >}}

### Export

使用 redis-dump 匯出資料至檔案：
``` bash
redis-dump -h 0.0.0.0 -d 0 > data.dump
```
redis-dump optino
- h - hostname
- p - port
- a - auth password
- d - database(db0)
- f - filter

### Import

使用 redis-cli 匯入資料至 redis：
``` bash
data.dump | redis-cli -n 0
```
redis-cli optino
- h - hostname
- p - port
- a - auth password
- n - database(db0)

### Other Recommend

另外，推薦幾個 redis 資料遷移的命令，但因為有權限和環境的限制較適合 DBA 使用。
- DUMP/RESTORE:輸出序列化的值/將值反序列化輸入。
- MIGRATE:將當前資料庫遷移至指定的資料庫。
- MOVE:將當前資料庫的 key 移動到指定的資料庫當中。

### Reference

[redis](https://redis.io/commands)
[redis-dump](https://www.npmjs.com/package/redis-dump)