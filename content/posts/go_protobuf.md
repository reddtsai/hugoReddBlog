---
description: using go modules
draft: true
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

``` bash
brew install protobuf
brew --version
```

``` bash
go get -u -v github.com/golang/protobuf/proto
go get -u -v github.com/golang/protobuf/proto-gen-go
```

``` bash
sudo cp /Users/reddtsai/go/bin/protoc-gen-go /usr/local/bin/
```

``` bash
mkdir protoDir
cd protoDir
touch main.go foo.proto
```

``` bash
syntax = "proto3";
package protobuf;

message foo {
    int64 id = 1;
    string name = 2;
}
```

``` bash
protoc --go_out=. *.proto 
```

``` go
```