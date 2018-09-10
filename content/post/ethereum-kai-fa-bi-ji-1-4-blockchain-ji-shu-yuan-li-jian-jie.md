---
title: "Ethereum 開發筆記 1–4：Blockchain 技術原理簡介"
date: 2018-09-08T15:22:20+08:00
draft: false
tags:
- ethereum
- blockchain
- bitcoin
---

之前我們簡單地介紹過 Blockchain 了，但我們還是對 Blockchain 背後的技術原理不是那麼了解，我們知道 Blockchain 是因為一個數位貨幣帳本這樣的概念被創造出來的，而數位貨幣最擔心的是什麼問題呢？其實就是雙重支付（Double-Spending）這樣的問題。

數位貨幣不像實體貨幣，數位資產比起實體資產容易複製，也因此如果花用數位貨幣的行為如果沒有處理好，就會產生憑空多出其他交易，這就像是偽鈔一樣，會造成通貨膨脹而導致貨幣貶值，讓人不再信任並願意持與流通。因此數位貨幣的支付通常需要一個受信任的第三方來做驗證，這樣的做法雖然簡單，卻存在單點脆弱性，只要這第三方受到攻擊或是監守自盜也一樣會讓這個數位貨幣變成一個失敗的貨幣。

分散式去中心化帳本能解決單點脆弱性的問題，但在驗證正確性這點難度卻很高，所有的節點都有記帳的權利，要如何確定由誰來記帳、記的帳對不對？如果無法確定帳是對的，那就存在雙重支付的風險。

為了改善單點脆弱性及雙重支付這樣的問題，許多分散式的雙重支付防範方法慢慢被提出來，中本聰提出了去中心化（以受信任第三方為中心）的方法來展示解決雙重支付問題，並實作出了 Bitcoin，使用共識機制來解決記帳及驗證的問題，這帶來去中心化數位貨幣帳本的成功。

Bitcoin 的共識協議主要由「工作量證明」（Proof-of-Work, PoW）和「最長鏈機制」兩部分組成，Bitcoin 上的各個節點就是透過共識機制中的工作量證明來決定誰有記帳權，然後取得記帳權的節點就能將新的區塊記帳加到最長鏈上並給予該節點獎勵（新區塊獎勵及交易費收益）。

Bitcoin 的 工作量證明大概會做以下的事情：

1. 收集還未記到帳上的交易
2. 檢查每個交易中付款地址有沒有足夠的餘額
3. 驗證交易是否有正確的簽名
4. 把驗證通過的交易信息進行打包（組成 Merkle Tree）
5. 為自己增加一個交易紀錄獲得 Bitcoin 獎勵金
6. 計算合法的 hash 爭奪記帳權

計算合法 hash 的方式請見下方影片說明，個人覺得這個影片是目前將 Blockchain 加密機制說明得最清楚的影片。我這邊簡略說明一下，合法的 hash 公式大致看起來像這樣：hash(交易內容+交易簽名+nonce+上一個區塊的 hash)，我們要取得記帳權，就需要找出前面開頭有 N 個 0 的 hash，由於交易內容、交易簽名及上一個區塊的 hash 都是不可變的，所以每個節點就是不斷的調整 nonce 來計算得出不同的 hash，直到找到開頭 N 個 0 的 hash 為止，第一個找的節點就能獲得記帳權，而其他的節點只要計算 hash 對不對就能驗證這個帳對不對。其中 N 個 0 開頭的 hash 就代表了計算的難度，越多 0 代表越難找到這樣的 hash，也因此可以調整計算難度。就是這樣的設計解決了去中心化分散式系統驗證資料及決定記帳順序的難題，也就改善了數位貨幣單點脆弱性及雙重支付的問題。

<iframe style="margin-top: 30px;" width="560" height="315" src="https://www.youtube.com/embed/_160oMzblY8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/xIDL_akeras" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

以上的內容看完應該就能大體了解 Blockchain 的原理了，甚至要自己做一個 Blockchain 都沒問題！了解了 Blockchain 的技術原理之後，應該能更信任去中心化的數位貨幣的安全性，或許有天大家都信任了去中心化的數位貨幣我們就真的能廣泛使用數位貨幣，為經濟活動帶來更有效率的流通。

## 延伸閱讀
1. Blockchain Demo https://anders.com/blockchain/
2. 區塊鏈 Blockchain — 共識機制之工作量證明 Proof-Of-Work https://www.samsonhoi.com/360/blockchain_proof_of_work
3. 區塊鏈 Blockchain — 創世區塊、區塊、Merkle Tree、Hash https://www.samsonhoi.com/274/blockchain_genesis_block_merkle_tree
4. 比特幣如何達成共識 — 最長鏈的選擇 https://hk.saowen.com/a/6e038c8f7813d07e59249c2dae9f7064018b50da1604373aa608a61b033a80e1
