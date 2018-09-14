---
title: "Ethereum 開發筆記 2–1：Ethereum 開發整體脈絡"
date: 2018-09-14T17:48:00+08:00
draft: false
tags:
- ethereum
- blockchain
---

在第一次接觸 Ethereum 應用程式開發時，會發現有各式各樣工具，不知要從何下手，我們用一個圖來說明一下與 Ethereum 互動時的整體脈絡及這之間的工具主要做了什麼事，了解之後自己就可以挑選開發時、甚至使用在產品上時要用什麼適合的工具了。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-2-1-01.png">
</p>

要在自己的機器接上 Ethereum 首先需要安裝 Ethereum Node，我們之前安裝的 Mist 其實就會在我們的機器上安裝 Ethereum Node 並同步帳本，而像這樣安裝 Node 並同步帳本甚至進行挖礦的軟體有很多，大家可以去選擇適合自己使用的。Mist 其實是將一個叫 geth 的軟體用 GUI 包裝起來，如果是開發者的話，可以選擇直接安裝 geth。

geth 提供了許多 API 指令可以讓我們跟 Ethereum 做互動，但有時下指令並不是那麼親和，所以 geth 提供了 RPC(Remote Procedure Calls) 與 IPC(Inter-process Communications) 兩種方式來與 geth 互動，如果你要在 local 機器連上 geth，那就可以使用 IPC；如果要讓遠端連上 geth，那就使用 RPC，可以開 HTTP 或 Web Socket 兩種方式來讓遠端使用。

以上就是 Ethereum 應用程式開發的基礎環境，接下來跟開發網頁應用程式一樣，Ethereum 應用程式也分成後端與前端，後端程式就是 Smart Contract，前端程式就是 Dapp。Smart Contract 可使用 Solidity 撰寫，目前也有許多其他語言可以撰寫 Smart Contract。Smart Contract 要在 Ethereum 上的 EVM 執行要先 Compile 成 Byte Code 之後，再透過 IPC 或 RPC 發佈到 Ethereum 上。前端程式的 Dapp 可用 Web3 JavaScript 透過 RPC 接上 Ethereum，以及使用網頁應用常用到的 HTML、CSS、JavaScript 製作成使用者互動介面，如此就能執行發佈在 Ethereum 上 Smart Contract 所提供的一些程式功能了。

以上整體脈絡如果了解了，那在 Ethereum 應用程式開發上就跨進了第一步，後續我們會循著這個脈絡來一步一步學習 Ethereum 開發。
