[TOC]

## 总结

## 分类练习

- 724. 寻找数组的中心索引 (E)
- 289. 生命游戏 (M)
- 两数之和为定值的数对
- 有序数组合并
- 118. 杨辉三角 (E)(待做)
- 491. 对角线遍历 中等(待做)
- 54. 螺旋矩阵 中等(待做)
- 209. 长队最小的子数组 中等(待做)

### 724.寻找数组的中心索引 简单

1. 解题思路：前缀和
   1. 判断当前索引 i 是否满足 leftsum==S-nums[i]-leftsum 并动态计算 leftsum 的值。

```python
def pivotIndex(nums):
    S = sum(nums)
    leftsum = 0
    for i, x in enumerate(nums):
        if leftsum == (S - leftsum - x):
            return i
        leftsum += x
    return -1
```

### 289.生命游戏

    简介：根据四周8个方格中1的数量，决定当前方格中的数字；要求原地、一次性赋值。
    思路1，笨办法，数组循环遍历，并记录周围1的数量；最后对原始数组进行一次性赋值。
    思路2：利用cnn中的卷积（但性能有点差）

```python
def gameOfLife1(board):
    row, col = len(board), len(board[0])
    life_list = {}

    def if_alive(i, j, row, col):
        cnt = 0
        for (x, y) in [(0, 1), (1, 1), (1, 0), (-1, 1), (-1, 0), (-1, -1),
                       (0, -1), (1, -1)]:
            ci, cj = i + x, j + y
            if ci < 0 or cj < 0 or ci >= row or cj >= col:
                continue
            else:
                if board[ci][cj] == 1: cnt += 1
        if board[i][j] and (cnt == 2 or cnt == 3):
            return 1
        elif not board[i][j] and cnt == 3:
            return 1
        else:
            return 0

    for i in range(row):
        for j in range(col):
            life_list[str(i) + '-' + str(j)] = if_alive(i, j, row, col)
    for k in life_list.keys():
        m, n = map(int, k.split('-'))
        board[m][n] = life_list[k]
    print(board)
```

### 巧解：利用 cnn 中的卷积（但性能有点差）

```python
def gameOfLife2(board):
    import numpy as np
    r, c = len(board), len(board[0])
    # 下面两行做zero padding
    board_exp = np.array([[0 for _ in range(c + 2)] for _ in range(r + 2)])
    board_exp[1:1 + r, 1:1 + c] = np.array(board)
    # 设置卷积核
    kernel = np.array([[1, 1, 1], [1, 0, 1], [1, 1, 1]])
    # 开始卷积
    for i in range(1, r + 1):
        for j in range(1, c + 1):
            # 统计细胞周围8个位置的状态
            temp_sum = np.sum(kernel * board_exp[i - 1:i + 2, j - 1:j + 2])
            # 按照题目规则进行判断
            if board_exp[i, j] == 1:
                if temp_sum < 2 or temp_sum > 3:
                    board[i - 1][j - 1] = 0
            else:
                if temp_sum == 3:
                    board[i - 1][j - 1] = 1
```

### 两数之和为定值的数对:无序，且可能存在重复

```python
def two_sum(arr, k):
    from collections import defaultdict
    mvoc = defaultdict(list)
    res = []
    for i, v in enumerate(arr):
        mvoc[v].append(i)
    for i in arr:
        if k - i in mvoc:
            res += [(i, k - i)] * len(mvoc[k - i])
    return res
```

### 有序数组合并 ：nums2 合并到 nums1 上，要求在 nums1 上直接修改

```python
def concate2(nums1, m, nums2, n):
    # i、j为当前位置，k表示处理的原数组nums1第几个元素
    i, j, k = 0, 0, 0
    # 当指针在有效数字内
    while k < m and j < n:
        if nums2[j] < nums1[i]:
            nums1.insert(i, nums2[j])
            nums1.pop()
            i += 1
            j += 1
        else:
            i += 1
            k += 1
    while j < n:
        nums1[i] = nums2[j]
        i += 1
        j += 1
    return (nums1)
```
