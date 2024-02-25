---
title: IN R 如何解決爬蟲失敗的問題
author: Helen Liu
date: '2020-04-17'
slug: in-r-resolve-web-crawler-issue
categories:
  - R
tags: [R、web crawler]
---

![image](https://thumbs.dreamstime.com/z/%E7%88%AC%E8%A1%8C%E5%8A%A8%E7%89%A9%E4%BC%A0%E6%9F%93%E5%AA%92%E4%BB%8B%E5%8A%A8%E7%89%A9%E7%88%AC%E8%99%AB%E7%B1%BB%E5%AD%97%E7%AC%A6%E8%9C%A5%E8%9C%B4%E4%B9%8C%E9%BE%9F%E9%AC%A3%E9%B3%9E%E8%9C%A5%E5%92%8C%E5%8F%98%E8%89%B2%E8%9C%A5%E8%9C%B4%E5%AE%A0%E7%89%A9%E4%BE%8B%E8%AF%81%E5%A5%97%E9%B3%84%E9%B1%BCvaran-137353409.jpg)

近來，因為論文需要而開始接觸爬蟲。意識到即使是科技先進的現在，文字對我們還是非常重要的。
在這資訊爆炸的時代光網上的訊息就已讓人難以消化，可見資訊分析是多麼重要的一門學科。
既然要分析資料，前提是要先有資料，在這過程中遇到了一些問題，在這裡和大家分享與討論!

![image](https://storage.googleapis.com/support-forums-api/attachment/thread-4933947-14733031649636786814.JPG)

P.S. 爬蟲目的不是為了癱瘓人家網站，只是為了學術分析用途!!\
爬蟲非比爬蟲!!言歸正傳，有的網站會為了防止某些惡意軟體或人存取網站資料或消耗目標流量，因而阻擋某些流量異常的IP，使用者常見的404 NOT FOUND頁面就出現了!

綱要:\
在爬蟲過程中，下載我的論文所需資料僅有在爬Jamie Oliver文章時，遭遇每抓20幾篇文章就會被封鎖IP，每次封鎖大約2至3分鐘。一開始使用的解決方法是使用R軟體的Sys.sleep( )函數。讓R軟體在下載資料時，依隨機秒數停頓運作，減緩下載速度。但在經過一番試驗過後，以所需最短秒數讀取文章，篩選符合所需條件文章，同時下載內容，所需時間預估需要一天。

最終採用解決辦法是減少code中request次數，降低被封鎖IP的機會。配合使用for( )迴圈，將網頁內容全數下載。過程中再加用Sys.sleep( )，使用rvest套件讀取文章，而得所需資料。

解決方式有許多種，下列只是我使用過的幾種方法。若還想了解更多相關解決方法，現在網上也有許多文章討論可做為參考。前面的幾種我嘗試了要不失敗，要不就是半途停下來，不然就是要花好多時間太不划算。

要用哪種就看你比較在意的是什麼，自己衡量看看囉!

...失敗\
1.使用Proxy代理伺服器，多個代理IP輪流用:\
(a.)use_proxy( ):\
網上可以找到一些免費的代理IP服務，但是速度通常不會很快。這些服務是直接提供ip，使用者可用編譯器撰寫程式使用。\
(b.)有些軟體安裝了以後，直接就能使用了。但這些公司服務免費版多會有些許限制，例如可能會有使用時間限制或是使用多少時間就需要手動控制等。\
END.....我試了以後發現下載效果真的不是很好，需要花比較多時間或手續,所以需要試試其他方法。\
2.Sys.sleep( )，讓code隨機幾秒休息一次，打破下載規律，避免被偵測到。\
END.....還是被擋，只是拖了一些時間而已。

...較容易成功\
1.直接下載整篇文章，再另外分析。\
...減少code中request次數，降低被封鎖IP的機會。\
範例:
```r 
i <- 1
j <- 1      #i,j只是要把被擋機率降低，你也可以用一個就好
while(i<1000){
  j<- j
  while (j<i+15){
    j=j+1
    loc=all_links[j]
    links=paste(j,".html",sep="")      #產生網頁的路徑和檔名
    download.file(loc,links)        
    Sys.sleep(runif(1,1,3))            # 讓每次迴圈休息幾秒  
  }
  i=i+15                               # 讓每15次迴圈休息幾秒更保險
  Sys.sleep(runif(1,20,30))       
}

```
2.另外，有時可能因為許多因素，下載到的網址中就有幾個會有問題，例如網頁內根本沒任何字只有影片等。\
...這時可以使用try( )或tryCatch( )，跳過錯誤。\
範例:
```r 
if (!file.exists("all_recipes.RData")){
  all_recipes_df <- get_recipe_data(paste0("",all_links$all_links[1],"")) 
  
  # use a for look just to see the progress
  for (i in 1:length(all_links$all_links)){  
    link_to_recipe <- paste0("",all_links$all_links[i],"")
    
    #pass the error
    tryCatch({       
      all_recipes_df <- rbind(all_recipes_df, get_recipe_data(link_to_recipe))
      cat("Scraped recipe: ", i, " of ", length(all_links$all_links), "\n")
    },error=function(e){
      cat("ERROR :",conditionMessage(e),"\n")
      })
    }
  all_recipes_df <- cbind(all_recipes_df,categories)
  all_recipes_df <- all_recipes_df[which(!(all_recipes_df$ingredients=="")),]
  
  # Save for later use
  save(all_recipes_df, file="all_recipes.RData")
  } else {
    load(file = "all_recipes.RData")
    }
```
