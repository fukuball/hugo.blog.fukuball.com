---
title: "林軒田教授機器學習技法 Machine Learning Techniques 第 16 講學習筆記"
date: 2018-02-12T10:24:38+08:00
draft: false
tags:
- machine learning
---

### 前言

本系列部落格文章將分享我在 Coursera 上台灣大學林軒田教授所教授的機器學習技法（Machine Learning Techniques）課程整理成的心得，並對照林教授的投影片作說明。若還沒有閱讀過 [第 15 講](https://blog.fukuball.com/lin-xuan-tian-jiao-shou-ji-qi-xue-xi-ji-fa-machine-learning-techniques-di-15-jiang-xue-xi-bi-ji/) 的碼農們，我建議可以先回頭去讀一下再回來喔！

### 範例原始碼：[FukuML - 簡單易用的機器學習套件](https://github.com/fukuball/fuku-ml)

我在分享機器學習基石課程時，也跟著把每個介紹過的機器學習演算法都實作了一遍，原始碼都放在 [GitHub](https://github.com/fukuball/fuku-ml) 上了，所以大家可以去參考看看每個演算法的實作細節，看完原始碼會對課程中的數學式更容易理解。

如果大家對實作沒有興趣，只想知道怎麼使用機器學習演算法，那 [FukuML](https://github.com/fukuball/fuku-ml) 絕對會比起其他機器學習套件簡單易用，且方法及變數都會跟林軒田教授的課程類似，有看過課程的話，說不定連文件都不用看就會使用 [FukuML](https://github.com/fukuball/fuku-ml) 了。不過我還是有寫 [Tutorial](https://github.com/fukuball/FukuML-Tutorial) 啦，之後會不定期更新，讓大家可以容易上手比較重要！

### 熱身回顧一下

上一講中我們學到了如何使用矩陣分解方法來解推薦問題，機器學習技法課程也到這邊告一段落了，這一講終將會總結回顧一下我們在機器學習技法中學到的所有機器學習演算法，也許還有許多算法沒有介紹到，但基本概念都可以延伸。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-01.png">
</p>

### 特徵技巧：Kernel

我們學習到了如何使用 Kernel 來表現資料特徵，使用到 Kernel 技巧的相關演算法如下：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-02.png">
</p>

### 特徵技巧：Aggregation

我們也可以使用 Aggregation 方法來結合資料特徵，藉以合成更強大的學習演算法，使用到 Aggregation 技巧的相關演算法如下：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-03.png">
</p>

### 特徵技巧：Extration

我們可以使用 Extration 技巧來取得重要的資料特徵，使用到 Extration 技巧的相關演算法如下：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-04.png">
</p>

### 特徵技巧：Low-Dim

我們也會使用降維這個特徵技巧來取得資料的重要特徵，用到降維技巧的相關演算法如下：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-05.png">
</p>

### 優化技巧：Gradient Decent

在類神經網路大量用到了 Gradient Decent 技巧來進行 Error 優化，用到 Gradient Decent 技巧的相關演算法如下：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-06.png">
</p>

### 優化技巧：Equivalent Solution

在許多困難的問題，我們很難找到優化的方法，我們會使用 Equivalent Solution 找到優化的方法，例如 Dual SVM 我們使用 covex QP、Kernel LogReg 我們用 representer、PCA 我們用 eigenproblem 來解。

未來若需要發展自己的演算法，也可以朝 Equivalent Solution 去想優化方法，只是這可能需要大量的數學推理知識。

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-07.png">
</p>

### 優化技巧：Multiple Steps

有一些演算法我們會用 Multiple Steps 來一步一步進行優化，，用到 Multiple Steps 技巧的相關演算法如下：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-08.png">
</p>

### 過擬似技巧：正規化

由於演算法的能力越來越強，也因此很容易過擬似（Overfitting），所以我們必須要有方法來避免過擬似，其中一個方式就是正規化，我們大致學過的正規化方法如下：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-09.png">
</p>

### 過擬似技巧：Validation

另外我們也需要使用 Validation 方法讓我們在訓練過程就可以避免過擬似，在機器學習技法中我們學到的一些演算法有因為演算法特性而發展出來的 Valdation 方法：

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-10.png">
</p>

### 機器學習叢林

林軒田老師在機器學習技法課程的一開始就有放過這樣一張投影片，我們進入的是一個機器學習的叢林，從一開始可能對這投影片的所有演算法都不了解，但在這課程的尾聲我們重新回顧，相信大家多少都已經認識了這個叢林的險惡，也了解這個叢林是個多麽有趣與豐富！

<p style="text-align:center">
    <img src="http://static.obeobe.com/image/blog-image/Machine-Learning-Techniques-16-11.png">
</p>
