- 特殊二元樹(完全二元樹)
- min heap/ max heap
- min heap:
	parent node < children node forever
- max heap:
	parent node > children node forever
- 使用串列實作, min heap 對於所有從0start的k都滿足
	heap[k] <= heap[2*k+1] and heap[k]<=heap[2*k+2]
- why need heap?
- 在python 可將list輸出給heapq來order
	- heap queue:
		堆積佇列,也可叫priority queue
	-  when elements insert heap  or from heap del , heap will keep original feature
		- not need consider remain's parts,資結會自動adjust
		- 所以當指考慮前幾個small/large val,heap會有優勢
	-  haepq 相關操作
		- heapify(將一個list轉loop)
		- heappush/heappop/heappushpop(放入/取出/先放入後取出)
		- nlargest/nsmallest(取前n大/前n小的elements)
### 適用範圍:
- 當 need min.heap時,直接用heapq.heapify(list)
- 當 need max.heap時,將elements代入負號帶入,取出再處理即可
- 如果題目指要求前n大/前n小的數,可以依靠只塞入固定大小處理(後面用heappushpop)
### 1046 last stone weight:
相消後大小改變:
- 每次拿大的,所以heapq,但元素帶負號
- 先進行heapify(h),h為stone數字全加負號串列
- if h 長度>1時,要持續crush:
	- 拿兩次就是兩個最大的石頭,假設值是y,x
	- if x == y  -> 相消,不管
	- if x != y ->取差值,要放入h還是要負的, so y-x(because y<x)
	- 最終 h 為empty,return 0
	- 否則 return -h[0]
### sol:
- init h=[-x for x in stones]
- 對 h進行heapify
- if len(h)>1,執行loop:
	- 從h中heappop兩次得y,x
	- if x!=y 將y-x進行heappush到h中
- 最後當 len(h) == 0 -> return 0
- else return  -h[0] 
``` python
class Solution:

    def lastStoneWeight(self, stones: List[int]) -> int:

        h=[-x for x in stones]

        heapq.heapify(h)

        while len(h)>1:

            y=heapq.heappop(h)

            x=heapq.heappop(h)

            if y != x:

                heapq.heappush(h,y-x)

        if len(h)==0:

            return 0

        else:

            return -h[0]
           


```
### 451. sort charter By Frequency
按出現frequence排序
- 我們可用collections.counters計數(也可用dict)
- 先走一遍出現字母k及頻率f
- 將頻率帶負號,加上出現字母乘上頻率(-f,k*f)推入list
- 在進行heapify
- 接下來依序heappop出來並將res組上string
- * if s length 限定小範圍,could consider bucket sort
### sol :
- init cnt= Collections.Counter(s)
- heap from cnt.item()取出以(-f,k*f)放入list
- 對heap進行heapify
- 初始res=''
- if haep still has a val, proceed loop:
	- every time get val, add res
- return res
```python
class Solution:

    def frequencySort(self, s: str) -> str:

        cnt=collections.Counter(s)

        heap=[(-f,k*f) for k,f in cnt.items()]

        heapq.heapify(heap)

        res=''

        while heap:

            f,k =heapq.heappop(heap)

            res+=k

        return res
```
