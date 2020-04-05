---
title: "Dotnet_tool"
date: 2020-04-04T15:35:18+08:00
description: 
draft: true
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- .Net
categories:
- .Net
---


```
dotnet tool list
dotnet tool list -g
```

Install a global tool
```
dotnet tool install -g
```

local manifestfile
Install a local tool
```
dotnet new tool-manifest
dotnet tool install
```

update tool
```
dotnet tool update
```