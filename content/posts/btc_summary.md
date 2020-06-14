---
title: "加密貨幣交易所：Bitcoin Implementation 1"
date: 2020-06-14T15:52:22+08:00
description: Introduce how to reference bitcoin in the exchange.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Bitcoin
- Exchange
categories:
- Blockchain
---

身為一個加密貨幣交易所的後台工程師，記錄一下我如何與比特幣(Bitcoin)一起工作。

### Bitcoin

比特幣(Bitcoin)是一種基於去中心化，採用點對點網路與共識主動性，開放原始碼，以區塊鏈作為底層技術的加密貨幣，目前由社群共同維護這個協議([BIP](https://github.com/bitcoin/bips))。而比特幣何運作主要由四個功能組合而成：
- 錢包(wallet)
- 區塊鏈(blockchain)
- 節點(node)
- 礦工(miner)

![Alt text](/images/btc-function.png)

### Exchange

加密貨幣交易所是一種中心化，提供兌換和投資加密貨幣的服務，把加密貨幣視為金融商品。以比特幣兌換服務來說，A 想用 US$9000 買一顆比特幣，當 A 支付完 US$9000，透過交易所錢包建立一個交易，傳至一個比特幣節，然後等到礦工確認完這個交易，A 的比特幣錢會收到一顆比特幣。從這個說明可看出，交易所通常只需要使用比特幣錢包和節點的功能。後續會介紹怎麼在交易實作這二個功能。

### Reference

參考來源

[bitcoin](https://bitcoin.org/)