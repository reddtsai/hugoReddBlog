---
title: "CentOS 7 如何安裝 Elasticsearch"
date: 2019-06-27T14:37:52+08:00
toc: true
---

<!--more-->

## 安裝 Elasticsearch

### 1. 安裝 OpenJDK

```bash
sudo yum  install java-1.8.0-openjdk-devel
java -version
```

### 2. 安裝 Elasticsearch 簽章公鑰

```bash
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

### 3. 安裝 Elasticsearch

建立 Elasticsearch 套件容器設定檔 /etc/yum.repos.d/elasticsearch.repo

填入套件容器參數

```text
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

安裝 Elasticsearch

```bash
sudo yum install elasticsearch
```

### 4. 啟動 Elasticsearch 服務

```bash
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
```

測試 Elasticsearch

```bash
sudo systemctl status elasticsearch.service

curl -X GET http://localhost:9200
```

也可檢查服務記錄訊息

```bash
sudo journalctl --unit elasticsearch --since  "20 min ago"
```

### 5. 設定防火牆和IP

新增

```bash
sudo firewall-cmd --new-zone=elasticsearch --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --zone=elasticsearch --add-source=<IP_ADDRESS>/32 --permanent
sudo firewall-cmd --zone=elasticsearch --add-port=9200/tcp --permanent
sudo firewall-cmd --reload
```

修改

```bash
sudo firewall-cmd --zone=elasticsearch --add-source=<IP_ADDRESS> --permanent
sudo firewall-cmd --reload
```

### 6. 配置 Elasticsearch

Elasticsearch 配置檔 /etc/elasticsearch/elasticsearch.yml

Elasticsearch JVM 配置檔 /etc/elasticsearch/jvm.options

__◎記憶體配置不可小放實體記憶體50%__

```text
-Xms2g
-Xmx2g

network.host: 0.0.0.0
```

[參考官方說明]:https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html
