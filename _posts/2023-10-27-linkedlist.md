# 算法记录Day 4|链表


## 24. 两两交换链表中的节点 

![image](https://github.com/aprilmingyli/aprilmingyli.github/assets/148889738/940a7567-07b1-4e4a-be98-a8328ba69ef5)

这里就是利用递归，首先因为题目要求两两交换，所以我们就刚好利用pre和cur这一对儿，把next和pre进行交换就完成了。之后持续对以next为head的后续链表两两交换直到结束。

每次iteration之后，cur node becomes the **new head** of the swapped pair. 所以最后要return cur
- After swapping the current pair of nodes, the cur node points to the new head (the node that was the second element in the pair before swapping).

```

class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        # 待翻转的两个node分别是pre和cur
        pre = head
        cur = head.next
        next = head.next.next
        
        cur.next = pre  # 交换
        pre.next = self.swapPairs(next) # 将以next为head的后续链表两两交换
         
        return cur

```

##  19.删除链表的倒数第N个节点  
讲解：https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html#%E6%80%9D%E8%B7%AF

这里需要注意的一个点就是：fast首先走n + 1步 ，原因是只有这样同时移动的时候slow才能指向删除节点的上一个节点（方便做删除操作）

for循环的目的是为了将 fast 指针初始化到一个与 slow 指针相隔 n+1 个节点的位置，在 for 循环之后，fast 指针已经初始化到了第 n+1 个节点（从 slow 指针的位置算起）。然后，while 循环用来同时移动 fast 和 slow 指针，直到 fast 到达链表的末尾，这样 slow 恰好停在倒数第 n 个节点的前一个节点上。

```
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # 创建一个虚拟节点，并将其下一个指针设置为链表的头部
        dummy_head = ListNode(0, head)
        
        # 创建两个指针，慢指针和快指针，并将它们初始化为虚拟节点
        slow = fast = dummy_head
        
        # 快指针比慢指针快 n+1 步
        for i in range(n+1):
            fast = fast.next
        
        # 移动两个指针，直到快速指针到达链表的末尾
        while fast:
            slow = slow.next
            fast = fast.next
        
        # 通过更新第 (n-1) 个节点的 next 指针删除第 n 个节点
        slow.next = slow.next.next
        
        return dummy_head.next
```

## 160.链表相交

https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html#%E6%80%9D%E8%B7%AF
简单来说，就是求两个链表交点节点的指针。交点不是数值相等，而是指针相等。
为了方便举例，假设节点元素数值相等，则节点指针相等。
