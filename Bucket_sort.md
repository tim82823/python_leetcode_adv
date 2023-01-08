### 桶排序
- 將數值依照桶子對應分桶(同一桶按照大小順序排列)
- 桶子要能覆蓋表達數值範圍
- 裝完以後，在按桶子順序排列
- Time complexity: O(n+k)
	- n ->個數
	- k ->桶子總數
- worst case: O(n^2)
- 非比較排序(comparsion sort)，所以最佳解不受 O(nlogn)限制
- 空間複雜度:O(nk)
#### 優點:
- 在range小可quick sort
- 最佳解會比O(nlogn)快
#### 缺點:
- 需要多餘空間
- 數值範圍大/桶數多速度慢
#### 應用範圍:
- 類似 hash table提目
- 題目有限定範圍
### 409 longest palindrome
