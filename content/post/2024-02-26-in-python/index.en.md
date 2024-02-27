---
title: 自動預約掛號 in Python
author: Helen Liu
date: '2024-02-26'
slug: in-python
categories:
  - Python
tags:
  - Python
---
![黃牛](https://raw.githubusercontent.com/610611108/Helen-Liu-blog/master/blogger%20pictures/1811.gif)

我讓黃牛失業了!之前因為家人有掛號需要,發現連幾天每次都是開放之前就守在電腦前等待,仍舊掛不到號.= =. 到底啥時開始連掛號也跟台鐵一樣難搶,想起大學時,一遇連假就需早上六點前守在電腦前準備買票.不知大家是否也有一樣的回憶呢?掛到號後,想寫文章跟大家分享好久了.拖拖拖拖,拖到現在.

&ensp;&ensp;這次撰寫程式時,使用的是Python的Anaconda.記得使用自動掛號時一定要設個停止條件和休息時間,免得被以為是惡意攻擊導致被鎖.別像我一樣第一次就被鎖了一整天.不知是不是跟著IP還是自己的ID鎖的,就算換電腦手機都不行.

&ensp;&ensp;這次要預約的是屏東我家附近的太丞中醫,院長的號真的是有夠難掛的.聽鄰居說這個醫生很厲害,因為妹妹每次經痛都痛的想死,起初只是試試,沒想到真的有效.想掛到號靠人工真的太累,這時發現找到學寫程式的第二好處了!哈哈...

寫此程式要注意的有幾點:  
  - 這家醫院預約流程是一個月前開始掛號,預約時間是醫院的診療時間(這家是早上8:30)才能開掛.所以需要設定掛號時間區間.除此之外,這個if條件對只有筆電的人來說超重要.因為筆電不像桌電可以預約開機和啟動程式.要不需要提前起來開機跑程式,要不就須睡前讓程式開跑.而提前開跑不設動作時間會浪費cpu,第一次寫的時候沒想到這個,結果他就當給我看.而且不是關機重新開anaconda就好了.需要進入cmd刪除記錄才行.當下真得嚇死,研究整天的程式就這樣失效還把電腦搞壞???...
  - 為避免被鎖要設按預約確定鍵的休息時間,大概3到5秒就行了.
  - 不知為何程式填資料時,不設休息時間的話,有時會填失敗.不過設個1秒就能解決這問題了.

```python
# 在cmd模式下,用於刪除使Anaconda開不了的紀錄.
# jupyter nbconvert --ClearOutputPreprocessor.enabled=True --inplace tcyi_appointment_mon.ipynb

pip install selenium
pip install interval
pip install schedule

import time
from interval import Interval
import schedule
```

```python
def tcyi(id_num,bir_year,bir_month,bir_day):
    
    from selenium import webdriver
    driver = webdriver.Chrome()
    
    driver.get('http://tcyj.1655.com.tw/mobile/on_link.php')
    
    # https://selenium-python-zh.readthedocs.io/en/latest/locating-elements.html

    driver.find_element('name', '4').click()
    
    from selenium.webdriver.common.by import By
    n = len(driver.find_elements(By.CLASS_NAME, "regbtn"));n
    
    driver.find_elements(By.CLASS_NAME, "regbtn")[n-1].text
    
    for i in range(1,n-1):
        if driver.find_elements(By.CLASS_NAME, "regbtn")[n-i].text !='已額滿^':
            driver.find_elements(By.CLASS_NAME, "regbtn")[n-i].click()
        
            time.sleep(1)  
            driver.find_element(By.NAME, "reg_no").text
            from selenium.webdriver.support.ui import Select
            select_1 = Select(driver.find_element(By.NAME, "reg_no"))
            # for op in select_1.options:
            #     print(op.text)
            select_1.select_by_index(1)
        
            time.sleep(1)
            select_2 = Select(driver.find_element(By.NAME, "ddl_year"))
            # for op in select_2.options:
            #     print(op.text)
            select_2.select_by_visible_text(bir_year)
        
            time.sleep(1)
            select_3 = Select(driver.find_element(By.NAME, "ddl_month"))
            # for op in select_3.options:
            #    print(op.text)
            select_3.select_by_visible_text(bir_month)
        
            time.sleep(1)
            select_4 = Select(driver.find_element(By.NAME, "ddl_day"))
            # for op in select_4.options:
            #     print(op.text)
            select_4.select_by_visible_text(bir_day)
        
            time.sleep(1)
            driver.find_element('name', 'txtID').send_keys(id_num)
            driver.find_element(By.ID, "sub1").click()
        
            time.sleep(3)  
        
            alert = driver.switch_to.alert # 切換到彈窗
            alert.accept() # 接受
        
            time.sleep(3) 
            break
```

```python
while True:
    # https://www.runoob.com/python/att-time-strftime.html
    now_localtime = time.strftime("%Y-%m-%d,%H:%M:%S", time.localtime())
    now_time = Interval(now_localtime,now_localtime)
    print(now_time)
    
    time_interval = Interval("2023-02-20,08:30:00","2022-02-20,08:45:00")
    print(time_interval)
    
    if now_time in time_interval:
        tcyi("t220000000","49","12","30") #第一個人
        time.sleep(4) 
        tcyi("a123456789","48","12","02") #第二個人
        time.sleep(4)  
```
