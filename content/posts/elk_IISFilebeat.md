---
title: "如何使用 Filebeat 收集 Logs"
date: 2019-07-05T11:28:52+08:00
toc: true
tags: ["elastic"]
---

<!--more-->

Filebeat 是個檔案分析工具，最常用來收集日誌檔，收集的過程分為

1. 收集數據 (讀取日誌)

2. 解析數據

    常見的日誌格式(IIS、NGINX、MySQL)大多可透過 Filebeat 內建模組來解析，那也可透過新增模組來客製格式。

3. 傳送數據 (Logstash 或 Elasticsearch 儲存)

> 版本
>
> * Filebeat 6
>
> 安裝請參考下一篇文章[Windows 如何安裝 Filebeat](https://reddtsai.github.io/posts/elk_windowsfilebeat/)

以下為該如何使用 Filebeat 模組

#### 模組設定

Filebeat 提供多樣模組來收集常見的日誌，這邊以 IIS 日誌為例，說明如何設定模組。

1. 設定日誌路徑

    加入 IIS 日誌路徑

    編輯 [Windows 安裝路徑]\filebeat\module\iis\access\manifest.yml

    ```text
        default.paths:
            default:
                - C:/inetpub/logs/LogFiles/*/*.log
    ```

2. 啟用模組

    開啟 IIS 模組，在 PowerShell 執行下方命令

    ```bash
    cd [Windows 安裝路徑]
    .\filebeat.exe modules enable iis
    ```

#### 新增模組

新增一個模組需透過 Beats 程式碼來逹成，這裡只會說明新模組需要作那些設定，完整資訊請參考 github Beats repository

新增一個模組後，可看到所產生的檔案結構，以下方我所建立的 nlog module 為例

```text
filebeat
├ module
│   ⎩ nlog
│       ⎩ app
│           ├ manifest.yaml
│           ├ config
│           │   ⎩ fileset.yml
│           ⎩ ingest
│               ⎩ pipeline.json
├ modules.d
│   ⎩ nlog.yml.disabled

```

##### manifest.yaml

* 模組控制檔

    日誌路徑

    ```text
    var:
    - name: paths
        os.windows
        - C:/log/*.log
    ```

    參考

* 設定日誌路徑

* 設定 Ingest Pipeline

* 指定設定檔

filebeat modules.d

test

```bash
curl -X DELETE http://elasticsearch:9200/_ingest/pipeline/filebeat-6.7.0-iis-access-default
```

#### 測試模組

1. 測試設定檔

    ```bash
    .\filebeat.exe test config -c filebeat.yml
    .\filebeat.exe test output filebeat.yml
    ```

2. 執行 Filebeat

    ```bash
    .\filebeat.exe -e -c filebeat.yml -d "*"
    ```

    > 注意  
    > 請先清除前次測試所讀取的記錄，刪除目錄 filebeat\data\registry

3. 檢查 Ingest & Pipeline

    透過 elasticsearch api 查詢 Ingest & pipeline (例如 IIS 模組 pipeline filebeat-6.7.0-iis-access-default)

    ```bash
    curl -X GET http://elasticsearch:9200/_cat/indices?v
    curl -X GET http://elasticsearch:9200/_ingest/pipeline
    curl -X GET http://elasticsearch:9200/_ingest/pipeline/filebeat-6.7.0-iis-access-default
    ```

4. 確認資料

    開啟 Kibana，透過 Logs 介面，確認是否有顯示 log 資料

    ![Alt text](/images/kibana_logs.PNG)

<br/>

> 文章參考來源為 Elastic 官方文檔
