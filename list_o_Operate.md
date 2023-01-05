- (array/list) operate
- init: list() or []
- 位移值選取(可用負倒數)
- for element in list
	- 可指定對應單位數彈性選取:
	- l=[[1,3],[2,5],[-88,2],["XD",5]]
	- for i,j in l:
	     print(f"{i},{j})
	- slice:[start:end:step]
#### 串列常見用法:
- l [ -1] ->最後一個元素, l[::-1] ->反轉串列
- append() -> 作為一個單位附加到尾端
- extend() ,+= -> 將後面串列按單位一一接到尾端
- insert(index,element) -> 插入到指定位置(超過則尾端)
- del [shift] -> 刪除
- remove(value),pop()
- index(), in, count(), len()
- join(),split()
- sort(),copy()
- list comprehension(串列表達式/列表生成式)
#### 串列常見的坑
- 請部要用[ [0*n]*m]的方式生成 m * n 的二維串列
	- [ [0]*n for __　ｉｎ　ｒａｎｇｅ（ｍ）］－＞正解

- 用＝來指派，用ＣＯＰＹ來複製
	- 指派一個相同的東西的話，一個ｃｈａｎｇｅ，另一個也會ｃｈａｎｇｅ
	- 要ｃｏｐｙ一個ｌ串列變成一個新串列，可以使用：
		-　ｒ＝ｌ．ｃｏｐｙ（）ｏｒ　ｒ＝ｌｉｓｔ（ｌ）ｏｒ　ｒ＝ｌ［：］
		-   sort ()　會對原本串列排序 -> 排完後原先東西會變
		-  sorted() 會給出一個新的排序串列回傳 ->排完後原先東西不變
###  適用範圍:
- 所有基礎資料操作
- 留意python多維串列宣告方法:
- Queue/Stack 實作可用list
	- 但對queue效率較差
	- 請使用deque
- 先整理題目邏輯，在應用基本操作
####  926. flip string to Montone increasing
	eg.
		000110
		output:1
		需要翻轉一次
先理出邏輯:
- 由於單調遞增，從左到右某點變為1，其後通通變為1
- 最後完成的結果有兩種:
	- 1.全部都是0
	- 2.前面是0，後面是１
-  在第 i 位時要保持單調遞增次數:
	- a. s[ i ]== 0 ,condition 0 ->不需要任何是即可保持
	- b. s[ i ] == 1, condition 0 ->要將s[ i ]變成 0 (翻轉次數+1)
	- c. s[ i ] == 0 , condition 1 ->要將s[ i ]變成1(翻轉+1)
	- c. s[ i ] == 1, condition 1 ->第 i-1時選 0 or 1 都可
	- 假設前一步為止狀況0所需翻轉次數為f0，狀況1的翻轉次數為f1
	- 從 i-1更新道第i位的 f0, f1
		 對前面a/c的條件,f0不變，但f1增加
		  對前面b/d的條件，f1取f0，f1當中較小的值, f0必須要增加
		  (因為走0或1均可)
	- 所以.先設 f0 , f1 均為 0 開始，一路向右走，按照碰到字元操作更新 f0 跟 f1 完成後取 f0 跟 f1中較小值即可
	#### sol:
	- 令 f0 , f1 為0
	- 從 s 迴圈中取出字元 ch:
		-  if ch == '0' ->f1 increase
		- else f1值 f0, f1中較小值，再將 f0 增加
	- 最後回傳 f0, f1 中較小值
```python
class Solution:
    def minFlipsMonoIncr(self, s: str) -> int:
        f0,f1=0,0
        for ch in s:
            if ch=="0":
                f1+=1
            else:
                f1=min(f0,f1)
                f0+=1
        return min(f0,f1)
```

### 238 product of array except self

- eg.
若要計算的是[2,3,4,5]
結果會是 [ 3*4*5,2*4*5,2*3*5,2*3*4]
解析:
| num | 2 | 3 | 4 | 5 |
| res | 3 * 4 * 5 | 2 * 4 * 5 | 2 * 3 * 5 | 2 * 3 * 4 |  
| l |  | 2 | 2 * 3  | 2 * 3 * 4 |  
| r | 3 * 4 * 5 |  4 * 5 | 5 |  | 
- 把左邊算一組，會是 nums [ i-1 ]一路乘過去
- 把右邊算一組，會是 nums [ n-1 ]一路往回乘
#### sol:
- 初始化 n = len(nums)及 res=[0]*n
- res[0]=1 (因為要乘過去)
- 先處理左邊，進行loop index i 為 range(1,n):
      res[i]= res[i-1]*nums[i-1]
- *前面完成的結果會長的跟 l 一樣(留意最後左側是 1)
- 定義一個 r=1，做相乘用
- 在處理右邊，進行loop，index i 為 range(n-1,-1,-1):
    res[i]*=r
    r*=nums[i]
- 完成後r的部分就乘進res裡，只須回傳 res
```python
class Solution:

    def productExceptSelf(self, nums: List[int]) -> List[int]:

        n=len(nums)

        res=[0]*n

        res[0]=1

        for i in range(1,n):

            res[i]=res[i-1]*nums[i-1]

        r=1

        for i in range(n-1,-1,-1):

            res[i]*=r

            r*=nums[i]

        return res
```
