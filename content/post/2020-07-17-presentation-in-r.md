---
title: Presentation in R
author: ''
date: '2020-07-17'
slug: presentation-in-r
categories: [R]
tags: [ioslides presentation、R]
---
第一次用R寫PPT有點茫然，網上講解的文章也不多。用久了發現ioslides presentation其實還滿好用的，做起來的簡報也挺簡潔的。
其中有些的code寫法，有一些和網頁編碼相通。

首先需要用R開一個ioslides presentation，這個rmd檔可以放在自己想要放的資料夾裡。在knit後，資料夾中會自動生出簡報的html檔和一個css檔。若需要更改封面與內頁字形大小與顏色或更換封面背景圖，屆時都要寫在這個檔案裡。
![image](https://github.com/610611108/Helen-Liu-blog/blob/master/presentation%20in%20r/01.jpg?raw=true)
![image](https://github.com/610611108/Helen-Liu-blog/blob/master/presentation%20in%20r/02.jpg?raw=true)
![image](https://github.com/610611108/Helen-Liu-blog/blob/master/presentation%20in%20r/03.jpg?raw=true)
基本上，這已是一個完整的ppt了! ## 就是一張投影片，如果想要花俏一點就繼續看下去!

**在css檔中更改html code**
1. **換封面圖片:**

body {\
&emsp;     background-image: url(圖片網址)或同資料夾裡的圖片名加副檔名; \
&emsp;     background-position: center center; \
&emsp;     background-attachment: fixed; \
&emsp;     background-repeat: no-repeat; \
&emsp;     background-size: 100% 100%; \
} 

.title-slide {\
&emsp;    background-image: url(圖片網址)或同資料夾裡的圖片名加副檔名; \
&emsp;    background-position: center center; \
&emsp;    background-attachment: fixed; \
&emsp;    background-repeat: no-repeat; \
&emsp;    background-size: 100% 100%; \
}

.section .reveal .state-background {\
&emsp;    background-image: url(圖片網址)或同資料夾裡的圖片名加副檔名; \
&emsp;    background-position: center center; \
&emsp;    background-attachment: fixed; \
&emsp;    background-repeat: no-repeat; \
&emsp;    background-size: 100% 100%; \
}     

2. **換封面主標題字型大小顏色**

title部分(p.s.可以自己再多加subtitle:)\
slides > slide.title-slide p { \
&emsp;  color: white; \
}

slides > slide.title-slide hgroup h1 { \
&emsp;  color: white; \
&emsp;  font-size: 60px; \
&emsp;  letter-spacing: 10; \
}

author以下部分\
slides > slide.title-slide hgroup h2 { \
&emsp;  color: white; \
&emsp;  font-size: 40px; \
&emsp;  letter-spacing: 10; \
}

3. **更改所有投影片字形，可以到電腦中的"系統內容"，搜尋"字形"的名稱。**\
slides > slide {\
&emsp;  font-family: "Microsoft JhengHei", Times, serif;\
}

4. **投影片內頁標題字形大小和顏色**\
h1,h2,h3, h4, h5, h6, h7, h8 { \
&emsp;    color: #444444; \
&emsp;    font-size: 60px; \
}

5. **投影片內容字形顏色**\
p在網頁編碼中是文章段落的意思，而li通常是列表。所以當投影片中直接列1.2.3...時，他會自動視為列表，字的顏色就會是#444444了!若1.2.3....前還加了段文字，會視為文章，字的顏色就會是#000000。

body p { \
&emsp;  color: #000000; \
}

li { \
&emsp;  color:#444444; \
}


6. css檔裡所有的編碼都要寫在<style>和</style>之中。 

**在rmd檔中編寫**

1. **投影片背景**
```r
## 標題 {
data-background="url"       #網址"或圖片名加副檔名 
data-background-size=cover  #背景覆蓋全頁
         }
```
2. **投影片內容分兩欄 **
![image](https://github.com/610611108/Helen-Liu-blog/blob/master/presentation%20in%20r/01.jpg?raw=true)
內容不夠長，可導致有些內容消失在頁面中可用\<br>代替。\<br>是強迫換行的意思。
```r
<div class="columns-2">
...
<br><br>
...
</div>
```
3. **改變某行字形大小與顏色**\
```r
<font color="#8C0044" font size="7">你寫的內容。</font> 
```
4. **空格**\
\&emsp;  .....全型\
\&ensp;  .....半型\
[詳見其他人的blogger說明](https://note.charlestw.com/html-space/)

5. 工具書:R Markdown: The Definitive Guide \
by Yihui Xie, J. J. Allaire, Garrett Grolemund \
2020-04-26 \
[CH.4.1](https://bookdown.org/yihui/rmarkdown/ioslides-presentation.html)
