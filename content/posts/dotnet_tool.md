---
title: "在 Mac 上安裝 dotnet tool"
date: 2020-04-04T15:35:18+08:00
description: 使用 CLI 來安裝 dotnet-ef
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- dotnet
categories:
- dotnet
---

至從開始使用 dotnet core 後，個人的開發方式也從 Visual Studio on Windows，轉換為 Visual Studio Code on Mac，轉變的過程有很多值得記錄。例如最近開發上需要安裝一個 dotnet 工具，如果是用 Visual Studio，會交由擴充管理員來安裝工具，那在沒有 Visual Studio 的協助下呢？這裡會介紹如何使用 CLI 來安裝 dotnet-ef。

### Requirements

首先準備好環境，以我為例：
- macOS 10.15
- .NET Core 3.1

環境確認後，透過以下指令，列出已安裝的 dotnet tool。
``` bash
dotnet tool list
dotnet tool list -g
```
{{< alert theme="success" >}}
g：全域
{{< /alert >}}

### Install

透過 dotnet CLI 來安裝工具，這裡介紹安裝 dotnet-ef 工具到全域或本地。
- Global
  ``` bash
  dotnet tool install dotnet-ef -g
  ```
- Local
  安裝本地工具，需要定義 manifest file。首先準備好 dotnet project，在專案下執行以下命令，它會產生一個 dotnet-tools.json 記錄本地工具。
  ``` bash
  dotnet new tool-manifest
  dotnet tool install
  ```
  .config/dotnet-tools.json
  ``` json
  {
    "version": 1,
    "isRoot": true,
    "tools": {
      "dotnet-ef": {
        "version": "3.1.3",
        "commands": [
          "dotnet-ef"
        ]
      }
    }
  }
  ```

### Update

更新 dotnet tool，一樣分為全域和本地，如下示範：
- Global
  ``` bash
  dotnet tool update -g dotnet-ef
  ```
- Local
  ``` bash
  dotnet tool update dotnet-ef
  ```

### Uninstall

卸載已安裝的工具：
- Global
  ``` bash
  dotnet tool uninstall -g dotnet-ef
  ```
- Local
  ``` bash
  dotnet tool uninstall dotnet-ef
  ```

### Reference

[.NET Core tools](https://docs.microsoft.com/zh-tw/dotnet/core/tools/global-tools)