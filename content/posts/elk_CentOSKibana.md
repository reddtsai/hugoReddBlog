---
title: "CentOS 7 如何安裝 Kibana"
date: 2019-06-27T14:38:02+08:00
description: 介紹在 CentOS 中安裝 Kibana
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

版本

* Kibana 6.8

環境

* CentOS 7

* * * *

#### 1. 安裝 OpenJDK

```bash
sudo yum  install java-1.8.0-openjdk-devel
java -version
```

* * * *

#### 2. 安裝 Elasticsearch 簽章公鑰

```bash
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

* * * *

#### 3. 安裝 Kibana

建立 Kibana 套件容器設定檔 /etc/yum.repos.d/kibana.repo

填入套件容器參數

```text
[kibana-6.x]
name=Kibana repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

安裝 kibana

```bash
sudo yum install kibana
```

* * * *

#### 4. 配置 kibana

kibana 配置檔 /etc/kibana/kibana.yml

```text
elasticsearch.hosts: http://localhost:9200

server.host: 0.0.0.0
```

[參考官方說明]:https://www.elastic.co/guide/en/kibana/current/settings.html

* * * *

#### 5. 設定防火牆和IP

新增

```bash
sudo firewall-cmd --new-zone=kibana --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --zone=kibana --add-source=<IP_ADDRESS>/32 --permanent
sudo firewall-cmd --zone=kibana --add-port=5601/tcp --permanent
sudo firewall-cmd --reload
```

修改

```bash
sudo firewall-cmd --zone=kibana --add-source=<IP_ADDRESS> --permanent
sudo firewall-cmd --reload
```

測試

```bash
sudo firewall-cmd --zone=kibana --list-all

sudo netstat -plnt
```

* * * *

#### 6. 啟動 kibana 服務

```bash
sudo systemctl enable kibana.service
sudo systemctl start kibana.service
```

測試 kibana

http://localhost:5601

```bash
sudo systemctl status kibana.service
```

也可檢查服務記錄訊息

```bash
sudo journalctl --unit kibana --since  "20 min ago"
```

* * * *

#### 參考

[Elastic 官網](https://www.elastic.co/cn/)
