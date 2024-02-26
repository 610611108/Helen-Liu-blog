---
title: Kaggle實作inR---ICR - Identifying Age-Related Conditions
author: Helen Liu
date: '2024-02-25'
slug: kaggle-inr-icr-identifying-age-related-conditions
categories:
  - R
tags:
  - R
  - Kaggle
---
InVitro Cell Research, LLC (ICR)是一家成立於2015年,專注於再生和預防性個人化醫療的私人投資公司。他們位於大紐約市地區的辦公室和實驗室提供最先進的研究空間。InVitro Cell Research 的科學家使他們與眾不同，幫助指導和確定他們研究如何快速修復衰老人群的使命。年齡只是一個數字，但隨著老化會帶來一系列健康問題。從心臟病和癡呆到聽力損失和關節炎，老化是許多疾病和併發症的危險因子。不斷發展的生物資訊領域包括對有助於減緩和逆轉生物衰老以及預防與年齡相關的主要疾病的干預措施的研究。即使樣本數量很少，數據科學也可以在開發解決不同數據問題的新方法方面發揮作用。

[ICR - Identifying Age-Related Conditions](www.kaggle.com/competitions/icr-identify-age-related-conditions/overview/description)  

<font size="10">**Goal**</font>&ensp;本次資料分析目標是預測一個人是否患有與年齡相關的三種疾病中的任何一種。最終須預測此人是否患有三種醫療狀況（1 類）中的任何一種或多種，或者三種醫療狀況（0 類）中的任何一種或多種,即資料內的Class。基於健康特徵(
資料內的AB,AF,...,GL)測量進行訓練的模型。

&ensp;&ensp;要確定某人是否患有這些疾病，需要一個漫長且侵入性的過程來收集患者的資訊。借助預測模型，我們可以透過收集與病情相關的關鍵特徵，然後對這些特徵進行編碼，來縮短此過程並保持患者詳細資訊的隱私。建模可幫助研究人員發現某些特徵的測量值與潛在患者狀況之間的關係。  

&ensp;&ensp;Kaggle本次提供資料已經進行加密,不須事先進行資料轉換。  
- 資料轉換：將資料轉換為統一的格式和度量單位，以便進行後續分析。資料轉換可能包括：
  - 資料標準化：將資料縮放到相同的數值範圍，如 0 到 1 之間。
  - 資料歸一化：將資料轉換為具有單位範數的形式，以消除量級上的差異。
  - 類別資料編碼：將類別資料轉換為數值形式，如 one-hot 編碼。  
  &ensp;
  &ensp;
<iframe src="https://enjoy-life-with-helen.netlify.app/ICR - Identifying Age-Related Conditions.html" width="800" height="1000"></iframe>

