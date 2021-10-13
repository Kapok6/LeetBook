[TOC]

## 总结

### DFS 总结

1. 模板

```
    1. dfs(结点/坐标)
    2. 终止条件:到达边界
    3. 当前步操作：已访问、更新结果
    4. 递归遍历其兄弟/邻居
```

## DFS

- 690.员工的重要性 E
- 999 可以被一步捕获的棋子数（易）
- 820 单词的压缩编码 （中） 真题
- 面试题 13. 机器人的运动范围
- 463.岛屿的周长（易）
- 695 岛屿的最大面积 （中）
- 934.最短的桥 中等 DFS
- 117.填充每个节点的下一个右侧节点指针 II 中
- 685.冗余连接 困难 DFS（待做）

- 面试题 17.22. 单词转换 中等 专业级模拟试题一（待做）
- 542.01 矩阵 （中）（待做）
- 207.课程表 M （待做）
- 210.课程表 II （待做）
- 827.最大人工岛 （难）（待做）
- 531.孤独像素 I 中等 会员 DFS（待做）
- 533.孤独像素 II 中等 会员 DFS（待做）

### 690 员工的重要性 E

DFS(id)，每次遍历雇员信息，找到该员工，递归遍历其下属；

```python
class Solution690:
    def getImportance(self, employees: List['Employee'], id: int) -> int:
        if not employees:return 0
        res = [0]
        def helper(node): # id
            if not node:return
            for i in employees:#找到id对应的员工
                if i.id==node:
                    res[0] += i.importance
                    for sub in i.subordinates:#递归遍历其下属
                        helper(sub)
        helper(id)
        return res[0]
```

### 999 可以被一步捕获的棋子数（易）

- 在车的四个方向上进行深度搜索
- 车的移动规则：一步之内只能走直线，上/下/左/右，步长不限；

```python
def numRookCaptures(board):
    # 在四个方向上，分别进行深度搜索
    def get_cnt(i,j):
        cnt = 0
        for x, y in [(-1,0),(0,1),(1,0),(0,-1)]:
            # 注意，step重置要放在外层
            step = 0
            # 在单一方向上，进行深搜
            while True:
                sx = i + step*x
                sy = j + step*y
                if sx<0 or sx>=8 or sy<0 or sy>=8 or board[sx][sy]=="B": break
                if board[sx][sy]=="p":
                    cnt += 1
                    break
                step += 1
        return cnt
    row, col = 8,8
    # 找到R点位置后，调用深搜方法
    for i in range(len(board)):
        for j in range(len(board[0])):
            if board[i][j] == "R":
               cnt = get_cnt(i,j)
               return cnt

```

### 820 单词的压缩编码 （中） 真题

存储后缀集，集合，discard 去掉集合中指定元素

```python
def minimumLengthEncoding(words):
    word_set = set(words)
    for word in words:
        # 遍历每个单词，从后缀集合中去掉该单词的后缀
        for i in range(1,len(word)):
            # discard去掉集合中指定元素
            word_set.discard(word[i:])
    # 每个单词长度加1
    return sum(len(j)+1 for j in word_set)

```

### 面试题 13. 机器人的运动范围

1. 解题思路 1：非算法类，直接根据题意来编码
   1. 定义可达矩阵 res，初始化全为 0，将 res[0][0]置为 1
   2. 逐行遍历整个矩阵，判断是否可达，原则如下：
      若下标各位之和小于 k+1，且左边元素/上侧元素可达，则将该元素置为可达；
   3. 注意：“下标各位之和小于 k+1”的计算方式：
      - 题目规定了矩阵大小为（1,100），所以下标 i 最多 2 位数,各位之和=i//10+i%10;
      - 若扩展到无数位，各位之和：
      ```
      def sums(x):
          s = 0
          while x != 0:
                  s += x % 10 # 余数直接归入结果
                  x = x // 10 # 非余数部分，继续看是否能被10整除
          return s
      ```
2. 解题思路 2：DFS
   - DFS 思想：先朝一个方向搜到底，再回溯至上个节点，沿另一个方向搜索，以此类推。
   1. 递归参数： 当前元素在矩阵中的行列索引 i 和 j。
   2. 终止条件： 当行列索引越界 或 数位和超出目标值 k 或 当前元素已访问过 时，返回 0 ，代表不计入可达解。
   3. 递推工作：
      - 标记当前单元格 ：将索引 (i, j) 存入 Set visited 中，代表此单元格已被访问过。
      - 搜索下一单元格： 计算当前元素的 下、右 两个方向元素的数位和，并开启下层递归 。
      - 回溯返回值： 返回 1 + 右方搜索的可达解总数 + 下方搜索的可达解总数，代表从本单元格递归搜索的可达解总数。
3. 解题思路 3：BFS
   - BFS 思想：按照“平推”的方式向前搜索。
   1. 初始化： 将机器人初始点 (0, 0)(0,0) 加入队列 queue ；
   2. 迭代终止条件： queue 为空。代表已遍历完所有可达解。
   3. 迭代工作：
      - 单元格出队： 将队首单元格的 索引、数位和 弹出，作为当前搜索单元格。
      - 判断是否跳过： 若 行列索引越界 或 数位和超出目标值 k 或 当前元素已访问过 时，执行 跳过 continue 。
      - 标记当前单元格 ：将单元格索引 (i, j) 存入 visited 中，代表此单元格 已被访问过 。
      - 单元格入队： 将当前元素的 下方、右方 单元格的 索引、数位和 加入 queue 。
      - 返回值： visited 的长度 len(visited) ，即可达解的数量。

```python
def movingCount1(m,n,k):
    res_arr = [[0]*n for _ in range(m)]
    def cnt(i,j):
        return (i%10+i//10)+(j%10+j//10)
    res_arr[0][0]=1
    for i in range(m):
        for j in range(n):
            # 判断i and res_arr[i-1][j]，只有i,j不为0，即非边界时，才判断其左侧或上侧；
            if cnt(i,j)<k+1 and ((i and res_arr[i-1][j]) or (j and res_arr[i][j-1])):
                res_arr[i][j] =1
    return sum(sum(i) for i in res_arr)

def movingCount2(m,n,k):
    visited = set()
    def dfs(i,j):
        if i>=m or j>=n or (i%10+i//10+j%10+j//10)>k or (i,j) in visited: return 0
        visited.add((i,j))
        return 1+dfs(i,j+1)+dfs(i+1,j)
    return dfs(0,0)
```

### 463. 岛屿的周长（易）

- 思路 1：如何在 DFS 遍历时求岛屿的周长：岛屿的周长就是岛屿方格和非岛屿方格相邻的边的数量,
  即“每当在 DFS 遍历中，从一个岛屿方格走向一个非岛屿方格，就将周长加 1”。
  思路 2：直接遍历，如果当前值为 1，加 4（四条边），如果左边有 1，减 2（两条边重合），上面有 1，减 2。
  最后相加即可。（因为是层序遍历，所以只用考虑“左边和上面”）

```python
def islandPerimeter(grid):
    res = 0
    r,c = len(grid),len(grid[0])
    def dfs(i,j):
        if i<0 or i>=r or j<0 or j>=c or grid[i][j]==0:return 1
        if grid[i][j]==2:return 0
        grid[i][j]=2
        return dfs( i - 1, j)+ dfs(i + 1, j)+ dfs(i, j - 1)+ dfs(i, j + 1)
    for i in range(r):
        for j in range(c):
            if grid[i][j]==1:
                res =dfs(i,j)
    return res
def islandPerimeter2( grid):
    res=0
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j]==1:
                res+=4
                if i-1>=0 and grid[i-1][j]==1:
                    res-=2
                if j-1>=0 and grid[i][j-1]==1:
                    res-=2
    return res
```

### 695 岛屿的最大面积 （中）

- 题目：给定一个包含了一些 0 和 1 的非空二维数组  grid , 一个   岛屿   是由四个方向 (水平或垂直) 的  1 (代表土地) 构成的组合。你可以假设二维矩阵的四个边缘都被水包围着。找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0。)

```python
class Solution695:
    def solved(self,grid):
        res = 0
        r,c = len(grid),len(grid[0])
        def dfs(i,j):
            if i<0 or i>=r or j<0 or j>=c or grid[i][j]!=1:return 0
            grid[i][j]=2
            # 递归方法1
            cnt = 1
            for (x,y) in [(-1,0),(1,0),(0,-1),(0,1)]:
                 cnt+=dfs(i+x,j+y)
            return cnt
            # 递归方法2
            # return dfs( i - 1, j)+ dfs(i + 1, j)+ dfs(i, j - 1)+ dfs(i, j + 1)
        for i in range(r):
            for j in range(c):
                if grid[i][j]==1:
                    res = max(dfs(i,j),res)
        return res
```

### 934. 最短的桥 中等 DFS

1. 题目：二进制矩阵中有 2 座桥，桥用 1 标识；问 2 桥之间的最短距离。
2. 思路：
   1. DFS：找到其中一座桥，染色为 2。
   2. bfs：从该桥出发，bfs 搜索，遇到 1 的步数，即为最短桥。

```python
class Solution:
    def shortestBridge(self, A: List[List[int]]) -> int:
        from collections import deque
        row, col = len(A), len(A[0])
        queue = deque()
        flag = True # 跳出循环的标志
        cnt = 0 # 步数
        def dfs(x,y):
            A[x][y]=2
            for nx,ny in [(x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1)]:
                if 0<=nx<len(A) and 0<=ny<len(A[0]) and A[nx][ny]==1:
                    queue.append([nx,ny])
                    dfs(nx,ny)
        # 技巧：跳出双重循环
        for i in range(row):
            for j in range(col):
                if A[i][j]==1:
                    flag=False
                    queue.append([i,j])
                    dfs(i,j)
                    break
            if not flag:
                break
        while queue:
            size = len(queue)
            for _ in range(size):
                x,y = queue.popleft() # ！先入先出
                for nx,ny in [(x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1)]:
                    # 遇到1，一定是另一个岛
                    if 0 <= nx < row and 0 <= ny < col and A[nx][ny] == 1:
                        return cnt
                    # 遇到0，则证明是 岛屿1的边界 向外扩展了一步
                    if 0 <= nx < row and 0 <= ny < col and A[nx][ny] == 0:
                        A[nx][ny] = 2
                        queue.append([nx, ny])
            # 走到这里，一定是将当前层级的结点遍历了一遍。（queue初始时，为岛屿1全部结点，且为同一层）
            cnt += 1
        return cnt

```

### 117.填充每个节点的下一个右侧节点指针

1. 思路 me：迭代+层序遍历+暂存下一层结点
   1. 层序遍历，并暂存下一层的所有节点
   2. 遍历下一层的所有节点，当前节点的 next，即为下一个结点。
2. 可优化点：

   1. 无右邻，无需手动设置 next 为 None
   2. 无需用临时变量 记录每层结点，用于设置其 next；技巧：用 2 个指针，指向前一个结点、下一个结点。

3. 思路 2（推荐）：递归+层序遍历+暂存下一层结点（无需手动设置“无右邻的 next 为 None”）

   1. oneLayer 存当前层结点，nextLayer 存下一层所有结点.
   2. 遍历 nextLayer，每个结点的后一个结点，即为 next 指向.
   3. 注意：题目要求 若没有右邻，next 设为 NULL。其实不设置，默认就为 NULL，若要设置，可设为 None.

4. 思路 3(官方)：层序遍历+前后指针
   1. 层序遍历，利用双指针 pre,cur，来设置每个结点的 next

```python
# ME
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        from collections import deque
        if not root:
            return
        queue = deque([root])
        while queue :# 控制迭代是否终止
            size = len(queue)
            temp_node = []
            for _ in range(size):# 遍历当前层
                cur = queue.popleft()
                temp_node.append(cur) # 暂存下一层所有节点
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            j = 0
            if len(temp_node)==1: # 若只有一个结点
                temp_node[0]=None
            else:
                while j<len(temp_node)-1: # 大于1个结点，遍历、设置
                    temp_node[j].next = temp_node[j+1]
                    j+=1
                temp_node[j].next = None
        return root
```

```python
# 推荐1
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if root == None:
            return root

        def BFS(oneLayer):
            nextLayer = []   # 每一次都搞个空的列表，去建立nextLayer
            for i in oneLayer:
                if i.left:
                    nextLayer.append(i.left)
                if i.right:
                    nextLayer.append(i.right)

            if len(nextLayer) > 1:  # 一共只有1个节点就没必要搞了。
                for j in range(0, len(nextLayer) - 1):
                    nextLayer[j].next = nextLayer[j + 1]  # 每个节点的next指向后面一个节点。

            if nextLayer:  # nextLayer不是空的话就继续往下走。
                BFS(nextLayer)

        BFS([root])  # 最开始就是只有一层一个根节点。
        return root
```

```python
# 推荐2
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        head = root
        while head:#当前层的头节点
            cur = head #当前层处理节点
            pre = head = None#初始化下一层头节点和前置节点
            while cur:
                if cur.left:
                    if not pre:#若尚未找到下一层前置节点，则同步更新下一层头节点和前置节点
                        pre = head =cur.left
                    else:#已找到下一层前置节点，则将前置节点指向当前子节点，并前移pre
                        pre.next = cur.left
                        pre = pre.next
                if cur.right:
                    if not pre:
                        pre = head = cur.right
                    else:
                        pre.next = cur.right
                        pre = pre.next
                cur = cur.next
        return root
```

```python
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        from collections import defaultdict
        if root == None:
            return root
        queue = deque([root])
        while queue:
            size = len(queue)
            pre = None
            for i in range(size):
                cur = queue.popleft()
                if cur.left: queue.append(cur.left)
                if cur.right: queue.append(cur.right)
                if i!=0: pre.next = cur
                pre = cur
        return root
```

### 685.冗余连接

### 面试题 17.22. 单词转换 中等 专业级模拟试题一

### 542. 01 矩阵 （中,待做）

### 207. 课程表 M （待做）

### 210. 课程表 II （待做）

### 827. 最大人工岛 （难）（待做）

### 531. 孤独像素 I 中等 会员 DFS

### 533. 孤独像素 II 中等 会员 DFS

## 回溯

- 22 括号生成 （中）
- 46.全排列(字符无重复)
- 47.全排列 II(字符有重复) (易错)
- 17.电话号码的字母组合 M：回溯（待做）
- 77.组合 M 回溯
- 78.子集 M (易错)
- 面试 08.12 八皇后
- 51.N 皇后 H (统计具体方案) （待做）
- 52.N 皇后 II H (统计方案数)（待做）
- 36.有效的数独 M（待做）
- 37.解数独 M（待做）
- 60.第 k 个排列 中等 工作级模拟试题一（待做）
- 112.路径总和 (直接判断) （易错）见树专题
- 113.路径总和 II M （路径和为一个定值） （易错）见树专题
- 437.路径总和 III M （不限定起点和终点）（易错）见树专题

### 22 括号生成 （中）

1. 解题思路 1：回溯
2. 回溯思想：
   1. 递归参数：当前字符串(路径)、左括号数、右括号数(选择列表);
   2. 终止条件：当前剩余左、右括号数均为 0;(将字符串加入结果集)
   3. 递归工作：
      - 若剩余左括号的数量，大于 0，添加左括号；
      - 若剩余右括号的数量，大于左括号的数量，则添加右括号；

```python
def generateParenthesis(n):
    res = []
    # cur_str:路径，l_num，r_num:选择列表
    def dfs(cur_str,l_num,r_num):
        if l_num==0 and r_num==0:
            res.append(cur_str)
        if l_num>0: dfs(cur_str+'(',l_num-1,r_num)
        if r_num>l_num and r_num: dfs(cur_str+')',l_num,r_num-1)
    dfs('',n,n)
    return (res)
```

### 46.全排列(字符无重复)

题目：给定数字列表(无重复)，求出其全排列；
思路：（回溯方法） 1. 方法入参：选择列表(题目所给的字符列表，若非列表，可转换)、已选路径、结果集(可选)； 2. 先判断是否终止递归？不终止，进入 3； 3. 遍历选择列表，这里可以用下标控制；
3.1 进行选择；
3.2 递归(选择列表、已选路径)，注意：若只在 3.2 传参时修改 2 个入参，则可省略 3.1 和 3.3；
3.3 撤销选择，回退选择列表、已选路径；
关键： 1. 注意:题目要求，是需要字符串，还是列表； 2. 注意:给结果赋值时，涉及到数组，根据递归传参的不同，有时需要用深拷贝 res=path[:]； 3. 技巧:仅在递归调用时，改变“选择列表”和“路径”，可省略递归后“取消选择”；
相似题目：

1. 求 1-n 个数的全排列（输入:n,输出:1-n 的全排列，字符串)；
2. 面试 08.07 无重复字符串的排列组合(输入:无重复字母串，输出:字符串)；

```python
class Solution46:
    def getAllSequence(self,nums):
        if not nums:return []
        res = []
        # 路径，选择列表
        def helper(path,s):
            if len(s)==0:
                res.append(path)
            for i in range(len(s)):
                # 技巧：仅在传参时，改变“选择列表”和“路径”
                helper(path+[s[i]],s[:i]+s[i+1:])
        helper([],nums)
        return res
```

### 47.全排列 II(字符有重复) (易错)

题目:给定一个可包含重复数字的序列，返回所有不重复的全排列。
思路:回溯+减枝 1. 在 46 题（无重复序列）的基础上，遍历选择列表时，进行减枝; 2. 减枝：若当前元素跟前一个一样，则该遍历重复；
相似题目：1.面试 08.08 有重复字符串的排列组合；

```python
class Solution47:
    def permuteUnique(self, nums):
        if not nums:return []
        res = []
        nums.sort()
        # 路径，选择列表
        def helper(path,s):
            if len(s)==0:
                res.append(path)
            for i in range(len(s)):
                # 减枝,若跟前一个一样，则相同
                if i>0 and s[i]==s[i-1]:
                    continue
                # 技巧：仅在传参时，改变“选择列表”和“路径”
                helper(path+[s[i]],s[:i]+s[i+1:])
        helper([],nums)
        return res
```

### 77.组合 M 回溯

通过控制可选路径的范围[index,n]，达到去重；

```python
class Solution:
    def combine(self, n, k) :
        if not n or not k:return []
        res = []
        def dfs(path,index):
            if len(path)==k:
                res.append(path[:])
            for i in range(index,n+1):
                dfs(path+[i],i+1)
        dfs([],1)
        return res

```

### 78.子集 M (易错)

1. 通过控制可选路径的范围[index,n]，达到去重（类似 77）
2. 因子集长度[0,n]，所以分 n+1 次调用回溯方法；

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def backtrack(first = 0, curr = []):
            if len(curr) == k:
                res.append(curr[:])
            for i in range(first, n):
                backtrack(i + 1, curr+nums[i])
        res = []
        n = len(nums)
        for k in range(n + 1): #子集长度，从0到n
            backtrack()
        return res
```

### 面试 08.12 八皇后

1. 题目：n\*n 的棋盘，同一行、同一列、及斜对角会相互攻击，输出可共存的矩阵；
   输入/出：4，[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
2. 思路 1:回溯/基础/慢
   1. 回溯入参:行号、已选路径记忆矩阵、已放置皇后数；
   2. 通过行号，控制变量，保障变量只有列号；
3. 思路 2:回溯/优化“斜线”的判断
   1. 二维矩阵中左对角线上元素特点:i-j 或 j-i，均相等；右对角线上元素特点:i+j 相等；
   2. 回溯入参:已放置 Q 的“左对角线特征集”(即 i-j 的值)，已放置 Q 的“右对角线特征集”(即 i+j 的值)，已放置 Q 的列坐标集；
   3. 由于入参已有“列坐标集”columns，用 len(columns)可表示当前行号，省去一个变量；

```python
class Solution0812:
    # 比较笨的思路
    def solveNQueens1(self,n):
        # 路径、选择列表
        res = []
        vis = [['.']*n for _ in range(n)]
        cnt = 0
        # 判断该节点是否合规
        def not_dangerous(i,j):
            if vis[i][j]=='Q':return False
            # 斜对角方向是否已存在Q
            for r in range(n):
                for c in range(n):
                    if vis[r][c]=='Q' and abs(r-i)==abs(c-j):
                        return False
            # 错误，只考虑了斜方向上的第一步可达点
            # for (x,y) in [(-1,1),(1,1),(1,-1),(-1,-1)]:
            #     if (x+i)>=0 and (y+j)>=0 and (x+i)<n and (y+j)<n :
            #         if vis[x+i][y+j]=='Q':
            #             return False

        def dfs(r,vis,cnt):
            # 入参：r:限定当前选择的行数(选择列表)，vis：已选路径，cnt已放置国王数；
            if cnt ==n:
                temp = []
                for i in vis:
                    temp.append(''.join(i))
                res.append(temp)
                return
            for j in range(n):
                if not_dangerous(r,j):
                    vis[r][j]='Q'
                    dfs(r+1,vis,cnt+1)
                    vis[r][j] = '.'
        dfs(0,vis,cnt)
        return res

    # 优化对角线的判断
    def solveNQueens2(self,n):
        board = [['.' for _ in range(n)] for _ in range(n)]  # 建立N×N的棋盘
        res = []
        # 假设行、列的坐标分别为i,j;
        # left_diagonal：存储已经放置的Queen在左对角线上的属性(i-j)；点[i,j]的左对角线上所有的点的i-j或j-i均相同；作为约束条件；
        # right_diagonal：存储已经放置的Queen在右对角线上的属性；点[i,j]的右对角线上所有的点的i+j或j+i均相同；作为约束条件；
        # column：存储已经放置的Queen所在的列号；另外，用len(column)代表当前行号；
        def helper(left_diagonal, right_diagonal, column):
            # 当前行号
            i = len(column)
            if len(column) == n:
                tmp = []
                for k in board:
                    tmp.append(''.join(k))
                res.append(tmp)
            # 行已固定，因此变量只有列
            for j in range(n):
                # 判断将要放入棋盘的点[i,j]，判断是否合规：是否在同一列中/是否在同一左对角线中/是否在同一右对角线中
                if (i in column) or (i - j in left_diagonal) or (i + j in right_diagonal):
                    continue
                board[i][j] = 'Q'
                # 更新棋盘状况与左对角线/右对角线/列的约束条件；并将其带入函数中，进行下一步的摆放
                helper(left_diagonal + [i - j], right_diagonal + [i + j], column + [j])
                board[i][j] = '.'

        helper(left_diagonal=[], right_diagonal=[], column=[])
        return res
```

## BFS

- 130.被围绕的区域（BFS）
- 广度搜索应用 ：A-B 是否可达
- 1311 获取你的好友已观看的视频 M
- 752.打开转盘锁 中等 专业级模拟试题二
- 407.接雨水 II 困难 专业级模拟试题二
- 127.单词接龙
- 139.单词拆分
- 279.完全平方数
- 1162.地图分析
- 994.腐烂的橘子
- 494.目标和

### 130. 被围绕的区域

1. 题目：给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。
2. 思路：
   1. 要找被 x 包围的区域，换种思路，从 4 个边开始，找到以'O'起始的**非包围区域**，并将其置为其他符号，如'#'。（DFS/BFS）
   2. 遍历整个区域，将非包围区域的'O'置为'X'，'#'置为'O'。

```python
class Solution130:
    def solvedfs(self,board): #DFS
        if len(board) == 0: return
        r, c = len(board), len(board[0])
        if r<3 or c<3: return #长宽中有一个小于3，不可能有被围绕的区域
        def dfs(i,j):
            if i<0 or j<0 or i>=r or j>=c or board[i][j]!= 'O': return
            # if 0<=i<r and 0<=j < c and board[i][j] == 'O':
            board[i][j] = '#'
            for x,y in [(-1,0),(0,1),(1,0),(0,-1)]:#遍历上右下左4个方向
                dfs(i+x,j+y)
        for i in range(r):#遍历左右2条边
            dfs(i,0) #最左边
            dfs(i, c-1) #最右边
        for i in range(c):#遍历上下2条边
            dfs(0,i) #最上边
            dfs(r-1,i) #最下边
        for i in range(r):
            for j in range(c):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                if board[i][j] == '#':
                    board[i][j] = 'O'
        print(board)
    def solvebfs(self, board): # BFS
        if not board or not board[0]:
            return
        row = len(board)
        col = len(board[0])
        def bfs(i, j):
            from collections import deque
            queue = deque()
            queue.appendleft((i, j))
            while queue:
                i, j = queue.pop()
                if 0 <= i < row and 0 <= j < col and board[i][j] == "O":
                    board[i][j] = "B"
                    for x, y in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                        queue.appendleft((i + x, j + y))
        for j in range(col):
            if board[0][j] == "O":# 第一行
                bfs(0, j)
            if board[row - 1][j] == "O": # 最后一行
                bfs(row - 1, j)
        for i in range(row):
            if board[i][0] == "O":
                bfs(i, 0)
            if board[i][col - 1] == "O":
                bfs(i, col - 1)
        for i in range(row):
            for j in range(col):
                if board[i][j] == "O":
                    board[i][j] = "X"
                if board[i][j] == "B":
                    board[i][j] = "O"
```

### 广度搜索应用 ：A-B 是否可达

```python
def bfs(start,end):
    # graph：散列表，存储朋友；start,end：搜索起止点；
    from collections import deque
    # 构造图：朋友圈
    def constructGraph():
        graph = {}
        graph['bob'] = ['lily', 'jack']
        graph['jack'] = ['tim', 'tom']
        graph['lily'] = ['tommy','jack']
        graph['tim']=[]
        graph['tom']=[]
        graph['tommy']=[]
        return graph
    graph = constructGraph()
    s_queue = deque()
    s_queue+=graph[start]
    visited = []
    # 程序终止条件：1）队列已空 2）找到lily
    while s_queue:
        person = s_queue.popleft()
        if person not in visited:
            if person == end:
                return True
            # 当前节点
            else:
                s_queue+=graph[person]
                visited.append(person)
    return False
```

### 1311 获取你的好友已观看的视频 M

1. 关键：字典的多条件排序(词频、字典序：sort 默认字典序)；BFS(排除已访问结点)

```python
class Solution:
    def watchedVideosByFriends(self, watchedVideos, friends, id, level) :
        from collections import Counter
        from collections import deque
        res = []
        def get_friends(id,owner):
            return [i for i in friends[id]]
        def get_video(id_list):
            res = []
            for i in list(id_list):
                res.extend(watchedVideos[i])
            nums = Counter(res)
            res = sorted(nums.items(),key=lambda x:(x[1],x[0]))
            return [i[0] for i in res]
        def bfs(owner):
            queue = deque([owner])
            visited = set()
            depth = 0
            visited.add(owner)
            while queue:
                if depth==level:
                    return get_video(queue)
                size = len(queue)
                for i in range(size):
                    cur = queue.popleft()
                    childs = get_friends(cur,owner)
                    for child in childs:
                        if child not in visited:
                            queue.append(child)
                            visited.add(child)
                depth += 1
        res = bfs(id)
        return res

    def watchedVideosByFriends2(self, watchedVideos, friends, id, level):
        n = len(friends)
        used = [False] * n
        q = collections.deque([id])
        used[id] = True
        for _ in range(level):
            span = len(q)
            for i in range(span):
                u = q.popleft()
                for v in friends[u]:
                    if not used[v]:
                        q.append(v)
                        used[v] = True

        freq = collections.Counter()
        for _ in range(len(q)):
            u = q.pop()
            for watched in watchedVideos[u]:
                freq[watched] += 1

        videos = list(freq.items())
        videos.sort(key=lambda x: (x[1], x[0]))

        ans = [video[0] for video in videos]
        return ans
```


### 127.单词接龙M
1. 思路:BFS(找最短路径长)
```python
from collections import deque


def bfs(beginWord, endWord, wordList):
    if endWord not in wordList:
        return 0
    words = set(wordList)
    queue = deque()
    queue.append(beginWord)
    visited = set()
    visited.add(beginWord)
    depth = 0
    while queue:
        depth += 1
        size = len(queue)
        for _ in range(size):
            cur = queue.popleft()
            # 结束
            if cur == endWord:
                return depth
            for i in range(len(cur)):
                for j in range(26):
                    temp = cur[:i] + chr(97 + j) + cur[i + 1:]
                    if temp not in visited and temp in words:
                        queue.append(temp)
                        visited.add(temp)
    return 0
```


### 139.单词拆分 M
1. 思路1：回溯
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        import functools
        @functools.lru_cache(None)
        def back_track(s):
            if(not s):
                return True
            res=False
            for i in range(1,len(s)+1):
                if(s[:i] in wordDict):
                    res=back_track(s[i:]) or res
            return res
        return back_track(s)
```

## 字典树

- 820.单词的压缩编码 中等 字典树
- 648.单词替换 中等 字典树(待做)
- 实现 Trie (前缀树) 中等 字典树(待做)
- 1231.分享巧克力 困难 会员 贪心+字典树(待做)

### 820.单词的压缩编码 中等 字典树

存储后缀集，集合，discard 去掉集合中指定元素

```python
def minimumLengthEncoding(words):
    word_set = set(words)
    for word in words:
        # 遍历每个单词，从后缀集合中去掉该单词的后缀
        for i in range(1,len(word)):
            # discard去掉集合中指定元素
            word_set.discard(word[i:])
    # 每个单词长度加1
    return sum(len(j)+1 for j in word_set)
```

### 648. 单词替换 中等 字典树

### 实现 Trie (前缀树) 中等 字典树

### 1231.分享巧克力 困难 会员 贪心+字典树

## 待做

- 429,994,1162,286,847,854，
- 79.单词搜索 中等（待做）
- 105.从前序与中序遍历序列构造二叉树 中等（待做）
- 329.矩阵中的最长递增路径 困难（待做）
- 980.不同路径 III 困难（待做）
- 332.重新安排行程 （待做）
- 365 水壶问题 (M)：数学 贝祖定理，dfs/bfs
