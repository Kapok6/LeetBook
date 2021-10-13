## 总结

### 并查集总结

并查集模板

```python
class UnionFind:
    def __init__(self, n):
        self.count = n #联通分量：所有节点数
        self.parent = [i for i in range(n)] #父项表：每个节点父为自身
        self.rank = [1 for _ in range(n)] #树高：1
    def get_count(self):
        return self.count
    def find(self, p):
        # 路径压缩：树高于3层时，触发压缩；非递归形式
        while p != self.parent[p]:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p
    def union(self, p, q):
        p_root = self.find(p)
        q_root = self.find(q)
        if p_root == q_root:
            return
        # 更新树高：仅当合并前，2节点数高相同时，才需要
        if self.rank[p_root] > self.rank[q_root]:
            self.parent[q_root] = p_root
        elif self.rank[p_root] < self.rank[q_root]:
            self.parent[p_root] = q_root
        else:
            self.parent[q_root] = p_root
            self.rank[p_root] += 1
        self.count -= 1
```

## 分类练习

### 并查集

- 547.朋友圈 中等
- 684.冗余连接 中等
- 200.岛屿数量 中等
- 1102.得分最高的路径（会员） 中等 会员(待做)
- 1135.最低成本联通所有城市（会员） 中等 会员(待做)
- 924.尽量减少恶意软件的传播 困难(待做)
- 737.句子相似性 II 中等 会员

#### 547.朋友圈 M

1. 思路 1：DFS(效率偏低)
   1. 将所有朋友，用一维数组表示
   2. 遍历每个“未访问”过的人，并深度遍历其朋友的朋友，同时标记为“已访问”
2. 思路 2：并查集（推荐）
   1. 初始化父项数组，设置，存储每个节点的父项；
   2. 核心方法：找父项、合并 2 节点。
   3. 遍历邻接矩阵，若 2 人是朋友，则“合并”为一个圈，并减少并查集数量

```python
class Solution547:
    # DFS
    def findCircleNum1(self, M: List[List[int]]) -> int:
        if not M:
            return 0
        visited = set()
        res = 0
        def dfs(i):
            for j in range(len(M)):
                if M[i][j] and j not in visited:
                    visited.add(j)
                    # 传递性：对其朋友的朋友，进行深度遍历
                    dfs(j)
        for i in range(len(M)):
            # 仅当未访问时，才作为一个新的朋友圈
            if i not in visited:
                res += 1
                visited.add(i)
                dfs(i)
        return res

    # 并查集，某大佬，精炼
    def findCircleNum21(self, M) -> int:
        father = [i for i in range(len(M))]
        def find(a):
            if father[a] != a:
                father[a] = find(father[a])
            return father[a]
        def union(a, b):
            father[find(b)] = find(a)
            return find(b)
        for a in range(len(M)):
            for b in range(a):
                if M[a][b]: union(a, b)
        # 压缩路径
        for i in range(len(M)): find(i)
        return len(set(father))

    # 并查集，常规写法
    def findCircleNum22(self, M: List[List[int]]) -> int:
        # 需调用 并查集模板 （见文首）

        # 1、对相识的朋友进行合并;
        uf = UnionFind(len(M))
        for a in range(len(M)):
            for b in range(a):
                if M[a][b]: uf.union(a, b)
        return uf.get_count()
```

#### 684.冗余连接 M

1. 题目：
   节点下标 1-N
2. 思路 1：DFS(效率偏低) O(en)节点\*边
   1. 构造邻接矩阵
   2. 对于每条边(u,v)，dfs 搜索是否可达，若已可达，则成环，去掉该边
3. 思路 2：并查集（推荐）
   1. 改变 union 方法：若开始未联通，则返回 True；若已联通过，则返回 False（证明会形成环）

```python
class Solution:
    def findRedundantConnection_dfs(self, edges: List[List[int]]) -> List[int]:
        if not edges:return []
        n = len(edges)
        neighbors = [[0]*(n+1) for _ in range(n+1)]
        # 根据边，构造邻接矩阵
        def construct_matrix(edges):
            nonlocal neighbors
            for edge in edges:
                neighbors[edge[0]][edge[1]]=1
        # 判断是否可达
        def dfs(start,end):
            nonlocal neighbors
            if neighbors[start][end]:
                return True

    def findRedundantConnection_union(self, edges: List[List[int]]) -> List[int]:
        class UnionFind:
            def __init__(self, n):
                self.parent = [i for i in range(n)]
            def find(self, p):
                while p != self.parent[p]:
                    self.parent[p] = self.parent[self.parent[p]]
                    p = self.parent[p]
                return p
            def union(self, p, q):
                p_root = self.find(p)
                q_root = self.find(q)
                if p_root == q_root:
                    return False
                self.parent[q_root]=p_root
                return True
        if not edges:return []
        # 因为节点下标为[1,N]，所以父项表大小定为n+1,下标0的不用。
        n = len(edges)+1
        uf = UnionFind(n)
        for i in edges:
            if not uf.union(i[0],i[1]):
                return [i[0],i[1]]
        return []
```

#### 200.岛屿数量 M

题目：
给定一个由 '1'（陆地）和 '0'（水）组成的的二维网格，计算岛屿的数量。一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设网格的四个边均被水包围。
思路 1：DFS(效率偏低) 1. 从左到右、从上到下依次‘递归访问’每个‘陆地’，且岛屿+1。 2. 递归访问每个‘陆地’：
终止条件：边界/结点不为'1'陆地。
递归：访问其上、下、左、右。
思路 2：并查集（推荐） 1.遍历邻接矩阵，遇到‘陆地’，则将其相邻的 4 个方向上的‘陆地’，进行‘合并’。
技巧：将 2 维矩阵转为 1 维度：new_index = x*col+y
200 与 547 区别： 1. 邻接矩阵代表的节点是否唯一：547 朋友圈，不唯一，矩阵沿斜对角线对称分布。200 岛屿，代表内容唯一。 2. 两者矩阵遍历方式不同：547 只遍历一半，200 全量； 3. 节点数&父项表，547 大小为矩阵长/宽，200 大小为 row*col，赋值时，需将 2 维矩阵坐标压缩为 1 维；

```python
class Solution200:
    def numIslands_dfs(self,grid):
        if len(grid) == 0: return 0
        r, c = len(grid), len(grid[0])
        mcnt = 0
        def dfs(i,j):
            if i<0 or j<0 or i>=r or j>=c or grid[i][j] != '1': return
            grid[i][j] = '#' # 标识为已访问
            for x,y in [(-1,0),(0,1),(1,0),(0,-1)]: # 遍历上右下左4个方向
                dfs(i+x,j+y)
        for i in range(r):
            for j in range(c):
                if grid[i][j] == '1':
                    mcnt = mcnt+1
                    dfs(i,j)
        return mcnt

    def numIslands_union(self,grid):
        if len(grid) == 0: return 0
        row,col = len(grid),len(grid[0])
        def get_index(x,y):
            return x*col+y
        # 需调用 并查集模板 （见文首）

        # 1、统计水域数量；2、对相连陆地进行合并;
        space = 0
        uf = UnionFind(row*col)
        for i in range(row):
            for j in range(col):
                # 因为水域没有进行合并，所以每个水域都算一个联通分量，最后要去掉；
                if grid[i][j] == '0':
                    spaces += 1
                else:
                    # 由于遍历顺序的关系，只需访问右、下2个方向上的邻接点
                    if i + 1 < row and grid[i + 1][j] == '1':
                        uf.union(get_index(i, j), get_index(i + 1, j))
                    if j + 1 < col and grid[i][j + 1] == '1':
                        uf.union(get_index(i, j), get_index(i, j + 1))

        return uf.get_count() - spaces
```

#### 1102.得分最高的路径（会员） M

#### 1135.最低成本联通所有城市（会员） 中等 会员

#### 924.尽量减少恶意软件的传播 困难

#### 737 句子相似性 II 中等 会员

pairs:[['fine','great'],['act','draw']]

```python
class Solution737:
    def areSentencesSimilarTwo(self,words1,words2,pairs):
        if len(words1)!=len(words2):return False
        from collections import defaultdict
        voc_map = defaultdict(List)
        for i in pairs:
            voc_map[i].append(pairs[i])
            voc_map[pairs[i]].append(i)
        def dfs(w1,w2):
            if word not in voc_map:
                return
            for i in voc_map[w1]:
                if i==w2:
                    return True
                dfs(voc[i],w2)
            return False
        for i in range(len(word1)):
            w1,w2 = word1[i]，word2[i]
            if dfs(w1,w2):
                continue
            else:
                return False
        return True
```

### 拓扑排序

- 210. 课程表 II 中等(待做)
- 444. 序列重建 中等 会员(待做)
- 269. 火星词典 困难 会员(待做)

#### 210. 课程表 II 中等

```python

```

#### 444. 序列重建 中等 会员

```python

```

#### 269. 火星词典 困难 会员
