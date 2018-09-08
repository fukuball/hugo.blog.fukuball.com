---
title: "Ethereum 開發筆記練習 1：使用 Mist 發行自己的 Token"
date: 2018-09-08T11:54:00+08:00
draft: false
tags:
- ethereum
- blockchain
- mist
- smart contract
---

之前說過，Blockchain 基本上是因為金流帳本這樣的問題而被創造出來的，也就是說區塊鏈非常適合運用在金流的應用上，我們也可以建立自己的 Blockchain 來搭建自己的金流系統，不過在 Ethereum 上 Smart Contract 這種設計讓我們擁有可以在 Ethereum 區塊鏈上創造自己金流系統的能力，如此我們就不需要自己建一條鏈了。

我們使用 Smart Contract 仿造貨幣性質創造了數位資產（說穿了其實就是在 Smart Contract 上紀錄的變數而已），而這種具貨幣性質的數位資產又被稱作 Token，如此我們就可以在應用程式中使用這個去中心化的金流系統，由於 Token 的應用很普遍，大部分的功能都已經標準化了，我們只要仿造標準來實作就可以發行自己的數位貨幣了。

在這邊我們就練習一下怎麼使用 Mist 發佈 Token Smart Contract 來發行自己的數位貨幣。（目前我們還沒有學習過如何撰寫 Smart Contract，因此這邊會先直接提供範例程式碼，實作的部分我們之後再慢慢學習）

以下是我們的範例程式碼：

<script src="https://gist.github.com/fukuball/cee33c8a16548cf5e17920550b08f7cc.js"></script>

請打開 Mist，如下圖點擊 Contract，然後點擊 Deploy New Contract。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-01.png">
</p>

你會看到如下圖的頁面，請在 Solidity Contract Source Code 中貼上我們上面提供的範例程式碼。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-02.png">
</p>

貼上範例程式碼之後，Mist 會自動編譯程式，檢查是否有語法上的錯誤，如果沒問題，右方的 Select Contract to Deploy 就會出現選項，在這邊我們選擇 Token ERC 20。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-03.png">
</p>

選擇 Token ERC 20 之後，右方會出現要初始化 Contract 的參數表單，有 Initial supply、Token name、Token symbol 需要填寫。Initial supply 代表 Token 的總發行量是多少，我這邊設定成 7777777777，你可以設成你想要的數字。Token name 就是這個 Token 要叫什麼名字，這邊我設定成 7 Token，你想要取 Dog Coin 或是 Cat Coin 也都可以。Token symbol 就是這個 Token 要用什麼代號，像是美金就是用 $、Ether 是用 ETH，這邊我設定成 7token，你可以取自己覺得帥的代號。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-04.png">
</p>

借下來捲動頁面到底下，這邊你可以設定 Gas Fee 要用多少，這邊就看自己高興，我是沒有做任何調整。最後按下 Deploy！

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-05.png">
</p>

與區塊鏈互動基本上就是做交易，所以發佈 Smart Contract 也就需要發出一個交易，Mist 會彈出視窗顯示交易資訊及可能的 Gas Fee，請輸入密碼進行交易。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-06.png">
</p>

等待一下子就可以看到我們的 Smart Contract 發佈交易已經出現在頁面底端了，只要等待交易被確認，那一個新的數位貨幣就誕生了！

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-07.png">
</p>

Smart Contract 發佈完成後，請點擊你的帳戶，如下圖所示。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-08.png">
</p>

你會發現你的帳戶底下多了一個 Token 紀錄，在這邊我擁有了 7 Token 共 7,777,777,777 顆！如果這個 Token 被承認，那我就是超級有錢人啦！

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-09.png">
</p>

接下來我們來實際轉一些 Token 給朋友看看，在區塊鏈的世界我們不需要銀行及任何中心化的系統就可以將錢轉給朋友了，也就是我們現在擁有了一個去中心化的金流系統！讓我們來實際感受一下吧！

請點擊 7 Token 選項右邊的 Send，如下圖所示：

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-10.png">
</p>

填入朋友的 Ethereum 帳戶位址到 To 這個欄位，Amount 填入你想要匯出的 Token 數量，在這邊我填 40，然後捲動頁面到底端送出交易。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-11.png">
</p>

等待一下子交易確認後，40 個 Token 就完成匯出了！

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-12.png">
</p>

我們可以到 Etherscan上確認交易是否真的完成：https://ropsten.etherscan.io/address/0xed29cd5a72b06793601da5f0c4ec3ef5224037c7#tokentxns

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-p1-13.png">
</p>

的確有 40 個 7 Token 轉到朋友帳戶了！

在這個練習中，我們了解了 Token 到底是什麼，然後我們也實際發行了自己的數位貨幣，完成了自己的去中心化的金流系統，當我們想要轉帳時，我們再也不需要銀行及任何中心化的系統就可以將錢轉給朋友了，只要我們雙方都信任這個數位貨幣，價值的交換就能無遠弗屆地進行了！
