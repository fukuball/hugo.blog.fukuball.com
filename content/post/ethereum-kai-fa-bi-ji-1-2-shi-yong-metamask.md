---
title: "Ethereum 開發筆記 1–2：使用 MetaMask"
date: 2018-09-06T17:46:00+08:00
draft: false
tags:
- ethereum
- blockchain
- metamask
---

前一回稍微對 Blockchain、Bitcoin、Ethereum 做了一個科普的簡介，我們可以知道 Blockchain 就是一個帳本，每一個加入 Blockchain 的節點都會下載整個帳本在本地端，所以我們就可以在自己的節點（系統）寫入資料到帳本並透過 Blockchain 背後的機制同步到所有的節點。

但實際在節點帳本上寫入交易紀錄前，我們先使用 MetaMask 這個工具來跟 Ethereum 互動吧，不然要裝好 Ethereum 節點、下載好帳本可能會花上不少時間，在這之前就失去耐性的話可不是一個好的開始。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-01.png">
</p>

MetaMask 這個工具可以讓你不用安裝節點就跟 Ethereum 上的帳本做互動，這背後的原理其實就是使用別人幫忙維護的節點，如此就可以不用自己安裝節點、同步帳本。

首先請到 MetaMask 上安裝 Chrome（或 Firefox）外掛，並請依指示安裝，MetaMask 會創建 Ethereum 帳戶及相關密鑰，MetaMask 也會管理密鑰，讓你可以方便地使用密鑰來與 Ethereum 互動，請記下密碼及 12 個單字的帳戶復原字，這 12 復原字可以用來讓你回復 Ethereum 帳戶，如下圖。

<p style="text-align:center">
    <img style="width: 50%;" src="/images/ethereum/ethereum-1-2-02.png">
</p>

Ethereum 上流通的貨幣就是 Ether，它用來當成 Ethereum Blockchain 得以運作的貨幣，Ethereum 上的節點想在 Ethereum 上做運算或是記下資料，那就需要付 Ether 當手續費，而在 Ethereum 上當礦工的節點就可以提供運算資源收取 Ether 當報酬。就如同現實世界一樣，貨幣的流通形成了資源的流通，讓世界可以正常運作。

我們先切換到 Ropsten Test Network（Ethereum 的測試鏈，上面的 Ether 是沒有任何價值的）來感受一下在 Blockchain 上怎麼進行交易。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-03.png">
</p>

現在我們還沒有任何 Ether 可以使用，這樣我們就沒辦法與 Ethereum 做互動，也就是無法做任何交易，讓我們來跟水龍頭要一些 Ether 來花吧！（在測試鏈上有佛心水龍頭，但正式鏈上就要自己挖礦或是花錢買 Ether 了！）

我個人提供了一個水龍頭 Dapp，請前往這個網址來取得 Ether：https://blog.fukuball.com/dapp/faucet/

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-04.png">
</p>

這個 Dapp 會讀取你的帳戶位址到 Send To 欄位，你也可以自己複製位址到 Send To 欄位，點擊 Send To 之後不久就可以收到 Ether 了！

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-05.png">
</p>

如上圖，我們不久之後就收到了 0.5 Ether，接下來我們來把 0.1 Ether 捐回去給水龍頭提供者看看。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-06.png">
</p>

請在 Credit 欄位上輸入 0.1，並點擊 Credit。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-07.png">
</p>

這時 MetaMask 會彈出交易視窗，顯示將要匯出 0.1 Ether（約 22.78 美金），然後手續費 Gas Fee（礦工運算顯）是 0.0001 Ether。（手續費現實世界通用的，但在 Ethereum 的世界叫 Gas Fee，之後文章將都統一使用 Gas Fee）

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-08.png">
</p>

你可以點擊 Edit 調整 Gas Fee，簡易說明下 Gas Fee，Gas Fee 就是 Gas Price（以 Gwei 為單位）X Gas Limit 的計算結果，因此這個例子的 Gas Fee 就是 0.0001 Ether，Gas Price 影響的是礦工運算的優先度，Gas Limit 影響的則是可用多少運算資源。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-2-09.png">
</p>

不久之後，我們的 Ether 降到了 0.399978，我們成功地匯回了 0.1 Ether，而 Gas Fee 沒有花完所以得到的 balance 是 0.399978，並非 0.3999（Gas Fee 花光會有交易失敗風險）。

我們成功地透過了 MetaMask 在 Ethereum 上做交易，我們可以不用透過銀行就可以將錢匯來匯去了！只要會寫 Dapp，我們就可以從世界各地賺錢，讓使用者直接與我們交易，中間不需要再接銀行的金流了，這樣的世界是不是很棒呢！