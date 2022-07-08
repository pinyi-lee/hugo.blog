---
title: "Nats Cluster"
date: 2022-07-11T21:01:00+08:00
draft: true
tags: ["nats",]
---

您可以將服務器集群在一起，以實現大容量消息傳遞系統<!--more-->
> 此篇為各筆記之整理，非原創內容，資料來源可見文後參考資料連結。

----

#### Connecting to a Cluster
```golang
servers := []string{"nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"}
nc, err := nats.Connect(strings.Join(servers, ","))
if err != nil {
    log.Fatal(err)
}
defer nc.Close()
```

[參考連結](https://docs.nats.io/using-nats/developer/connecting/cluster)

----

#### Cluster Test

```html
brew services install nats-server
brew install nats-io/nats-tools/nats

視窗一  
nats-server -p 4222 -cluster nats://localhost:4248 --cluster_name test-cluster

視窗二  
nats-server -p 5222 -cluster nats://localhost:5248 -routes nats://localhost:4248 --cluster_name test-cluster

視窗三  
nats-server -p 6222 -cluster nats://localhost:6248 -routes nats://localhost:4248 --cluster_name test-cluster

視窗四  
nats sub -s "nats://127.0.0.1:4222" hello

視窗五  
nats pub -s "nats://127.0.0.1:5222" hello world_5222 
nats pub -s "nats://127.0.0.1:6222" hello world_6222
```

[參考連結](https://docs.nats.io/running-a-nats-service/configuration/clustering)

----

#### Nats Topology
- Server
- Cluster
- Super Cluster
- Leaf Node


[參考連結](https://dev.to/karanpratapsingh/nats-topology-318g)


----