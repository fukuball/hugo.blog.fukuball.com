---
title: "林軒田教授機器學習技法 Machine Learning Techniques 第 15 講學習筆記"
date: 2018-02-12T09:03:17+08:00
draft: false
tags:
- machine learning
---

### 前言

本系列部落格文章將分享我在 Coursera 上台灣大學林軒田教授所教授的機器學習技法（Machine Learning Techniques）課程整理成的心得，並對照林教授的投影片作說明。若還沒有閱讀過 [第 14 講](https://blog.fukuball.com/lin-xuan-tian-jiao-shou-ji-qi-xue-xi-ji-fa-machine-learning-techniques-di-14-jiang-xue-xi-bi-ji/) 的碼農們，我建議可以先回頭去讀一下再回來喔！

### 範例原始碼：[FukuML - 簡單易用的機器學習套件](https://github.com/fukuball/fuku-ml)

我在分享機器學習基石課程時，也跟著把每個介紹過的機器學習演算法都實作了一遍，原始碼都放在 [GitHub](https://github.com/fukuball/fuku-ml) 上了，所以大家可以去參考看看每個演算法的實作細節，看完原始碼會對課程中的數學式更容易理解。

如果大家對實作沒有興趣，只想知道怎麼使用機器學習演算法，那 [FukuML](https://github.com/fukuball/fuku-ml) 絕對會比起其他機器學習套件簡單易用，且方法及變數都會跟林軒田教授的課程類似，有看過課程的話，說不定連文件都不用看就會使用 [FukuML](https://github.com/fukuball/fuku-ml) 了。不過我還是有寫 [Tutorial](https://github.com/fukuball/FukuML-Tutorial) 啦，之後會不定期更新，讓大家可以容易上手比較重要！

### 熱身回顧一下

上一講介紹了 RBF Network，基本上就是透過距離公式及中心點來對資料點進行投票的一個算法，這一講將介紹矩陣分解系列的算法。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-01.png">
</p>

### 推薦系統問題

之前的課程中曾經提到過推薦系統的問題，我們的資料集是使用者對電影的評分，希望讓機器學習算法學習到可以推薦使用者也會高評分的電影，這樣的問題 Netflix 曾經舉行過競賽。我們如何解這樣的問題呢？

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-02.png">
</p>

### 類別編碼

這個問題首先會需要進行編碼，因為使用者資料可能只是一連串的使用者編號，這是類別資料，不太能用來直接用於運算（僅有 Decision Tree 可以直接用來做類別運算），所以我們會先將類別資料編碼成數值資料，編碼的方法常用 binary vector encoding，如下所示：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-03.png">
</p>

### 特徵選取

我們可以將使用者評分電影的過程視做一組特徵轉換，能夠將 X 轉換成 Y，轉換的過程如果分成兩個矩陣 Wni、Wim，那左邊的矩陣代表的意義就是使用者對電影中哪些特徵很在意，右邊的矩陣代表的意義就是電影中有哪些特徵成份。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-04.png">
</p>

### 矩陣分解

因此這個推薦問題可以寫成底下的矩陣分解，首先我們把評分做成一個 Rnm 矩陣，需要嘗試把它分解成 VT W 兩個矩陣，使用者的喜好會對映一組特徵，電影的成分也會對應到這組特徵，我們要將這組特徵萃取出來。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-07.png">
</p>

### 矩陣分解學習 Alternating Least Squares

矩陣分解學習算法如下，首先決定特徵維度 d，然後隨機初始化使用者對特徵的喜好 Vn，電影中特徵的強度 Wm，然後優化 Ein，先固定 Vn 去優化 Wm，再固定 Wm 去優化 Vn，如此重複直到收斂，這樣就可以得到與 Rnm 最相似的 Vn X Wm。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-09.png">
</p>

### Linear Autoencoder vs 矩陣分解

之前介紹過的 Linear Autoencoder 基本上也是一種矩陣分解，但意義上有些不同。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-10.png">
</p>

### 使用隨機梯度下降法解矩陣分解

我們也可以使用隨機梯度下降法來解矩陣分解的問題，與 Alternating Least Squares 比起來，隨機梯度下降法速度較快，且比較簡單。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-11.png">
</p>

### 觀察 Error Function

隨機梯度下降是計算某個點的梯度來進行優化，所以我們可以用一個點作為範例來看看梯度優化的式子，例如我們要對 Vn 進行優化時，只要對 Vn 進行偏微分，即可得到優化的數學式，同理要對 Wm 進行優化時，也只要對 Wm 進行偏微分即可。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-12.png">
</p>

### 矩陣分解學習 SGD

因此 SGD 矩陣分解學習算法如下，首先一樣先決定特徵維度 d，然後隨機初始化使用者對特徵的喜好 Vn，電影中特徵的強度 Wm，然後隨機選取 Rnm 中的一點，計算 Rnm - WmVn，然後各自做偏微分取得新的 Vn 及 Wm 直到收斂。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-13.png">
</p>

### 矩陣分解學習 SGD 實務

林軒田老師分享了矩陣分解學習 SGD 算法在實務上的應用，由於在 KDDCup 2011 年的問題中，測試資料與訓練資料是在不同時間收集到的，因此可以說是不同的資料分布，在做訓練上可能需要將時間的因素考量進去這樣未來做預測才會準確。

使用 SGD 矩陣分解學習算法，我們可以在訓練過程中讓後半段的訓練資料都選取較新的訓練資料，因此可以將時間因素也同時考量在訓練過程中了，這樣的調整讓台大隊伍拿下了比賽的冠軍。如果了解了機器學習算法的細節，我們就可以因應不同的問題做調整。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-14.png">
</p>

### 總結

在這一講中，我們學到了如何使用矩陣分解方法來解推薦問題，矩陣分解的演算法可以使用 Alternating Least Squares 或是隨機梯度下降法，雖然目前網路上已經一大堆矩陣分解程式可以使用，但當遇到要適度調整演算法的時候，了解實作演算法細節便可以自行調整以解決真實世界會遇到的問題。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-15-18.png">
</p>
