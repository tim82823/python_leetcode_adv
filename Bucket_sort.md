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
- "aA"視作不一樣
- 構成Palindrome的要件
	-  Palindrome裡面字元要兩兩成對
	- 允許唯一例外
	- 所以我們要記住每一地個大小寫字母出現幾次
		- 所有成對的部分均可放入，所以成對的均計入長度
		- 最檢有沒有機會放單一一的字元(只能放入一次)
	- 可以串列做為桶，也可以用字典
### sol:
- 宣告 I=0及一個長度128的串列(ASCII code len)
- 將每個s裡的字母迴圈放入cnt中(使用ord()函式)
- 進行迴圈，i 的範圍為range(65,91)(即'A' to 'Z')
	- 將 I 加上 cnt[i] //2 * 2('A' to 'Z')
	-  將 I 加上 cnt[i+32] //2 * 2('a' to 'z')
- 再進一次迴圈，i 的range(65,91)
	- if cnt[i] 或 cnt[i+32]除以2餘1，並將l++並break
- 最後回傳 l 即可
```python
class Solution:

    def longestPalindrome(self, s: str) -> int:

        l=0

        cnt=[0]*128

        for ch in s:

            cnt[ord(ch)]+=1

        for i in range (ord('A'),ord('Z')+1):

            l+=cnt[i]//2*2+cnt[i+32]//2*2

        for i in range(65,91):

            if cnt[i]%2==1 or cnt[i+32]%2==1:

                l+=1

                break

        return l
```
