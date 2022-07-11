---
title: "資料庫的擴展"
date: 2022-07-11T21:00:00+08:00
draft: false
tags: ["database",]
---

分庫 & 分表<!--more-->

----

#### CAP
- 分庫：降低單台機器的負載，CPU瓶頸、網路連線數瓶頸、IO瓶頸
- 分表：提高數據訪問的效率，CPU瓶頸、IO瓶頸

> Partition分片、Sharding分區，兩個都有分而治之的核心概念，差別在於Sharding是指資料分佈在多台機器節點上，但Partition沒有

----

#### 種類
- 讀/寫分離 Read/Write Splitting
- 垂直分庫 Vertical Sharding (不同業務、不同資料庫)
- 水平分庫 Horizontal Sharding
- 垂直分表 Vertical Partition (就是把欄位做分表)
- 水平分表 Horizontal Partitioning (ex: tab_0000, tab_0001...)

----

[參考連結](https://ithelp.ithome.com.tw/articles/10250117)

----