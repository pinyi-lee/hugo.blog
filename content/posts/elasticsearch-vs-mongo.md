---
title: "Elasticsearch vs Mongodb"
date: 2022-07-08T14:00:00+08:00
draft: true
tags: ["elasticsearch", "mongodb", "database",]
---

為了考慮吞吐量，從 sql 轉移到 分散式 db，我們來比較 Elasticsearch 與 Mongodb<!--more-->
> 此篇為各筆記之整理，非原創內容，資料來源可見文後參考資料連結。

----

#### Elasticsearch 弱項
- 缺乏 transaction
- join 功能弱
- aggregation 功能弱
- 運行時大量資料存於記憶體中，不適合作為 data warehouse

----

#### Elasticsearch 有更強的全文檢索

> Elasticsearch was originally designed to support full text search, 
and provides advanced features to support search, such as tokenizers, token filters and analyzers. 
It is also commonly used for log analysis, forming part of the popular Elasticsearch, Logstash and Kibana (ELK) stack.

> MongoDB is more suitable to manage NoSQL data requiring create, read, update and delete (CRUD) operations. 
It offers high scalability, reliability, and performance. MongoDB also uses text-based indexes for full-text queries, 
but the search is slow, and the search server does not provide tokenizers and analyzers like Elasticsearch does.

[參考連結](https://cloud.netapp.com/blog/cvo-blg-elasticsearch-vs-mongodb-6-key-differences)

----

#### MongoDB 有更好的吞吐

> MongoDB is built for 1) high write and 2) update throughput without causing high CPU and disk I/O issues.

[參考連結](https://logz.io/blog/elasticsearch-vs-mongodb/)

----

#### MongoDB 為主、Elasticsearch 為輔

> The typical use case and the most widely used scenario is that ElasticSearch is a sink in a data pipeline and with another system/database mastering the data. In case of data loss, there is a way to replay data from upstream.

[參考連結](https://medium.com/@merrinkurian/elasticsearch-as-the-primary-database-5e41b2a0189d)

----

#### Elasticsearch 的中文字分詞功能偏弱
- 可以利用爬字典(wiki)、餵食插件，讓他有更好的分詞功能

----