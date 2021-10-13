[TOC]

## 总结

总结：递归快、非递归节省空间
### 层序遍历
1. BFS+队列

## 分类练习

【构造】

- 根据层序数组构造二叉树

【DFS】

- 二叉树的前、中、后序遍历 144 94 145
- N 叉树的前、后序遍历 589 590
- 100 相同的树 E （待做）

【回溯】

- 112.路径总和 (直接判断) （易错）
- 113.路径总和 II M （路径和为一个定值） （易错）
- 437.路径总和 III M （不限定起点和终点）（易错）

【BFS】

- 102 二叉树的层序遍历
- 429 N 叉树的层序遍历
- 103 二叉树的锯齿形遍历 M
- 二叉树的最大深度 104
- N 叉树的最大深度 559
- 111.二叉树的最小深度 ：建议 BFS
- 199 二叉树的右视图 E BFS
- 101 对称二叉树 E （易错）
- 226 翻转二叉树 E （待做）
- 297.二叉树的序列化与反序列化 H (待做)
- 894.所有可能的满二叉树 中等（待做）

【树高】递归

- 平衡二叉树 110 E （易错）

【二叉搜索树】

- 501 二叉搜索树中的众数 E
- 236 二叉树的最近公共祖先 M
- 671 二叉树中第二小的节点 （待做）
- 98 验证二叉搜索树 中等 （待做）
- 173 二叉搜索树迭代器 中等 （待做）
- 230 二叉搜索树中第 K 小的元素 中等 （待做）
- 1038 从二叉搜索树到更大和树 （待做）
- 538 把二叉搜索树转换为累加树 （待做）
- 700 二叉搜索树中的搜索 （待做）

## 构造

### 根据层序数组构造二叉树

```python
class TreeNode:
    def __init(self,x):
        self.val = x
        self.left = None
        self.right = None
class Tree:
    def create_tree(self,node_list):
        # node_list 数组
        if not node_list:
            return None
        root = TreeNode(node_list[0])
        index = 1
        queue = [root]
        for q in queue:
            if not q:
                continue
            # 添加左右子节点
            if index==len(node_list):
                return root
            q.left = TreeNode(node_list[index]) if node_list[index]!=-1 else None
            queue.append(q.left)
            index+=1
            if index==len(node_list):
                return root
            q.right = TreeNode(node_list[index]) if node_list[index]!=-1 else None
            queue.append(q.right)
            index+=1
```

## DFS

### 二叉树的前、中、后遍历

1. 思路
   - 递归实现：改变插入点位置
   - 非递归实现：前序是基础，后序同前序；巧用栈结构，改变根位置
2. 代码

```python
class TwoRecursion:
    # 【递归】
    def tranverse1(self,root):
        res = []
        def helper(node):
            if not node:return
            res.append(node)# 添加结果的位置，决定遍历的顺序：前/中/后
            helper(node.left)
            helper(node.right)
        helper(root)
        return res

    # 【迭代】
    def tranverse2(self,root):
        res = []
        stack = []
        cur = root
        # 中序
        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.left
            cur = stack.pop()
            res.append(cur.val) # 特别
            cur = cur.right
        return res

        # 前序:不断找左子树，并存储;到底部，回溯找右子树并存储；DLR；
        while stack or cur:
            while cur:
                res.append(cur.val) # 特别
                stack.append(cur)
                cur = cur.left
            cur = stack.pop()
            cur = cur.right
        return res

        # 后序:前序基础上，适当修改，DRL顺序存储，最后取反LRD；
        while stack or cur:
            while cur:
                res.append(cur.val)# 同前序
                stack.append(cur)
                cur = cur.right # 跟前序相反，先找右分支
            cur = stack.pop()
            cur = cur.left
        return res[::-1] # 最后取反
```

### 前/后序遍历 N 叉

1. 递归实现
2. 非递归实现

```python
class NRecursion:
    # 递归
    def tranverse1(self, root: 'Node') :
        res = []
        def helper(root):
            if not root:
                return
            res.append(root.val) #前序
            for child in root.children:
                helper(child)
            # res.append(root.val) #后序
        helper(root)
        return res

    # 迭代
    def tranverse2(self, root: 'Node') :
        if not root:
            return []
        stack = [root]
        res = []
        # 前序
        while stack:
            node = stack.pop()
            res.append(node.val)
            stack.extend(node.children[::-1])
        return res

        # 后序
        while stack:
            cur = stack.pop()
            res.append(cur.val)
            stack.extend(cur.children)
        return res[::-1]
```

### 100.相同的树 E

1. 思路：
2. 代码

```python

```

## 回溯

### 112.路径总和

1. 题目： 判断是否存在：二叉树上根>叶子节点的路径和为一个目标值。
2. 注意：
   1. 判断是否叶子节点，只能通过左、右子树是否均为空；
   2. 每次遇到叶节点，若成功，则更新 res；最终递归完，再返回 res；

```python
class Solution112:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if not root:return False
        res = [False]
        def dfs(root,key):
            if not root: return
            if not root.left and not root.right:
                if key==0:
                    res[0] = True #记录成功结果
            if root.left:dfs(root.left,key-root.left.val)
            if root.right:dfs(root.right,key-root.right.val)
        dfs(root,sum-root.val)
        return res[0]
```

### 113.路径总和 II M

1. 题目： 找到二叉树中，所有符和要求的路径和。
2. 关键： 递归调用时，通过传参改变 path（回溯思想）

```python
class Solution113:
    def hasPathSum(root,target):
        if not root:return []
        res = []
        def dfs(node,path):
            if not node:
                return
            if not node.left and not node.right:
                if sum(path)==target:
                    res.append(path)
            if node.left: dfs(node.left,path+[node.left.val])
            if node.right: dfs(node.right,path+[node.right.val])
        dfs(root,[root.val])
        return res

```

### 437.路径总和 III M

1. 题目：不限定起点和终点
2. 思路：
   1. 前序遍历一颗树；
   2. 针对每个结点，dfs 找路径和

```python
class Solution437:
    # 超时了
    def part_sum1(root,target):
        if not root:return 0
        res = []
        def dfs(node,path):
            if not node:
                return
            if sum(path)==target:
                res.append(path)
            if node.left: dfs(node.left,path+[node.left.val])
            if node.right: dfs(node.right,path+[node.right.val])
        def helper(node):
            if not node:return
            dfs(node,[node.val])
            if node.left:helper(node.left)
            if node.right: helper(node.right)
        helper(root)
        return len(res)

    # 正确：其他人的思路，不记录路径，只更新路径和
    def part_sum2(root,target):
        if not root:return 0
        cnt = [0]
        def dfs(node,sum):
            if not node:
                return
            sum -= node.val
            if sum==0:
                cnt[0] += 1
            dfs(node.left,sum)
            dfs(node.right,sum)
        def helper(node,target):
            if not node:return
            dfs(node,target)
            helper(node.left,target)
            helper(node.right,target)
        helper(root,target)
        return cnt[0]
```

## BFS

### 102.二叉树的层序遍历

1. 层序遍历（2 叉、N 叉）
2. 注意：层序一般为非递归

```python
def layer(root):
    from collections import deque
    if not root:return []
    res = []
    queue = deque([root])
    while queue:
        size = len(queue)
        layer = []
        for _ in range(size):
            cur = queue.popleft()
            layer.append(cur.val)
            if cur.left:queue.append(cur.left)
            if cur.right:queue.append(cur.right)
        res.append(layer)
    return res
```

### 429.N 叉树的层序遍历

```python
def n_layer(root):
    from collections import deque
    if not root:return []
    res = []
    queue = collections.deque([root])
    while queue:
        size = len(queue)
        layer = []
        for _ in range(size):
            cur = queue.popleft()
            layer.append(cur.val)
            for ch in cur.children:# 当子节点存在时再加入队列，减少加入结果集时的判断
                queue.append(ch)
        res.append(layer)
    return res
```

### 103.二叉树的锯齿形遍历 M

1. 思路：BFS，层序遍历，偶数层逆序加入；

```python
class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root: return []
        import collections
        res = []
        depth = 1
        queue = collections.deque([root])
        while queue:
            layer = []
            for _ in range(len(queue)):
                cur = queue.popleft()
                layer.append(cur.val)
                if cur.left: queue.append(cur.left)
                if cur.right: queue.append(cur.right)
            if depth % 2 == 0:
                res.append(layer[::-1])
            else:
                res.append(layer)
            depth += 1
        return res
```

### 二叉树的最大深度 104

- 思路 1：递归
- 思路 2：迭代:dfs，过程中记录树的高度

```python
class Solution104:
    # 递归：自顶向下
    def max_height1(self,root):
        res = [0]
        def dfs(root, depth):
            if not root:return
            res[0] = max(res[0] , depth)
            dfs(root.left, depth + 1)
            dfs(root.right, depth + 1)
        dfs(root, 1)
        return res[0]
    # 递归：自下向上
    def max_height2(self,root):
        def dfs(root):
            if not root:return 0
            left_depth = dfs(root.left)
            right_depth = dfs(root.right)
            return max(left_depth, right_depth) + 1
        res = dfs(root)
        return res
    # 迭代
    def maxDepth(self, root):
        stack = []
        # 存储根节点
        if root is not None:
            stack.append((1,root))
        depth = 0
        while stack != []:
            cur_depth,root = stack.pop()
            if root is not None:
                depth = max(depth,cur_depth)
                stack.append((cur_depth+1,root.left))
                stack.append((cur_depth+1,root.right))
```

### N 叉树的最大深度 559

```python
class Solution559:
    # 自顶向下
    def max_height1(self,root):
        res = [0]
        def dfs(root,depth):
            if not root:return
            res[0] = max(res[0],depth)
            for i in root.children:
                dfs(i,depth+1)
        dfs(root,1)
        return res[0]
     # 自下向上
    def max_height2(self,root):
        def dfs(node):
            if not node:return 0
            elif node.children==[]:return 1
            else:
                return max([dfs(i) for i in node.children])+1
        return dfs(root)

```

### 111.二叉树的最小深度

### 199.二叉树的右视图

1. 题目：如题，返回值`[...]`
2. 思路
   1. DFS+递归：1.先访问右树》左树;2.用 depth 记录层数；
   2. DFS+栈：
      1. 入栈：所有孩子入栈，左树》右树；
      2. 出栈：用 setdefault()，可只设置不存在的键值，避免重复设置；
   3. BFS+队列：1.左树》右树入队；2.给字典重复赋值，最后一次即右端点；
3. 代码

```python
class Solution:
    def dfs_recur(self, root):
        res = []
        def dfs(root,depth):
            if not root:return
            # 未添加时：depth==res；添加完：depth<res;
            if depth==len(res):
                res.append(root.val)
            dfs(root.right,depth+1)
            dfs(root.left,depth+1)
        dfs(root,0)

    def dfs_stack(self, root):
        version = dict()
        stack = [(root, 0)]
        max_depth = 0  # 用于生成结果集时，大小控制
        while stack:
            node, depth = stack.pop()
            max_depth = max(max_depth, depth)
            if node is not None:  # 判空，否则node.val报错（添加左右孩时未判断；）
                version.setdefault(depth, node.val)
                stack.append((node.left, depth + 1))
                stack.append((node.right, depth + 1))
        return [version[depth] for depth in range(max_depth)]
    def bfs(self, root):
        if not root: return []
        queue = collections.deque([root])
        res = []
        while queue:
            size = len(queue)
            temp = -1  # 记录每层最右侧结点的值
            for _ in range(size):
                cur = queue.popleft()
                temp = cur.val
                if cur.left: queue.append(cur.left)
                if cur.right: queue.append(cur.right)
            res.append(temp)
        return res
```

### 101.对称二叉树 E

1. 思路 1：BFS 迭代，存储每层节点，看逆序是否相等(注意孩子为空，也要存)
2. 思路 2：最慢，DFS 递归，对称的性质：左树前序遍历=右树后续遍历的逆序 DLR=LRD[::-1]
3. 思路 3：最快，大佬，BFS 迭代，每次对比(左树左孩子,右树右孩子)，及(左树右孩子,右树左孩子);
4. 思路 4：大佬，DFS 递归，每次对比(左树左孩子,右树右孩子)，及(左树右孩子,右树左孩子);
5. 总结：思路 1/2 较容易想到，思路 3/4,需要对加深对递归的理解；
6. 速度：思路 3 最快，思路 1/4 近似，思路 2 最慢；

```python
class Solution101:
    def isSymmetric1(self, root):
        if not root:return True
        queue = collections.deque([root])
        while queue:
            size = len(queue)
            layer = []
            for _ in range(size):
                cur = queue.popleft()
                if cur:
                    layer.append(cur.val)
                    queue.append(cur.left) if cur.left else queue.append(None)
                    queue.append(cur.right) if cur.right else queue.append(None)
                else: # 节点为空，也要添加
                    layer.append(-1)
            if layer!=layer[::-1]:
                return False
        return True

    def isSymmetric2(self, root):
        bli = []     # 用来存左子树的前序遍历
        fli = []     # 用来存右子树的后序遍历
        if root == None:   # 无根节点
            return True
        if root and root.left == None and root.right == None:  # 只有根节点
            return True
        if root and root.left and root.right:
            self.pre_order(root.left, bli)
            self.post_order(root.right, fli)
            fli.reverse()            # 将后序遍历的列表倒序
            if bli == fli:
                return True
            else:
                return False
    def pre_order(self,root,li):    # 二叉树的前序遍历
        if root:
            li.append(root.val)
            self.pre_order(root.left,li)
            self.pre_order(root.right,li)
        elif root == None:
            li.append(None)
    def post_order(self,root,li):   # 二叉树的后序遍历
        if root:
            self.post_order(root.left,li)
            self.post_order(root.right,li)
            li.append(root.val)
        elif root == None:
            li.append(None)

    def isSymmetric3(self, root):
        queue = collections.deque()
        queue.append((root, root))
        while queue:
            left, right = queue.popleft()
            if not left and not right:
                continue
            if not left or not right:
                return False
            if left.val != right.val:
                return False
            queue.append((left.left, right.right))
            queue.append((left.right, right.left))
        return True

    def isSymmetric4(self, root):
        if not root:return True
        def helper(l_node,r_node):
            if not l_node and not r_node:
                return True
            if not l_node or not r_node or l_node.val!=r_node.val:
                return False
            return helper(l_node.left,r_node.right) and helper(l_node.right,r_node.left)
        return helper(root.left,root.right)
```

### 226.翻转二叉树 E

### 297.二叉树的序列化与反序列化 H
1. 思路
    1. 序列化（树》层序遍历数组/字符串）：(1)借助queue，逐层遍历结点，并添加结果；
    2. 反序列化（数组/字符串》树）：(1)借助queue，存储当前层节点；(2)遍历数组，获取子节点的值，并加入树上；
2. 代码
```python
from collections import deque
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Codec:
    def serialize(self, root):
        if not root:
            return []
        queue = deque([root])
        res = []
        while queue:
            cur = queue.popleft()
            if cur:
                res.append(cur.val)
                queue.append(cur.left)
                queue.append(cur.right)
            else:
                res.append('null')
        return res

    def get_node(self, index, arr):
        if index < len(arr):
            if arr[index] != 'null':
                return TreeNode(arr[index])
            else:
                return None

    def deserialize(self, data):
        node_list = data
        node_len = len(node_list)
        if node_len == 0:
            return None
        index = 0
        root = TreeNode(node_list[0])
        queue = deque([root])
        while queue:
            cur = queue.popleft()
            if not cur:
                continue
            index += 1
            if index < node_len:
                cur.left = self.get_node(index, node_list)
                queue.append(cur.left)
            index += 1
            if index < node_len:
                cur.right = self.get_node(index, node_list)
                queue.append(cur.right)
        return root
```

## 树高

### 平衡二叉树 110 E （易错）

1. 题目：左右树高，相差不多于 1

```python
class Solution:
    def get_height(self,root):
        if not root: return 0
        return max(self.get_height(root.left), self.get_height(root.right)) + 1
    def isBalanced(self, root: TreeNode) -> bool:
        if not root:return True
        left = self.get_height(root.left)
        right = self.get_height(root.right)
        return (abs(left-right)<=1 and self.isBalanced(root.left) and self.isBalanced(root.right))

```

## 二叉搜索树

### 501 二叉搜索树中的众数 E

1. 思路：(ct.values())；遍历 dict(ct).items(),存储 value 为最大的

```python
class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        if not root:return []
        from collections import Counter
        res = []
        frequency = []
        def get_max(root):
            if not root: return
            get_max(root.left)
            frequency.append(root.val)
            get_max(root.right)
        get_max(root)
        voc = Counter(frequency)
        max_cnt = max(voc.values())
        for i,v in dict(voc).items():
            if v==max_cnt:
                res.append(i)
        return res
```

### 236 二叉树的最近公共祖先 M

1. 题目:
   由题知：root 为节点 p,q 某公共祖先，前提是：root.left 和 root.right 都不是 p,q 的公共祖先。因此：若 root 是 p,q 的最近公共祖先，则只可能为以下情况之一：
   1. p 和 q 在 root 的子树中，且分别在 root 子树的异侧；
   2. p/q 为根；
2. 思路：后序遍历+递归
   1. 终止条件：到达叶节点，或 node 为 p/q；
   2. 递归：左子树、右子树遍历
   3. 返回值:根据 left 和 right ，可展开为四种情况；
      1. 当 left 和 right 同时为空 ：说明 root 的左 / 右子树中都不包含 p,q，返回 null ；
      2. 当 left 和 right 同时不为空 ：说明 p, q 分列在 root 的异侧（分别在 左 / 右子树），因此 root 为最近公共祖先，返回 root；
      3. left 为空，right 不为空 ：p,q 都不在 root 的左子树中，直接返回 right。具体可分为两种情况：
         - p,q 其中一个在 root 的 右子树 中，此时 right 指向 p（假设为 p ）；
         - p,q 两节点都在 root 的 右子树 中，此时的 right 指向 最近公共祖先节点 ；
      4. 当 left 不为空，right 为空 ：与情况 3. 同理；

```python
class Solution:
    def lowestCommonAncestor(self, root, p, q) :
        if not root or root == p or root == q: return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left and not right: return # 1.
        if not left: return right # 3.
        if not right: return left # 4.
        return root # 2. if left and right
```

### 671 二叉树中第二小的节点 （待做）

### 98 验证二叉搜索树 中等 （待做）

### 173 二叉搜索树迭代器 中等 （待做）

### 230 二叉搜索树中第 K 小的元素 中等 （待做）

### 1038 从二叉搜索树到更大和树 （待做）

### 538 把二叉搜索树转换为累加树 （待做）

### 700 二叉搜索树中的搜索 （待做）
