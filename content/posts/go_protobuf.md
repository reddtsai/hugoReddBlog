---
title: "使用 Protocol Buffers 結構化資料 In Go"
date: 2020-07-19T23:37:24+08:00
description: how to use protocol buffers with go
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

因為工作上需要使用 Protocol Buffers 當作資料交換的結構，那麼就來看看 Protocol Buffers 有什麼過人之處。

{{< alert theme="info" >}}
以下說明所使用的環境
OS    ：Mac
Golang：1.14
Editer：Visual Studio Code
{{< /alert >}}

### Protocol Buffers

Protocol Buffers 是一種序列化資料結構，由 Google 設計維護，目前有二個版本 proto2 和 proto3，主打比其它資料結構更小、更容易使用。

### Insatll

1. 安裝 protocol compiler：
    ``` bash
    brew install protobuf
    brew --version
    ```
2. 安裝 protobuf runtime：
    因為我是使用 golang，所以安裝 golang 的 protobuf runtime。
    ``` bash
    go get -u -v github.com/golang/protobuf/proto
    go get -u -v github.com/golang/protobuf/protoc-gen-go
    ```
    設定環境變數
    ``` bash
    sudo cp $(gopath)/bin/protoc-gen-go /usr/local/bin/
    ```

### .proto

安裝完 Protocol Buffers 後，接著建立一個 user.proto，Protocol Buffers 定義檔的副檔名為 .proto。
``` bash
mkdir goprotobuf/model
cd goprotobuf/model
touch user.proto
```
user.proto
``` bash
syntax = "proto3";
package protomvc;

option go_package = ".;model";

message user {
    int64 id = 1;
    string name = 2;
    string email = 3;
    string addr = 4;
    string phone = 5;
}
```
可以看到 user.proto 的內容是有逹到 Google 所宣稱的容易簡單，其它 message type 可參考[available wire types](https://developers.google.com/protocol-buffers/docs/encoding)。

### .pb.go

當定義好 Protocol Buffers 資料傳輸結構後，接著將 user.proto 轉成 go struct，加入我們的 go package，透過 protocol compiler 來產生 user.pb.go。
``` bash
protoc --go_out=. *.proto 
```
user.pb.go
``` go
// ~~~~ 略

type User struct {
	state         protoimpl.MessageState
	sizeCache     protoimpl.SizeCache
	unknownFields protoimpl.UnknownFields

	Id    int64  `protobuf:"varint,1,opt,name=id,proto3" json:"id,omitempty"`
	Name  string `protobuf:"bytes,2,opt,name=name,proto3" json:"name,omitempty"`
	Email string `protobuf:"bytes,3,opt,name=email,proto3" json:"email,omitempty"`
	Addr  string `protobuf:"bytes,4,opt,name=addr,proto3" json:"addr,omitempty"`
	Phone string `protobuf:"bytes,5,opt,name=phone,proto3" json:"phone,omitempty"`
}

// ~~~~略
```

{{< alert theme="info" >}}
user.pb.go 的內容 是由 protocol compiler 所自動生成，請不要直接修改。
{{< /alert >}}

### Marshal/Unmarshal

接下來建立一個 user 資料，然後產生 Protocol Buffers 序列資料，範例如下：
``` go
func main() {
	user := &model.User{
		Id:    1,
		Name:  "john",
		Email: "john@gmail.com",
		Addr:  "taipei",
		Phone: "0212348765",
	}
	buf, err := proto.Marshal(user)
	if err != nil {
		fmt.Println(err)
	}
    fmt.Println(buf)
    newUser := &model.User{}
	err = proto.Unmarshal(buf, newUser)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(newUser)
}
```

{{< alert theme="warning" >}}
這裡特別需注意 grpc，protobuf 和 protoc-gen-go 版本是否相容。
假設當中某一個的版本較高，可能會有找不到方法的問題 proto.Marshal: missing method ProtoReflect。
{{< /alert >}}

### Polymorphism

接著說說 Protocol Buffers 的進階用法，例如你回應內容可能會有多種類型，Protocol Buffers 也提供了 any 的類型，比如：
``` bash
syntax = "proto3";
package model;

import "google/protobuf/any.proto";
option go_package = ".;model";

message body1 {
    string name = 1;
}

message body2 {
    int age = 1;
}

message Response {
    google.protobuf.Any message = 2;
}
```
Protocol Buffers any 在 golang 的方法：
``` go
import (
	"github.com/golang/protobuf/ptypes"
	"google.golang.org/protobuf/proto"
)

func main() {
    body := &model.body1{
		Name: "redd1",
	}
	m, err := ptypes.MarshalAny(body)
	if err != nil {
		fmt.Println(err)
    } 
    fmt.Println(m.TypeUrl)

}
```
UnmarshalAny 需先判斷資料的 TypeUrl，在使用相對定義結構來反序列。

### Cap

最後看看 Google 宣稱的容量比較小，以下的範列 Protocol Buffers 容量 44，JSON 容量 96。
``` go
func main() {
	user := &model.User{
		Id:    1,
		Name:  "john",
		Email: "john@gmail.com",
		Addr:  "taipei",
		Phone: "0212348765",
	}

	bufProto, err := proto.Marshal(user)
	if err != nil {
		fmt.Println(err)
	}

	bufJson, err := json.Marshal(user)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(cap(bufProto))
	fmt.Println(cap(bufJson))
}
```

### Reference

[Protocol Buffers](https://developers.google.com/protocol-buffers)
[golang protobuf](https://github.com/golang/protobuf/)