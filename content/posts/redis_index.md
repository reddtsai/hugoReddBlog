---
title: "Redis 有序集合"
date: 2020-07-25T23:37:24+08:00
description: how to use Redis sorted sets
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Redis
categories:
- Database
---

Redis 已被廣泛的運用在快取機制上，但是如果想查找符合條件的資料，或是某個範圍的資料時，除了用 scan 一個一個查找符合的資料，有沒有更好的方法？

### Sorted Sets

Redis 有序集合是由 score 和 member 所組成，score 用來排序可重複，member 是個鍵值不可重複，來看看官方的範例：
這個範例 score 指的是年齡，member是姓名，這是個由年齡排序的集合。

| score | member |
| ----- | ------ |
| 33 | 小明 |
| 38 | 小張 |
| 38 | 老王 |
| 40 | 大雄 |


### ZADD

這裡介紹 ZADD 命令來添加有序集合的元素。
``` bash
ZADD AGEINDEX 38 "老王"
ZADD AGEINDEX 40 "大雄"
ZADD AGEINDEX 38 "小張" 33 "小明"
```

{{< alert theme="info" >}}
Redis 2.4 版後才可使用一次加入多個元素
{{< /alert >}}

命令選項(options)：
- XX：更新已存在元素，不會新增元素。
- NX：新增元素，不會更新已存在元素。
- CH：回覆命令執行後，元素被更新和新增數量。
- INCR：增加元素 score。
``` bash
ZADD AGEINDEX XX 34 "小美"
```

{{< alert theme="info" >}}
Redis 3.0.2 版後才可使用 options
{{< /alert >}}

### ZRANGE

Redis 有序集合順序是由小到大，第一個位置是 0，第二個位置是 1，...，-1 是倒數第一，-2 是倒數第二，...。
查找倒數第二個元素：
``` bash
ZRANGE AGEINDEX -2 -2
```
查找第二到第三的元素：
``` bash
ZRANGE AGEINDEX 1 2
```

{{< alert theme="info" >}}
由大至小排序可使用 ZREVRANGE
{{< /alert >}}

### ZRANGEBYSCORE

ZRANGEBYSCORE 是透過 score 來查找元素，例如例出 30 歲到 40 歲的成員：
``` bash
ZRANGEBYSCORE AGEINDEX 30 40
```
例出 30 歲以上到 40 歲的成員：
``` bash
ZRANGEBYSCORE AGEINDEX (30 40
```

### Reference

[Redis sorted Sets](https://redis.io/commands/zadd)