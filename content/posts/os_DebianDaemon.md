---
title: "系統服務 In Debian"
date: 2020-03-29T15:22:21+08:00
description: 將 cloud sql proxy 加入系統服務
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Daemon
- Debian
categories:
- OS
---

後端工程師的工作中很常需要一個常駐程式，幫我們處理非即時的工作或例行性的排程工作，在 Windows Sever 中有大家熟悉的服務和工作排程，那 Unix-Like Server 呢？？在 Unix-Like 系統中比較常聽到的是 Daemon (守護)，它就是系統服務。
工作上有個需求，要在 Debian 下安裝 cloud sql proxy，然後，要它在背景執行，開機時自己啟動，在這裡作個記錄 。(想要進一步瞭解 Daemon 的大大，可問谷歌先生 "Unix-Like Daemon")

### Systemd - 服務管理機制

這次工作所使用的系統為 Debian，在 Debian 及其它 Unix-Like 系統中，都有個服務管理機制 systemd，透過這個機制來啟動、關閉與觀察系統服務或你所自定的服務。
可分為幾個類型如下：
- .service   一般服務
- .socket    程序資料交換
- .target    執行環境
- .mount     檔案系統掛載
- .automount 檔案系統掛載
- .path      偵測特定檔案或目錄
- .timer     循環執行

> 存放位置
> /usr/lib/systemd/system/：啟動腳本設定檔
> /run/systemd/system/：執行過程中所產生的服務腳本
> /etc/systemd/system/：管理員所建立的執行腳本

那要怎麼將 cloud sql proxy 加入 systemd 中？？本次會使用一般服務 cloud_sql_proxy.service，將檔案放入啟動位置 /etc/systemd/system/。
範例 /etc/systemd/system/cloud_sql_proxy.service
``` text
# 服務解釋、相依性
[Unit]
Description=Connecting MySQL Client from Compute Engine using the Cloud SQL Proxy
Requires=networking.service
After=networking.service

# 服務執行的指令參數
[Service]
WorkingDirectory=/usr/local/bin
ExecStart=/usr/local/bin/cloud_sql_proxy -dir=/var/run/cloud-sql-proxy -instances=redd-side-project:asia-east1:db-mysql-001=tcp:3306
Restart=always
StandardOutput=journal
User=root

# 要掛載哪個 target 底下
[Install] 
WantedBy=multi-user.target
```

### Systemctl - 服務管理指令

初步認識 systemd 後，還需認識一個管理程式 systemctl，主要會透過 systemctl 的指令來處理我們的服務。
接下來透過 systemctl 來操控 cloud_sql_proxy.service。
啟用/停用，下次開機生效
``` bash
sudo systemctl enable cloud-sql-proxy.service
sudo systemctl disable cloud-sql-proxy.service
```
立刻啟動/關閉
``` bash
sudo systemctl start cloud-sql-proxy.service
sudo systemctl stop cloud-sql-proxy.service
```
列出狀態
``` bash
sudo systemctl status cloud-sql-proxy.service
```
常用
``` bash
-- 列出所有啟動的 unit
sudo systemctl
-- 列出所有 .service
sudo systemctl list-units --type=service --all
-- 列出所有 cpu 為名 .service
sudo systemctl list-units --type=service --all | grep cpu
-- 列出相依
sudo systemctl list-dependencies
```

到這邊就完成了這次的工作任務，將 cloud_sql_proxy.service 在背景執行及開機時自己啟動。

### Timer - 排程

一開始除了服務，還有提到例行性的排程工作，那要怎麼使服務定期執行某個工作？？現學現賣，拿 systemd.timer 來作個實例(大多數的資訊都會用 crond 來定期處理服務😸😸)。
我希望每日執行一次我的 APP
1. 啟動系統 timer.target
2. app.service 你的工作
3. app.timer 你的排程(與你的工作同名)
範例 [side project](https://github.com/reddtsai/pythonLotteryScraper)
/etc/systemd/system/app.service
``` text
[Unit]
Description=taiwan lottery scarper
[Service]
WorkingDirectory=opt/app/gce/env/bin
ExecStart=python /opt/app/app.py
[Install]
WantedBy=multi-user.target
```
/etc/systemd/system/app.timer
``` text
[Unit]
Description=scheduling taiwan lottery scarper
[Timer]
OnActiveSec=1m
OnCalendar=Sun *-*-* 01:00:00
Persistent=true
Unit=lotto-scraper.service
[Install]
WantedBy=multi-user.target
```

### Reference

[鳥哥](http://linux.vbird.org/linux_basic/0560daemons.php#systemctl_timer)