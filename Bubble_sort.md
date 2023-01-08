- 正常狀況Time是O(n^2)
- 運作流程:
- 從串列開頭起，比較相鄰的樹，如果排靠前的較大，將其交換
- 跑完一次迴圈後，最後一個數就是最大
- 再跑第二次迴圈後，但結束點微陣列倒數第二個數，完成後倒數第二個數為次大的數
- 以此類推，全數做完及排序結束
### 適用範圍
- 不適用解題，因為 O(n^2)傷不起
- 不過若考官問O(n^2)的時間複雜度:
	- bubble,insertion,selection

#### eg. example code
```python
n=len(nums)
for i in range(n):
	for j in range(0,n-1-i):
		if num[j]>nums[j+1]:
			num[j],num[j+1]=num[j+1],num[j]
```
