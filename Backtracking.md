- DFS延伸
- choose 某條路,會change路徑上的status
- 因而在此路不通,遞迴到上一層:
	- 復原沿路更動的con
	- 將前面的 Record的info還原
### 甚麼狀況要backtracking?
- 假設一個棋盤,我們要特定con經過上面的格子
- 且不能repeat
- 這時我們就need visited [ i ]  [ j ] 來記錄(i,j)格子被走過
	- if not visited [ i ]  [  j ] :
	- visited [ i ]  [ j ] =True
	- .....(做相關的操作的遞迴function)
	- visited [ i ]  [ j ] =False
- 因此,退回到上一層,就不影響其他路線
### 適用範圍:
- almost all遞迴型態且需要 rcord condiction的 question
- 只要確定是DFS,就CONSIDER是否要recording
- 留意要小心處理還原的action
	- 所以被動的事情都要記錄
### 37. Sudoku solver
#### solve sudoku
- 每一直行只能剛好有1-9之間各1個
- 每一橫列只能有1-9之間各1個
- 3*3方格的9大格 range也只能 1-9各1
- 所以要先將題目給定盤面狀況 recording下來
	- 記下 row 0-8/ col 0-8/ sqa(方格) 0-8 出現那些數字
	- if not.等下列為要處理的方格(待填的坑)
- 每次取一個待處理的坑,從1-9個填入試試看
- 如果已在對應的row/col/sqa其一位置出現的話表示不能填,必須換下一個
- 可以填的狀況下,就將對應的狀態設定好,在往下看下一個
- 如果後續發現這樣往下填不行,則往上一層要回復(recover)
- 直到順利完成,pits內沒有坑,表示全部結束
### sol:
- init rows,cols,sqas,pits: the first three are defaultdict(set), pit is deque([ ])
- we need function: init(),setN(r,c,n),unsetN(r,c,n),dfs()
- init():初始化整個盤面,將沒填入的值(".")的座標放到pits
	- 遍歷 all board(double loop tracing r,c)
		- 當board[ r ]  [ c ] 不是"."時  ->board [ r ]  [ c ] 加到rows[r] and col[c] and sqas[(r//3,c//3)]中
		- else ->將(r,c)加到pits
- SetN(r,c,n):當確認r,c,n部會衝突時,將對應n加上
	- 將n加到對應的board/rows/cols/各自座標,對pits進行popleft()
- UnSetN(r:c:n):還原SetN的操作
	- 將n從對應的board/rows/cols/sqas各自座標中遺棄(n)pit進行appendleft((r,c))
- dfs():主要遞迴式
	- if pit is empty,return True
	- first looking first points pits[0],
	- loop to check 是否 "1"~"9"可以作為合法n值放入(r,c) position:
		- if yes:
			- setN(r,c,n) ->這將會pits[0]設定並從pits中pop
			- if dfs():return True
			- unsetN(r,c,n) ->這會將pits[0]recover並加回到 pits leftside
- recall init() to init, recall dfs()
```python
class Solution:

    from collections import defaultdict,deque

    def solveSudoku(self, board: List[List[str]]) -> None:

        rows,cols,sqas,pits=defaultdict(set),defaultdict(set),defaultdict(set),deque([])

        def init():

            for r in range(9):

                for c in range(9):

                    if board[r][c]=='.':

                        pits.append((r,c))

                    else:

                        rows[r].add(board[r][c])

                        cols[c].add(board[r][c])

                        sqas[(r//3,c//3)].add(board[r][c])

        def setN(r,c,n):

            board[r][c]=n

            rows[r].add(n)

            cols[c].add(n)

            sqas[(r//3,c//3)].add(n)

            pits.popleft()

        def unsetN(r,c,n):

            board[r][c]='.'

            rows[r].discard(n)

            cols[c].discard(n)

            sqas[(r//3,c//3)].discard(n)

            pits.appendleft((r,c))

        def dfs():

            if not pits:

                return True

            r,c=pits[0]

            for n in ['1','2','3','4','5','6','7','8','9']:

                if n not in rows[r] and n not in cols[c] and n not in sqas[(r//3,c//3)]:

                    setN(r,c,n)

                    if dfs():

                        return True

                    unsetN(r,c,n)

            return False

        init()

        dfs()

        """

        Do not return anything, modify board in-place instead.

        """
```
### 51 N-Queens
- 互相無法吃到 ->同一row/同一col/同一對角線均不能有兩個queens
- 我們可以用一個為n的串列來表示皇后的位置
  (row r 上的皇后在 col c)
- 這樣只剩check col c及對角線是否repeat
- 在相同對角線上的值,其row和col相減的abs相等
- 同樣,每次在新r上選c來擺queen,同樣check是否相符合c和對角線condiction,不符合就表示try失敗,需要還原狀態並嘗試下一個,if符合則要看r是不是last one,yes的話就是ans
- when find one ans,都要將盤面加res,並回到上一層
	(because find other ans)
### sol:
- first init res=[] , state=[-1]*n,r=0(所有盤面解,當前盤面,當前走到row r)
	(abs (state[r]-state[i]))== r-i or state[i] == state[r],符合就 return False)
	- all range(r) check過後沒問題return True
	- getPath(State):將確定是解的state組合出".''和"Q"的組合並return
	- dfs(state,r,res):
		- if r == len(state)時 ->表示find one solution, getpath加上並return 
		- 以loop序將state[r]以0~n-1帶入check去檢查,if符合則r increase,並recall dfs遞迴
		- 回到這層時,將r decrease即可(because check 時邊界setting為r,所以不用消去state值)
	- dfs(state,r,res)代入去recursive get res all ans
	- return res
```python
class Solution:

    def solveNQueens(self, n: int) -> List[List[str]]:

        res=[]

        state=[-1]*n

        r=0

        def check(state,r):

            for i in range(r):

                if abs(state[r]-state[i])==r-i or state[r]==state[i]:

                    return False

            return True

        def getpath(state):

            path=[]

            for i in range(n):

                tmp_row='.'*state[i]+'Q'+'.'*(n-state[i]-1)

                path.append(tmp_row)

            return path

        def dfs(state,r,res):

            if r == len(state):

                res.append(getpath(state))

            else:

                for i in range(n):

                    state[r]=i

                    if check(state,r):

                        r+=1

                        dfs(state,r,res)

                        r-=1

        dfs(state,r,res)

        return res
```
