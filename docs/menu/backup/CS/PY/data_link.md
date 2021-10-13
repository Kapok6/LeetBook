[TOC]

## 总结

- 1 找环入口，找交点：双指针，到头重启其中一个；
- 2 判断是否有环：快慢指针；
- 3 画图辅助思考

## 做题记录

- 21 合并两个有序的列表 (E)
- 23 合并 K 个排序链表 困难(待做)

- 206 反转链表 (E) (易错)
- 24 两两交换链表中的节点 (M)
- 19 删除链表的倒数第 N 个节点 中等

- 203 移除链表元素 (E)
- 83.删除排序链表中的重复元素 E
- 从有序链表中删除重复节点（83）

- 160 找链表交点 (E)
- 141 环形链表 (E)
- 142 环形链表 2 中等
- 160 相交链表 (待做)
- 707 设计链表 中等 (待做)

- 回文链表：判断链表是否为回文（234）

### 21 合并两个有序链表 (E)

1. 思路 ：递归/迭代

```python
# 思路1
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 is None:
            return l2
        elif l2 is None:
            return l1
        elif l1.val<l2.val:
            l1.next = self.mergeTwoLists(l1.next,l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1, l2.next)
            return l2

# 思路2
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        n_head = ListNode(-1)
        p = n_head
        while l1 and l2:
            if l1.val<l2.val:
                p.next = l1
                l1 = l1.next
            else:
                p.next = l2
                l2 = l2.next
            p = p.next # 若缺少这条，无法形成链表
        p.next = l1 if l1 else l2
        return n_head.next
```

### 206 反转链表 (E) (易错)

```python
def reverse(head):
    newhead = None
    while head:
        temp = head.next # 后续链表
        head.next = newhead
        newhead = head
        head = temp
    return newhead
```

### 24 两两交换链表中的节点 (M)

1. 题目：1-2-3-4 => 2-1-4-3
2. 思路:总结递推公式
   1. 若当前及其之后的 1 个结点不空：交换前 2 个结点=>并返回“2-1-继续递归(2 后面的结点，即 2 后面的链表)”
   2. 否则：返回链表当前节点
3. 注意:返回链表某个结点，得到的是该结点及其之后的链！

```python
class Solution:
    # 直接将给定的方法，定义为递归函数
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        first_node = head
        second_node = head.next
        # 1>>2.next 用“递归”求2.next之后的链
        first_node.next  = self.swapPairs(second_node.next)
        # 2.next>>1
        second_node.next = first_node
        return second_node

    # 在给定的方法中，定义新的递归函数
    def swapPairs1(self,head):
        def swap(head):
            if not head or not head.next:
                return head
            f,s = head, head.next
            f.next = self.swapPairs(s.next)
            s.next = f
            return s
        return swap(head)
```

### 2 两数相加 M

1. 思路 1：链表
   1. 循环处理共长部分：2 个变量，存储 进位、余数
   2. 循环处理较长的一个链：注意要加上上面的进位
2. 思路 2：转数组》转链表

```python
class Solution:
    def addTwoNumbers1(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        n_head = ListNode(-1)
        p = n_head
        res, remain = 0, 0
        while l1 and l2:
            # print(l1.val,l2.val,res,remain)
            temp = res
            res = (l1.val + l2.val + res) // 10
            remain = (l1.val + l2.val + temp) % 10
            p.next = ListNode(remain)
            p = p.next
            l1 = l1.next
            l2 = l2.next
        while l1:
            temp = res
            res = (l1.val + res) // 10
            remain = (l1.val + temp) % 10
            p.next = ListNode(remain)
            p = p.next
            l1 = l1.next
        while l2:
            temp = res
            res = (l2.val + res) // 10
            remain = (l2.val + temp) % 10
            p.next = ListNode(remain)
            p = p.next
            l2 = l2.next
        if res:
            p.next = ListNode(res)
        return n_head.next
```

```python
# coding:utf-8

"""
### 21 合并两个有序的列表
解题思路：（新建链）
    新建链存储合并后链表，分别比较2个链的值，先插入小的值，后移（时刻关注两个链为空的情况）
"""
def mergeTwoLists( l1: ListNode, l2: ListNode) -> ListNode:
    n_head = ListNode(-1)
    p = n_head
    while l1 and l2:
        if l1.val<l2.val:
            p.next = l1
            l1 = l1.next
        else:
            p.next = l2
            l2 = l2.next
        p = p.next # 若缺少这条，无法形成链表
    p.next = l1 if l1 else l2
    return n_head.next


"""
203 移除链表元素 (E)：去掉链表中的某个值
"""
def removeElements(head, val):
    nhead = ListNode(None)
    nhead.next = head
    p = nhead
    while p:
        if p.next and p.next.val == val:
            p.next = p.next.next
        else:
            p = p.next
    return nhead.next


'''
83.删除排序链表中的重复元素
'''
def delete_node(head):
    nhead = ListNode(None)
    nhead.next = head
    p = nhead
    while p:
        if p.next and p.val==p.next.val:
            p.next = p.next.next
        else:
            p = p.next
    return nhead.next


"""
160 找链表交点
解题思路：若存在交点，则：a+c+b=b+c+a;(其中c为两链表的公共部分)
   1. 为空判断
   2. 两个链表同时开始后移；当指向为空时，链尾指向令一个链表头部，继续后移；（直到2者指向相同）
"""
def get_insert(headA,headB):
    if headA == None or headB == None:
        return None
    na = headA
    nb = headB
    while na != nb:
        na = headB if na == None else na.next
        nb = headA if nb == None else nb.next
    return na


"""
206 反转链表

解题思路：（头插法、递归）
   1. 创建新的头节点newnode；
   2. 当头head不空时，遍历：
      1. 创建中间节点tmp，存储原链head.next下一个节点；
      2. 将原链的next指向新头
      3. 将新头指向原链
      4. 将原链头部head后移，移到tmp位置；
"""
def reverse(head):
    newhead = None
    while head:
        tmp = head.next
        head.next = newhead
        newhead = head
        head = tmp
    return newhead



'''
141 环形链表 简单 (判断是否有环)

解题思路1：（快慢指针） 空间复杂度O(1)
    1. 快指针，每次步进2；慢指针步进1；
    2. 若存在环，则2指针一定有相遇的时候；
'''
hasCycle(head):
    if head == None: return False
    p1, p2 = head, head
    while True：
        if not(p1,p2.next):
            return False
        p1 = p1.next
        p2 = p2.next.next
        if p1 == p2:
            return True

'''
142 环形链表2 中等（找环的入口）

解题思路1：（快慢指针）
    1. 先用快慢指针, 找到他们相遇点(如果存在环);
    2. 再让快指针从链表头开始,与慢指针共速，再次相遇就是环的入口;

证明：
    变量说明：a链头到环入口距离，b环的周长；
    1. 第1次相遇，2指针步数关系：f = 2s; f = s+nb; 因此：f=2nb;s=nb;两者差nb；
    2. 每个经过环入口的结点，走过的距离为：k = a+nb;又由1得 s=nb；
        2.1* 所以：慢指针第一次相遇后，再走a,即可到环换入口(第1次相遇点到环入口距离为a)；
        2.2 第一次相遇点到环入口距离 = 链头到环入口的距离 = a；
    3. 由2.1得，让快指针从链头开始，慢指针从第一次相遇点开始，同速前进，则到入口相遇；（第2次相遇）

解题思路2：（哈希法）
     把遍历过的节点记录,当发现遍历的节点下一个节点遍历过, 返回它；
'''
def detectCycle(head):
    if head==None:return head
    slow,fast = head, head
    while True:
        if not(fast and fast.next):return None
        slow = slow.next
        fast = fast.next.next
        if slow == fast:break
    fast = head
    while fast!=slow:
        slow = slow.next
        fast = fast.next
    return fast


'''
19 删除链表的倒数第N个节点	中等

难点：不知道链表总长度（一趟扫描实现）

解题思路：（双指针）
    变量说明：假设链表总长为s
    1. 第1个指针先走 n 步；(不是n-1)
    2. 第2个指针加入，从头开始走；
    3. 第1个到达链尾时，第2个指针走了s-n,则“下一个结点”，刚好为倒数第n个结点的位置；
    4. 删除该位置结点；

易错点：
    1. 该题原链不含头结点/头指针（每次首先打印下head）
    2. 第2个指针开始前进的条件：第一个指针运行第n个数（而不是n-1)
'''
def removeNthFromEnd1(head,n):
    if not head:return head
    newhead = ListNode(-1)
    newhead.next = head
    p1,p2 = newhead,newhead
    for i in range(n):
        p1 = p1.next
    while p1 and p1.next:
        p1 = p1.next
        p2 = p2.next
    p2.next = p2.next.next
    return newhead.next

def removeNthFromEnd2(head,n):
    if not head: return head
    newhead = ListNode(-1)
    newhead.next = head
    p1,p2 = newhead,newhead
    cnt = 0
    while p1.next:
        p1 = p1.next
        cnt += 1
        if cnt>n:
            p2 = p2.next
    p2.next = p2.next.next
    return newhead.next


'''
交换链表中的相邻结点（24中）
思路：
1. 新加一个头s，原链a>b>c>d
2. 完成a,b对调：s>b, a>c, b>a
3. 更新s位置：s跳转到b的位置
4. 继续循环2,3，直到s.next和s.next.next不存在
'''
def swapPairs(self, head: ListNode) -> ListNode:
     newHead = ListNode(0)
     newHead.next = head
     cur = newHead // 过程节点
     while c.next and c.next.next:
         a,b = c.next,c.next.next
         c.next,a.next = b,b.next
         b.next = a
         c = c.next.next // 将c指向b
     return newHead.next


'''
回文链表：判断链表是否为回文（234）
思路：
1. 设置快慢指针
2. 每次快指针增加两个，慢指针增加一个(这样当快指针结尾时，慢指针指向了链表的中间)
3. 用慢指针**逆序**链表的后半部分，利用Python交换的特性，不需要额外的tmp结点
4. 一个从头开始，一个从尾开始，判断两者是否相同
'''
def isPalindrome(self, head: ListNode) -> bool:
    if head==None or head.next==None:
        return True
    fast,slow,pre = head,head,None
    while fast!=None: // 让慢指针定位到链表中点(通过让快指针以步长2到达终点)
        slow = slow.next
        fast = fast.next.next if fast.next!=None else fast.next
    while slow!=None: // 引入前序指针Pre，使其最终指向链尾，后半段逆序
        slow.next, slow, pre= pre, slow.next, slow //每对等号同时进行
    while head and pre: //正序遍历前半段，同时逆序遍历后半段，比较是否相等
        if head.val!=pre.val:
            return False
        head = head.next
        pre = pre.next
    return True
```
