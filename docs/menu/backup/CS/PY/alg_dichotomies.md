[TOC]

## 总结

### 二分模板 2

1. 使用场景：根据当前值及其右邻，判断是否为目标值；
2. 条件：

- 初始条件：left = 0, right = length
- 终止：left == right(结束时，最好判断下是否为目标值)
- 向左查找：right = mid（结果不在右邻这侧）
- 向右查找：left = mid+1（结果在右邻这侧）

3. 例题：

- 第一个错误的版本 278 (E)
- 寻找峰值 162 (M)
- 寻找旋转排序数组中的最小值 153 (M)

```python
class Model:
    def binarySearch1(self, nums, target):
        pass
    def binarySearch2(self, nums, target):
        if len(nums) == 0:
            return -1
        left, right = 0, len(nums)
        while left < right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid
        # End Condition: left == right
        # 最好判断下是否为目标值，防止数组中无该目标
        if left != len(nums) and nums[left] == target:
            return left
        return -1
```

## 分类练习

- 35 搜索插入位置 E
- 34 在排序数组中查找元素的第一个和最后一个位置 M
- 875 爱吃香蕉的珂珂.M (下边界)
- 69 x 的平方根 E (易错) 待做
- 33 搜索旋转排序数组 M (易错)
- 74 搜索二维矩阵 M
- 240 搜索二维矩阵 II
- 278 第一个错误的版本 (E)
- 162 寻找峰值 162 (M)
- 153 寻找旋转排序数组中的最小值 (M) 待做

### 35.搜索插入位置 E

二分、也可使用 bisect.bisect 查找插入位置（若存在，会返回其后的坐标）

```python
# 二分、也可使用bisect.bisect查找插入位置（若存在，会返回其后的坐标）
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        if target<=nums[0]:return 0
        if target>nums[-1]:return len(nums)
        left,right = 0,len(nums)
        while left<=right:
            mid = left+(right-left)//2
            if nums[mid]==target:return mid
            elif nums[mid]<target:
                left=mid+1
            else:
                right=mid-1
        return left
```

### 875.爱吃香蕉的珂珂.中等 (下边界)

```python
class Solution:
    def minEatingSpeed(self, piles: List[int], H: int) -> int:
        # Can Koko eat all bananas in H hours with eating speed K?
        def possible(K):#防止p<K
            return sum((p+K-1) // K for p in piles) <= H

        lo, hi = 1, max(piles)
        while lo < hi:
            mi = lo+(hi - lo) // 2
            if not possible(mi):
                lo = mi + 1
            else:
                hi = mi
        return lo
```

### 34. 在排序数组中查找元素的第一个和最后一个位置 M

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if target<nums[0] or target>nums[-1]:return -1

```

### 69. x 的平方根 E (易错) 待做

### 33.搜索旋转排序数组

1. 解题思路: 双指针
   1. 双指针指向首尾
   2. 若 nums[0]<=nums[mid]，即左半段单调增（旋转点在 mid 的右侧）：
      2.1 若 target 在[nums[0],nums[mid])中间：右指针左移
      2.2 否则：左指针右移 s = mid+1
   3. 否则，右半段单调增（旋转点在 mid 的左侧）：
      3.1 若 target 在(nums[mid],nums[n-1]]中间：左指针右移
      2.2 否则：右指针左移 e = mid-1
      注意点：旋转点的判断，要一直跟 nums[0]和 nums[n-1]比较，而不是 nums[s]或 nums[e]

```python
def search(nums, target):
    mlen =  (nums)
    if mlen==0: return -1
    if mlen==1:
        return 0 if nums[0]==target else -1
    s,e = 0,mlen-1
    # 注意可以相等
    while s<=e :
        mid = (s+e)//2
        if nums[mid]==target:return mid
         #左半段单调，是与num[0]比较，而不是与nums[s]
        if nums[0]<=nums[mid]:
            if target>=nums[0] and target<nums[mid]:
                e = mid-1
            else:
                s = mid+1
        #右半段单调
        else:
            # 注意等号
            if target>nums[mid] and target<=nums[mlen-1]:
                s = mid+1
            else:
                e = mid-1
    return -1
```

### 74. 搜索二维矩阵 M

从右上角开始找

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix:return False
        row,col = 0,len(matrix[0])-1
        while row<len(matrix) and 0<=col:
            if matrix[row][col]==target:
                return True
            elif matrix[row][col]<target:
                row+=1
            else:
                col-=1
        return False
```

### 240. 搜索二维矩阵 II 待做

### 278 第一个错误的版本 (E)

题目:项目版本迭代，已知，若一个版本错误，则后续均为错误
思路:双指针

1. 若 mid 值为“正确版本”：则 mid 及其左侧均“正确”，s=mid+1;
2. 若 mid 值为“错误版本”：则 mid 及其后面的值，均“错误”，e=mid;

```python
class Solution278:
    def firstBadVersion(self, n):
        if n <=0: return -1
        s,e = 1,n
        while s<e:
            mid = s+(e-s)//2
            if not isBadVersion(mid):
                # 结果在右侧
                s = mid+1
            else:
                # 结果在右侧
                e = mid
        return s
```

### 162 寻找峰值 (M)

题目：峰值定义，峰值后面的值，均比当前值小；

1. 场景 1：峰值
2. 场景 2：
3. 场景 3：
   思路：
4. 找左边界

```python
class Solution162:
    def findPeakElement(self, nums: List[int]) -> int:
        n = len(nums)
        if n<=0:return -1
        s,e = 0,n-1
        while s<e:
            mid = s +(e-s)//2
            if nums[mid]>=nums[mid+1]:
                e = mid
            else:
                s=mid+1
        return s
```

### 153 寻找旋转排序数组中的最小值 (M) 待做

## 分治

- 973 最接近原点的 K 个点
- 1111 有效括号的嵌套深度
- 914 卡牌分组
- 53.最大子序和 E

### 973. 最接近原点的 K 个点

思路 1:内置 sort 进行排序；思路 2：分治，每次选一个值，将其他值分为（大于，小于）2 份；

```python
class Solution(object):
    def kClosest(self, points, K):
        points.sort(key = lambda P: P[0]**2 + P[1]**2)
        return points[:K]
```

### 1111 有效括号的嵌套深度

- 思路：
  - 如何保证划分后，嵌套深度最小？
    - 对括号进行嵌套层数记录
    - 奇数层括号分到一组，偶数层括号分到另一组
  - 如何计算每个括号对应的嵌套深度？
    - 利用栈的思想：遇到左括号，（入栈），嵌套深度+1
    - 遇到右括号，计算完分组下标后，（出栈），嵌套深度-1。
  - 编程思想：
    - 变量初始化：当前括号嵌套深度 depth,最终的分组下标 mindex
    - 遍历括号字符串：
      - 遇到左括号：深度 depth+1，根据当前括号“嵌套深度”的奇偶性，确定分组下标并记录；
      - 遇到右括号：根据当前括号“嵌套深度”的奇偶性，确定分组下标并记录，深度-1；
    - 返回分组下标 mindex
  - 关于性能优化：
    - mindex 记录分组下标时，用索引比 append 要快
    - 判断嵌套深度的奇偶时，depth%2 可直接代替 depth%2==1

```python
def maxDepthAfterSplit(seq):
    depth = 0
    mqueue = []
    mindex = []
    for i in seq:
        if i == "(":
            mqueue.append(i)
            depth += 1
            mindex.append(0 if depth%2==1 else 1)
        else:
            mqueue.pop()
            mindex.append(0 if depth%2==1 else 1)
            depth -= 1
    return mindex
```

### 914 卡牌分组

思路：最大公约数

```python
class Solution:
    # 暴力法：穷举X, (2, N+1)
    def hasGroupsSizeX(deck):
        from collections import Counter
        # Counter()构造 “值:次数” 键值对
        count = Counter(deck)
        N = len(deck)
        for X in range(2, N+1):
            # x 一定能被 N 整出
            if N % X == 0:
                # 每个值都能被 X 整除
                if all(v % X == 0 for v in count.values()):
                    return True
        return False
    # 最大公约数：求“所有数出现次数”的最大公约数，看是否大于1
    def hasGroupsSizeX1(deck):
        from collections import Counter
        from math import gcd  # 求两个数的最大公约数 如：gcd(a,b)
        from functools import reduce
        mcnt_list = Counter(deck).values()
        return reduce(gcd,mcnt_list)>=2

```

### 53.最大子序和 E

```python
def maxSubArray(nums):
    res = 0 # 过程值
    icon = float('-inf') # 负无穷大，最终结果
    for i in nums:
        if res < 0: # 若对结果无贡献，直接舍前面的和
            res = i
        else:
            res += i
        if res > icon:
            icon = res
    return icon
```

## 待做

- 374 (待做)
- 875 (待做)
- 704 二分查找 简单 (待做)
- 34 在排序数组中查找元素的第一个和最后一个位置 中等(待做)
- 658 找到 K 个最接近的元素 中等(待做)
- 50 Pow(x, n) 中等(待做)
