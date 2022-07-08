---
title: "SQL ACID"
date: 2022-07-11T21:03:00+08:00
draft: true
tags: ["sql", "database",]
---

指資料庫管理系統在寫入或更新資料的過程中，為保證事務（transaction）是正確可靠的，所必須具備的四個特性<!--more-->
> 此篇為各筆記之整理，非原創內容，資料來源可見文後參考資料連結。

----

#### ACID
- Atomicity 原子性 : 一個事務中的所有操作，或者全部完成，或者全部不完成，不會結束在中間某個環節。
- Consistency 一致性 : 在事務開始之前和事務結束以後，資料庫的完整性沒有被破壞。
- Isolation 隔離性 : 資料庫允許多個並發事務同時對其數據進行讀寫和修改的能力，隔離性可以防止多個事務並發執行時由於交叉執行而導致數據的不一致。
- Durability 永久性 : 事務處理結束後，對數據的修改就是永久的，即便系統故障也不會丟失。

----

#### 不預期的錯誤
- 髒讀 Dirty Read
  > 一個事務在處理過程中讀取了另外一個事務未提交的數據。

- 不可重複讀 Non-repeatable read
  > 一個 Transaction範圍內，多次查詢某個數據，卻得到不同的結果。

- 幻讀 Phantom Read
  > Transaction A 讀取與搜索條件相匹配的若干行。  
    Transaction B 以插入或刪除行等方式來修改 Transaction A 的結果集合，然後再提交。 
    Transaction A 再次讀取與搜索條件相匹配的若干行，兩次筆數不相同。

----

#### 事務隔離級別
- 未提交讀（Read uncommitted）
  > 這是最低的隔離層級，SELECT 指令可以讀取其他 transaction 尚未 commit 的結果，如果這個尚未被 commit 的結果後來被 rollback 了，就會讀取到被取消的資料，也就是剛剛提過的「髒讀」  

  > 這種隔離層級可能產生：髒讀、不可重複讀、幻讀

- 提交讀（read committed）
  > 這個隔離層級只允許讀取其他 transaction commit 過的資料，因此可以解決髒讀的問題，不過如果一個 transaction 的兩個 SELECT 語法間有另一個交易 commit 了新資料，會造成第一次讀取與第二次讀取結果不一致的問題，也就是上面介紹過的不可重複讀  
  
  > 這種隔離層級可能產生：不可重複讀、幻讀

- 可重複讀（repeatable read）
  > 不會考慮別的 transaction 的修改，同一個 transaction 內，除非自己修改，否則多次 SELECT 的結果都會相同，解決了不可重複讀的問題  

  > 這種隔離層級可能產生：幻讀

- 串行化（Serializable）
  > 一個用效能換取一致性的隔離層級，讓所有 transaction 序列化執行，避免併發可能會造成的問題。

----

[參考連結](https://ithelp.ithome.com.tw/articles/10247034)  
[參考連結](https://zh.wikipedia.org/zh-tw/ACID)

----