---
title: "NoSQL CAP"
date: 2022-07-11T21:02:00+08:00
draft: true
tags: ["nosql", "database",]
---

在計算機科學中，它指出對於一個分布式計算系統來說，不可能同時滿足CAP<!--more-->
> 此篇為各筆記之整理，非原創內容，資料來源可見文後參考資料連結。

----

#### CAP
- Consistency 一致性 : 所有節點訪問同一份最新的數據副本。
- Availability 可用性：每次請求都能獲取到非錯的響應，但是不保證獲取的數據為最新數據。
- Partitioning Tolerance 分區容錯性：系統中任意信息的丟失或失敗不會影響系統的繼續運作。

----

#### Database
- AP Database — 犧牲一致性的分散式資料庫，例如：Amazon DynamoDB
- CP Database — 犧牲可用性的分散式資料庫，例如：MongoDB

----

#### Transaction
分散式架構下我們無法僅靠 transaction 就確保一致性，必須額外搭配共識（Consensus）的機制  

----

#### Two Phase Commit
為了使分布式系統架構下的所有節點，在進行事務提交時保持一致性，而設計的一種演算法

----

[參考連結](https://blog.longwin.com.tw/2013/03/nosql-db-choose-cap-theorem-2013/)  
[參考連結](https://zh.wikipedia.org/zh-tw/%E4%BA%8C%E9%98%B6%E6%AE%B5%E6%8F%90%E4%BA%A4)

----