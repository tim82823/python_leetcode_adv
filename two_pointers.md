### 雙指標
- pointer - > 指標
- 標記當前位置
- 透過移動兩個標記，縮小range
- 對當前點操作，右邊往左移，左邊往右移
- 也可能同向移動
### 適用範圍
- 資料通常是單維或線性
- 資料自己有排序特性時，先或不排時間會超過O(logN)
- 資料有前後交換需求時
- 變形: sliding window
### 15. 3 sum
- 先試著想一下暴力解方法，再享優化
- 此題要先排序，時間複雜度為O(NlogN)
- 再者左邊固定一個數X，那麼剩下的我們要找 two sum 跟 sum = - x
- 從剩下的 num LIST中的左側跟右指定一個目標側去做搜尋
	-  eg: 剩下的和是12左邊是3,右邊是15
	- 由於 3+15>12，我們需要較小的，只能將r往左移( l 往又會使 sum變更大，所以不可行)
	- 反之，如果遇到 l 跟 r 所對到的值<12，則要將 l 往右移
	- 剛好若碰到廂等條件時，把( x, nums[l], nums[r] ) 記起來，l往右移，r往左移
	- 掃過一遍，時間複雜度為o(N)
	- 將最左邊的固定的nums一路向右，在兩層loop情形下，時間複雜度為O(N^2)
#### sol:
- 對 nums 數列排序
- 以loop遍歷nums，index為 i=0 到 nums的倒數第3個
- 另 j,k =i+1,nums倒數第一個，target=0-nums[i]
- 進行loop,當j>k:
	- 若 nums[i]+nums[k] == target :
		將res 加上[ nums[i], nums[j], nums[k]}, j++.k-- 
		(increase or decrease to repeat nums)
		
		否則 若 nums[j] + nums[k] < target 則 j++ or k--
	- 最後 return res


```python
class Solution:

    def threeSum(self, nums: List[int]) -> List[List[int]]:

        nums.sort()

        res=[]

        for i in range(0,len(nums)-1):

            if i==0 or (i>0 and nums[i]!=nums[i-1]):

                j,k,target=i+1,len(nums)-1,0-nums[i]

                while j<k:

                    if nums[j]+nums[k]==target:

                        res.append([nums[i],nums[j],nums[k]])

                        while j<k and nums[j]==nums[j+1]:

                            j+=1

                        while j<k and nums[k]==nums[k-1]:

                            k-=1

                        j+=1

                        k-=1

                    elif nums[j]+nums[k]<target:

                        j+=1

                    else:

                        k-=1

        return res
```

### 75. sort color

- sort in-place
	0     1     2
	R    W    B
- 兩次遍歷法:
	- 將所有 0,1,2個數記下
	- 依照順序填入
	- 要經過兩次遍歷

- 一次遍歷法:
-  設 i,i0,i2(當前所在，下一個零，下一個2)
- 當 i 走到與 i 交叉前，進行loop:
	- 碰到0 -> i 和 i0 的值交換，並將i0++
	- 碰到 2 ->i 和 i2 的位置交換，並將i2--
		- 由於i2的位置可能本來就是2，這會讓他可以被交換，所以 i --
		- 將 i++(所以碰到2的狀況 i++被抵銷掉了)
	 - 遍歷完即為正解
```python
class Solution:

    def sortColors(self, nums: List[int]) -> None:

        """

        Do not return anything, modify nums in-place instead.

        """

        i,i0,i2=0,0, len(nums) - 1

        while i<=i2:

            if nums[i]==0:

                nums[i],nums[i0]=nums[i0],nums[i]

                i0+=1

            elif nums[i]==2:

                nums[i],nums[i2]=nums[i2],nums[i]

                i2-=1

                i-=1

            i+=1
```
### permutation in string
- s2需要含有s1的 permutation ->s1的len<=s2的len
	- 同時需驗證 s1or s2 是否存在
- 先將s1的組成都記下來
- 設定 l,r,cnt,l及r均從index 0 開始，cnt則為s1的長度，用來記錄是否構成s1組成
- 當r 尚未超過s2的尾端:
	- check s1的組成有沒有s2[r],有的話表示組成之一，將cnt遞減
	- 將組成的字典對應值遞減，並將r遞增備用
	- 此時如果cnt歸0的話就代表找到全部組成， 則return True
	- check 是否 r和 l len等於s1長度
		是的話，代表無法構成s1的permutation
		- Check l 的位置是否為s1構成，是的話將其補回 cnt遞增
		- 將 l 遞增，保持 l 跟 r len恰等於 s1 
		- 這樣只要r開始移到len(s1)，r跟 l 同步平移保持等距
#### sol:
- check orginal con s1 len is <=s2，不符 return false
- 先初始化 s1h作為s1字元的紀錄
	- 可先從26字母給字典初始為0
	- 每碰到一字元將對應值+1
- 初始化cnt,l,r=len(s1),0,0
- 若 s1h[s2[r]]>0 -> cnt --
- 遞減 s1h[s2[r]] (後續離開時加回),並遞增r
- if cnt為0，return True
- if r-l == sl 長度:
	- 若s1h[s2[l]]=0 -> cnt遞增(因為又要再找嘞)
	- s1h[s2[l]]遞增,l遞增
- 離開迴圈若沒return，代表沒找到，return false
 ```python
class Solution:

    def checkInclusion(self, s1: str, s2: str) -> bool:

        if not s1 or not s2 or len(s1)>len(s2):

            return False

        s1h={}

        for c in 'abcdefghijklmnopqrstuvwxyz':

            s1h[c]=0

        for c in s1:

            s1h[c]+=1

        cnt,l,r =len(s1),0,0

        while r <len(s2):

            if s1h[s2[r]]>0:

                cnt-=1

            s1h[s2[r]]-=1

            r+=1

            if cnt==0:

                return True

            if r-l==len(s1):

                if s1h[s2[l]]>=0:

                    cnt+=1

                    s1h[s2[l]]+=1

        return False
```
