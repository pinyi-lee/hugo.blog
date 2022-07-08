---
title: "TOTP"
date: 2022-08-01T22:00:00+08:00
draft: true
tags: ["security",]
---

是一種根據預共享的金鑰與當前時間計算一次性密碼的演算法<!--more-->
> 此篇為各筆記之整理，非原創內容，資料來源可見文後參考資料連結。

----

#### TOTP 
- 是雜湊訊息鑑別碼當中的一個例子。
- 結合一個私鑰與當前時間戳，使用一個密碼雜湊函式來生成一次性密碼。 
- 時間戳通常以30秒為間隔。 

[參考連結](https://zh.wikipedia.org/zh-tw/%E5%9F%BA%E4%BA%8E%E6%97%B6%E9%97%B4%E7%9A%84%E4%B8%80%E6%AC%A1%E6%80%A7%E5%AF%86%E7%A0%81%E7%AE%97%E6%B3%95)

----

#### Start Step 
1. Server 生成帶有 Secret Key 的 QRCode
2. 用 Authentication App 掃描 QRCode，App 就會把 Secret Key 存起來
3. 以後 App 就會用這個 Secret Key 生成 TOTP
4. Server 驗證兩邊生成相同的 TOTP，確保 Secret Key 正確
5. Server 啟動 TOTP 成功

> 因為 App 希望每 30 秒產生出不同的 TOTP，所以他會把 timestamp/30 的整數部分跟 Secret Key 這兩個東西丟進去 HMAC 做雜湊，雜湊的結果轉成十進位取最後六碼

[參考連結](https://medium.com/starbugs/totp-2fa-algorithm-in-10-mins-25acc3c35df9)

----

#### Security
- 因為 TOTP 有六位數，所以有 100 萬種可能性，而且每 30 秒就換一個
- 如果你想要在 30 秒內猜出來，那每秒就需要進行超過 3 萬次登入
- 除非你的 secret key 一開始就被偷走，不然是足夠安全的

[參考連結](https://medium.com/starbugs/totp-2fa-algorithm-in-10-mins-25acc3c35df9)

----

#### IBM TOTP API 

[參考連結](https://www.ibm.com/docs/zh-tw/security-verify?topic=areuc-using-time-based-one-time-password-totp-in-api-client)

----