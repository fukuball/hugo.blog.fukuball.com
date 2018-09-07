---
title: "Ethereum 開發筆記 1–3：使用 Mist"
date: 2018-09-07T15:45:00+08:00
draft: false
tags:
- ethereum
- blockchain
- mist
---

Mist 跟前回介紹的 MetaMask 一樣是可以與 Ethereum 進行互動的工具，除了可以管理 Ethereum 相關密鑰之外，Mist 還包含了 Ethereum 節點以及網頁瀏覽器，方便大家瀏覽 Dapp 網頁。

首先請到[這邊](https://github.com/ethereum/mist/releases)安裝 Mist，請選擇適於自己的作業系統安裝。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-01.png">
</p>

由於 Mist 會安裝節點在你的電腦裡，也因此會同步整個帳本下來，所以會花上不少時間同時也會佔用許多硬碟空間。我們目前僅是要使用測試鏈，所以請切換到 Ropsten 測試鏈（如下圖），這樣就不用花這麼多時間與空間了。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-02.png">
</p>

在 Mist 的左下角可以觀察目前已同步到你的電腦的區塊數（如下圖），如果這個數字跟 [Etherscan](https://ropsten.etherscan.io/)（Etherscan 是一個可以查看 Ethereum 區塊鏈所有交易的網站） 上的最新區塊數一致的話，那就代表已經同步完成了。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-03.png">
</p>

接下來讓我們用 Mist 開一個 Ethereum 帳戶，請點擊 Add Account，並依指示輸入密碼後創建帳號，密碼請務必要記下來，將來交易時都會需要輸入你的密碼。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-04.png">
</p>

學會創建 Ethereum 帳戶之後，我們要來看一下 Mist 要怎麼備份帳號，請點擊 Mist 上方選單的 File -> Backup ->Accounts（如下圖），這樣就會打開帳號存放的資料夾，所有的帳號都會加密存在這邊，所以只要備份這些檔案及當時設定的密碼，你就可以在別台電腦復原你的帳號。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-05.png">
</p>

現在你這個 Ethereum 帳戶還沒有任何 Ether，我們仿造之前用 MetaMask 來跟水龍頭要 Ether 的步驟來取得 Ether 看看。

我個人提供了一個水龍頭 Dapp，請前往這個網址來取得 Ether：https://blog.fukuball.com/dapp/faucet/

由於 Mist 也是一個 Dapp 網頁瀏覽器，請在 Mist 上方的網址列輸入：https://blog.fukuball.com/dapp/faucet/

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-06.png">
</p>

Mist 在揭露你的 Ethereum 帳戶資訊給 Dapp 網頁時都會詢問你的同意，請先選擇要瀏覽這個 Dapp 網頁的帳號（你可能在 Mist 有多個帳號，所以就需要選擇目前要用哪個帳號瀏覽這個網頁）。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-07.png">
</p>

然後點擊 Authorize，這樣就可以連上 Dapp 網頁了。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-08.png">
</p>

你可以看到跟 MetaMask 一樣，Ethereum 帳號（public address）已經被填寫到 Send To 欄位了，只要按下 Send To 之後不久你就可以從水龍頭收到 Ether 了。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-09.png">
</p>

果然不久之後我們就收到了 0.5 Ether！

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-10.png">
</p>

接下來我們一樣練習一下把 0.1 Ether 匯回給水龍頭，請在 Credit 欄位輸入 0.1，然後按下 Credit。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-11.png">
</p>

這時 Mist 跟 MetaMask 一樣會彈出一個視窗顯示 Gas Fee 等資訊，不同的地方是 Mist 需要輸入密碼來授權這個交易。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-12.png">
</p>

交易進行時，你會收到一個 Tx id，在我這邊的例子是：0x82407e0aac7cc5d3ef485ffba78f279b37aaba50e64396c477b1b19ee5590793，你可以到 Etherscan 去查看這筆交易進行的狀態：https://ropsten.etherscan.io/tx/0x82407e0aac7cc5d3ef485ffba78f279b37aaba50e64396c477b1b19ee5590793

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-13.png">
</p>

不久之後，等交易確認，你就可以看到 Ether 變成 0.4 了，你成功匯回了 0.1 Ether。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-14.png">
</p>

如同使用 MetaMask，我們也可以使用官方提供的 Mist 來與 Ethereum 區塊鏈做互動，其實都不錯用，但 Mist 相對肥大很多，也因此有時候交易會卡住，畢竟 Mist 在你的機器上安裝了 Ethereum 節點，所以比起 MetaMask 複雜許多，也比較容易遇到問題。如果你遇到問題了，可能重開 Mist 就能解決，如果還是不能解決，那就 google 吧！

## Appendix

Mist 在系統背景開了一個叫 geth 的程序，這個 geth 就是主要用來與 Ethereum Network 互動的程式，未來我們會再多說一點 geth，在這邊我們先稍微看一下就好。

請在 Terminal 輸入指令：

```
ps aux | grep geth
```

你會看到 geth 真的有被跑在背景執行：

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-15.png">
</p>

我們也可以進入 geth 的指令介面來使用 geth 更多功能，請在 Terminal 輸入指令：

```
/Users/username/Library/Application\ Support/Mist/binaries/Geth/unpacked/geth attach ~/Library/Ethereum/geth.ipc
```

你會看到像這樣的互動介面：

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-1-3-16.png">
</p>

在這邊可以使用 geth 更多與 Ethereum 互動的指令，我們後續會學到更多，在這邊先簡單感受一下就好，你可以輸入指令：

```
net.peerCount
```

這樣 geth 就會回覆目前你的節點有多少的 peer 連結，其他的功能，我們就以後再說吧！
