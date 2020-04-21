---
title: "快速建立 Compute Engine"
date: 2020-04-09T02:15:16+08:00
description: Write a script to create Compute Engine
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- GCE
categories:
- GCP
---

分享個人如何將雲端虛擬機器結合到開發工作中，例如，當專案需要二台主機，一台用來作使用者測試，另一台給未來營運使用，以下介紹個人的作法給大家參考。未來作持續更新部署(CI/CD)或自動擴展(Auto Scaling)也會使用這個自動化腳本。

### Create a Compute Engine instance

透過 CLI 的方式來介紹如何建立一台虛擬機器，這次使用的是 Google Cloud 提供的虛擬機器 Compute Engine。
{{< alert theme="success" >}}
請先準備好你的 Google 帳號和 SDK
{{< /alert >}}
1. 設定 VM 所在區域 (Zone)
    ``` bash
    gcloud config set compute/zone [zone]
    gcloud config set compute/region [region]
    ```
    {{< alert theme="info" >}}
東南亞可選擇新加坡，東北亞可選擇首爾。
[Regions and Zones](https://cloud.google.com/compute/docs/regions-zones)
    {{< /alert >}}
2. 新增一台 VM (Compute Engine)
    ``` bash
    gcloud compute instances create [instance]
    ```
    {{< alert theme="info" >}}
機器類型：虛擬化硬體規格。ex. --machine-type=g1-small
作業系統：Windows, Linux。ex. --image-family=debian-9
    {{< /alert >}}
3. 設定防火墻 (Firewall)
    Enable port 80 for web site
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

### Create Deployment Script

接下來，將建立容器的動作，透過 shell script 製作成腳本。後續你可將這個腳本加入版本控制，持續更新部署。
使用者測試環境，Create_SIT_GCE.sh。
``` shell
MY_INSTANCE_NAME="sit-vm"
ZONE=asia-east1-a

gcloud compute instances create $MY_INSTANCE_NAME \
    --image-family=debian-9 \
    --image-project=debian-cloud \
    --machine-type=g1-small \
    --scopes userinfo-email,cloud-platform \
    --zone $ZONE \
```
營運環境，Create_PRD_GCE.sh。
``` shell
gcloud config set compute/zone asia-east1-a

gcloud config set compute/region asia-east1

gcloud compute instance-templates create prd-template \
    --image-family=debian-9 \
    --image-project=debian-cloud \
    --machine-type=g1-small \
    --scopes userinfo-email,cloud-platform

gcloud compute target-pools create prd-pool

gcloud compute instance-groups managed create prd-group \
    --base-instance-name prd \
    --size 2 \
    --template prd-template \
    --target-pool prd-pool

gcloud compute forwarding-rules create lb-prd \
            --ports 80 \
            --target-pool prd-pool
```

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