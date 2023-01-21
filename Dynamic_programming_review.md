### 動態規劃
- 目標不容易直接透過簡單計算處理
- 但走到第n步的result,可能可以用前面的result表達
- 最開始的 result可直接得到
- 透過loop or recursive完成
- 有不確定因子拆分討論
- 可以透過適當操作降低記憶體需求
### 64 Minimum path sum
最小路徑和
- 有沒有像之前的unique path?
- 先前是值進行相加,現在是要取比較小的值
- 最後 return tail即可
### sol:
- init r,c = len(grid),len(grid[0])
- 將上面一排and左邊一排先直接依序疊加(因只有一條不用比較)
- 使用double loop: i ->range(1,r),j ->range(1,c)
	- grid [ i ]  [ j ] 要加上grid [ i-1 ]  [ j ]  和 grid[ i ]  [ j-1 ] 的較小值
- return grid [r-1]  [c-1]即可
``` python
class Solution:

    def minPathSum(self, grid: List[List[int]]) -> int:

        r,c=len(grid),len(grid[0])

        for i in range(1,r):

            grid[i][0]+=grid[i-1][0]

        for j in range(1,c):

            grid[0][j]+=grid[0][j-1]

        for i in range(1,r):

            for j in range(1,c):

                grid[i][j]+=min(grid[i-1][j],grid[i][j-1])

        return grid[r-1][c-1]
```
### 91.decode ways
A-Z一共26個字母
- if  string is empty,表示無法mapping,return 0(if dp[0]視為1)
- 我們可推0~i的字串的decode種類,會跟0~i-1子字串/ 0~i-2字串有關
- 只要s[i-1]不是"0"的話,就代表它可以獨立做一個字母,所以s[i-1],s[i]各自分開consider的種類就是0~i-1種類(becasue s[i]自己成一種)
- if s[i-2]是"1",or s[i-2]是"2"且 s[i-1]在"6"以下的話,s[i-2]和s[i-1]會combine,然後s[i]自己一種
### sol:
- when s is empty string,return 0
- init l=len(s)
- init dp=[0]*(l+1)
- dp[0], dp[1] (但dp[1]在s[0]="0"要設0)
- init prev,curr = "0",s[0]
- 進行loop,index i 的range(2,l+1):
	- 遞移prev跟curr為curr s[i-1] 
	- if curr 不為"0"的話 ->dp[i]要加上dp[i-1]
	- if prev and curr組起來"1x"~26 ->dp[i]要加 dp[i-2]
- return dp[l]
```python
class Solution:

    def numDecodings(self, s: str) -> int:

        if not s:

            return 0

        l=len(s)

        dp=[0]*(l+1)

        dp[0],dp[1]=1,1 if s[0]!='0' else 0

        prev,curr='0',s[0]

        for i in range(2,l+1):

            prev=curr

            curr=s[i-1]

            if curr != '0':

                dp[i]+=dp[i-1]

            if prev=='1' or (prev=='2' and curr<='6'):

                dp[i]+=dp[i-2]

        return dp[l]
```
