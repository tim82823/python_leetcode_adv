-  FIFO(fisrt in, first out)
- 向排隊買票一樣，先排先買
- offer(enqueue)入隊
	poll(dequeue)出隊
	peak 偷看
- 在python中請使用deque,因為list操作效率極差
	- offer ->  append()
	- poll -> popleft()
	- paekm -> deque[0]
- 適用範圍:
	- 每次需要被檢查的東西就是按剩下順序放入
	- 當需要按順序儲存的data structure(且不希望一次全數取用)
	- 幾乎所有需要看最先進入的資料值題目
	- 可能搭配遞迴/迴圈使用
		- 常使用佇列可將遞迴改迭代解
		- 所以要用哪一個,就看 require order
	- deque也可用於stack,因為double link(但其實可用list即可)
	- 所有比較在乎頭尾新增和del的效率,用deque較ok
### 429 N-ary Tree level order Traversal
- N-ary代表每個節點的子傑點最多可連接到多少個(不一定要連到N)
- 底下的子節點用children表示(可以用list表示)
- level order traversal ->按層排列
- 每次走完一層,中間依照順序將節點放入Queue即可
### sol:
- init q= collections.deque([root]),res=[ ]
- check root is None or not, if None return res
- when q value esxit,go loop:
	- init lvl to put same level's node,cnt to recording 當前q的len
	- 執行一個迴圈cnt次數(每遞減cnt次數)
		- 將q取最 left side point node n,將其值家到 lvl
		- 從n.children中依序取出子點c,附加到q上
	- 最後將 |v|附加到res上
- 迴圈 finish, return res
```python
"""

# Definition for a Node.

class Node:

    def __init__(self, val=None, children=None):

        self.val = val

        self.children = children

"""

  

class Solution:

    def levelOrder(self, root: 'Node') -> List[List[int]]:

        res=[]

        if not root: return res

        q=collections.deque([root])

        while q:

            cnt=len(q)

            lvl=[]

            while cnt:

                n=q.popleft()

                lvl.append(n.val)

                for c in n.children:

                    q.append(c)

                cnt-=1

            res.append(lvl)

        return res
```

### 1161 Maxium level sum of binary Tree
- level x of maximum level sum
	- 表示每一層都要sort,且要記得x哪層
- init level,res,reslevel,q
- 和上一題相似
	- 不過變成每次算完這層的sum
	- 沿路n.lfet/n.right加入到q中
- 當check這層 sum比較大,updtae to res/reslevel
- 往下層時,level ++
- 最後return reslevel
### sol:
- init level,res,reslevel=1,0,1
- q=collections.deque([root])
- if q is not empty,enter loop:
	- init cnt=len(q),cur=0
	- using cnt 進行loop:
		- 從q取出node n,並將cur將上n的值,cnt--
		- 將n.left/n.right當中並非None的node附加在q上
	- if cnt>res -> update res,reslevel=cur,level
	- level++
	- return reslevel
```python
# Definition for a binary tree node.

# class TreeNode:

#     def __init__(self, val=0, left=None, right=None):

#         self.val = val

#         self.left = left

#         self.right = right

class Solution:

    def maxLevelSum(self, root: TreeNode) -> int:

  

        level,res,reslevel=1,root.val,1

        q=collections.deque([root])

        while q:

            cur=0

            cnt=len(q)

            while cnt:

                n=q.popleft()

                cur+=n.val

                if n.left:q.append(n.left)

                if n.right:q.append(n.right)

                cnt-=1

            if cur > res:

                res=cur

                reslevel=level

            level+=1

        return reslevel
```