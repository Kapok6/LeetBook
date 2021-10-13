[TOC]

## 总结

## 分类练习

【栈】

- 20 有效的括号 E
- 155 最小栈 E
- 232 用栈实现队列 E
- 394 字符串解码 M
- 71 简化路径 M（待做）
- 150 逆波兰表达式求值 M（待做）
- 42 接雨水 H（待做）
- 84 柱状图中最大的矩形 H（待做）

【单调栈】

- 739.每日温度
- 496.下一个更大元素 I
- 503.下一个更大元素 II
- 901.股票价格跨度
- 登峰造极
- 42.接雨水（hard）
- 239.滑动窗口最大值（hard）
- 84.柱状图中最大矩形（hard）

【队列】

- 225.用队列实现栈 简单
- 622.设计循环队列 中等（待做）
- 621.任务调度器 中等（待做）
- 641.设计循环双端队列 中等（待做）
- 363.矩形区域不超过 K 的最大数值和 困难（待做）
- 933.最近的请求次数 简单（待做）

### 栈

#### 20 有效的括号 E

1. 题目要求
   1. 左括号必须用相同类型的右括号闭合
   2. 左括号必须以正确的顺序闭合
   3. 空字符串，可被认为是有效括号
2. 思路：
   1. 建立映射“右》左”
   2. 遇到左括号，直接入栈
   3. 遇到右括号，出栈，判断是否符合映射，是则继续，否则返回 False
   4. 最后判断栈是否为空，非空则 False，空则 True

```python
def isValid(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    for char in s:
        if char in mapping:  # 闭括号，出栈
            top = stack.pop() if stack else '#'
            if mapping[char] != top:
                return False
        else:
            stack.append(char)
    return not stack
```

#### 155 最小栈 E

1. 题目：设计一个支持 push()，pop，top ,getMin()操作，并能在常数时间内检索到最小元素的栈。
2. 关键：最小辅助栈：元素出栈时要同步出；元素入栈时，入 min(helper[-1],x)；

```python
class MinStack:
    # 设置辅助栈：同步栈（妙！）
    def __init__(self):
        self.stack = []
        self.helper = []

    def push(self, x):
        self.stack.append(x)
        if len(self.helper) == 0 or x < self.helper[-1]:
            self.helper.append(x)
        else:
            self.helper.append(self.helper[-1])

    def pop(self):
        if self.stack:
            self.helper.pop()
            return self.stack.pop()

    def top(self):
        if self.stack:
            return self.stack[-1]

    def getMin(self):
        if self.helper:
            return self.helper[-1]
```

#### 232 用栈实现队列 E

1. 题目：实现 push,pop,top,empty
2. 思路：
   1. 设置 2 个栈
   2. 入栈：直接入栈 1
   3. 出栈时
      1. 当栈 2 不空，直接出
      2. 栈 2 空，将栈 1 全部弹出到栈 2,再出

```python
class MyQueue:

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def push(self, x: int) -> None:
        self.stack1.append(x)

    def pop(self) -> int:
        if not self.stack2:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2.pop()

    def peek(self) -> int:
        if not self.stack2:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2[-1]

    def empty(self) -> bool:
        if not self.stack2 and not self.stack1:
            return True
        else:
            return False
```

#### 394 字符串解码 M

1. 题目： 字符串形式 `3[a2[c]]`；会有嵌套；`重复次数[重复字符]`配套出现；
1. 思路
    1. while 迭代，遍历字符串：
        1. ']'：出栈，重复字符为所有连续的字母；重复次数times为所有连续的数字；
        2. 否则，即为“字母/数字”：入栈
    2. 拼接 stack 并返回；

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []
        res = ''
        i = 0
        while i < len(s):
            if s[i] != ']':
                stack.append(s[i])
            else:
                repeat, times = '', ''
                while stack and stack[-1].isalpha():
                    repeat = stack.pop() + repeat
                stack.pop()  # '['出栈
                while stack and stack[-1].isdigit():
                    times = stack.pop() + times
                times = int(times)
                stack.append(repeat * times)
            i += 1
        return ''.join(stack)
```

#### 71.简化路径 （中等） （待做）

1. 题目：
2. 思路：

#### 150 逆波兰表达式求值 中等（待做）

#### 42 接雨水 困难（待做）

#### 84 柱状图中最大的矩形 困难（待做）

### 单调栈

#### 每日温度 739 M

1. 题目：找到第一个比当前温度高的值
2. 解题思路 1：单调不增栈(顺序遍历，栈存待匹配目标的元素，找栈中元素的目标)
   1. 维护一个单调不增栈，存储未找到较大值的元素下标。
   2. 遍历温度表 arr:
      1. 遇到比栈顶更大的值，即 arr[i]大：
         1. 则栈顶元素的“较大值”，即为 arr[i]，加入结果集；
         2. 不断弹出栈顶元素，直到栈顶值大于 arr[i]，进入 2.2；
      2. 否则：栈空/arr[i]小，均为将 arr[i]入栈；
   3. 若栈非空：则栈中对应的元素，均无较大值，设置结果集；
3. 解题思路 2：单调不减栈(逆序遍历，栈存较大后继，找当前元素的目标值)
4. 总结
   1. 思路 1 中：2.1 与 2.2 的顺序，2.2 合并了两种情况，所以放到后面。

```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        from collections import defaultdict
        stack = []
        res = [0] * len(T)
        for k, v in enumerate(T):
            while stack and T[stack[-1]] < v:
                index = stack.pop()
                res[index] = k - index
            stack.append(k)
        while stack:
            res[stack.pop()] = 0
        return res
    # 逆向思维，倒叙遍历，单调不减栈
    def dailyTemperatures1(T):
        stack = []
        ans = [0] * len(T)
        for i in range(len(T) - 1, -1, -1):
            while stack and stack[-1][1] <= T[i]:
                stack.pop()
            # 找到当前元素的较大后继，更新res并入栈
            ans[i] = stack[-1][0] - i if stack else 0
            stack.append([i, T[i]])
        return ans
```

#### 496 下一个更大元素 I (简单)

1. 题目： 从 nums2 中，找到 nums1 中每个元素对应的第 1 个比其大的值；
2. 思路： 单调栈(单调不增)
   1. 总体思路：遍历 nums2，维护一个单调不增栈 》得到结果集；遍历 nums1，返回结果；
   2. 初始化一个单调栈，暂存未找到最大值的元素；
   3. 遍历数组 arr[i]，直到为空：
      1. while 栈不空，且栈顶值比 arr[i]小：
         - 则找到较大值，将栈顶{s[-1]：arr[i]}加入结果集，并弹出 s[-1]；
      2. 不满足上述条件，则栈顶更大一些，arr[i]非较大值：直接将 arr[i]入栈；（维持单调不增）
      3. 若栈非空，则剩余元素均无较大值，即-1；

```python
def nextGreaterElement(nums1, nums2):
    if len(nums1) == 0: return []
    res = {}  # 结果集
    m_stack = []
    for i in range(len(nums2)):
        while m_stack and m_stack[-1] < nums2[i]:
            res[m_stack[-1]] = nums2[i]
            m_stack.pop()
        m_stack.append(nums2[i])
    while m_stack:
        res[m_stack.pop()] = -1
    return [res[i] for i in nums1]
```

#### 503 下一个更大元素 II

1. 题目：
   1. 循环数组，返回每个位置上对应的第一个比它大的数（可能在右边，也可能在“左边”）；
   2. 设置下标范围为 2 倍数组长度：[0,2*n-1]，遍历时设置下标为 i%n，从而模拟循环遍历数组；
2. 思路： 单调栈、 单调不增
   1. 初始化一个单调不增栈，每次遍历到比栈顶小的元素时，则找到元素的较大值，加入结果集；
   2. 逆序遍历两圈数组；
      1. 栈非空，且栈顶较小：一直弹出；
      2. 若栈顶较大，入栈；
      3. 若栈顶较大，第二轮遍历时，将{arr[i%n],res[-1]}加入结果集；

```python
def nextGreaterElements(nums):
    n = len(nums)
    if n == 0: return []
    stack = []
    res = [-1] * n
    # 逆序遍历
    for i in range(n * 2 - 1, -1, -1):
        # 若栈顶元素较小，则找到
        while stack and stack[-1] <= nums[i % n]:
            stack.pop()
        # 当第二轮时，才开始加入结果集
        if i < n:
            res[i] = stack[-1] if stack else -1
            # 栈空/栈顶较大时，入栈
        stack.append(nums[i % n])
    return res
```

#### 901.股票价格跨度

#### 登峰造极

#### 42.接雨水（hard）

#### 239.滑动窗口最大值（hard）

#### 84.柱状图中最大矩形（hard）（待做）

### 队列

#### 用队列实现栈 225 E

```python
class MyStack:
    from collections import deque
    def __init__(self):
        self.stack = deque([])

    def push(self, x: int) -> None:
        self.stack.append(x)

    def pop(self) -> int:
        return self.stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def empty(self) -> bool:
        if self.stack:
            return False
        else:
            return True
```

#### 622.设计循环队列 中等（待做）

#### 621.任务调度器 中等（待做）

#### 641.设计循环双端队列 中等（待做）

#### 363.矩形区域不超过 K 的最大数值和 困难（待做）

#### 933.最近的请求次数 简单（待做）
