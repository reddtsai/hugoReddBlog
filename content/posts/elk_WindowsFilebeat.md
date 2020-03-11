---
title: "Windows 如何安裝 Filebeat"
date: 2019-07-30T15:59:54+08:00
description: 介紹在 Windows 中安裝 Filebeat
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Filebeat
categories:
- Elastic
---


<!--more-->

版本

* Filebeat 6

環境

* Windows 10

#### 1. 下載 Filebeat

Download [Filebeat](https://www.elastic.co/cn/downloads/past-releases#filebeat) for Windows

* * * *

#### 2. 安裝 Filebeat

解壓縮至 C:\Program Files\Filebeat，以 admin 角色開啟 PowerShell

```bash
    cd 'C:\Program Files\Filebeat'
    .\install-service-filebeat.ps1
```

* * * *

#### 3. 設定 Filebeat

編輯 filebeat.yml，設定 elasticsearch 和 kibana 的位置

```text
    output.elasticsearch:
        hosts: ["localhost:9200"]
    setup.kibana:
        host: "localhost:5601"
```

* * * *

#### 4. 設定 Filebeat 模組

收集 IIS Log

編輯 filebeat\module\iis\access\manifest.yml

```text
    default.paths:
        default:
            - C:/inetpub/logs/LogFiles/*/*.log
```

啟用 iis 模組

```bash
    .\filebeat.exe modules enable iis
```

* * * *

#### 5. 啟動 Filebeat 服務

```bash
    .\filebeat.exe setup
    Start-Service filebeat
```

![Alt text](/images/filebeat_service.PNG)
