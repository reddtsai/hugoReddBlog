---
title: "認識運動彩卷賠率"
date: 2020-07-12T21:24:38+08:00
description: 籃球賠率計算
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Margin
categories:
- Bet
libraries:
- mathjax
---

### Odds(賠率)

假設籃球A隊和B隊他們的對戰記錄如下表：

| 對戰 | A | B |
| ------ | ------ | ------ |
| 1 | 勝 | 負 |
| 2 | 勝 | 負 |
| 3 | 勝 | 負 |
| 4 | 負 | 勝 |
| 5 | 勝 | 負 |
| 6 | 勝 | 負 |
| 7 | 勝 | 負 |
| 8 | 勝 | 負 |
| 9 | 負 | 勝 |
| 10 | 勝 | 負 |

籃球比賽的結果只有勝跟負(2-way)，當不考慮其它因素，那麼勝率的倒數就是它的賠率。
- A隊
    贏了8場，勝率80%，賠率為 ${1 \over 0.8} = 1.25$，如果你投注100，獲勝時可贏得25。
- B隊
    贏了2場，勝率20%，賠率為 ${1 \over 0.2} = 5$，如果你投注100，獲勝時可贏得400。

賠率的觀念就介紹到這，想了解更多可進到台灣運彩官網，可以看到各個隊伍的數據，實際賠率會由這些數據來計算。

### Margin(利潤)

比賽賠率計算，除了隊伍的資訊因素還保含了莊家的利潤(margin)，以剛剛的賠率為例，不包含利潤的公式：
$$1 = {{1 \over Odds-A} + {1 \over Odds-B}}$$
假設莊家想獲得總投注5%的利潤，那麼計算公式就會變成這樣：
$$1.05 = {{1 \over Odds-A} + {1 \over Odds-B}}$$

### Reference

[台灣運彩](https://www.sportslottery.com.tw)
[calculator](https://www.pinnacle.com/en/betting-resources/betting-tools/margin-calculator)