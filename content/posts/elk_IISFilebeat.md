---
title: "Windows 如何安裝 Filebeat 收集 IIS Logs"
date: 2019-07-05T11:28:52+08:00
toc: true
tags: ["ELK"]
---

<!--more-->

## 安裝 Filebeat 6

### 1. 下載 Filebeat

Download [Filebeat](https://www.elastic.co/cn/downloads/past-releases#filebeat) for Windows

### 2. 安裝 Filebeat

解壓縮至 C:\Program Files\Filebeat，以 admin 角色開啟 PowerShell

```bash
    cd 'C:\Program Files\Filebeat'
    .\install-service-filebeat.ps1
```

### 3. 設定 Filebeat

編輯 filebeat.yml

```text
    output.elasticsearch:
        hosts: ["localhost:9200"]
    setup.kibana:
        host: "localhost:5601"
```

### 4. 設定 Filebeat 模組

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

### 5. 啟動 Filebeat 服務

```bash
    Start-Service filebeat
```

![Alt text](/images/filebeat_service.PNG)