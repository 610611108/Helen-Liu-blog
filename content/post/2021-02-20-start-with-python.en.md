---
title: START with Python
author: ''
date: '2021-02-20'
slug: start-with-python
categories: [Python]
tags: [Python,Anaconda,Kaggle]
---

起初會想學Python是因為了參加kaggle舉辦的一個比賽，其中大多參賽者都是使用Python。次外當我把data下載後，想以R打開train.csv，系統卻跳出音資料量太大而無法讀取。無奈之下開始了Python學習之路，雖說用了Python才發現依樣問題也是存在，需另闢新徑解決問題，哈哈。

網上很多教學，所以簡短總結開始學習要搜索的路程。

- 安裝
裝python或Anaconda都可以。看自己喜歡哪個，我是選了Anaconda。

- 開始寫程式
與R相似，R第一次使用package要安裝和每次使用前要library，而python有自帶一些常用package，若有額外需求再自己安裝，每次使用前需要先import。

```python
import numpy as np
# numpy 是你想要使用的package, np是自己取的名字方便日後使用不用寫一長串。
```
其餘基本的寫法網上都能找到參考寫法，寫過R的人應該會很快上手。
[詳見其他人的blogger說明](http://tw.gitbook.net/python/python_basic_operators.html)

- Kaggle
1.Kaggle裡有個notebook的功能，可在裡面直接挑選寫r或python...。若參加比賽，直接在比賽那頁開個notebook，寫個一行字就能直接導入data。

```python
import xxx
env = xxx.make_env() # initialize the environment xxx，xxx是比賽給的資料庫名稱。
```
2. 免費雲端Jupyter Notebook
免費的線上gpu運算功能，雖有限制，但夠了。
[詳見其他人的blogger說明5.](https://zhuanlan.zhihu.com/p/166151381)

3. 寫完code上傳kaggle
kaggle的比賽會有個deadline要注意!而且sub結果一天限制5次，第一次參加比賽最後一天才上傳結果一直失敗。查找許多文章(有史以來看最多英文帖子的一次= =)，改了許多程式碼，後來發現是最後的結果表格不能用submission = ...，要改成例如sample_sub = ...。