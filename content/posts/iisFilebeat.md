---
title: "windows 如何安裝 Filebeat 收集 IIS Logs"
date: 2019-07-05T11:28:52+08:00
toc: true
tags: ["ELK"]
---

## windows 如何安裝 Filebeat 收集 IIS Logs

1. Download [Filebeat](https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.2.0-windows-x86_64.zip) for Windows

2. 解壓縮至 C:\Program Files\Filebeat，以 admin 角色開啟 PowerShell

    ```bash
    cd 'C:\Program Files\Filebeat'
    .\install-service-filebeat.ps1
    ```

3. 設定 Filebeat

    編輯 filebeat.yml

    ```text
    output.elasticsearch:
        hosts: ["localhost:9200"]
    setup.kibana:
        host: "localhost:5601"
    ```

4. 設定 Filebeat 模組

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

5. 啟動 Filebeat 服務

    ```bash
    Start-Service filebeat
    ```
