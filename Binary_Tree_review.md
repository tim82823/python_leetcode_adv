### 二元樹review
- root/left side/right side/node
- every node has val's left/right side
- NIL(None/null)
- if any node tree < root < righttree's value, we can said it's a (BST)(Binary search tree)
---
- traversals:
	- preorder(root/left/right)
	- inorder(left/root/right) ->BST 
	- psotorder(left/right/root)
### 124. Bianary tree Maximum path sum
find the Max path sum
- 對某節點n來說,經過n可能path有4種:
	- 自己單一node(n.val)
	- left side +自己(l+n.val)
	- right side +自己(r+n.val)
	- 左+右+自己(r+l+n.val)
	- 只有前三種 max value對 n的parent有用(因為路徑不能分岔)
	- 先將res設定為root.val往下dfs
	- 利用l,r,m(m為上面前3種max)or計算res並逐層return m即可
### sol:
- init self.res=0
- 主函式self.res設為root.val,並代入self.res中, final return self.res
- dfs(self,n:TreeNode) -> int:
	- l.val需 check n.left,其存在則代入dfs中計算,否則為0;r同理
	- init m=(n.val+n.val,r+n.val,l)中較大者
	- update sele.res and m,l+r+n.val compare
	- return m
	
	```python
	
class Solution:

    def maxPathSum(self, root: Optional[TreeNode]) -> int:

        self.res=root.val

        self.dfs(root)

        return self.res

    def dfs(self,n):

        l=self.dfs(n.left) if n.left else 0

        r=self.dfs(n.right) if n.right else 0

        m=max(n.val,n.val+l,n.val+r)

        self.res=max(self.res,m,n.val+l+r)

        return m
```


### 1008 construct Binary tree from Preorder Traversal

### Preorder Traversal

- preorder ->根左右 ->當我們遇到first node時作為root,其left tree node必須要小於跟,所以邊界root.val;其右子樹的node必須大於根,所以邊界為根的parent的值 
- 所以 recording一個變數i,用來計算當前走過的location
- 當i的len到len(A)(串列長), or index i 的值已超過bound時,return None(因己結束/或是超過這max val)
- 用 A[self.i]來建立一個TreeNode,作為 new root increase self.i
- root.left 可以遞迴call建立,其邊界root.val
- root.right 也可,但邊界bound(上一層代下邊界limit)
- return root 做子樹的根
### sol:
- init i=0
- check self.i 是否為len(A) or A[self.i]>bound(超出range)
	- 是的話 return None
- build root node to TreeNode(A[self.i]), i++
- using recursive to get root.left and root.right,range分別為root.val,bound
- return root

``` python

class Solution:

    i=0

    def bstFromPreorder(self, A: List[int],bound=float('inf')) -> Optional[TreeNode]:

        if self.i==len(A) or A[self.i] >bound:

            return None

        root=TreeNode(A[self.i])

        self.i+=1

        root.left=self.bstFromPreorder(A,root.val)

        root.right=self.bstFromPreorder(A,bound)

        return root
```