- 平均即best time 為 O(nlogn)時間複雜度，worst case: O(n^2)
- 每次選定一個pivot
- 將所有比基準點小的數排到基準左邊
- 將所有比基準點大的數排到基準右邊
- 此時 pivot這個點的最後index即固定
- 將 nums[:index]即nums[index:]分別帶入遞迴，直到所有位置固定，即為完成排序
### sort array:
- 選取 pivot可以隨自己喜好
	- (left side/right side/medium/random)
- if in-place方式，留意記得最後要交換

- 整個quick sort 的關鍵；
	- quick sort(choose pivot 代入 and 遞迴)
	- partition(排出左右兩區)
- 適用範圍:
	- 需要切partition或找特定排名
		- 此時又稱quick sort
	- 被要求使用時間(容易詢問優缺點)
		- good: 可以做到 in-place
		- bad:時間為O(n^2)
	- 其他時間使用 merge sort
### 215.k th largest element in array
- 找第k大的元素，等同n-k+1的元素(n=len(nums))
	- index會是n-k
- 做partition以後會找到一個新pivot的index值
- if pivot index比n-k小 ->從pivot右側為基準找剩下
- if pivot index比n-k大 ->從pivot左側為基準來找剩下
- 兩者相等 -> FIND,直接return 該點value
### sol:
- 先轉換 k=len(nums)-k(所以現在找第k小的值)
- 定義一個用遞迴的select(quick sort) function,參數為nums跟k
- pivot隨機選0~len(nums)-1之間的數(可用 random uniform)
- l, r 初始化為兩個空的串列，並依序check  nums每個數n:
	(這裡拿空間O(n)換時間)
	- n <=nums[pivot] ->append l (但不能是pivot)
	- n >=nums[pivot] ->append r
	- 放完以後len(l)的值即為新的 pivot index
	- k == len(l) ->return nums[pivot]
	- k < len(l) ->return select(1,k)
	- k >len(l) -> return select(r,k-len(l)-1)
		(因為扣掉l全長和pivot)
- 以nums跟k做為參數取select的return即為ANS
```python

from random import uniform

class Solution:

    def findKthLargest(self, nums: List[int], k: int) -> int:

        k=len(nums)-k

        def select(nums,k):

            pivot=int(uniform(0,len(nums)))

            l,r=[],[]

            for i,n in enumerate(nums):

                if n<=nums[pivot] and i!=pivot:

                    l.append(n)

                if n>=nums[pivot]:

                    r.append(n)

            if k==len(l):

                return nums[pivot]

            elif k<len(l):

                return select(l,k)

            else:

                return select(r,k-len(l)-1)

        return select(nums,k)
```
