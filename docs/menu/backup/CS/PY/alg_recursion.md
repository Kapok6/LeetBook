[TOC]

## 总结

1. 递归方法的写法：(1)将给定的方法，定义为递归函数，无需自行调用；(2)在给定方法中/类中，自定义新的递归方法，需要自行调用；
2. 算法：基本情况、当前层处理逻辑、子问题处理逻辑(递归公式)；
3. 时间复杂度：执行树；
4. 空间复杂度：递归相关(函数返回地址、函数传参、函数中变量)、
5. 避免重复计算：记忆化，即哈希存储中间结果；
6. 尾递归：一个函数中所有递归形式的调用都出现在函数的末尾(且无额外计算)。优势：无需记录函数返回的位置，节省空间。

## 分类练习

【回溯】详见搜索专题

- 46.全排列 M 回溯
- 77.组合 M 回溯

【链表】详见链表专题

- 206 反转链表 (E)
- 21 合并两个有序链表 (E)
- 24 两两交换链表中的节点 (M)

【树】详见树的专题

- 104 二叉树的最大深度 (E)
- 894.所有可能的满二叉树 中等（待做）

【数学】

- 344 反转字符串 (E)
- 118 杨辉三角 (E)
- 119 杨辉三角 II (E)
- 509 斐波那契数 (E)
- 70 爬楼梯 (E)
- 50 pow (M)
- 62 圆圈中最后剩下的数字 E
- 报数
- 78.子集 M 递归（待做）
- 698.划分为k个相等的子集 中等（待做）
- 687.最长同值路径 简单（待做）
- 726.原子的数量 困难（待做）

### 344 反转字符串 (E)

思路:首尾双指针+递归交换元素

```python
# me：思路有点绕远
class Solution:
    def reverseString(self, s):
        self.helper(s,len(s),0)
        return s
    def helper(self,s,n,index):
        if n%2==0:
            if index <= n//2-1:
                s[index], s[n-index-1] = s[n-index-1], s[index]
                self.helper(s,n,index+1)
            else:
                return
        else:
            if index < n//2:
                s[index], s[n-index-1] = s[n-index-1], s[index]
                self.helper(s,n,index+1)
            else:
                return
# 官网递归法：简洁，首、尾双指针，且无需区分是否“奇偶”
class Solution:
    def reverseString(self, s):
        def helper(left, right):
            if left < right:
                s[left], s[right] = s[right], s[left]
                helper(left + 1, right - 1)
        helper(0, len(s) - 1)
```

### 118 杨辉三角 (E)

- 题目：输出 n 行所有数据
- 思路 1：递归
  - 递推关系:第 r 行，第 c 列的数 f(r,c)=f(r-1,c-1)+f(r-1,c)
- 思路 2：动态规划(优)
  - 状态转移方程:非首尾,row[j] = res[i-2][j-1]+res[i-2][j];首尾,row[j]=1。

```python
# me:
class Solution:
    def generate(numRows):
        res = []
        res_map = {}
        def rec(r,c):
            if c==1 or r==c: return 1
            temp1 = str(r)+'-'+str(c)
            temp2 = str(c)+'-'+str(r)
            if temp1 in res_map:
                return res_map[temp1]
            elif temp2 in res_map:
                return res_map[temp2]
            else:
                res = rec(r-1,c-1)+rec(r-1,c)
                res_map[temp1] = res
                return res
        for i in range(1,numRows+1):
            cur = []
            for j in range(1,i+1):
                cur.append(rec(i,j))
            res.append(cur)
        return res

# 优:动态规划
class Solution:
    def generate(numRows):
        res = []
        for i in range(1,numRows+1):
            row = [None for _ in range(i)]
            # 首尾置为1
            row[0],row[-1]=1,1
            # 非首尾，用迭代公式（即状态转移方程）；注意这里用res[i-2]；
            for j in range(1,i-1):
                row[j] = res[i-2][j-1]+res[i-2][j]
            res.append(row)
        return res
```

### 119 杨辉三角 II (E)

- 题目:只输出行下标为 n 的数据
- 思路 1:动态规划 1. 开始全置为 1（无需后续再单出设置首尾为 1； 2. 逐行计算，且每行从后往前计算(首尾除外)，遵循 row[j]=row[j]+row[j-1];如 1,2,1=>1,1+2,1+2,1;

```text
    1
    1 1
    1 2 1
    1 3 3 1
```

```python
class Solution:
    # 行下标rowIndex，第1行为0
    def generate(rowIndex):
        row = [1 for _ in range(rowIndex+1)]
        for i in range(rowIndex+1):
            for j in range(i-1,0,-1):
                row[j]+=row[j-1]
        return row
```

### 509 斐波那契数 (E)

```python
class Solution:
    def fib(N):
        cache =  {}
        def help(n):
            if n<2: return n
            if n in cache: return cache[n]
            else:
                temp = help(n-1)+help(n-2)
                # 存储中间结果，防重复计算
                cache[n] = temp
                return temp
        return help(N)
```

### 70 爬楼梯 (E)

- 思路 1：递归法+记忆化
- 思路 2：动态规划
- 思路 3：斐波拉切数：辗转相加

```python

# 思路1
class Solution:
    def climbStairs(self, n: int) -> int:
        # f(n) = f(n-1)+f(n-2)
        cache = {}
        def help(n):
            if n<=2:return n
            if n in cache: return cache[n]
            else:
                temp = help(n-1)+help(n-2)
                cache[n] = temp
                return temp
        return help(n)

# 思路2
class Solution:
    #dp[j] = dp[j-1] + dp[j-2]
    def climbStairs(self, n: int) -> int:
        dp = [None for _ in range(n+1)]
        if n<=2 : return n
        dp[0],dp[1],dp[2], = 0,1,2
        for i in range(3,n+1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
# 思路3
class Solution:
    def climbStairs(self, n: int) -> int:
        if n<= 2: return n
        f,s = 1, 2
        for i in range(3,n+1):
            temp = f+s
            f = s
            s = temp
        return s
```

### 50 pow (M)

- 题目：求 x^N
- 思路 1：暴力：递归
- 思路 2：递归快速幂，可少递归一半 1. 若 n 为偶数，则 x^N=(x^n)^2，N=2n,可少递归一半; 2. 若 n 为奇数，则 x^N=(x^n)^2\*x;

```python
# 思路2
class Solution:
    def myPow(self, x: float, n: int) -> float:
        def help(x,n):
            if n ==0 : return 1.0
            half = self.myPow(x,n//2)
            if n %2 ==0:
                return half*half
            else:
                return half*half*x
        if n<0:
            x = 1/x
            n = -n
        return help(x,n)
```

### 62 圆圈中最后剩下的数字

- 题目：n 个数(0,1,...n-1)，每次去掉第 m 个数，问最后剩下哪个数字？
- 思路：

```text
1. 正推：列出每次的数字变化，找出最后一个数；
   01234 去 2
   3401 去 0
   134 去 4
   13 去 1
   3
2. 反推：观察最后去掉的数，在每次变化中的下标变化(下标每次在上一次基础上增加 3)，找规律；
   3 0
   13 (0+3)%2=1
   134 (1+3)%3=1
   3401 (1+3)%4=0
   01234 (0+3)%5=3
   发现:第 n 次去掉数的下标为 f(n) = (f(n-1)+3)%n，f(0)=0
```

```python
class Solution:
    def lastRemaining(n,m):
        if n == 1:return n
        res = 0
        # 从2个数开始，到n个数结束
        for i in range(2,n+1):
            res = (res+m) % i
        return res
```

### 报数

1. 题目：序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。如，21 被读作 "one 2", "one 1"
2. 思路
   - 先设置上一人为'1'
   - 开始外循环
     - 每次外循环先置下一人为空字符串，置待处理的字符 num 为上一人的第一位，置记录出现的次数为 1
   - 开始内循环，遍历上一人的数，如果数是和 num 一致，则 count 增加。
     - 若不一致，则将 count 和 num 一同添加到 next_person 报的数中，同时更新 num 和 count
     - 别忘了更新 next_person 的最后两个数为上一个人最后一个字符以及其出现次数！

```python
def countAndSay1(n):
    # 非递归
    prev_person = '1'
    for i in range(1,n):
        # print('i:',i)
        next_person, num, count = '', prev_person[0], 1
        for j in range(1,len(prev_person)):
            if prev_person[j] == num:count += 1
            else:
                next_person += str(count) + num
                num = prev_person[j]
                count = 1
        next_person += str(count) + num
        prev_person = next_person
    return prev_person
def countAndSay2(n):
     # 递归
    if n == 1:
        return "1"
    s = countAndSay(n - 1)  # 递归
    count = 0  # 某个数字连续出现的次数
    temp_cha = s[0]  # 当前数字
    new_str = ""  # 新的报数序列
    for cha in s:  # 遍历旧的报数序列
        if temp_cha == cha:  # 相同连续数字累加
            count += 1
        else:  # 出现不相同数字就补充新报数序列
            new_str = new_str + str(count) + temp_cha
            temp_cha = cha  # 清零，从头累加新的数字
            count = 1
    new_str = new_str + str(count) + str(temp_cha)  # 最后一个数字补充到新报数序列中
    return new_str
```
