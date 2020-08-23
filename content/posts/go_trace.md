---
title: "Go 分析工具 Trace"
date: 2020-08-23T21:11:40+08:00
description: a tour of trace in go
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

程式開發的過程中，你可能需要解決效能瓶頸的問題(Bottleneck)或分析程式內部調用的狀況，Golang 也提供了這方面的工具來幫助你分析，這裡記錄一下如何使用。

### Trace

首先，將 runtime/trace 這個套件加入你想分析的程式中，透過它來收集程式執行期間所有的活動(such as goroutine creation/blocking/unblocking, syscall enter/exit/block, GC-related events)。
1. 範例程式
    ``` go
    package main

    import (
        "os"
        "runtime/trace"
    )

    func main() {
        trace.Start(os.Stderr)
        defer trace.Stop()
        ch := make(chan int)
        go func() {
            ch <- 42
        }()
        <-ch
    }
    ```
2. 產生 trace file
    ``` bash
    go run main.go 2> trace.out
    ```
3. 開啟診斷工具
    ``` bash
    go tool trace trace.out
    ```

{{< alert theme="info" >}}
診斷工具會透過瀏灠器開啟，有些分析會由圖表呈現，需要先行安裝 Graphviz。
{{< /alert >}}

### Reference

[Go Trace](https://golang.org/pkg/runtime/trace/)
[Go Diagnostics](https://golang.org/doc/diagnostics.html)
[Example](https://github.com/reddtsai/goreddprints/tree/master/trace)