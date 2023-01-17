- Depth first Search
- 從起點開始，一路往深的走
- 到滿足下兩者旗一錢不回頭
	- find ans/滿足condition
	- 碰壁(確定和ans違背or 死路)
- 可能繞路
	- if find the path,the path maybe would not the shortest path
	- 不一定要走過all data
- 一般遞迴基本上都是DFS
- 假設一個函式add底下呼叫twice add(call ddd',ddd")
- every time when the function goes down, it will call ddd' first
- 接下來往下一層遞迴時，一樣會call ddd'
- 直到ddd'走到底，無法在走時，回到上層call ddd"
- 所以同一層一定是先碰到底往下走的
### 適用範圍:
- 所有遞迴題目
- 樹的走訪
- 迷宮/路徑
- 找錯還原走過痕跡:
	- 回朔(back track)
### 687. Longest univalue path:
- 一條路徑在幾種 univalue path:
	- 學前節點是路徑最上面的值
		- 那只要找 left side/right side longest univalue path
		- 兩個相家可得總路徑
	- 當前 node 不是path 中最上面的value ->one side need to fall away
		- 取當前最left side /right side的路徑較大值+1，最為return 往上return 長度(這樣才有chance 銜接)
	- 取得一條路徑長，跟全局max compare 並 update
### sol:
- 初始化設定最大路徑為self.max=0
- 在函式中，先check root 是否為 NIL -> Yes,return 0
- 將root ，root.val代入遞迴式count to start DFS
- 在(count,val)中:
	- 一樣check root 是否為NIL -> YES,return 0
	- 分別用root.left,root.val and root.right,root.val代入count,取得當前value為univalue,往左/往右路徑為 l,r
	- check l+r 和self.max大小,更新較大值
	- if root.val和先前傳入val 一樣，還可以 return max(l,r)+1做延續長度，else return 0

```python

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def __init__(self):
        self.mx=0
    def longestUnivaluePath(self, root: TreeNode) -> int:
        if not root:
            return 0
        self.count(root,root.val)
        return self.mx
    
    
    def count(self, root:TreeNode , val:int) -> int:
        if not root:
            return 0
        l=self.count(root.left,root.val)
        r=self.count(root.right,root.val)
        self.mx=max(self.mx,l+r)
        return 0 if val!= root.val else 1+ max(l,r)
```






	