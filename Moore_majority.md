### vote algorithm
- 每次刪去一個數列中的兩個不同，不會影響該數列的majority element
- eg.
	若有一群人投票，已知A過半
	若有候選人A.B.C任取兩個仁取消他們的投票資格，會怎樣?
	 - (B,C)被取消 -A還是過半
	 - (A,C),(A,B)被取消
	   -> 一個投A和不投A的會被排除
	   ->無法改A的狀態
	 - 同理，候選人不止3人時，每次會取消2人資格，結果不變
#### 適用範圍:
- 判斷一個過半的element

#### 169. majority element I
- 先假定nums的第一個是Majority element，並記錄次數
- if 碰到別的numbers的話，跟次數取消
- 抵銷到0的話，就拿nums的值當新的res
- 最後留下的定是majority element
#### sol:
- 初始化 res=nums[0],cnt=1
- 從i =1開始搜索完nums，進行loop:
	- if res== nums [i] ->cnt++
	- else if cnt>0 -> cnt--
	- else let res == num[i] and cnt== 1
	- return res
```python
class Solution:

    def majorityElement(self, nums: List[int]) -> int:

        #tracing method

        res,cnt=nums[0],1

        for i in range(1,len(nums)):

            if res==nums[i]:

                cnt+=1

            elif cnt>0:

                #count in >0 will -1

                cnt-=1

            else:

                res=nums[i]

                cnt=1

        return res
```

