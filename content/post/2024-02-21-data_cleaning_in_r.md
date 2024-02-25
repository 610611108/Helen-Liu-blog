---
title: 資料清理要做的幾件事
author: Helen Liu
date: '2024-02-21'
slug: ''
categories:
  - R
tags:
  - R
  - Data Cleaning
---

<img src="https://raw.githubusercontent.com/610611108/Helen-Liu-blog/master/blogger%20pictures/work.jpg" alt="image" width="300">

**Data Cleaning**

資料清理是每次分析前必做的事,目的是清除並修正資料中,
可能遇到人為或非人為導致的資料不完整或錯誤的發生.
確保日後能更好的保證分析與建模的準確性.

必做的幾點事~**步驟**:
1. 先看看資料的結構與變數代表意義,讓心裡有個底.
知道之後要做的事大概有哪些.

2. 缺失值處理,例如null.na....
    - 各個變數缺失值數量>95%時,可以直接刪除該變數.數量太少,就算用預測填補也無補啊!
    - 各變數缺失值數量<95%時,除了手動檢查原始資料外,還需考慮以哪種方式填補.\
      例如**偏態變數以中位數**填補,**正態以平均數填補**或以線性迴歸計算缺失值.

3. 重複值處理

4. 噪音處理（異常值檢測）:常見方法有分箱法、回歸法與聚類法，藉以找出極端值.

5. 離散值:檢查有否宣告factor.

6. 特徵挑選:兩個都考慮,再挑選哪些變數留下.    
    - Correlation : 考慮適度刪除高度相關的變數.  
    - 特徵重要程度排序 : caret package   

7.資料標準化與歸一化

```r
#變數結構
str(data)
summary(data)
```
```r
#1資料清洗
#1-1.缺失值處理,例如null.na....
n <- length(which(is.na(data)))+length(which(is.null(data))) 
#各變數缺失值數量>95%可以直接刪除該變數.
#各變數缺失值數量<95%需考慮以哪種方式填補,除了手動檢查原始資料外,例如偏態變數以中位數填補,正態以平均數填補或以線性迴歸計算缺失值.
if(n!=0){
  seat <- c() # seat:含缺失值變數位置
  na <- c()  # na:變數缺失值數量>95%變數位置
  for (i in 1:length(data[1,])) {
    if(length(which(is.na(data[,i])))+length(which(is.null(data[,i])))!=0){
      seat <- c(seat,i)
    }
  }
  for (i in seat) {
    if((length(which(is.na(data[,i])))+length(which(is.null(data[,i]))))/length(data[,i])>0.95){
      na <- c(na,i)
    }
  }
  if(is.null(na)=='FALSE'){
    data <- data[ ,-na]}
}
```
```r
#1-2.1 重複值處理
n <- length(which(duplicated(data)=='TRUE')) #重複值數量
if(n!=0){
  data <- data[!duplicated(data), ]
}
```
```r
#1-2.2 null.na值處理,直接刪除或以中位數或均值替補.
#方法1.直接刪除
#方法2.偏態以中位數替補,正態以平均數替補
#用直方圖與盒鬚圖辨別辨別分佈型態
par(mfrow=c(2,4))
for(i in 1:length(seat)){
  hist(data[,seat[i]],freq = TRUE,
       main = paste("Histogram of" , colnames(data)[seat[i]]),
       xlab = colnames(data)[seat[i]])
  boxplot(data[,seat[i]],
          main = colnames(data)[seat[i]],
          xlab = "time",outcol="red")
}

#缺失值替代
MAX <- function(df){
  if(length(which(is.na(df))!=0)){df <- df[-which(is.na(df))]}
  Q1 <- as.data.frame(quantile(df, 1 / 4))[1,1] 
  Q3 <- as.data.frame(quantile(df, 3 / 4))[1,1]
  IQR <- Q3-Q1
  MAX <- Q3+1.5*IQR
  return(MAX)
}
MIN <- function(df){
  if(length(which(is.na(df))!=0)){df <- df[-which(is.na(df))]}
  Q1 <- as.data.frame(quantile(df, 1 / 4))[1,1] 
  Q3 <- as.data.frame(quantile(df, 3 / 4))[1,1]
  IQR <- Q3-Q1
  MIN <- Q1-1.5*IQR
  return(MIN)
}

seat_1 <- c(which(is.null(data[,seat[1]])),which(is.na(data[,seat[1]])));#BQ中位數
seat_2 <- c(which(is.null(data[,seat[5]])),which(is.na(data[,seat[5]])));#EL均值

#BQ
for (i in 1:length(seat_1)) {
  data[seat_1[i],seat[1]] <- median(data[,seat[1]],na.rm = TRUE)
}

#EL
for (i in 1:length(seat_2)) {
  data[seat_5[i],seat[5]] <- mean(data[,seat[5]],na.rm = TRUE)
}
```
```r
#Correlation : 考慮適度刪除高度相關的變數.
par(mfrow=c(1,1))
correlation <- cor(data[,-c(1,41,58,59)]) #Id與離散型變數不能算Correlation
corrplot(correlation)

diagonal <- c()
for (i in 1:55) {
  diagonal <- c(diagonal,c(55*(i-1)+i))
}
seat <- setdiff(which(correlation>=0.7),diagonal)
I <-c()
for (i in 1:length(seat)) {
  if(55*seat[i]%/%55+(seat[i]%/%55+1)<=seat[i]&seat[i]<=55*(seat[i]%/%55+1)){I <-c(I,i)}
}
seat <- seat[I]
x <- c()
y <- c()
for (i in 1:length(seat)) {
  x <- c(x,seat[i]%%55)
  y <- c(y,seat[i]%/%55+1)
}
cor <- c()
for(i in 1:length(x)){cor <- c(cor,correlation[x[i],y[i]])}
cor <- data.frame(row = rownames(correlation[x,]),
           col = colnames(correlation[,y]),
           cor = cor)
cor <- cor[order(cor$cor,decreasing = TRUE),]
rownames(cor) <- 1:length(x);cor
```
```r
library(caret)
#特徵重要程度排序
# prepare training scheme
control <- trainControl(method="repeatedcv", number=10, repeats=3)
# train the model
model <- train(Alpha~., data=data[,-c(1,41,58)], method="lvq", preProcess="scale", trControl=control)
# estimate variable importance
importance <- varImp(model, scale=FALSE)
# summarize importance
print(importance)
plot(importance,10)
```

