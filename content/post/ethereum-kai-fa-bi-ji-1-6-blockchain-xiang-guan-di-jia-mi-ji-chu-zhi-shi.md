---
title: "Ethereum 開發筆記 1–6：Blockchain 相關的加密基礎知識"
date: 2018-09-09T12:03:29+08:00
draft: false
tags:
- ethereum
- blockchain
- merkle tree
---

Blockchain 裡應用了一些加密技術來保證及驗證交易訊息的正確性，這也更加強了 Blockchain 資料不可竄改的特性。我們來介紹其中比較重要的「公私鑰加密」以及「Merkle Tree」加密樹。

## 公私鑰加密

公私鑰加密算法是目前資訊通訊安全的基石，它保證了加密訊息不可被破解，相關的加解密原理大家可以參考這兩篇文章：

1. RSA算法原理（一）http://www.ruanyifeng.com/blog/2013/06/rsa_algorithm_part_one.html
2. RSA算法原理（二）http://www.ruanyifeng.com/blog/2013/07/rsa_algorithm_part_two.html

## 加密與解密

公私鑰加密方法是一種非對稱式加密，透過公鑰加密過後的訊息只有私鑰可以解密，也因此只要保護好私鑰就能保證資訊的安全。

現在假設 Alice 要傳一個訊息給 Bob，希望訊息加密過後只有 Bob 可以解密，大概會經過如下步驟：

1. Bob 傳他的公鑰給 Alice
2. Alice 使用 Bob 的公鑰加密訊息
3. Alice 將加密過後的訊息傳給 Bob
4. Bob 用他的私鑰解密訊息

我們這邊使用 openssl 來練習一下加密與解密，首先我們來產生一對公私鑰：

```
// Create RSA private key
$ openssl genrsa -des3 -out rsa-key.pem 2048
// Create public key
$ openssl rsa -in rsa-key.pem -outform PEM -pubout -out rsa-key-pub.pem
```

其中 rsa-key.pem 就是私鑰，rsa-key-pub.pem 為公鑰，私鑰會要求設置密碼，請妥善記下密碼。

我們先用 rsa-key-pub.pem 加密資料：

```
openssl rsautl -encrypt -pubin -inkey rsa-key-pub.pem -in helloworld.txt -out helloworld.enc
```

其中 helloworld.enc 就是被加密過後的資料，接下來我們用 rsa-key.pem 來解密：

```
openssl rsautl -decrypt -inkey rsa-key.pem -in helloworld.enc -out helloworld2.txt
```

解密過後我們查看 helloworld2.txt 就會發現內容與 helloworld.txt 完全一致，我們成功解密了。

## 簽名與驗證

有時我們會想知道訊息是否是由本人傳送的，這時我們就會使用公私鑰來做簽名與驗證，大致會經過如下步驟：

1. Bob 傳他的公鑰給 Alice
2. Bob 將他的訊息使用私鑰進行簽名
3. Bob 將他的訊息以及簽名傳給 Alice
4. Alice 使用 Bob 的公鑰驗證簽名是否來自 Bob
5. 我們這邊使用 openssl 來練習一下簽名與驗證，首先我們使用私要進行簽名：

```
openssl dgst -sign rsa-key.pem helloworld.txt > signature.bin
```

接下來我們要用公鑰驗證簽名：

```
openssl dgst -verify rsa-key-pub.pem -signature signature.bin helloworld.txt
```

我們會發現如果沒有正確的簽名，或內容不正確就無法通過驗證，也因此我們可以知道能通過驗證的內容就是沒有被竄改過且簽名也一定是來自本人。

通常實務上我們會將公私鑰加解密與簽名驗證一起使用，以確保資訊傳遞的安全。

## Merkle Trees

Blockchain 中的交易會組成 Merkle Tree 的形式，再將 Merkle Root 放入 Blockchain 的區塊中，Merkle tree 的資料結構如下：

- 樹葉節點放的就是資料
- 父節點放的是子節點資料的 hash 值

有了這樣的 Merkle Tree，我們就可以：

- 用 hash 來代表資料，即使是很大量的資料也可以用一個簡單的 hash 來表示
- 如此要下載資料與驗證資料就可以同步進行，這在分散式系統非常重要

底下有一個影片前顯易懂地說明了 Merkle Tree 的結構。

<iframe style="margin-top: 30px;" width="560" height="315" src="https://www.youtube.com/embed/h1wzzhkHfTk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

以上就是學習 Blockchain 必備的一些相關的加密基礎知識。
