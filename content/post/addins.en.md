---
title: R更新,Addins裡的選項不見了
author: Helen Liu
date: '2024-02-18'
categories: 
  - R
tags: 
  - R、blogdown
slug: addins
---

![](https://github.com/610611108/Helen-Liu-blog/blob/master/blogger%20pictures/Pngtree.png?raw=true)

好久沒用R寫文章了!天~\
因為最近在Kaggle找了個題目重新練習一下,\
其中遇到一些問題想記錄下來.\
才發現因為太久沒發文,居然忘了用R寫文章要選開哪個文檔.\
晴天霹靂,真的是把每個看起來像的都試了!但...都不是,哈哈...= = \
只能上網爬下文,搜尋blogdown in r就有一堆教學了.

我遇到的問題1.:Addins裡的選項不見了.

看完後試了好多次,Addins裡的選項就是不見了.再怎麼試new post就是沒跑出來.\
試了一下blogdown:::new_post_addin()才發現,原來問題在沒安裝blogdown.\
裝完Addins裡找不到的new post選項終於出現了!\
看來更新後有些安裝包會不見,原先還以為只要有裝過就能用.

我遇到的問題2.:怎麼試blogdown:::serve_site(),public裡就是沒有新文章\
其實我也不知道怎麼變正常的,\
只是照著書裡寫的blogdown::check_site()跑了一下.\
把所有[TODO]黑色實心點點都照做.\
把Hugo(blogdown::install_hugo())和Git(網上直接下載.exe安裝)都更新.

我遇到的問題3.:電腦的hugo versionc和使用的theme不同,\
記得要到github下載新的,太舊好像也會有問題.

最後記得寫完文章要blogdown::build_site(local=TRUE).\
寫一下,怕我之後又忘了!

爬文的時候發現有講如何用R創建網站的書,\
分享給大家,[blogdown: Creating Websites with R Markdown](https://bookdown.org/yihui/blogdown/).
