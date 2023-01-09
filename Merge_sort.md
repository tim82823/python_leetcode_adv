- 固定 O(nlogn)的時間複雜度
- 分成一半，將其排好重組
- 遞迴往下，最終一定len剩1
	(長度剩1定是排好的)
- 對於兩個排好的數列，怎麼merge?
	- 從各自min開始compare
	- 將較小的val insert
		 (相同的則先 insert left side)
- cut left side and right side
- recursive mergesort function
- 將兩數列依大小排入
- return上一層
### 適用範圍:
- 留意整個mergesort(),合併merge()
- 適用(divide and conqur)
- 想組合數個自己排序的串列/跟陣列
- 需求O(nlogn)的排序演算法
- 不限定 n-place
### 148.sort list
對 Linkedlist sort
- question request O(nlogn)
- 可以merge sort
- 如何切半? 使用一快一慢的two pointers
	- fast兩步，slow一步
	- 當fast走到底，slow會走到中間位置
	- 取head和slow即為半的兩個list開頭
- 分割開來，遞迴呼叫合併
- 如何merge?
	- 使用 dummy node 做 head
	- compare two linklist併按一個大小街上去
### sol:
- sortlist:
	- check head and head.next is none or not(if yes return head)\
	- init prev,slow,fast is None,head,head
	- 當fast即fast.next均非none時，迴圈執行 :
	- prev,slow,fast=slow,slow.next,fast.next.next
		 (prev的目的是要記住slow的前一個node,用來切斷)
	- 將prev.next設定為None
		 (這樣left side的最後一個node就和右邊第一個node分割開了)
	- 將head和slow分別代入sort list 遞迴並取得n1,n2兩個list
	- 將n1,n2 merge
	- merge(n1,n2):
	- init dummy node 命名為n
	- init 一個用遍歷node命名為ite=n
	- 當n1,n2都還有node,進行loop:
		- 並將較small 的 node 往下走
		(也就是n1=n1.next or n2=n2.next)
		- ite goes down
		- n1 or n2 還有剩時，直接全數接到ite end
	- final retrurn n.next
```python
# Definition for singly-linked list.

# class ListNode:

#     def __init__(self, val=0, next=None):

#         self.val = val

#         self.next = next

class Solution:

    def sortList(self, head: ListNode) -> ListNode:

        if not head or not head.next:

            return head

        prev,slow,fast=None,head,head

        while fast and fast.next:

            prev=slow

            slow=slow.next

            fast=fast.next.next

        prev.next=None

        n1=self.sortList(head)

        n2=self.sortList(slow)

        return merge(n1,n2)

    def merge(self , n1:ListNode, n2:ListNode)-> ListNode:

        n=ListNode(0)

        ite=n

        while n1 and n2:

            if n1.val<n2.val:

                ite.next=n1

                n1=n1.next

            else:

                ite.next=n2

                n2=n2.next

            ite=ite.next

        if n1:

            ite.next=n1

        if n2:

            itw.next=n2

        return n .next
```