# 算法记录Day 3|链表

链表的基础：

https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html
- 数组是在内存中是连续分布的，但是链表在内存中可不是连续分布的。链表是通过指针域的指针链接在内存中各个节点。
- 增添和删除都是O(1)操作，也不会影响到其他节点。但是要注意，要是删除第五个节点，需要从头节点查找到第四个节点通过next指针进行删除操作，查找的时间复杂度是O(n)。

链表的生成模板
```
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
```

## 203.移除链表元素
逻辑很简单，当current的下一个节点等于val时，我们直接把他的next和他的next.next重连
```
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # 创建虚拟头部节点以简化删除过程
        dummy_head = ListNode(next = head)
        
        # 遍历列表并删除值为val的节点
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        
        return dummy_head.next
```

## 707.设计链表  

这道题目设计链表的五个接口：

1.获取链表第index个节点的数值
2.在链表的最前面插入一个节点
3.在链表的最后面插入一个节点
4.在链表第index个节点前面插入一个节点
5.删除链表的第index个节点

```py
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
        
class MyLinkedList:
    def __init__(self):
        self.dummy_head = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        current = self.dummy_head.next
        for i in range(index):
            current = current.next
            
        return current.val

    def addAtHead(self, val: int) -> None:
        self.dummy_head.next = ListNode(val, self.dummy_head.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummy_head
        while current.next:
            current = current.next
        current.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = ListNode(val, current.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = current.next.next
        self.size -= 1


# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

## 206.反转链表

双指针法。这个方法稍微有点难理解，这里用一个简单的例子进行step by step分析

`1 -> 4 -> 2 -> 3`是我们的example linked list

Before the loop starts:

cur = 1

pre IS NULL

While loop started:

*round 1:*

temp = 4

`cur.next = pre` 相当于断开cur和cur.next的链接，而把cur.next联到pre上，这样，linked list就变为

1 -> NULL   4 -> 2 -> 3

之后对于pre和cur进行更新：
- pre指针移到cur上，cur现在是1，那么pre就被更新为1。
- cur也更新为temp 4

*round 2:*

`temp = cur.next`, cur is 4, cur.next = 2, so temp = 2

`cur.next = pre` linked list就变为:
    4 -> 1 -> NULL  2 -> 3
    
之后对于pre和cur进行更新：
- pre指针移到cur上，cur现在是4，那么pre就被更新为4。
- cur也更新为temp 2

...继续循环直至结束

```
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur = head   
        pre = None
        while cur:
            temp = cur.next # 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = pre #反转
            #更新pre、cur指针
            pre = cur
            cur = temp
        return pre
```

递归写法没有理解，链接放在这里之后再看
https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html#%E6%80%9D%E8%B7%AF
