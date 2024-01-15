[TOC]
# 反序列化
## pickle的反序列化
- 如果有人load一個任意的物件進來要如何攻擊那個點呢
> ![image](https://hackmd.io/_uploads/H1V-MLxda.png)

- 先定義一個class並裡頭要有一個method叫reduce然後會回傳兩個turple，第二個turple是第一個function的argument:目的是把turple的第二部分丟給function的第二部份當argument來吃，因此dump出來後再丟load就會觸發反序列化
> ![image](https://hackmd.io/_uploads/BkPGQUl_p.png)

- 範例:
> ![image](https://hackmd.io/_uploads/rJ4HNUlOT.png)

- 用pickletools的dis(disassemble)可以幫我們分析剛剛序列化的那串是甚麼
> ![image](https://hackmd.io/_uploads/HyKqEIld6.png)
- 這串的大概說明
> ![image](https://hackmd.io/_uploads/r1_FYLl_6.png)
 
## PHP 反/序列化
- PHP序列化大概的格式:有形別標記告訴你是boolean還是object或是array
> ![image](https://hackmd.io/_uploads/B1nymLZOT.png)

- PHP的magic method 
> ![image](https://hackmd.io/_uploads/HJUKmI-_p.png)

- destruct()的話直接new他就成功
> ![image](https://hackmd.io/_uploads/rJ3z6q-up.png)

- toString要再把他轉成string才會顯示出來
> ![image](https://hackmd.io/_uploads/HJ62i9Wda.png)

- __wakeup()是反序列化的時候會自己呼叫起來,php所有變數都是用$開頭
- 原本序列化只會出現序列化的變數，當你反序列化後就自動call了wakeup
> ![image](https://hackmd.io/_uploads/B1x4CcZuT.png)

- 一個攻擊的例子，只要把聲音改成了id就達成command injection
> ![image](https://hackmd.io/_uploads/S1aryi-Oa.png)

- 在pwn那邊有教學，去補(ROP chain)，現實沒有那麼容易達成Injection，是由很多片段組合而成的
> ![image](https://hackmd.io/_uploads/rkFt7ib_a.png)

- [PHPGGC](https://github.com/ambionics/phpggc)這拿來找chain很好用
- 範例:在wakeup的時候對magic用cast方法，而$casr_func把它當屬性值並接上可控的$val
> ![image](https://hackmd.io/_uploads/Bkj-KCGO6.png)
> 
> ![image](https://hackmd.io/_uploads/S1XTHoZOT.png)

- 解釋
> ![image](https://hackmd.io/_uploads/ry50Ss-_6.png)
- 就變成這樣了
> ![image](https://hackmd.io/_uploads/HJJchAfOa.png)
- 一些PHP的小知識，可以直接whoami
- 1. 可以把任意字串當成function
> ![image](https://hackmd.io/_uploads/HyZ7ARzu6.png)
- 2. 這個也是 跟上面功能一樣
> ![image](https://hackmd.io/_uploads/rJ9u0Af_6.png)

### lab:magic cat 先跳過


## Java反序列化
- 跟PHP滿類似，也有一個好用的tool:[ysoserial](https://github.com/frohoff/ysoserial)
>　![image](https://hackmd.io/_uploads/HJQv_MX_6.png)

# 前端安全
> ![image](https://hackmd.io/_uploads/r1Y-tfm_a.png)

- 同源政策為前端安全最基本要素
> ![image](https://hackmd.io/_uploads/By0i8NE_6.png)

- 跨域不能做的事
> ![image](https://hackmd.io/_uploads/HJzy7Ypup.png)

- 跨域可以做的
> ![image](https://hackmd.io/_uploads/rksxQYaup.png)

> ![image](https://hackmd.io/_uploads/ByJPdPMYp.png)


## CSRF
- 核心價值為濫用同源政策
> ![image](https://hackmd.io/_uploads/BkvfzB4O6.png)

- 定義
> ![image](https://hackmd.io/_uploads/SyC53DzKT.png)

- 不想用get改用post的例子
> ![image](https://hackmd.io/_uploads/SkpRhwMta.png)

- 把她的action改成送一個表單
> ![image](https://hackmd.io/_uploads/H14I6DfF6.png)

### 如何防禦CSRF
- CSRF token
> ![image](https://hackmd.io/_uploads/r1z3CDGFa.png)

- 當你進行偽造的時候 後端會比對這個token
> ![image](https://hackmd.io/_uploads/r1W7e_GF6.png)

- 此時駭客不會知道csrf token是多少
> ![image](https://hackmd.io/_uploads/r1CIeuMKp.png)

- 以下這三種狀況都不能達成CSRF :第一個是當不是用get或post的情況下
> ![image](https://hackmd.io/_uploads/SyXsmdzFa.png)

- samesite cookie
>![image](https://hackmd.io/_uploads/S1Nzr_MKT.png)

## XSS
> ![image](https://hackmd.io/_uploads/SkVTLdGF6.png)

- 隨便找地方一直貼script發現可以運作就代表有XSS
> ![image](https://hackmd.io/_uploads/BkmwIuMYa.png)

- 如何避免就是把一些html的字元像是小括號做htmlenties encode就不會被觸發了
> ![image](https://hackmd.io/_uploads/SkpsU_Mtp.png)

- self-xss:像是點了惡意網站FB一直被發貼文
> ![image](https://hackmd.io/_uploads/BkJ7D_MF6.png)

- xss的三種類型
> ![image](https://hackmd.io/_uploads/rkP6D_GY6.png)

### reflected XSS
> ![image](https://hackmd.io/_uploads/SJ0Rvuftp.png)

### stored xss
- 比較powerful的XSS
>　![image](https://hackmd.io/_uploads/HyuSO_MKa.png)

### DOM-based XSS
> ![image](https://hackmd.io/_uploads/S1H3_OGFp.png)

### 除了script可以觸發到js的東西
- event handler
> ![image](https://hackmd.io/_uploads/HJSZ5OMta.png)

- js:scheme
> ![image](https://hackmd.io/_uploads/HyZ8c_MYT.png)

### 如何防止XSS
- 最簡單的:黑名單
> ![image](https://hackmd.io/_uploads/rJKmsdMYa.png)

- 但有JSFuck這種東西，因為JS很自由，可以把js encode成怪怪的東西
>　![image](https://hackmd.io/_uploads/HkdoiuftT.png)

- 一個完整的XSS payload 清單:[XSS payload](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
>　![image](https://hackmd.io/_uploads/r1F72_zF6.png)

- XSS能做的事情
> ![image](https://hackmd.io/_uploads/rkjA3dGKa.png)

- 防禦方法
> ![image](https://hackmd.io/_uploads/rk8-pOGKa.png)

- 一些難防禦的地方
> ![image](https://hackmd.io/_uploads/SJzg1YzKa.png)

- [DOMPurify](https://cure53.de/purify) 一個正規一點的HTML snytax過濾器
> ![image](https://hackmd.io/_uploads/Syyc0OzFa.png)

- Content-security -policy 一種有效防禦XSS的policy
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/) 可以用來幫你確認你設置的policy好不好
> ![image](https://hackmd.io/_uploads/rkk9JYGFa.png)

- JOSNP 介紹
> ![image](https://hackmd.io/_uploads/H1YM-KzKT.png)

### XS-Leak
> ![image](https://hackmd.io/_uploads/B1BPGtGK6.png)

- 核心概念:找到A在有資料跟沒資料前後的行為差異性
> ![image](https://hackmd.io/_uploads/B1ptGYzFa.png)

- 雖然無法知道iframe裡面有甚麼資料，但你可以知道那裏有個iframe
> ![image](https://hackmd.io/_uploads/BywTzYfYT.png)

### DOM Clobbering
- 如何在js 模擬點擊按鈕，但你也可以只寫click()
> ![image](https://hackmd.io/_uploads/BywIXYfF6.png)

> ![image](https://hackmd.io/_uploads/Bkr5mYMKa.png)

- 特性是透過輸入id就可以得到元素
> ![image](https://hackmd.io/_uploads/SkykVYfYa.png)

- 因此可以想辦法蓋掉她的id改成自己的，這是最核心原理

- 一個可以用生成payload的工具:[DOM Clobber3r](https://splitline.github.io/DOM-Clobber3r/)
> ![image](https://hackmd.io/_uploads/SyIaVtMtT.png)
