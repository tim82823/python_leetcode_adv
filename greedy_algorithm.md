- 當前每一步都是best c the hoice
- general situtation,將condition 拆成一個subset
	- subset question互不相關,或至少不因前面的question產生choice上的變化
	- 較嚴謹的話使用need proof
- 適用/不適用
- 短視近利不會出狀況
	- EX.假設某個金額最少coin數組合
	- 所以適不適合要依照對應condition決定!
- 動態規劃question,usually不適用
	- 有機會Dynamic用dynamic
- 步步為營question
- 每一分段choose best solution
- choose不能因為前後狀態而被effect到
- 能用Dynamic 還是priority first choice!
### 55. jump game
跳坑
- if index i有個坑(num [i] == 0)
	- 前面必須要可以jump超過i的點
- 前面到不了,後面更到不了
- 所以沿路往下走,當我們可以到 longest lenth <當前point means 到不了 ->坑不過去,死路	
- 或是,從尾往頭看,當看到一個gap,gap setting to 1,往前看 if nums[i] can't jump the gap nums[i],present this point can't jump at the gap,所以要i-1的position觀察
	- 此時 gap++, i-- 
	- if can jump out the gap ,can find the next gap(reset)
	- 若走到頭的位置還沒辦法jump out the gap,means it can't achieve the goal
### sol:
- continue loop i=n-1 -> 0
- if num [ i ] == 0:
	- setting gap=1
	- loop check: if gap > nums [ i ] and gap++ and i--
		- if i<0 return false
	- break all loop means check out return false
```python
class Solution:

    def canJump(self, nums: List[int]) -> bool:

        for i in range(len(nums)-2,-1,-1):

            if nums[i]==0:

                gap=1

                while gap>nums[i]:

                    gap+=1

                    i-=1

                    if i <0:

                        return False

        return True
```

### other sol:
- init farest=0
- continue loop, i to 0 -> n-1:
	- if  i > farest return false
	- farest取farest and i+num[i]的較大值
- finish loop means to the end, just return True
```python
class Solution:

    def canJump(self, nums: List[int]) -> bool:

        farest=0

        for i in range(len(nums)):

            if i > farest:

                return False

            farest=max(farest,i+nums[i])

        return True
```
### 134 gas station
沒油就是要加好加滿
- 留意cost是i ->i+1的消耗
- 要跑完全程 ->每個站都要run
- 先假定起始從尾站start,結束要在index 0
	- 所以油箱存流量是gas[start]-cost[start]
- if start and end 還沒相遇(還沒走一圈)
	- 留存量 >=0:在end的position 加油在上
	 -> renew 油箱存量,並將end往右移
	 存留量 <0:表示start這個點開始會跑到沒油
	 ->將start往左,且更新對應留存量
- 最後兩者相遇，confirm還有油,retrurn to start
### sol:
- init start,end=len(gas)-1,0
- 油箱存量: S=gas[start]-cost[strat]
- if start > end , continue loop:
	- if s>=0  s add end 存量變化, end++
	- else ->start--,s add start存量變化
leave the loop , if s>=0 return start
```python
class Solution:

    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:

        start,end=len(gas)-1,0

        s=gas[start]-cost[start]

        while start > end:

            if s>=0:

                s+=gas[end]-cost[end]

                end+=1

            else:

                start-=1

                s+=gas[start]-cost[start]

        if s>=0:

            return start

        return -1
```