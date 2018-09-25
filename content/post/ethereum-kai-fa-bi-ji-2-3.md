---
title: "Ethereum 開發筆記 2–3：Smart Contract 初探，從 Bytecode 到 Solidity"
date: 2018-09-25T14:56:00+08:00
draft: false
tags:
- ethereum
- blockchain
- smart contract
- solidity
---

Ethereum 上的 EVM（Ethereum Virtual Machine）可以執行程式，而 EVM 上的可執行程式基本上是 Bytecode 的形式，所以所謂的 Smart Contract 就是存放在 Ethereum 上的 Bytecode，然後可由 EVM 來執行。

## Bytecode Smart Contract

### 直接用 Bytecode 寫 Smart Contract

我們來嘗試一下直接用 Bytecode 來寫 Smart Contract，以下這段程式碼主要內容是執行運算後，將運算結果存放在 0 這個位置：

```
PUSH1 0x03
PUSH1 0x05
ADD        // 3 + 5 -> 8
PUSH1 0x02
MUL        // 8 * 2 -> 16
PUSH1 0x00
SSTORE     // 將 16 存到 0 這個位置
```

這段程式轉成 Bytecode 就是：

```
0x60 0x03
0x60 0x05
0x01
0x60 0x02
0x02
0x60 0x00
0x55
```

也就是：

```
0x6003600501600202600055
```

接下來讓我們連上在上一篇文章中所建立的私有鏈 net42，我們藉由發送一個交易，但不要指定 to，如此就是告訴 EVM，我們要建立一個 Smart Contract，請在 Geth 輸入：

```
> var bytecode = "0x6003600501600202600055"

> var createTx = eth.sendTransaction({ from: eth.accounts[0], data: bytecode })
```

接下來請進行挖礦，以完成交易，交易完成後我們就可以看結果：

```
> miner.start(1)

> miner.stop()

> var created = eth.getTransactionReceipt(createTx).contractAddress

> eth.getStorageAt(created, 0)
"0x0000000000000000000000000000000000000000000000000000000000000010"
```

我們可以看到位置 0 存放了 0x10 這個值，這也就是運算結果 16 的 16 進位換算結果。

### 操作 Smart Contract

我們繼續來對 Smart Contract 做一些操作，我們使用以下的程式：

```
PUSH1 0x11 // 17
PUSH1 0x00
SSTORE     // 將 17 存放在 0 這個位置
```

然後在 Geth 送出交易，請注意這時要指定 to 為我們剛剛創建的 Smart Contract 位址，也就是 created：

```
> var bytecode2 = "0x6011600055"

> eth.sendTransaction({ from: eth.accounts[0], to: created, data: bytecode2 })
```

然後我們挖礦，完成交易後查看結果：

```
> eth.getStorageAt(created, 0)
"0x0000000000000000000000000000000000000000000000000000000000000010"
```

沒想到結果還是 0x10！為什麼？

我們來看看真實記在鏈上 Smart Contract 的原始碼好了：

```
> eth.getCode(created)
"0x"
```

我們並沒有存下程式碼在鏈上，一開始的 Bytecode 程式碼只是進行運算，並存放結果到位置 0 而已，我們要想辦法將我們之後要繼續使用的 function bytecode 存下來，以便之後能繼續使用。

### 撰寫「真的」Smart Contract

假設我們希望 Smart Contract 有一個 function 可在存取位置 0 存取資料，這段程式碼是：

```
PUSH1 0x00 CALLDATALOAD
PUSH1 0x00
SSTORE
```

Bytecode 就是：

```
0x600035600055
```

但要把這段 function 程式碼發佈成為真正可用的 Smart Contract 還需要做一些事來將這段程式碼「打包」，大概的程式碼如下：

```
00: PUSH1 0x06
02: PUSH1 0x0c
04: PUSH1 0x00
06: CODECOPY
07: PUSH1 0x06
09: PUSH1 0x00
0b: RETURN
0c: 600035600055
```

Bytecode 就是：

```
0x6006600c60003960066000f3600035600055
```

接下來我們到 Geth 試試看：

```
> var bytecode3 = "0x6006600c60003960066000f3600035600055"

> var createTx3 = eth.sendTransaction({ from: eth.accounts[0], data: bytecode3 })

> miner.start(1)

> miner.stop()

> var created3 = eth.getTransactionReceipt(createTx3).contractAddress

> eth.getCode(created3)
"0x600035600055"
```

這次我們有成功將 function 的原始碼存到 Ethereum 了！

### 操作「真的」Smart Contract

接下來我們就來操作看看這個 Smart Contract 的 function：

```
> eth.getStorageAt(created3, 0)
"0x0000000000000000000000000000000000000000000000000000000000000000"

> eth.sendTransaction({ from: eth.accounts[0], to: created3, data: "0x67" })

> miner.start(1)

> miner.stop()

> eth.getStorageAt(created3, 0)
"0x6700000000000000000000000000000000000000000000000000000000000000"
```

我們可以看到原本位置 0 是 0x00，經過操作之後就變成 0x67 了！

## Solidity Smart Contract

使用 Bytecode 來寫 Smart Contract 實在太 Hardcore 了！我們身為普通人還是用普通人的方式來寫 Smart Contact 好了，這邊提供一個用來寫 Smart Contract 的程式語言 — Solidity。

我們用 Solidity 提供的一些平易近人的語法，然後在同過編譯器來將 Solidity 轉譯成 Bytecode，如此寫 Smart Contract 就不會像之前用 Bytecode 寫這麼麻煩了！這邊我們稍微先簡單介紹一下 Solidity，之後我們會慢慢深入。

### 資料型態

Solidity 主要的資料型態有以下：

- bool
- int
- uint（Unsigned）
- address

其中 address 有 .balace、.transfer(unit) 及.send(unit)這些 function 可以使用，unit 以Wei 為單位。

### Functions

Solidity 可以定義 function：

- 要使用保留字 function
- function 可以有參數
- function 可設為 public 或 private
- function 可以回傳資料（或不回傳），如：returns (unit)

另外像是 for、do while、break、continue、if then else、return 等語法也有提供，如果 function 有要收取 Ether 的話，需要加上保留字 payable。

### 特殊變數

Solidity 有一些特殊的變數是與目前的交易相關的，例如：

- msg.sender，代表目前呼叫這個 function 的 address
- msg.value，代表目前呼叫這個 function 時所付的 Ether，以 Wei 為單位

### 簡單範例

我們先用一個簡單的範例示範一下如何用 Solidity 撰寫 Smart Contract 吧，範例程式碼如下：

```
pragma solidity ^0.4.22;

contract Owned {
  address public owner;

  constructor() public {
    owner = msg.sender;
  }
}
```

這個範例程式碼很簡單，在 Smart Contract 發佈時，建構子就會將 owner 成員變數設成是發佈 Smart Contract 的位址。

由於 Solidity 程式 EVM 看不懂，我們需要將 Solidity 程式碼轉成 Bytecode，我們這邊先使用 [Remix](https://remix.ethereum.org/) 這個工具來編譯看看 Solidity。請在編輯區複製貼上我們的範例程式碼，Remix 會自動編譯 Solidity，接下來請選擇 ”Compile“ “Owned” “Detail”，你可以在 ”Assembly“ 區塊看到編譯完後的 Solidity 實際做了什麼。

<p style="text-align:center">
    <img src="/images/ethereum/ethereum-2-3-01.png">
</p>

## 結語

我們從最底層的 Bytecode 介紹起，了解了用 Bytecode 撰寫 Smart Contract，但實在太麻煩，所以我們改使用 Solidity 來撰寫 Smart Contract，如此寫 Smart Contract 就不會像之前用 Bytecode 寫這麼麻煩了！後續我們會使用 Solidity 來撰寫功能更豐富的 Smart Contract，慢慢深入了解 Solidity。
