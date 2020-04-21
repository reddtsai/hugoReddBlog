---
title: "Linux + Nginx + .NET Core API"
date: 2020-04-15T13:47:50+08:00
description: 在 Linux 上使用 ASP.NET Core
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- GCE
- dotnet
- Debian
categories:
- GCP
- dotnet
---

主流程式語言大多可以跨平台作業，但對 .NET 來說是個新鮮事，這次就來分享將 API 部署到 Linux。

### Create a VM

首先請準備一台 Linux 主機，可參考[快速建立 Compute Engine](https://reddtsai.github.io/zh/posts/gcp_computeengine/)。

{{< alert theme="info" >}}
這次範例是使用 Debian 9 
{{< /alert >}}

### Dependencies and Requirements

接著安裝 ASP.NET Core 環境依賴關係和要求：
1. Install .Net Core 3.1
    - 安裝前請先註冊 Microsoft 金鑰和套件摘要：
        ``` bash
        # 註冊微軟金鑰
        wget -O- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
        sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
        # 註冊產品存儲庫
        wget https://packages.microsoft.com/config/debian/9/prod.list
        sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
        sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
        sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
        ```
    - 透過套件管理(apt-get)安裝 .NET Core：
        ``` bash
        # Install apt-get enable SSL
        sudo apt-get update
        sudo apt-get install -y apt-transport-https
        # Install the .NET Core runtime
        sudo apt-get update
        sudo apt-get install aspnetcore-runtime-3.1
        ```
2. Install Nginx
    - 安裝反向 Proxy 伺服器：
        ``` bash
        apt-get install -y nginx
        ```
    - 設置站台：
        修改 /etc/nginx/sites-available/default
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
    - 重新載入設定：
        ``` bash
        sudo nginx -s reload
        ```
3. Deploy your api
    請將你的 api 放置到這個位置 /var/www/your_api。
4. Monitor your api
    - 新增服務：
        /etc/systemd/system/sample_api.service
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
    - 啟動服務：
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

建立自動化腳本 [gce-startup.sh](https://github.com/reddtsai/dotnetLotteryAPI/blob/master/gce-startup.sh)。
``` shell
# Install Stackdriver logging agent
curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh
sudo bash install-logging-agent.sh

# Install apt-get enable SSL
sudo apt-get -yq install apt-transport-https

sudo apt-get -yq install git

# Register Microsoft key and feed
wget -O- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/debian/9/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list

。。。
```

{{< alert theme="success" >}}
這次範例所製作的腳本，每次部署都會建立全新的 VM 來部署程式，但在容器的部署上，會由 Image 持續來更新 Container，有機會在另外說明。
{{< /alert >}}

### Load Balance

最後來介紹一下如何建立負載平衡(Load Balance)，因為這次 Host 是使用 GEC，透過 GEC 執行個體群組和剛建立的自動化腳本就可快速完成。
修改一下腳本 [gce.sh](https://github.com/reddtsai/dotnetLotteryAPI/blob/master/gce.sh)：
``` shell
。。。

gcloud compute instance-groups managed create group-aspnet \
    --base-instance-name aspnet \
    --size 2 \
    --template tmp-aspnet-debian \
    --target-pool pool-aspnet

gcloud compute forwarding-rules create lb-aspnet \
            --ports 80 \
            --region asia-east1 \
            --target-pool pool-aspnet
```

![Alt text](/images/group-aspnet.png)
![Alt text](/images/pool-aspnet.png)
![Alt text](/images/gce-aspnet.png)

### Reference

[Sample Project](https://github.com/reddtsai/dotnetLotteryAPI)
[.NET](https://docs.microsoft.com/zh-tw/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-3.1#https-configuration)