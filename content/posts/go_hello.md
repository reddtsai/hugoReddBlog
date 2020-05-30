---
title: "初次見面 Golang"
date: 2020-05-29T22:49:38+08:00
description: 建立 golang 開發環境，進入第一個專案
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

第一次發 Golang 的文章，當然要來個 hello world 作為開場。

{{< alert theme="info" >}}
環境
OS    ：Mac
Golang：1.14
Editer：Visual Studio Code
{{< /alert >}}

### Installation

安裝 Go。
``` bash
brew install go@1.14

go version
```

{{< alert theme="info" >}}
[Stable versions](https://golang.org/dl/)
{{< /alert >}}

### First Package

環境準備就緒後，來建立第一個 Go Project。
``` bash
mkdir hello
cd hello
touch hello.go
code .
```
example. hello.go
``` go
package main

import "fmt"

func main() {
	fmt.Println("hello world")
}
```
來看到 hello.go 的內容，作個基本認識：
1. package
    - 空間(space)，其它語言稱為 namespace，一個 package 有一個或多個 .go 檔，但請記住要存放在同個目錄下(c# 很愛建目錄🤨🤨)。
    - 命名(naming)，建立 package 第一步是命名，請先參考 [package names](https://blog.golang.org/package-names) 瞭解命名規則。
2. import
    - 導入 local 或 remote 套件。
3. function
    - 名稱(name)，func name()
    - 輸入(input)，func(input)
    - 輸出(output)，func() (output)

執行 hello.go 顯示 hello world
``` bash
go run hello.go
```

### Modules

接著是套件管理，從 Go 1.11 開始有了 [Go Modules](https://blog.golang.org/using-go-modules)，這裡只要先有個觀念，不論 local 或 remote 套件請使用 Go Modules 來管理。

{{< alert theme="info" >}}
CLI - go mod
{{< /alert >}}

### Testing

當 Package 完成後，別忘了 go 有個很好用的 test framework。
example. hello_test.go
``` go
package main

import (
	"testing"
)

func TestHello(t *testing.T) {
	t.Log("hello world")
}
```
執行測試
``` bash
go test -v
```

### Tour of Go

如果你是第一次學習程式，對 Go 毫無概念，可從線上 Tour 開始。
[Tour of Go](https://tour.golang.org/welcome/1)

### Editer

介紹一個免費且功能強大的程式碼編輯器 [Visual Studio Code](https://code.visualstudio.com)，讓你編輯專案更有效率。
1. 下載安裝
    至[官網](https://code.visualstudio.com/download)下載最新的安裝程式，執行並安裝。
2. 安裝 Go 擴充工具
    開啟 Visual Studio Code，移至 Extensions 頁面，安裝 Go。

### Reference

[GO](https://golang.org/doc/install)