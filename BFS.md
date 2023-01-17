# BFS -> Breadth First Search
- 從起點start,每個可能方向都走一步
- 所有路的第 i 步走完，才會往下走第i+1 step
- 滿足以下棋一停止
	- find goal ans/滿足目標條件
	- 碰壁/死路(也可能和ans違背)
	- 如果是找路徑的話，可以find shortest path
	- 不會繞路,但每一步的可能都要走完
		 - 所以乳果目標在deepth location 會很耗時
- 一般需要資料結構輔助紀錄(quene)
- 一般基礎遞迴都不算BFS
	- 通常BFS會以LOOP加queue輔助
		- 造順序走過相同層 all position,並同時將每個可能的下一層往佇列放
		- 由於佇列是first-in-first-out比較下層的一定比較後面遍歷到
- 適用範圍:
- 分層按順序遍歷的題目
- 樹的走訪--層序(level-order)
- 迷宮/路徑 search
- 在意 shortest path solution
- 可用DFS者,通常也可用BFS,只是思考難度會依題而有所不同
### 111 minimum depth binary tree
- 怎麼知道某node所在depth?
	- 前一層深度+1
	- first layer deoth +1
	- 知道這兩點可用DFS
- 怎樣用BFS?
	- 我們只要走最淺層即可
	- 使用queue,先把(root,1)放進去(node,depth)
	- 每次從queue取出(first check node 是否NIL)
		- Check其左右node是否not exist
			- 兩個 not exist ->到此end,return當前d
			- else ->將(left node,d+1),(right node,d+1)放入queue中
### sol:
- 先check root 為None -> return 0
- 開一個queue 並將(root,1)放進去(可使用collection.deque)
- continue loop,when queue 有東西的話:
	- queue.popleft取得當前node n and depth d
	- if n.left or n. right 為none 則 return d
	- else queue 加上(n.left,d+1) and (n.right,d+1)
```python
# Definition for a binary tree node.

# class TreeNode:

#     def __init__(self, val=0, left=None, right=None):

#         self.val = val

#         self.left = left

#         self.right = right

class Solution:

    def minDepth(self, root: TreeNode) -> int:

        if not root:

            return 0

        left_depth=self.minDepth(root.left)

        right_depth=self.minDepth(root.right)

        if left_depth==0 or right_depth==0:

            return max(left_depth,right_depth)+1

        return min(left_depth,right_depth)+1
```

### 993. cousins in binary tree
cousins means:同輩但不同父母
- 一定要同一層才有意義 -> 所以每次看過一整層node檢查
- 按層order -> BFS
- 從每個Parent node 往left right node看,只要遇到其中一個node的value,就將parent記下來
- 當兩個node都找到,比對彼此的parents是否相等即可
- 如果都在同一層找到其中一個parent,代表不同輩(False)
### sol:
- init firstParent=None(recording first meet parent)
- init queue=collection deque([root])
- 當queue 的 node n 依序取出，並每次check n.left,n.right是否 exist x or y
	- if exist,則看first parent是否紀錄
		- 未紀錄 -> 將其存入
		- 以記錄 -> return (firstparent !=n) result
	- 將所有 n.left / n.right 的node都依序存入nextqueue
- 如果有一層完成, find first parent has record, means不同輩，return false
- 再將 nextqueue assgin to queue
```python
# Definition for a binary tree node.

# class TreeNode:

#     def __init__(self, val=0, left=None, right=None):

#         self.val = val

#         self.left = left

#         self.right = right

class Solution:

    def isCousins(self, root: TreeNode, x: int, y: int) -> bool:

        firstparent=None

        q=collections.deque([root])

        while q:

            nq=collections.deque([root])

            for n in q:

                for nextNode in(n.left,n.right):

                    if nextNode:

                        if nextNode.val in (x,y):

                            if not firstparent:

                                firstparent=n

                            else:

                                return firstparent!=n

                        nq.append(nextNode)

            if firstparent:

                return False

            q=nq
```
