---
title: "Ethereum 開發筆記 1–8：Ethereum 的獎勵機制"
date: 2018-09-11T18:05:00+08:00
draft: false
tags:
- ethereum
- blockchain
- uncle block
- ghost
---

Bitcoin 的獎勵機制基本上是挖到新區塊的節點獲得記帳權及獎勵，Ethereum 大體也是遵循這樣的概念，但做了一些調整與變化，讓我們整個脈絡了解一下。

由於 Blockchain 是一種去中心化的系統，所有的礦工（節點）可以同時挖礦（計算合法 hash），彼此獨立運作，所以極有可能出現兩的礦工同時發現不同的滿足條件的區塊，如此就會產生我們之前有提過的分叉（Fork）。

那我們該採用誰的區塊當主鏈呢？我們會先依工作量最大的區塊為主鏈，如果工作量一樣，就看誰先接了子區塊，一般來說只有成了主鏈的區塊才能獲得獎勵。但這樣沒有變成主鏈的區塊之前的算力就都白費了，所以 Ethereum 創造了 Uncle Block（叔塊）這樣的概念，不能成為主鏈的區塊如果後來被收留成為 Uncle Block，那這些沒有成為主鏈的區塊也有機會可以做為 Uncle Block 而獲得獎勵。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-8-01.png">
</p>

這就是 Ethereum 共識機制中的 GHOST（Greedy Heaviest Observed Subtree）協議，Ethereum 會這樣設計的原因，是由於 Ethereum 產生區塊的速度較快，也因此較容易產生分叉，也會使得新區塊較難以在整個網絡傳播，這對於傳播速度較慢的區塊並不公平。且分叉後的區塊可能在幾個區塊之後整併起來，我們會發現裡面的交易可能會與主鏈一致（雖然單獨查看分塊交易內容不同，不過數個區塊整體一起看交易內容就一致了），符合這種條件的分叉區塊我們就會納入主鏈參考，這些區塊就成了所謂的 Uncle Block，這某種角度也是更確認了 Blockchain 上的交易內容一致，因此 Uncle Block 也有貢獻，應該給予獎勵。

以上我們已經了解了 Ethereum 上的區塊大致分成兩種，普通區塊和 Uncle Block，Ethereum 對這兩種區塊的獎勵方式是不同的。我們分別來看一下。

### 普通區塊獎勵

- 固定獎勵 5 ETH
- 區塊內所有的 Gas Fee
- 如果區塊納入了 Uncle Block，那每包含一個 Uncle Block 可以得到固定獎勵 5 ETH * 1/32，也就是 0.15625 ETH，一個區塊最多隻能包含 2 個 Uncle Block，也因此不會無限延伸，同時又可鼓勵區塊納入 Uncle Block，增加交易內容的一致性。

### Uncle Block 獎勵

- 用公式計算：（Uncle Block 高度 + 8 - 包含此 Uncle Block 的區塊的高度）* 普通區塊固定獎勵 / 8

我們用個實例來看一下獎勵怎麼算。首先我們來看一個普通區塊：https://etherscan.io/block/1234757

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-8-02.png">
</p>

我們可以看到這個普通區塊的獎勵是 5.31485368 ETH，是由固定獎勵 5 ETH、總 Gas Fee 0.00235368 ETH 及包含了 2 個 Uncle Block 所以是 2*5*1/32 = 0.3125 ETH，所以結果就是 5.31485368 ETH。

接下來我們來看一個 Uncle Block：https://etherscan.io/uncle/0x54c3a32edc5b23dfeaac80ef50ed9a49faf269f2e7380b81b39f44b630346c70

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-8-03.png">
</p>

我們帶入公式運算一下，Uncle Block 高度是 1234756，包含此 Uncle Block 的區塊高度是 1234757，所以是 (1234756+8–1234757)*5/8 = 4.375 ETH。

以上就是 Ethereum Blockchain 的獎勵機制，應該還算淺顯易懂，我第一次看白皮書的 Uncle Block 完全不知道在講什麼啊，希望這樣有幫忙大家看懂。
