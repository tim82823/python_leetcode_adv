### sort method:
- 將一團mess的東西最排序
-  time: O(n)->O(nlogn)
- 不要拿O(n^2)的排序解題
- 穩定排序:相同大小的東西，排序完後先後順序不變
- 空間複雜度:不同排序方法需要不同space
	- 如果限制使用的空間有可能讓原本O(nlogn)排序方法變O(N^2)
- 一些相對應的概念其實在非排序數列的題目也能用到 
- Best/Average/worst case 的複雜度
- 一般常見的幾種排序法通常會有 Divide and Conquer的特性
### 912 sort an array
- 直接使用函數排序
```python
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        nums.sort()
        return nums
        
```
