---
title: "Ethereum 開發筆記 2–2：Geth 基礎用法及架設 Muti-Nodes 私有鏈"
date: 2018-09-23T18:34:28+08:00
draft: false
tags:
- ethereum
- blockchain
- private chain
---

要連上 Ethereum 就需要安裝 Ethereum Node，在這邊我們選擇使用 Geth 來安裝 Ethereum Node，接下來就來一步一步的學學怎麼使用 Geth，甚至如何使用 Geth 來架設自己的 Ethereum 私有鏈。

## 安裝環境

首先我們在 AWS 上開啟兩台 Ubuntu 虛擬機器，記得開 t2.medium（2 vCPU, 4 GB RAM）這個規格以上才跑得動，硬碟可以開 100 G，Security Group 將 TCP 30303 打開，Ethereum Node 之間是用 30303 這個 port 來溝通的。

接下來使用以下指令安裝 Geth：

```
$ sudo apt-get install -y software-properties-common
$ sudo add-apt-repository -y ppa:ethereum/ethereum
$ sudo apt-get update
$ sudo apt-get install -y ethereum
```

兩台虛擬機器都要安裝，應該幾分鐘就可以裝好了。

## 使用 Main Net

安裝完 Geth 之後，我們就可以透過 Geth 連上 Ethereum Network 了，我們就來連上 Main Net 看看：

```
$ geth
```

下了這個指令之後，你應該可以看到一些訊息，geth 開始同步帳本資料到你的虛擬機器了，我們打開另一個 terminal 連上同一台虛擬機器，我們可以看到 geth 在以下位置建立了一些資料。

```
$ ls ~/.ethereum/
geth  geth.ipc  keystore

$ ls ~/.ethereum/geth/
chaindata  ethash  LOCK  nodekey  nodes  transactions.rlp

$ du -hs ~/.ethereum
5.9M /home/xxx/.ethereum

$ du -hs ~/.ethereum/geth/chaindata
5.9M /home/xxx/.ethereum/geth/chaindata
```

稍微說明一下，在 ~/.ethereum/ 資料夾下的 geth.ipc 只有在 geth 正在執行的時候才會出現，這就是我們之前說的要在本機透過 geth 與 Ethereum 做互動的管道，基本上我們之前的 Mist 也是使用 geth.ipc 來與 Ethereum 做互動。

其中 chaindata 資料夾下載了 Ethereum 帳本資料，如果我們想要重新同步帳本可以直接刪除裡面的所有資料；而 transactions.rlp 則存放了在 local 端蒐集到且還未被挖掘記錄到帳本的交易資料；nodekey 則是代表這台機器的 private key，用以在 Ethereum Network 上辨別不同的機器；ethash 則是運算 proof-of-work 必須產生的資料夾。

其實這樣你的機器就已經連上 Ethereum Network，並成為 Ethereum 中的一份子了，但等待 Main Net 完成同步需要花很久的時間，我們這邊就先換到 Test Net 上繼續練習，請使用 CTRL-C 關閉 geth 程序。

## 使用 Test Net

接下來讓我們連上 Test Net 看看，Test Net 有個別名 “Ropsten”，network id 是 3 （Main Net 的 network id 是 1），Test Net 有可能會隨著時間做一些改變，請使用以下指令連上 Test Net：

```
geth --testnet
```

同樣的，你也會看到一些同步訊息，一樣打開另一個 terminal 連上同一台虛擬機器，我們可以看到 geth 在以下位置建立了一些資料。

```
$ ls ~/.ethereum/testnet
geth  geth.ipc  keystore

$ ls ~/.ethereum/testnet/geth
chaindata  ethash  LOCK  nodekey  nodes  transactions.rlp

$ du -hs ~/.ethereum/testnet
112M /home/xxx/.ethereum/testnet
```

其實與連上 Main Net 所建立的資料大同小異，只是變成放在 ~/.ethereum/testnet 這個資料夾底下。雖然 Test Net 同步的速度會比 Main Net 快一些，但仍然需要花一段時間才能完成同步，我們一樣使用 CTRL-C 關閉 geth 程序，把所有已同步的 Main Net 及 Test Net 資料都刪除，我們改使用建立自己的 Private Net 來繼續練習。

以下是刪除同步資料的指令：

```
$ geth --testnet removedb // 刪除測試鏈資料

$ geth removedb // 刪除主鏈資料
```

## 建立自己的 Private Net

要建立自己的 Ethereum Private Net 其實很簡單，只要先定義好以下這兩項就能建立自己的 Private Net：

- Network id，決定自己的 network id 是什麼
- Genesis 檔案，決定自己的創世區塊初始帳本資料

而當其他機器要連上這個 Private Net，就需要用一樣的 network id 及相同的 genesis 檔案（代表初始共識一致），如此就能夠連上這個 Private Net 了（由於 Private Net 可能較為稀疏，所以一開始可能還需要提供其他 Peers 的位址給 geth 才能連上 Private Net）。

### 創世區塊

我們會使用 Genesis 檔案建立我們的創世區塊，這是整條區塊鏈中，唯一由人類共識決定的區塊，我們來看一下 Genesis 檔案的格式：

<script src="https://gist.github.com/fukuball/7f01c70efa92ff31055bc1e01c4b631d.js"></script>

其中比較重要的是 chainId，這個就是我們剛剛說的 network id，在這邊我們設定為 42 （the answer to life, the universe, and everything），你可以設成任何想要的數字，但請避開主鏈及知名的測試鏈（基本上 1–10 盡量不要用）；而 difficulty 則定義了 proof-of-work 的難度，這邊設成 0x400 其實就代表 1024，代表運算難度只有 1024，讓我們的 private chain 大約只要 10–15 秒就能產生一個區塊。

### 執行 Private Chain

我們使用以上內容，新增 Genesis 檔案，將檔名設為 genesis42.json，放置在 ~/chain/genesis42.json （請自行新增 chain 資料夾），然後我們也必須告訴 geth 這個 private chain 的資料要放在哪個資料夾，我們就放在 ~/.ethereum/net42 這個資料夾好了，讓我們來將執行指令寫成 shell script：

```
#!/bin/bash

geth --datadir ~/.ethereum/net42 init ~/chain/genesis42.json
```

將以上內容新增為檔案放置在 ~/chain/net42-init.sh ，這是 private chain 初始化時需要執行的指令，每個 Node 只要執行一次就可以。

```
#!/bin/bash

geth --datadir ~/.ethereum/net42 --networkid 42 console
```

將以上內容新增為檔案放置在 ~/chain/net42-start.sh ，這個指令會使 private chain 的這個 Node 跑起來，所以每次要跑 Node 時都需要執行這個指令，需要注意我們要告訴 geth 我們跑的 networkid 是 42，這樣才會接上我們剛剛設定的 private chain，而最後的 console 則是讓我們進入 geth 的命令介面。

讓我們將上面兩個 shell script 變為可執行檔，並執行這些執行檔：

```
$ cd ~/chain

$ chmod a+x net42-init.sh

$ chmod a+x net42-start.sh

$ ./net42-init.sh

$ ./net42-start.sh
```

執行完後，我們會看到我們進入 console 命令介面了，基本上這是 Javascript console，我們可以下一些指令給 geth，讓我們來試試看下一些指令吧。我們先輸入 eth ，然後按兩下 TAB，你會發現 console 會給出一些指令建議（如果一開始輸入 2 個空格，再按兩下 TAB，則會建議最基礎的指令，如果什麼指令都忘記了，一定要記得輸入 2 個空格，再按兩下 TAB）。我們輸入看看：

```
> eth.accounts
[]
```

這代表取出這個 Node 的帳號，目前沒有任何帳號。我們再試一下其他指令：

```
> eth.blockNumber
0
```

這代表我們同步的 private chain 還沒有任何區塊，我們可以看一下這個區塊的資料（注意：這邊使用了 … 省略了一些真實資料）：

```
> eth.getBlock(0)
{
  difficulty: 1024,
  extraData: "0x00",
  gasLimit: 5000000,
  gasUsed: 0,
  hash: "...",
  logsBloom: "...",
  miner: "0x0000000000000000000000000000000000000000",
  mixHash: "",
  nonce: "0x0000000000000042",
  number: 0,
  parentHash: "...",
  receiptsRoot: "...",
  sha3Uncles: "...",
  size: 506,
  stateRoot: "...",
  timestamp: 0,
  totalDifficulty: 1024,
  transactions: [],
  transactionsRoot: "...",
  uncles: []
}
```

我們來看看這個 Node 是否有在挖礦：

```
> eth.mining
false
```

目前沒有在挖礦，我們檢查一下是不是在正確的鏈上：

```
> net.version
"42"
```

的確在 42 這條鏈上，接下來看有沒有其他 peers：

```
> net.peerCount
0
```

目前這個 Private chain 上，我們的 Node 並沒有跟其他 Node 有連接。

### 帳號

接下來我們需要一個帳號用來在這個 private chain 上面走跳，我們來創建一個帳號吧：

```
> personal.newAccount()
Passphrase:
Repeat passphrase:
"0xe00f575ea205035ca9c530effa728202e5385cb2"

> eth.accounts
["0xe00f575ea205035ca9c530effa728202e5385cb2"]

> eth.coinbase
"0xe00f575ea205035ca9c530effa728202e5385cb2"
```

請記得帳號的 passphrase，我們在做任何交易時，都會需要 passphrase 來解鎖帳號，這是為了保障帳號的安全性，解鎖的指令如下：

```
> personal.unlockAccount(eth.accounts[0])
Unlock account 0xe00f575ea205035ca9c530effa728202e5385cb2
Passphrase:
true
```

帳號其實就是這個帳號的 public key，也就是我們可以公開的 Ethereum 帳戶位址，而 private key 我們會存在 keystore 裡：

```
$ ls ~/.ethereum/net42/keystore/
UTC--2018-09-17T09-31-22.219973375Z--e00f575ea205035ca9c530effa728202e5385cb2
```

Private key 會用你所輸入的 passphrase 加密存在這個 keystore 檔案裡，如果要備份帳號到其他機器使用，那就需要備份這些 keystore，及其所對應的密碼。

我們來看看這個帳號有多少 Ether：

```
> eth.getBalance(eth.accounts[0])
0
```

目前當然是沒有任何 Ether，所以我們需要挖礦來獲取 Ether！

### 挖礦

進行挖礦時會將挖到的 Ether 給 eth.coinbase 這個帳號，eth.coinbase預設就是eth.accounts[0] ，我們來挖礦吧：

```
> eth.coinbase
"0xe00f575ea205035ca9c530effa728202e5385cb2"

> miner.start(1)
true
```

其中 miner.start(1) 就代表開始挖礦，而參數 1 就是代表要開幾個 process 來挖礦，我們設成 1 就好，因為我們是自己在挖，會跟自己競爭，參數越高反而不一定能越快挖到區塊。而在第一次執行挖礦時，geth 會下載 ethash 檔案，這個檔案很大，也因此需要花一些時間，所以我們要耐心等待（看到 mined potential block 的訊息時才代表真的開始挖礦了ㄋ），請喝杯咖啡再回來吧！

過一陣子之後，我們回來看挖礦結果：

```
> miner.stop() // 停止挖礦
true

> eth.blockNumber
7

> eth.getBalance(eth.accounts[0])
21000000000000000000

> eth.getBalance(eth.coinbase)
21000000000000000000

> web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
21
```

我們看到區塊數變成 7，且帳號裡的 Ether 變成 21 了，我們果然有成功挖到 Ether 了！

### 交易

目前我們都還沒有做任何交易：

```
> eth.getBlock(0).transactions.length
0

> eth.getBlock(7).transactions.length
0
```

我們再開一個帳號來進行一下兩個帳號之間的交易：

```
> eth.mining
false

> personal.newAccount()
Passphrase:
Repeat passphrase:
"0x7a5005c95252e35d58e38f6dc5d2ac6e4b6f625b"

> eth.accounts
["0xe00f575ea205035ca9c530effa728202e5385cb2", "0x7a5005c95252e35d58e38f6dc5d2ac6e4b6f625b"]

> eth.getBalance(eth.accounts[1])
0
```

我們來送一些 Ether 到第 2 個帳號：

```
> web3.toWei(5, "ether")
"5000000000000000000"

> eth.sendTransaction({ from: eth.accounts[0], to: eth.accounts[1], value: web3.toWei(5, "ether") })
Error: authentication needed: password or unlock
```

這時我們會看到 console 報錯，這是因為我們還沒有 unlock 帳號，在 from 的帳號需要 unlock，to 的帳號不需要 unlock，所以我們需要 unlock eth.accounts[0] ：

```
> personal.unlockAccount(eth.accounts[0])
Unlock account 0xe00f575ea205035ca9c530effa728202e5385cb2 Passphrase:
true

> var txHash = eth.sendTransaction({ from: eth.accounts[0], to: eth.accounts[1], value: web3.toWei(5, "ether") })

> txHash "0x9f494ebc7b9ad6e6a9abd36798c6d04dcd7e883acf553a99e581b8550ec16178"
```

我們可以成功送出交易了，讓我們看一下交易的詳細資料：

```
> eth.getTransaction(txHash)
{
  blockHash: "...",
  blockNumber: null,
  from: "0xe00f575ea205035ca9c530effa728202e5385cb2",
  gas: 90000,
  gasPrice: 20304857463,
  hash: "0x9f494ebc7b9ad6e6a9abd36798c6d04dcd7e883acf553a99e581b8550ec16178",
  input: "0x",
  nonce: 0,
  r: "...",
  s: "...",
  to: "0x7a5005c95252e35d58e38f6dc5d2ac6e4b6f625b",
  transactionIndex: null,
  v: "0x78",
  value: 5000000000000000000
}
```

我們可以看到這個交易的詳細資料中，blockNumber 是 null，這代表交易還未確認，如果我們查看交易收據：

```
> eth.getTransactionReceipt(txHash)
null
```

回傳得到 null，我們看看第 2 個帳號是否有收到 Ether：

```
> eth.getBalance(eth.accounts[1])
0
```

仍然是 0 Ether，所以第 2 的帳號還沒有收到 Ether，我們需要將挖礦打開，這樣新的交易才能在新的挖礦區塊裡被確認：

```
> miner.start(1)
true

// 等待挖礦完成幾個區塊之後

> miner.stop()
true

> eth.getBalance(eth.accounts[1])
5000000000000000000

> eth.getTransaction(txHash)
{
  blockHash: "...",
  blockNumber: 8,
  from: "0xe00f575ea205035ca9c530effa728202e5385cb2",
  gas: 90000,
  gasPrice: 20304857463,
  hash: "0x9f494ebc7b9ad6e6a9abd36798c6d04dcd7e883acf553a99e581b8550ec16178",
  input: "0x",
  nonce: 0,
  r: "...",
  s: "...",
  to: "0x7a5005c95252e35d58e38f6dc5d2ac6e4b6f625b",
  transactionIndex: null,
  v: "0x78",
  value: 5000000000000000000
}
```

我們可以看到第 2 個帳號拿到 5 Ether 了，而這個交易的 blockNumber 也變成了 8，接下來我們看看交易收據：

```
> eth.getTransactionReceipt(txHash)
{
  blockHash: "...",
  blockNumber: 8,
  contractAddress: null,
  cumulativeGasUsed: 21000,
  from: "0xe00f575ea205035ca9c530effa728202e5385cb2",
  gasUsed: 21000,
  logs: [],
  logsBloom: "...",
  status: "0x1",
  to: "0x7a5005c95252e35d58e38f6dc5d2ac6e4b6f625b",
  transactionHash: "...",
  transactionIndex: 0
}
```

我們可以看到交易收據的詳細資料了，其中的 status 代表交易的情況，0x1 代表交易完成，如果是 0x0 就代表交易發生錯誤了。

以上我們已經學會了如何自己架 Private Chain，如何自己挖礦，如何在自己的 Private Chain 上做交易了，很簡單吧！

## Private Chain 連結其他 Node

現在架一個 Private Chain 對我們來說不是問題了，不過現在整個 Private Chain 只有一個 Node，怎麼與其他 Node 連結呢？

我們一開始有開了兩台 AWS 機器，目前都在其中一台練習，另外一台現在也仿造之前的步驟裝好環境，使用相同的 Genesis.json（一定要一模一樣）檔案建立、開啟 Private Chain，並建立好一個帳號，然後先不要進行挖礦。

現在我們兩個 Node 是分開的，並不知道彼此的存在：

```
Node 1 > admin.peers
[]

Node 2 > admin.peers
[]
```

但查看 blockNumber：

```
Node 1 > eth.blockNumber
16

Node 2 > eth.blockNumber
0
```

我們會發現 Node 1 有 16 個區塊了，但 Node 2 還沒有任何區塊。我們在 Node 1 取得 Node 的位址相關資訊：

```
Node 1 > admin.nodeInfo.enode
"enode://cf2d54937d2e7ee080e69ecde67352837fd8230482fea2cc34ec756e2f36c10608d2cdbb66f7788c84a0069c162f043fa1f660a942dddf8fcdf9d74de87061b4@[::]:30303"
```

我們將enode://cf2d54937d2e7ee080e69ecde67352837fd8230482fea2cc34ec756e2f36c10608d2cdbb66f7788c84a0069c162f043fa1f660a942dddf8fcdf9d74de87061b4@[::]:30303 中的 [::] 改成 Node 1 的 Private IP，看起來可能會像：enode://cf2d54937d2e7ee080e69ecde67352837fd8230482fea2cc34ec756e2f36c10608d2cdbb66f7788c84a0069c162f043fa1f660a942dddf8fcdf9d74de87061b4@172.31.22.132:30303，然後在 Node 2 加入這個 Peer：

```
Node 2 > admin.addPeer("enode://cf2d54937d2e7ee080e69ecde67352837fd8230482fea2cc34ec756e2f36c10608d2cdbb66f7788c84a0069c162f043fa1f660a942dddf8fcdf9d74de87061b4@172.31.22.132:30303")
```

之後我們查看：

```
Node 1 > eth.blockNumber
16

Node 2 > eth.blockNumber
16
```

我們發現 eth.blockNumber 變成一樣了，如果查看其中每個區塊的資料也會發現一模一樣，我們成功將多個 Node 連上自己建立的 Private Chain 並讓帳本在多個 Node 同步了！真神奇！

## 結語

在這一篇文章中我們介紹了 geth 的一些基礎用法，我們知道如何連上 Main Net，如何連上 Test Net，而且我們也學會了如何自己架設區塊鏈，建立自己的 Private Net，我們可以建立帳號、解鎖帳號、進行挖礦、進行交易，並查看交易詳細資料，最後學會將多個 Node 連上自己建立的 Private Net，建立一個完整的共識系統。學會這些，基本上就可以到處唬人了呢！
