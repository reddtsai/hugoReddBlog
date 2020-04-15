---
title: "Dotnet_HostInGCE"
date: 2020-04-15T13:47:50+08:00
description: Write script to create Compute Engine
draft: true
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- GCE
- dotnet
categories:
- GCP
- dotnet
---

開始前，想先請詢問??
1. 你是使用 UI 介面還是 CLI 來部署雲端
2. 你部署的內容是只更新 APP 還是整個 Instance

為什麼會這麼問呢？？
使用雲端一定要先瞭解容器，每次的部署應該是更新容器。接下來部署容器一次，兩次，第三次就該建立自動化腳本來減少重工。

### Create a Compute Engine instance

這裡用 CLI 的方式，在 GCP 建立一台 Compute Engine
{{< alert theme="success" >}}
請先準備好你的 Google 帳號和 SDK
{{< /alert >}}
1. 設定 VM 所在區域 (Zone)
    ``` bash
    gcloud config set compute/zone [zone]
    gcloud config set compute/region [region]
    ```
2. 新增一台 VM (Compute Engine)
    ``` bash
    gcloud compute instances create [instance]
    ```
3. 設定防火墻 (Firewall)
    例如 web site 需要充許 80 port
    ``` bash
    gcloud compute firewall-rules create allow-80 --allow tcp:80
    ```
4. 建立負載平衡 (Load balancing)
    如果你需要建立負載平衡，依照 GCP 說明，要使用叢集(cluster)來建立 VM(取代第二步)。
    ``` bash
    gcloud compute instance-templates create [template]
    gcloud compute target-pools create [pool]
    gcloud compute instance-groups managed create [group] \
            --base-instance-name [name] \
            --size 2 \
            --template [template] \
            --target-pool [pool]
    ```
    負載平衡：
    ``` bash
    gcloud compute forwarding-rules create [loadbalance] \
            --ports 80 \
            --target-pool [pool]
    ```

完成後，你可以試著透過 SSH 登入 VM：
``` bash
gcloud compute ssh [instance]
```

### Deploy .NET API on Compute Engine

剛剛建立了一台 Compute Engine，現在來加入應用程式。
1. Install .Net Core 3.1
    在 Debian 系統下，透過以下指令來安裝 .NET Core。
    ``` bash
    # Register Microsoft key and feed
    wget -O- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
    sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
    wget https://packages.microsoft.com/config/debian/9/prod.list
    sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
    sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list

    # Install apt-get enable SSL
    sudo apt-get update
    sudo apt-get install -y apt-transport-https

    # Install the .NET Core runtime
    sudo apt-get update
    sudo apt-get install aspnetcore-runtime-3.1
    ```
2. Install Nginx
    安裝反向 Proxy 伺服器。
    ``` bash
    apt-get install -y nginx
    ```
    接著將站台(sample api)設置到 Nginx，透過修改 /etc/nginx/sites-available/default
    ``` text
    server {
        listen        80;
        server_name   example.com *.example.com;
        location / {
            proxy_pass         http://localhost:5000;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection keep-alive;
            proxy_set_header   Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto $scheme;
        }
    }
    ```
    重新載入設定：
    ``` bash
    sudo nginx -s reload
    ```
3. publish sample api
    首先將 sample api，部署到 /var/www/sample_api。
    接著透過服務來啟動 sample api：
    1.新增服務 /etc/systemd/system/sample_api.service
    ``` text
    [Unit]
    Description=.NET Web API running on Debian

    [Service]
    WorkingDirectory=/var/www/sample_api。
    ExecStart=/usr/bin/dotnet /var/www/sample_api/sample_api.dll
    Restart=always
    # Restart service after 10 seconds if the dotnet service crashes:
    RestartSec=10
    KillSignal=SIGINT
    SyslogIdentifier=dotnet-api
    User=www-data
    Environment=ASPNETCORE_ENVIRONMENT=Production
    Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

    [Install]
    WantedBy=multi-user.target
    ```
    2.啟動服務
    ``` bash
    sudo systemctl enable kestrel-helloapp.service
    sudo systemctl start kestrel-helloapp.service
    ```


完成後，你可由瀏覽器或 curl 作個測試：
``` bash
curl http://localhost
curl http://your ip
```

### Deployment Script

接下來，將建立容器和部署程式的動作，透過 Makefile 和 shell script 製作成腳本。


{{< alert theme="success" >}}
這次範例所製作的腳本，每次部署都會建立全新的 VM 來部署程式，但在容器的部署上，會由 Image 持續來更新 Container，有機會在另外說明。
{{< /alert >}}

[sample project](https://github.com/reddtsai/dotnetLotteryAPI)

###  Clean up Compute Engine instance

不需要時清除 VM，避免計費問題。
``` bash
gcloud compute forwarding-rules delete [loadbalance] 
gcloud compute instance-groups managed delete [group]
gcloud compute target-pools delete [pool]
gcloud compute instance-templates delete [template]
gcloud compute instances delete [instance]
gcloud compute firewall-rules delete allow-80
```

### Reference

[Compute Engine](https://cloud.google.com/compute/docs)
[.NET](https://docs.microsoft.com/zh-tw/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-3.1#https-configuration)