---
title: "Golang 套件管理系統 mod"
date: 2020-07-02T14:23:01+08:00
description: using go modules
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Go
categories:
- Go
---

Golang 官方在 1.11 版推出了 [Go modules](https://blog.golang.org/using-go-modules)，讓開發者透過 go mod 來管理 project 或 package 中所使用的套件。

### Init

假設今天需要建立一個 RESTful 專案，內容如下：
``` bash
mkdir api
cd api
touch main.go
```
main.go
``` go
package main

import "github.com/gin-gonic/gin"

func main(){
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "pong",
		})
	})
	r.Run()
}
```
可以看到這裡有使用 Gin 這個套件，接下來透過 go mod 來添加這個套件，首先需初始 go modules：
``` bash
go mod init api
```
完成後會產生一個 modules 設定檔 go.mod，接下來當你執行 build、run、test 等命令，modules 會自動幫你加入相依套件，當然你還是可以透過 get 來取得套件，下方是這次 go.mod 的內容：
``` go
module api

go 1.14

require github.com/gin-gonic/gin v1.6.3 // indirect
```


{{< alert theme="info" >}}
Golang 提供一個環境變數 GO111MODULE 來控制是否啟用 go modules
{{< /alert >}}

### Get

可使用以下方法來取得或更新相依套件：
``` bash
# 透過 go.mod 來取得或更新相依套件
go mod download
```
``` bash
# 使用指令來取得或更新相依套件
go get [-u]
```

如果有使用到私有套件，需要在 go 環境變數 GOPRIVATE 加入你的私有套件網域名，另外，你會需要透過登入或SSH來取得私有套件的權限。例設一個 private gitlab 套件，gitlab.com/name/repo，設定如下：
``` bash
go env -w GOPRIVATE=gitlab.com/name
```

### List

列出目前使用到的相依套件：
``` bash
go list -m all
```

### Clean

清除未使用的套件，執行前最好先清除暫存。
``` bsah
go clean
go mod tidy
```

### Reference

[Go modules](https://blog.golang.org/using-go-modules)