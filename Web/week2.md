- [Outline](#Outline)
- [Injection](#Injection)
    - [Code injection](#Code_injection)
    - [command injection](#command_injection)
        - [command substitution](#command_substitution)
            - [空格鍵被ban掉的時候](#空格鍵被ban掉的時候)
            - [bypass掉黑名單](#bypass掉黑名單)
    - [SQL injection](#SQLinjection)
        - [Data Exfiltration](#Data_Exfiltration)
        - [Union Based](#Union_Based)
        - [SQL injection lab:Log me in: Revenge](#SQL_injection_lab:Log_me_in:Revenge)
        - [information schema](#information_schema)
        - [Blind(Boolen base)](#Blind(Boolen_base))
        - [Blind(Time base)](#Blind(Time_base))
        - [Error_based or Out-of-band(噴到外網)](#Error_based_or_Out-of-band(噴到外網))
        - [如何用mySQL_讀檔寫檔](#如何用mySQL_讀檔寫檔)
    - [Template_injection(SSTI)](#Template_injection(SSTI))
    - [SSRF](#SSRF)
        - [Scheme的攻擊面](#Scheme的攻擊面)
        - [Scheme的攻擊面](#Scheme的攻擊面)
            - [gopher](#gopher)
            - [gopher_lab:Web_Preview_Card](#gopher_lab:Web_Preview_Card)
        - [決定可不可以被SSRF](#決定可不可以被SSRF)
    - [反序列化](#反序列化)
        - [反序列化案例-Pickle(一個for_python內建的序列化格式)](#反序列化案例-Pickle(一個for_python內建的序列化格式))


# Outline
> ![image](https://hackmd.io/_uploads/Sk_2CiSvT.png)

## Injection
> ![image](https://hackmd.io/_uploads/SyHm1nSDT.png)

### Code_injection
> ![image](https://hackmd.io/_uploads/SJFl-2rv6.png)
- 變成
> ![image](https://hackmd.io/_uploads/ryFbbnBv6.png)

- 容易產生code injection的function
- python 裡的exec和eval的差別，exec可以直接執行整段程式碼，eval只能一句
> ![image](https://hackmd.io/_uploads/HyPNWhBv6.png)

### command_injection
> ![image](https://hackmd.io/_uploads/r1UvYnHwT.png)
- 同等於
> ![image](https://hackmd.io/_uploads/HJ3KKhBva.png)
- 尾部加上;ls -al 用;結束前面的式子
> ![image](https://hackmd.io/_uploads/Sk8bv6rD6.png)
- 簡單的小trick
> ![image](https://hackmd.io/_uploads/r1oIPTSP6.png)
### command_substitution
- 小括號也可以 linux shell的小括號裡也可以執行
> ![image](https://hackmd.io/_uploads/B1w6qpSva.png)
#### 空格鍵被ban掉的時候
> ![image](https://hackmd.io/_uploads/S1YriarD6.png)
#### bypass掉黑名單
> ![image](https://hackmd.io/_uploads/rk7R26Bv6.png)
### SQL_injection
> ![image](https://hackmd.io/_uploads/B1UxsJ8wa.png)
- 萬能密碼
> ![image](https://hackmd.io/_uploads/Hyjq3JIvT.png)
#### Data_Exfiltration
> ![image](https://hackmd.io/_uploads/B1UtSeIw6.png)

#### Union_Based
- 用來把兩個欄位連起來
> ![image](https://hackmd.io/_uploads/Hyy7FeIPp.png)
- 幫他加上一個1,2的欄位
> ![image](https://hackmd.io/_uploads/Hkx5FgUwa.png)
- 此時幫他注入一個不存在的id 就會顯示出我們select進去的資料了
> ![image](https://hackmd.io/_uploads/ByUTYlUw6.png)
- 此時加入一些有趣的function就可以得到一些資料
> ![image](https://hackmd.io/_uploads/Bypb5e8Pp.png)
- 也可以去其他表拿user出來
> ![image](https://hackmd.io/_uploads/H1TEcxLvT.png)

#### SQL_injection_lab:Log_me_in:Revenge
> ![image](https://hackmd.io/_uploads/SkF6KupwT.png)
- 先去看source code,要求 cur的username跟password跟已存的一樣才會return flag
> ![image](https://hackmd.io/_uploads/Byaddu6wp.png)
- 密碼要輸的跟union的一樣,就可以get flag
> ![image](https://hackmd.io/_uploads/HyHofK6Da.png)


#### information_schema
- 要如何知道存在什麼table跟column?
- MySQL有一個叫information schema
> ![image](https://hackmd.io/_uploads/Hk7p9xUPT.png)
- 就可以用這種方法拿到table name,limit是說只要拿到第幾張表就好
> ![image](https://hackmd.io/_uploads/HJUvjxIDa.png)

#### Blind(Boolen_base)
> ![image](https://hackmd.io/_uploads/H1EhQZUP6.png)
- 可以用來查詢id存不存在
> ![image](https://hackmd.io/_uploads/HkAH4W8Dp.png)
- 也可以查詢user有幾個
> ![image](https://hackmd.io/_uploads/ryXxH-LPa.png)
#### Blind(Time_base)
> ![image](https://hackmd.io/_uploads/HJwISZIPT.png)
- ex:如果有大於80就sleep10秒鐘
> ![image](https://hackmd.io/_uploads/SkIqrZUva.png)
#### Error_based_or_Out-of-band(噴到外網)
> ![image](https://hackmd.io/_uploads/SyJ18bIwp.png)

#### 如何用mySQL_讀檔寫檔
> ![image](https://hackmd.io/_uploads/SJzFLbUD6.png)
### Template_injection(SSTI)
- 資料跟模板是分開的,透過render再喇在一起,而template injection就是指模板是可以控制的
> ![image](https://hackmd.io/_uploads/ByxiwbIwp.png)
- 乖乖輸入的樣子
> ![image](https://hackmd.io/_uploads/HySUubUwT.png)
- 輸入一個template的syntax,就會被當成模板來執行
> ![image](https://hackmd.io/_uploads/r19D_-8Dp.png)
- 如何判斷是用哪種模板引擎
> ![image](https://hackmd.io/_uploads/SkAgKZ8va.png)
- 也沒辦法直接import function 因為再類似沙箱的環境
> ![image](https://hackmd.io/_uploads/H1-1qbLD6.png)
- 因此要利用jinja2 內建的函式
> ![image](https://hackmd.io/_uploads/rJ2b9-LDp.png)
- 另一種payload,不需要依賴jinja2內建的函式
> ![image](https://hackmd.io/_uploads/HyTO9WUPT.png)
- 其他類比較簡單的SSTI
> ![image](https://hackmd.io/_uploads/SyrSjZ8Da.png)
### SSRF 
> ![image](https://hackmd.io/_uploads/BJFxX8LPa.png)
- 如何識別SSRF有沒有存在
> ![image](https://hackmd.io/_uploads/ByOB88IPa.png)
- SSRF通常由網址決定能不能成功
> ![image](https://hackmd.io/_uploads/H1OsIIIva.png)
#### Scheme的攻擊面
> ![image](https://hackmd.io/_uploads/Sy9gP8IPT.png)

> ![image](https://hackmd.io/_uploads/S1Wx9uID6.png)
##### gopher
> ![image](https://hackmd.io/_uploads/SJbDyBPPp.png)
- 用gopher造一次get 請求
> ![image](https://hackmd.io/_uploads/By16MrPvp.png)

#### gopher_lab:Web_Preview_Card
- 要構造一個post請求
> ![image](https://hackmd.io/_uploads/Hy1lk26wT.png)


#### 決定可不可以被SSRF
> ![image](https://hackmd.io/_uploads/Bk994SwvT.png)
- 簡單的bypass方法
> ![image](https://hackmd.io/_uploads/rJBAEHwwa.png)

> ![image](https://hackmd.io/_uploads/HyZMrSDva.png)
### 反序列化
- 序列化介紹: ex遊戲要下線存檔，要把玩家資料像是HP裝被轉成可以存的格式，反序列化就是把序列化的東西轉回來
- 下圖:把一個物件轉成可傳輸的JSON物件格式，而反序列化是最後一行把他轉回物件
> ![image](https://hackmd.io/_uploads/SJKkFa9vT.png)
- 用json來進行反序列化是沒問題的，但如果使用者可控資料且用的是eval就會出問題
> ![image](https://hackmd.io/_uploads/HJdkq69vT.png)
- 反序列化中最根本的問題
> ![image](https://hackmd.io/_uploads/BJTHqacwp.png)
#### 反序列化案例-Pickle(一個for_python內建的序列化格式)
- pickle的序列化與反序列化例子
> ![image](https://hackmd.io/_uploads/Byc1SRsD6.png)

- reduce的使用
> ![image](https://hackmd.io/_uploads/rkT4Tv6w6.png)

- 透過magic method: __ _reduce_ __ 舉的例子，reduce的功能是定義你再序列化的時候要序列化出什麼東西，像是剛剛寫在裡面的system跟id，在隨便一個地方都可以load進來
> ![image](https://hackmd.io/_uploads/HyswK0swT.png)
>
> ![image](https://hackmd.io/_uploads/ryaFYRiwp.png)


