---
title: "Elasticsearch Notes"
date: 2022-07-12T10:00:00+08:00
draft: true
tags: ["elasticsearch",]
---

因緣際會上了 Elasticsearch 課程，把覺得重要的筆記一下<!--more-->
> 此篇為各筆記之整理，非原創內容，資料來源可見文後參考資料連結。

----

#### query and filter

- query 根據相關性演算法計分，依照相關性高低回傳
- filter 不涉及計分，而且 filter 的結果會被 cache 起來

[參考連結](https://blog.csdn.net/laoyang360/article/details/80468757)

----

#### inverted index 、 segment file

- apache lucene 會將每筆被 index 的資料存成名叫 inverted index 的資料結構
- inverted index 中會存放各個文件中有出現的 terms，並記錄位置、偏移量、詞頻等
- inverted index 大到一定程度時，才寫成為一個 segment file
- segment file 不會再做任何更新或變動，只供讀取
- 一個實際的搜尋動作，會使用到許多 segment files

----

#### update api

> 並不會真的更新文件內容

- 從該文件的 _source 欄位中取出原內容
- 對該 json 進行修改
- 將舊文件標示為刪除
- 以新的 json 文件內容重新 index

----

#### seq no & primary term

> 用於確保在分散式架構上 cluster 內資料的一致性

- _primary_term：primary shard 改變時+1
- _seq_no：所有 primary shard 上的 operation 都+1
- _version：提供外部使用上能確保資料存取的順序性  

[參考連結](https://www.modb.pro/db/33685)

----

#### searching request work

> from 5, size 5  
搜尋時，各個 shard 都會取 10 個  
因此拉回 global priority queue 的筆數是 (from + size) * primary shard 數量

可以參考使用 [scroll](https://kucw.github.io/blog/2018/6/elasticsearch-scroll/)

----

#### score

> 資料少、Shard多時，(5個Shard，3個資料)  
分數會差異不大，因為每個資料都是單一Shard的唯一資料  
意思是，分數的比較，是跟單一Shard算出來的

----

#### persistence model

> transaction log  
1.每個操作都會被存成一條 transaction log  
2.每份被 index 的文件都會被 analyzed 並存在 memory buffer 中  
3.在 memory buffer 中 的文件不可以被搜尋到  
4.每 5s 把 transaction log 寫入 disk  

> search refresh  
1.每秒執行，執行後文件才能被搜尋到  
2.呼叫 lucene flush，此操作將把 memory buffer 中的文件寫入 directory  
3.位於 lucene directory 的文件可以被搜尋到，但還沒有 fsync 到磁碟上  

> search flush  
1.進行 lucene commit & 清理 transaction log  
2.lucene commit 昂貴，還沒 commit 的資料可能會遺失 (當機)   
3.因此利用 transaction log 做保障，commit 後，transaction log 也就可以清掉了  

> merge  
1.當 segment file 數量太多，lucene 會進行 segment merge  
2.merge 時，同時會移除被 delete 的文件  

[參考連結](https://i.imgur.com/Alqjr11.jpg)

----

#### object & nested

object 儲存到 es 的時候是被展打儲存的，可以改用 nested

```
以下是 object 範例
PUT mytest/doc/1
{
    "group": "fans",
    "user": [
        { "first": "John", "last": "Smith" },
        { "first": "Alice", "last": "White" }
    ]
}

轉換後的內部文檔
{
    "group": "fans",
    "user.first": [ "alice", "john" ],
    "user.last": [ "smith", "white" ]
}
```

[參考連結](https://kucw.github.io/blog/2018/6/elasticsearch-nested/)

----

#### routing paramter

> search 預設行為是 query 所有的 node  
但我們可以讓特定 document index 時，就放在某台 node 上  
search 時就不需用詢問全部 nodes  
可在建立 mapping 時，指定存取此 index 時必須在參數指定 routing value  

----

#### expensive queries
- prex/wildcard query
- fuzzy query
- regexp query
- script query
- join query

----

#### aggregation request work
> size 3  
因為是各個 shard 先取 3 個，放到 global priority queue 才 aggregation  
所以 aggregation 數字會失準

可以參考以下兩個回傳值  

doc count error upper bound，表示每個 shard 回傳的最後一筆資料，數字的加總  
若此數字為0，表示 aggregation 數字是準的

sum other doc count，表示每個 shard有多少資料沒有回傳  

----