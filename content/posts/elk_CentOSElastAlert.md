---
title: "CentOS 7 如何安裝 ElastAlert"
date: 2019-07-25T14:06:13+08:00
description: 介紹在 CentOS 中安裝 ElastAlert
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- ElastAlert
- CentOS
categories:
- Elastic
- OS
---

<!--more-->

版本
* ElastAlert 0.2

環境
* CentOS 7

安裝需求
* python 2.7
* python-dev
* python-pip
* dependency Package，參考 ElastAlert requirements.txt

* * * *

#### 1. 安裝 PIP、DEV

確認是否已安裝 python 2.7
```bash
python --version
```

安裝 python-pip
```bash
sudo yum -y install python-devel
sudo yum -y install epel-release
sudo yum -y install python-pip
pip --version
sudo pip install --upgrade pip
```

* * * *

#### 2. 安裝 ElastAlert

確認套件，安裝的過程中可能會遇到相依套件的問題，請參考 ElastAlert requirements.txt 中的需求套件清單
```bash
pip list
```
例如，在安裝 blist 套件時，需要安裝 GCC
```bash
sudo yum -y install gcc
```
例如，在更新 requests 套件時，需要強制移除安裝
```bash
sudo pip install requests --ignore-installed requests
```
安裝 ElastAlert
```bash
sudo pip install elastalert
```

* * * *

#### 3. 設定 ElastAlert

在 ElastAlert 目錄下新增設定檔 config.yaml。以我的環境為例 /usr/lib/python2.7/site-packages/elastalert
建議複製 ElastAlert config.yaml.example
```yaml
# This is the folder that contains the rule yaml files
# Any .yaml file will be loaded as a rule
rules_folder: /usr/lib/python2.7/site-packages/elastalert/alert_rules

# How often ElastAlert will query Elasticsearch
# The unit can be anything from weeks to seconds
run_every:
# seconds:
  minutes: 1
# hours:

# ElastAlert will buffer results from the most recent
# period of time, in case some log sources are not in real time
buffer_time:
  minutes: 15

# The Elasticsearch hostname for metadata writeback
# Note that every rule can have its own Elasticsearch host
es_host: elasticsearch.example.com

# The Elasticsearch port
es_port: 9200

# elastalert-create-index to set a mapping
writeback_index: elastalert_status

alert_time_limit:
  days: 2
```

* * * *

#### 4. 設定 Elasticsearch Client

確認 Elasticsearch Client 的版本和 Elasticsearch 的版本相符。以我的環境為例，需要將 lasticsearch Client 更新至 6.X 版
```bash
pip list
sudo pip uninstall elasticsearch
sudo pip install "elasticsearch<7"
```
在 Elasticsearch 增加一個 ElastAlert 的索引
```bash
elastalert-create-index
```

* * * *

#### 5. 設定通知規則

新增通知規則設定檔 filebeat_frequency.yaml。以我的環境為例 /usr/lib/python2.7/site-packages/elastalert/alert_rules
這裡以 IIS Log 為例，當 http status != 200 時，發送一個訊息至 Slack
```yaml
es_host: elasticsearch.example.com

es_port: 9200

name: Example frequency rule

type: frequency

index: filebeat-*

# (Required, frequency specific)
# Alert when this many documents matching the query occur within a timeframe
num_events: 50

# (Required, frequency specific)
# num_events must occur within this amount of time to trigger an alert
timeframe:
  hours: 4

filter:
- query:
    query_string:
      query: "NOT iis.access.response_code: 200 AND event.dataset: iis.access"

alert:
- slack:
    slack_webhook_url: "http://slack_webhook_url"
```
測試規則設定檔
```bash
elastalert-test-rule alert_rules/filebeat_frequency.yaml
```

* * * *

#### 6. 設定 ElastAlert 服務

執行測試
```bash
python -m elastalert --verbose --rule alert_rules/filebeat_frequency.yaml
```
新增服務
```bash
sudo mkdir -p /etc/elastalert/rules
cd /etc/elastalert/
sudo cp /usr/lib/python2.7/site-packages/elastalert/config.yaml config.yaml
sudo cp /usr/lib/python2.7/site-packages/elastalert/alert_rules/filebeat_frequency.yaml rules/filebeat_frequency.yaml
```
修改 config.yaml
rules_folder: /etc/elastalert/rules
新增服務設定檔
```bash
cd /etc/systemd/system/
sudo vi elastalert.service
```
elastalert.service
```text
[Unit]
Description=elastalert

[Service]
Type=simple
User=root
Group=root
Restart=on-failure
WorkingDirectory=/usr/lib/python2.7/site-packages/elastalert
ExecStart=/usr/bin/elastalert --config /etc/elastalert/config.yaml --rule /etc/elastalert/rules/filebeat_frequency.yaml
[Install]
WantedBy=multi-user.target
```

啟動服務

```bash
sudo systemctl enable elastalert.service
sudo systemctl start elastalert.service
sudo systemctl status elastalert.service
```

* * * *

#### 參考

[ElastAlert Repo](https://github.com/Yelp/elastalert)
[ElastAlert Doc](https://elastalert.readthedocs.io/en/latest/)
