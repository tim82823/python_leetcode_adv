- 篩法:將一群數根據條件進行篩選
- 質數篩法 ->只保留質數
- 一路將所有質數作為檢查用，將其倍數除去
#### 適用範圍:
- check質數
- 一次建立一個範圍的，而非檢查一個數
- 記得留意邊界
### 204. Count Primes
- 先將需要的ans表
- 宣告一組prime([1]*n or [true]*n)
- 初始化 prime[0] and prime[1] 為false
- 從 2 找到 sqrt(n),if see prime number,[ i*i :n :i] 對應值改掉
### sol:
- if n <3 -> return 0
- prime[0]=prime[1]=false
- 用一個for loop ，當中 i 的range(2,int(n**0.5+1))
	- 如果prime[i] true ->處理對應的部分變false
- 當loop 完成，只需return prime 中對的個數
```python
class Solution:

    def countPrimes(self, n: int) -> int:

        if n < 3 :

            return 0

        prime=[True]*n

        prime[0]=prime[1]=0

        for i in range(2,int(n**0.5+1)):

            if prime[i]==True:

                prime[i*i: n: i] = [False for _ in range(i*i, n, i)]

        return sum(prime)
```