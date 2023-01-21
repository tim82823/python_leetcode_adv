- node/head/node.val(data)
- next/prev
- if it is not a cycle, the last one node.next would be connect to NIL
- how to check the cycle?
	- using one slow and one faster two pointers method!
- disconnect link remember be aware front and back node relationship

### 61. rotate lsit
旋轉串列,不如先連起來
- 首先要check head和head.next不存在的condition,怎麼轉都一樣
- 接下來先遍歷整個串列,即可知道總長的cnt,將k取cnt的餘數
- 若 k% cnt為0,表一模一樣不需要rotate -> return head
- 將前面遍歷的尾端先接回head(變一個大環)
- 由於rotate k times(已取餘數)相當於第cnt -k個點的新head,所以 cnt-k-1走到ans上個node prev
- 新的 head node res=prev.next
- 再將 prev.next 接到none上, return res
### sol:
- if not head or not head.next then return head
- init cnt=1,ite=head
- loop traverse ite.next to empty(也就ite走到尾端)(每次cnt++)
- k%=cnt and will ite.next link to head
- let prev from head 走到cnt-k-1,走到node的前一步
- res=prev.next
- prev.next 斷開成None
- return res
``` python
# Definition for singly-linked list.

# class ListNode:

#     def __init__(self, val=0, next=None):

#         self.val = val

#         self.next = next

class Solution:

    def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:

        if not head or not head.next:

            return head

        cnt=1

        ite=head

        while ite.next:

            ite=ite.next

            cnt+=1

        if k%cnt==0:

            return head

        k %= cnt

        ite.next=head

        prev=head

        for _ in range(cnt-k-1):

            prev=prev.next

        res=prev.next

        prev.next=None

        return res
```

### 141 link list cycle
two pointers 偵測環
- 一快一慢,快地走兩步,慢得走一步
- 只要有環,肯定最後追上
- 沒有環,快的一定先到尾端
### sol:
- check head and head.next 是否為null
	- 是的話因為個數一個以下右沒連到自己,不可能形成環
	- return alse
- init fast/slow從head開始
- 當fast和˙fast.next沒遇到NIL,進行loop:
	- fast=fast.next.next
	- slow=slow.next
	- check if fast and slo重疊,return true
- 執行完畢在迴圈外代表fast先走到none了,所以return false
``` python
# Definition for singly-linked list.

# class ListNode:

#     def __init__(self, x):

#         self.val = x

#         self.next = None

  

class Solution:

    def hasCycle(self, head: ListNode) -> bool:

        if not head or not head.next:

            return False

        fast,slow=head,head

        while fast and fast.next:

            fast=fast.next.next

            slow=slow.next

            if fast==slow:

                return True

        return False
```
