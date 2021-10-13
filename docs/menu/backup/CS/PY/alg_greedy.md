[TOC]

## 总结

## 分类练习

- 55 跳跃游戏
- 495.提莫攻击 (M): 数组
- 605.种花问题 (E): 数组,贪心

【相似问题：栈+贪心】

- 402.移掉 K 位数字 M
- 316.去除重复字母 42.8% M
- 321 拼接最大数 H：分治+贪心

- 253.会议室 II 中等 会员

- 452.用最少数量的箭引爆气球
- 1231.分享巧克力
- 376.摆动序列
- 406 根据身高重建队列（待做）
- 加油站
- 任务调度器 真题
- 122.买卖股票的最佳时机 II（待做）
- 1094.拼车 中等 差分
- 1109.航班预订统计 中等 差分
- 860 柠檬水找零（待做）
- 135 分发糖果（待做）
- 1353.最多可以参加的会议数目 中等 排序+贪心
- 452.用最少数量的箭引爆气球 中等 贪心
- 376.摆动序列 中等 贪心
- 678.有效的括号字符串 中等 工作级模拟试题二
- 649.Dota2 参议院 (M): 贪心

【分治】

- 215 数组中的第 K 个最大元素 （待做）

### 待分类

#### 55 跳跃游戏（中）

```python
def jump(nums):
    mlen = len(nums)
    if not nums or mlen == 0: return False
    end = 0
    for i in range(mlen - 1):
        if end >= i:
            end = max(end, nums[i] + i)
        else:
            return False
    if end >= (mlen - 1):
        return True
    else:
        return False
```

#### 495.提莫攻击 (M)

题目：给出攻击点，和持续时间，问攻击有效时长；
思路 1： 1. 遍历攻击点，每次将 min(持续时长,下一个点-当前点)合入结果；

```python
class Solution495:
    def findPoisonedDuration(self, timeSeries, duration):
        n = len(timeSeries)
        if n == 0:
            return 0
        total = 0
        for i in range(n - 1):
            total += min(timeSeries[i + 1] - timeSeries[i], duration)
        return total + duration
```

#### 605.种花问题 (E): 数组

题目:给定[1,0,0,0,1]和 n，已知数组中的 1 不能连续，问能否向数组中再插入 n 个 1？
思路 1:贪心 1. 分情况讨论，首、尾、中间（滑动窗口，连续 2 个/3 个 0）
思路 2:贪心--经典 1. 基本情况：当前是否为 0？（左右两侧是否为 0？若是否为首尾？）则将不同情况融入判断条件，减少嵌套；

```python
class Solution605:
    def canPlaceFlowers1(self, flowerbed, n):
        mlen = len(flowerbed)
        # 仅含1个元素
        if mlen == 1: return not (flowerbed[0] and n)
        i = 0
        cnt = 0
        while i < mlen - 1:
            # 首:2个连续1
            if i == 0 and sum(flowerbed[i:i + 2]) == 0:
                flowerbed[0] = 1
                cnt += 1
                i += 1
            # 尾:2个连续1
            elif i == mlen - 2 and sum(flowerbed[i:i + 2]) == 0:
                flowerbed[-1] = 1
                cnt += 1
                break
            # 中间：3个连续1
            elif sum(flowerbed[i:i + 3]) == 0:
                flowerbed[i + 1] = 1
                cnt += 1
                i += 2
            else:
                i += 1
        return True if cnt >= n else False

    def canPlaceFlowers2(self, flowerbed, n):
        i, count = 0, 0
        while i < len(flowerbed):
            # 将不同情况融入判断条件，减少嵌套
            if (flowerbed[i] == 0) and (i == 0 or flowerbed[i - 1] == 0) \
                    and (i == len(flowerbed) - 1 or flowerbed[i + 1] == 0):
                flowerbed[i] = 1
                count += 1
            i += 1
        return count >= n
```

### 402.移掉 K 位数字 M

1. 题目：去掉 K 位数，保证得到的数最小，前置 0 会自动去掉；
2. 思路：不断对“左邻”进行取舍
   1. 变量：所给数字 num,结果列表 res；
   1. 从左到右遍历，持续比较当前元素 num[i]与已选择的“左邻”res[-1]：
      1. 若左邻小，保留左邻,并将 num[i]加入 res，待后续取舍；
      2. 若左邻大，持续 pop；
   1. 若遍历完，k>0，则去掉 res 末尾的 k 个元素（针对升序序列）；
3. 总结：
   1. 数学知识：两个相同位数的数字大小关系取决于第一个不同的数的大小。若 12a34>12b34,则有 a>b;
   2. 注意点：1 返回值为 str；2 去掉头部 0；3 结果为空，返回'0'；4.需要持续比较当前值与左邻；

```python
class Solution:
    def removeKdigits(self, num: str, k: int) -> str:
        if k == 0:
            return num
        num = list(num)
        res = []
        for i in range(len(num)):
            if i == 0:
                res.append(num[i])
                continue
            # 持续比较，判断res》res[-1]
            while res and num[i] < res[-1] and k > 0:
                res.pop()
                k -= 1
            res.append(num[i])
        if k > 0:
            res = res[:len(res) - k]
        res = ''.join(res).lstrip('0')
        return res or '0'
```

### 316.去除重复字母 42.8% 中等

1. 题目：去除字符串中重复字母，并保证字典序最小
2. 思路：不断对“左邻”进行取舍+每个字符统计一个次数
   1. 顺序遍历字符串：
      1. 若 stack 中未添加：
         1. 若当前元素小于栈顶，且栈顶元素后续会出现，则持续出栈；
         2. 否则：入栈；
      2. 将该字符次数-1；
3. 总结：
   1. 字符串计次：Counter；
   2. 数学知识：两个相同位数的字符串，字典序大小关系取决于第一个不同的字符的大小；

```python
from collections import Counter
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        voc_map = Counter(s)
        stack = []
        for e in s:
            if e not in stack:
                while stack and e < stack[-1] and voc_map[stack[-1]] > 0:
                    stack.pop()
                stack.append(e)
            voc_map[e] -= 1
        return ''.join(stack)
```

### 321 拼接最大数 H

1. 题目：从 2 个数组中，总共取 k 个数，使得结果最大（要求单个数组中的数相对顺序不变）
2. 思路：分治+贪心
   1. 分：从 num1 中取 k1 个数，从 num2 中取 k-k1 个数;
      1. 注意：k1 要枚举[0,x]，其中 x=min(k-len(num2),len(num1))
      2. 技巧：从 num 中选 k 个数》从 num 中去掉 len(num)-k 个数；（类似力扣 402）
   2. 治（妙）：合并这两个取出的数组，使用 A>B 判断，每次添加较大数组的首个元素；
3. 总结：
   1. 数组比较，A>B <=> 当且仅当 A 的首个元素字典序大于 B 的首个元素，例如[6,7]>[6,0,4]；
   2. 数组合并：不能每次只比较当前元素，若相当时，需要考虑后一位；简单方法：直接比较 2 个数组；
   3. 技巧：从 num 中选 k 个数》从 num 中去掉 n-k 个数；

```python
class Solution:
    # 技巧：提取k个数，得到最大》去掉len(nums)-k个数，使得结果最大；
    def get_max_k(self, nums, k):
        stack = []
        mlen = len(nums)
        drop_num = mlen - k
        for x in nums:
            while drop_num and stack and stack[-1] < x:
                stack.pop()
                drop_num -= 1
            stack.append(x)
        return stack[:k]

    def merge(self, A, B):
        ans = []
        while A or B:
            # 数组A>B:当且仅当A的首个元素字典序大于B的首个元素
            bigger = A if A > B else B
            ans.append(bigger[0])
            bigger.pop(0)  # bigger为引用，弹出会影响原数组
        return ans

    def maxNumber(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        len1, len2 = len(nums1), len(nums2)
        res = []
        k1 = min(len1, k)
        for i in range(k1 + 1):
            if k - i > len2 or i > len1:
                continue
            arr1 = self.get_max_k(nums1, i)
            arr2 = self.get_max_k(nums2, k - i)
            temp = self.merge(arr1, arr2)
            res = max(res, temp) if res else temp
        return res
```

### 253.会议室 II 中等 会员

1. 思路
   1. 会议室：map{id:[会议 1,...]}，id 可用会议室数标识,从 0 开始递增；
   2. 判断是否需要新开会议室：遍历已申请的会议室，判断其结束时间，是否不晚于“待安排的会议”的开始时间；
2. 总结
   1. 关键：首先对所有会议排序
   2. 判断是否新开会议室：要对现在已申请的会议室，判断

```python
from collections import defaultdict
class Solution:
    def minMeetingRooms(self, intervals) :
        res = defaultdict(list)
        intervals.sort()
        cnt = 0
        for i in intervals:
            if cnt == 0:
                cnt += 1
                res[cnt].append(i)
                continue
            # 需要对所有会议室执行判断
            label = False
            for k in res.keys():
                if i[0] >= res[k][-1][1]:
                    res[k].append(i)
                    label = True
                    break
            if not label:
                cnt += 1
                res[cnt].append(i)
        return len(res.keys())

if __name__ == '__main__':
    s = Solution()
    arr = [[9,10],[4,9],[4,17]] #2 需要对所有会议室执行判断
    print(res = s.minMeetingRooms(arr))
```
