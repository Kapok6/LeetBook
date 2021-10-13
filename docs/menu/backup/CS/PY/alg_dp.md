[TOC]

## 总结

## 分类练习

- 121 买卖股票最佳时机 I E (1 次)
- 122 买股票最佳时机 II E (不限次)
- 123 买卖股票最佳时机 III H (2 次)
- 188 买卖股票最佳时机 IV H (k 次)
- 309 最佳买卖股票时机含冷冻期 M (不限次+冷冻期)
- 714 买卖股票的最佳时机含手续费 M (不限次+手续费)

- 198 打家劫舍
- 213 打家劫舍 II
- 337 打家劫舍 III

- 面试题 08.11. 硬币 (中)
- 152 乘积最大子数组 (中)
- 62 不同路径 （待做）
- 674 最长连续递增序列（待做）
- 395 至少有 K 个重复字符的最长子串（待做）
- 124 二叉树中的最大路径和（待做）
- 279 完全平方数（待做）
- 1043 分隔数组以得到最大和 中等 DFS
- 416 分割等和子集 中等 DFS
- 零钱兑换
- 最长回文串
- 838 推多米诺 中等 专业级模拟试题二
- 264.丑数 II （待做）

### 股票系列

1. 121 买卖股票最佳时机 I
   1. 题目：
      股票每天的价格，用数组表示。若最多只允许完成一笔交易（一次买入和一次卖出），求可获取的最大利润。（注意你不能在买入股票前卖出股票。）
   2. 思路 1：二维 dp
      1. 状态：[天数][允许交易的最大次数][是否有股票]。
      2. dp 定义：dp[天数][允许交易的最大次数][是否有股票]，第 i 天...所能获得的最大收益。
      3. 转义方程：(k-1 的位置，放在买入/卖出时，都可以)
         dp[i][k][0]=max(dp[i-1][k][0],dp[i-1][k][1]+price[i]) #i-1 天卖出
         dp[i][k][1]=max(dp[i-1][k][1],dp[i-1][k-1][0]-price[i]) #i-1 天买入
      4. 优化：在这里 k 从 1 开始，因为 k=0,意味着不允许交易。所以上述 dp[i-1][k-1][0]=0，且可简化为 dp[][]。
         dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i])
         dp[i][1] = max(dp[i-1][1],-prices[i])
   3. 思路 2:2 个变量存历史状态
2. 122 买卖股票最佳时机 II
   1. 题目：交易次数不限，但不能同时参与。
   2. 思路：类似 121，改写转义方程，认为 k-1 和 k 等价。
      dp[i][0]=max(dp[i-1][0],dp[i-1][1]+price[i])
      dp[i][1]=max(dp[i-1][1],dp[i-1][0]-price[i]) #区别点
3. 123 买卖股票的最佳时机 III
   题目：最多可完成 2 笔交易
   思路：由于 K=2,需要对 k 进行穷举
4. 188 买卖股票的最佳时机 IV  
   题目：最多可完成 k 笔交易
   思路 1：3 维 dp(超时)
   由于 K 为正整数,需要对 k 进行穷举； 1. 若 k 有效，即 k<n//2，则对 k 进行遍历[1,k+1)（因为一次交易至少持续 2 天，所以有效正整数 k<n//2） 2. 若 k 无效，则视为 k 不限次，同 122。
   思路 2：2 维 dp，优化(超时) 1. 对 3 维 dp 压缩为 2 维，注意：压缩后，需逆序遍历内层循环。 2. 转移方程：dp[k][0/1]
   dp[j][0] = max(dp[j][0], dp[j][1] + prices[i])# 处理第 k 次卖出
   dp[j][1] = max(dp[j][1],dp[j-1][0]-prices[i])# 处理第 k 次买入
   思路 3：
5. 309 最佳买卖股票时机含冷冻期 M (不限次+冷冻期)
   1. 思路：类似 122，买入时，控制上次买入时间。注意 basecase 处理
      dp[i][0]=max(dp[i-1][0],dp[i-1][1]+price[i])
      dp[i][1]=max(dp[i-1][1],dp[i-2][0]-price[i]) #区别点
   2. 注意 basecase 处理: dp[0][1],dp[0][0],dp[1][1]，dp[1][0]
6. 714 买卖股票的最佳时机含手续费 M (不限次+手续费)

   1. 思路：类似 122，买入时，扣除手续费

7. 总结：
   1. 2 维 dp，base case：dp[0][1]=float('-inf')或 -prices[0]都对，为何？

```python
class Solution121:
    def stock(prices):
        mlen = len(prices)
        if mlen == 0:
            return 0
        dp = [[0] * 2 for _ in range(mlen)] #外层行，内层列
        for i in range(mlen):
            if i - 1 == -1:
                dp[i][0] = 0 #未开始，且不持有股票，收益0；
                dp[i][1] = -prices[i]# 由方程计算而来
                continue  #关键
            dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i]) #卖出
            dp[i][1] = max(dp[i-1][1],-prices[i])  #dp[i-1][0]=0,省略
        return dp[mlen - 1][0]  # dp[mlen-1][1]较小

    def stock(prices):
        # stock0的优化算法，复杂度降低到O(1)
        mlen = len(prices)
        if mlen == 0:
            return 0
        dp0, dp1 = 0, float('-inf')
        for i in range(mlen):
            dp0 = max(dp0, dp1 + prices[i])
            dp1 = max(dp1, -prices[i])
        return dp0
```

```python
class Solution122:
    def stock(prices):
        mlen = len(prices)
        if mlen == 0:
            return 0
        dp0, dp1 = 0, float('-inf')
        for i in range(mlen):
            # 不存临时变量，也可以
            dp0 = max(dp0, dp1 + prices[i])
            dp1 = max(dp1, dp0 - prices[i]) # 区别点

            # temp = dp0
            # dp0 = max(dp0, dp1 + prices[i])
            # dp1 = max(dp1, temp - prices[i]) # 区别点
        return dp0

```

```python
class Solution123:
    def stock(prices):
        mlen = len(prices)
        if mlen == 0:
            return 0
        dp10, dp20 = 0, 0
        dp21, dp11 = float('-inf'), float('-inf')
        for i in range(mlen):
            # 遍历顺序无影响
            dp20 = max(dp20, dp21 + prices[i])
            dp21 = max(dp21, dp10 - prices[i])
            dp10 = max(dp10, dp11 + prices[i])
            dp11 = max(dp11, - prices[i])
        return dp20

```

```python
class Solution188:
    # 3维，超时
    def maxProfit(self, k: int, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0
        def get_finite():
            dp = [[[0] * 2 for _ in range(k + 1)] for _ in range(n)]
            for i in range(k + 1):
                dp[0][i][0] = 0
                dp[0][i][1] = -prices[0]
            for i in range(1, n):
                for j in range(1, k + 1):
                    dp[i][j][0] = max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i])
                    dp[i][j][1] = max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i])
            return dp[n - 1][k][0]
        def get_infinite():
            dp0, dp1 = 0, float('-inf')
            for i in range(n):
                dp0 = max(dp0, dp1 + prices[i])
                dp1 = max(dp1, dp0 - prices[i])  # 区别点
            return dp0
        res = 0
        if k > n // 2:
            res = get_infinite()
            return res
        res = get_finite()
        return res
    # 2维，超时
    def maxProfit_pro(self, k: int, prices: List[int]) -> int:
        if not prices:
            return 0
        n = len(prices)
        # 当k非常大时转为无限次交易
        if k > n // 2:
            dp0, dp1 = 0, -prices[0]
            for i in range(1, n):
                dp0 = max(dp0, dp1 + prices[i])
                dp1 = max(dp1, dp0 - prices[i])
            return max(dp0, dp1)
        # 定义二维数组，交易了多少次、当前的买卖状态
        dp = [[0, 0] for _ in range(k + 1)]
        for i in range(k + 1):
            dp[i][1] = -prices[0] # 初始化
        for i in range(1, n):
            for j in range(k, 0, -1):
                dp[j][0] = max(dp[j][0], dp[j][1] + prices[i])# 处理第k次卖出
                dp[j][1] = max(dp[j][1],dp[j-1][0]-prices[i])# 处理第k次买入
        return dp[-1][0]
    # 1维，超时
    def maxProfit_pro1(self, k: int, prices: List[int]) -> int:
        n = len(prices)
        if n <= 1: return 0
        if k >= n//2:
            profit = 0
            for i in range(1, n):
                if prices[i] > prices[i - 1]:
                    profit += prices[i] - prices[i - 1]
            return profit
        else:
            cost = [float('inf') for i in range(k+1)]
            profit = [0 for i in range(k+1)]
            for p in prices:
                for i in range(1,k+1):
                    cost[i] = min(cost[i], p - profit[i-1])
                    profit[i] = max(profit[i], p - cost[i])
            return profit[-1]

```

```python
class Solution309:
    # 一维dp
    def maxProfit_pro(self, prices: List[int]) -> int:
        mlen = len(prices)
        if mlen == 0:
            return 0
        pre, dp0, dp1 = 0, 0, float('-inf')
        for i in range(mlen):
            temp = dp0
            dp0 = max(dp0, dp1 + prices[i])
            dp1 = max(dp1, pre - prices[i])
            pre = temp
        return dp0
            mlen = len(prices)
    # 2维dp
    def maxProfit(self, prices: List[int]) -> int:
        if mlen < 2:
            return 0
        dp = [[0] * 2 for _ in range(mlen)]
        # base case放在循环中不好处理，可单独考虑
        dp[0][0] = 0
        dp[0][1] = -prices[0]
        dp[1][0] = max(dp[0][0], dp[0][1] + prices[1])
        dp[1][1] = max(dp[0][1],dp[0][0]-prices[1])
        for i in range(2, mlen):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 2][0] - prices[i])
        return dp[-1][0]
```

```python
class Solution714:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        mlen = len(prices)
        if mlen == 0:
            return 0
        dp0, dp1 = 0, float('-inf')
        for i in range(mlen):
            dp0 = max(dp0, dp1 + prices[i])
            dp1 = max(dp1, dp0 - prices[i] - fee)  # 区别点
        return dp0
```

### 打家劫舍系列

- 198 打家劫舍
  - 题目：房子单行排列
  - 思路： 1. dp[i]定义&初始化：前 i 个房子，可获得最大金额；初始化为 nums。 2. i<=2: dp[0]=nums[0],dp[1]=max(nums[:2]) 3. i>2: dp[i]=max(dp[i-2]+dp[i],dp[i-1])
    优化：观察可知 dp[i]只与其前面 2 个元素相关，因此可用 2 个变量替代 dp 数组。
- 213 打家劫舍 II
  - 题目：房子环形排列，首尾不能连续偷。
  - 思路：
    1. 在上述基础上，封装 dp 函数；
    2. 调用函数，分别计算 nums[1:]与 nums[:n-1]两种情况下的最大金额，取最大。
- 337 打家劫舍 III
  - 题目：房屋二叉树结构，相邻不能同时打劫
  - 思路 1：dp+dfs 后序遍历
    1. dp，建立两个表（哈希表映射）；后续遍历结点
       1. 函数 f(node)为打劫当前节点 node 时的最大收益
       2. 函数 g(node)为不打劫当前节点 node 时的最大收益
    2. 两种情况：
       1. 当前节点被打劫，则其左子节点和右子节点没有被打劫:
          f(node)=g(node.left)+g(node.right)+node.val
       2. 当前节点没有被打劫，则其左子节点和右子节点可以被打劫或不打劫:
          g(node)=max(f(node.left),g(node.left))+max(f(node.right),g(node.right))

```python
class Solution198:
    def rob(self, nums: List[int]) -> int:
        # 状态/选择:是否选择
        # dp/函数定义：dp[i]:前i号房屋，获取的最大金额
        n = len(nums)
        if n == 0:
            return 0
        if n <= 2:
            return max(nums)
        dp = [0] * n
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        for i in range(2, n):
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])
        return dp[-1]
```

```python
class Solution213:
    # 低效
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n == 0:
            return 0
        if n <= 2:
            return max(nums)
        def helper(nums):
            n = len(nums) # 获取传入的数组长度
            dp = [0]*n
            dp[0] = nums[0]
            dp[1] = max(nums[0], nums[1])
            for i in range(2,n):
                dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])
            return dp[-1]
        return max(helper(nums[1:]), helper(nums[:-1]))

    # 简洁，但低效，优化点：用2个变量代替数组
    def rob_opt1(self, nums: [int]) -> int:
        def my_rob(nums):
            cur, pre = 0, 0
            for num in nums:
                cur, pre = max(pre + num, cur), cur
            return cur
        return max(my_rob(nums[:-1]),my_rob(nums[1:])) if len(nums) != 1 else nums[0]

    # 高效：不传nums数组，只传下标
    def rob_opt2(self, nums: List[int]) -> int:
        import copy
        n = len(nums)
        if n == 0:
            return 0
        if n <= 2:
            return max(nums)
        def helper(start, end, nums):
            n = len(nums[start:end])
            dp = copy.copy(nums[start:end])
            dp[0] = nums[start]
            dp[1] = max(nums[start], nums[start + 1])
            for i in range(2, n):
                dp[i] = max(dp[i - 2] + dp[i], dp[i - 1])
            return dp[-1]
        return max(helper(1, n, nums), helper(0, n - 1, nums))
```

```python
class Solution337:
    def rob(self, root: TreeNode) -> int:
        def dfs(root):
            if not root:
                return
            dfs(root.left)
            dfs(root.right)
            f[root] = g[root.left] + g[root.right] +root.val
            g[root] = max(f[root.left],g[root.left]) + max(f[root.right],g[root.right])
        # 初始化：节点None，返回0
        f = {None:0}
        g = {None:0}
        dfs(root)
        return max(f[root],g[root])
```

### 面试题 08.11. 硬币 (中)

- 题目分析：完全背包问题
- 解题思路 1：二维数组
  1. 在使用第 i 种硬币时，最终得到 j 分钱的表示法=使用了第 i 种钱+未使第 i 种钱；
  2. 状态转移方程：dp[i][j] = dp[i-1][j]+dp[i]j-coin[i]]；
- 解题思路 2：一维数组
  1. 根据 2 维矩阵的状态转移方程，发现可降低到一维；
  2. dp[j]=dp[j]+dp[j-coin[i]] (当 j>=coin[i]时)
  3. 注意：初始化 dp[]中所有的值为 1；

```python
def waysToChange(n):
    coin = [1,5,10,25]
    # dp[i][j]表示在使用第i种硬币时，最终得到j分的表示法
    # n的可能性：从0到n,共n+1种可能；硬币可能性：1,5,10,25，共4种可能；
    dp = [[0]*(n+1) for i in range(4)]
    # 最终0分，1种可能
    for i in range(4):
        dp[i][0]=1
    # 只有1分硬币时，无论最终结果如何，都只有1种可能
    for j in range(n+1):
        dp[0][j] = 1
    for r in range(1,4):
        for c in range(1,n+1):
            if c>=coin[r]:
                # 不使用coin[i]硬币的表示法+使用coin[i]分的硬币的表示法
                dp[r][c] = (dp[r-1][c]+dp[r][c-coin[r]])%1000000007
            else:
                dp[r][c] = dp[r-1][c]
    return dp[3][n]

def waysToChange1(n):
    coin = [1,5,10,25]
    dp = [1 for i in range(len(n)+1)]
    for i in range(1,4):
        for j in range(1,n+1):
            if j>=coin[i]:
                # 不断填写数组、更新dp值
                dp[j] = (dp[j]+dp[j-coin[i]])%1000000007
    return dp[n]
```

### 152 乘积最大子数组 (中)

```python
def maxProduct(nums):
    n = len(nums)
    if n==1:return nums[0]
    cmax,cmin,res = 1,1,float('-inf')
    for i in range(0,n):
        # 关键：若当前元素为负，则交换当前最大最小值
        if nums[i]<0:
            cmax,cmin = cmin,cmax
        cmax = max(nums[i],nums[i]*cmax)
        cmin = min(nums[i],nums[i]*cmin)
        res = max(cmax,res)
    return res
```
