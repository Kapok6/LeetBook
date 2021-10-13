[TOC]

## 1. 双指针：分类练习

1. 首尾指针：数组
2. 首尾指针：字符串（哈希）
3. 快慢指针：链表、链表、哈希
4. 滑动窗口

### 1.1. 首尾指针：数组

#### 1.1.1. 习题列表

涉及知识点：哈希、排序、二分、贪心

- 763 划分字母区间 贪心算法双指针 71.30% 中等
- 986 区间列表的交集 双指针 63.80% 中等 （易错）
- 56 合并区间 (M)
- 75 颜色分类 排序-数组-双指针 55.10% 中等 （易错）
- 287 寻找重复数 数组双指针二分查找 65.70% 中等
- 283 移动零 数组双指针 61.40% 简单
- 26 删除排序数组中的重复项 数组双指针 50.60% 简单
- 80 删除排序数组中的重复项 II 数组双指针 55.70% 中等
- 88 合并两个有序数组 数组双指针 47.90% 简单
- 11 盛最多水的容器 数组双指针 63.60% 中等
- 167 两数之和 II - 输入有序数组 E
- 15 三数之和 数组双指针 28.30% 中等
- 349 两个数组的交集 排序-哈希表-双指针 69.60% 简单
- 350 两个数组的交集 II 排序-哈希表-双指针 48.90% 简单
- 1213 三个有序数组的交集 哈希表双指针 75.80% 简单

#### 1.1.2. 763 划分字母区间

贪心、双指针 71.30% 中等

1. 题目:将字符串，分为尽可能多的区间，要求字符只能出现在一个区间内。（S 的长度在[1, 500]之间；S 只包含小写字母 'a' 到 'z'）
2. 思路 1--贪心-双指针
   1. 设置首尾结点，当起点未到达末尾时，遍历：
   2. 每次将待划分区域的开头，设定为起点，终点为其最后出现的位置：
      - 起点!=终点 ：遍历该区间内的值，不断更新右边界；(rfind()找右边界)
      - 起点=终点，存储该区间，进入下一个待划分区域；
3. 思路 2--贪心-双指针-哈希--经典
   1. 一次遍历，存储所有元素出现的最后位置，不用每次用 rfind 去更新

```python
class Solution:
    def partitionLabels1(self, S: str) -> List[int]:
        if not S: return []
        from collections import defaultdict
        res = []
        index_map = defaultdict(list)
        for i,v in enumerate(S):
            index_map[v].append(i)
        left,right = 0,0
        n = len(S)
        while left<n:
            cur = left
            right = max(index_map[S[cur]])
            while cur<right:
                cur += 1
                right = max(max(index_map[S[cur]]),right)
            res.append(right-left+1)
            left = right+1
        return res

    def partitionLabels2(self, S: str) -> List[int]:
        last_index = {c: i for i, c in enumerate(S)}
        res = []
        start = end = 0
        # 利用enumerate，减少嵌套层数
        for index, val in enumerate(S):
            end = max(end, last_index[val])
            if index == end:
                res.append(end - start + 1)
                start = end + 1
        return res
```

#### 1.1.3. 986 区间列表的交集

1. 题目:返回两个区间列表的交集,注：每个区间列表中，各区间无交集。如 A 中的[0,2]与 B 中的[1,5]，交集为[1,2];
2. 思路:
   1. 优化判断条件：s, e = max(A[i][0], B[j][0]), min(A[i][1], B[j][1]);if s <= e,才有交集；
   2. 只舍弃具有最小末端点的区间：A[i][1]<B[j][1]，舍弃 A[i][1]

```python
class Solution986:
    def intervalIntersection2(self, A, B):
        i, j = 0, 0
        res = []
        while i < len(A) and j < len(B):
            s, e = max(A[i][0], B[j][0]), min(A[i][1], B[j][1])
            if s <= e:
                res.append([s, e])
            # 合并判断条件，减少圈复杂度
            # 包含2种情况: 1)有交集B[j][0]<=A[i][1]<B[j][1] 2)无交集 A[i][1]<B[j][0]
            if A[i][1] < B[j][1]:
                i += 1
            # 包含2种情况: 1)有交集:A[i][0]<=B[j][1]<=A[i][1] 2)无交集 B[j][1]<A[i][0]
            else:
                j += 1
        return res

```

#### 1.1.4. 56 合并区间 (M)

1. 题目:将一个数组内的所有区间进行合并
2. 思路:优化-排序-双指针--经典
   1. 对待合并数组排序的排序，有 2 个好处：1）只需和 res 最后一个元素比较；2)无需比较 arr 在 res[-1]左侧的情况；
   2. 使用 arr[i]下标进行随机访问，速度比 for i in arr 快；
   3. 数组排序(设置键值)：intervals.sort(key=lambda x: x[0])

```python
class Solution56:
    def merge2(self, intervals):
        intervals.sort(key=lambda x: x[0])
        res = []
        for i in intervals:
            # 合并情况，减少圈复杂度;(注：数组判空，用not)
            if not res or i[0] > res[-1][1]:
                res.append(i)
            else:
                res[-1][1] = max(res[-1][1], i[1])
        return res
```

#### 1.1.5. 75 颜色分类

1. 题目：数组原地排序，只含 0,1,2，要求排序后为 0,1,2。
2. 思路：**荷兰国旗问题**（答案：3 个指针，1 次遍历，O(N), O(1)）
   1. 定义 3 个指针：p0, cur = 0,0; p2 = len(nums)-1
   2. 当 cur<p2 时，cur 逐渐后移：
   3. 若 nums[cur]==0：nums[cur]与 nums[p0]交换，且 p0、cur 后移；
   4. 若 nums[cur]==2：nums[cur]与 nums[p2]交换，且 p2 左移；
   5. 否则：cur+=1

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        p0,cur = 0,0
        p2 = len(nums)-1
        while cur<=p2:
            if nums[cur] == 0:
                nums[cur],nums[p0] = nums[p0],nums[cur]
                p0 += 1
                cur += 1
            elif nums[cur] == 2:
                nums[cur],nums[p2] = nums[p2],nums[cur]
                p2 -= 1
            else: cur += 1
```

#### 1.1.6. 寻找重复数 中等

1. 解题思路 1： narr.count()， 计数法
2. 解题思路 2：二分查找，结合 抽屉原理 O(nlog(n))
   1. 从待查区间[left,right]中取中间数 mid,初始为[1,n]
   2. 遍历并统计数组中不大于 mid 的个数 cnt
      2.1 cnt>mid？则重复的数在[left,mid]中,right=mid
      2.2 cnt<=mid:则重复的数在[mid+1,right]中,left=mid+1
   3. 不断缩小区间，直到找到重复的数
      【易错点】： 1. 重复 2. 思路 2：不要忘记 2.2 中的 mid+1,否则可能会无法缩小

```python
def findDuplicate1(nums):
    for i in nums:
        if nums.count(i) > 1:
            return i

def findDuplicate(nums):
    mlen = len(nums) - 1
    left, right = 1, mlen
    while left < right:
        # 区间中点的计算
        mid = left + (right - left) // 2
        cnt = 0
        for i in nums:
            if i <= mid:
                cnt += 1
        if cnt > mid:
            right = mid
        else:
            # 不要忘了+1
            left = mid + 1
    return left
```

#### 1.1.7. 移动零 (E)：将所有的 0 移动到末尾，其他元素相对位置不变，原地操作；

1. 思路：
   1. 遍历+统计 0 的数量，遇到非 0，则直接插入 i-cnt 位置；遇到 0，则计数加 1；
   2. 设置倒数 z_cnt 个数为 0；
2. 思路 2：
   1. 类似“插入排序”：将数组分为“结果集”和“待处理队列”；
   2. 变量 i 记录结果集末尾下标；
   3. 遍历数组，选择/处理元素（如非重复元素），加入“结果集”；

```python
def moveZeroes(nums):
    mlen = len(nums)
    if mlen <= 0:
        return
    z_cnt = 0  # 统计0的数量
    for i in range(0, mlen):
        if i == 0:  # 首位单独处理
            if nums[i] == 0:
                z_cnt += 1
        elif nums[i] == 0:  # 是0，计数加1；
            z_cnt += 1
        else:  # 非0，直接插入到前面的(i-z_cnt)位置；
            nums[i - z_cnt] = nums[i]
    while z_cnt:
        nums[mlen - z_cnt] = 0
        z_cnt -= 1
```

#### 1.1.8. 删除排序数组中的重复项

1. 题目：原地操作，前 n 项改为正确值，空 O(1)；

```python
def removeDuplicates(nums):
    i = 0
    for num in nums:
        # 已归档元素的最后一个数，与当前遍历的未归档元素不一致
        if nums[i] != num:
            i += 1  # 已归档元素下标后移
            nums[i] = num  # 将新的元素加入已归档序列
    # and：返回最后一个真值，或者第一个假值；
    # 即：若nums为空，返回0；否则返回i+1
    return len(nums) and i + 1
```

#### 1.1.9. 删除排序数组中的重复项 II

1. 题目：去掉重复元素，使得每个值最多出现 2 次；原地操作，前 n 项改为正确值，空 O(1)；
2. 思路：双指针、遍历
   1. 若 i,j 指向相同：判断重复元素是否超过 2(即 j-i>1)；超过 2（弹出 j 对应值，不超过，j 后移）
   2. 若 i,j 指向不同：i 更新为 j，j 后移；

```python
def removeDuplicates2(nums):
    if len(nums) < 2:
        return len(nums)
    i, j = 0, 1
    while j < len(nums):
        # 遇到重复元素：判断重复元素数是否超过2(检测i,j间距是否超过1)
        if nums[i] == nums[j]:
            # 若2者间距相差超过1，证明重复元素多于2个，直接弹出(删除)该元素；
            if j - i > 1:
                nums.pop(j)
            # 间距不超过1，j后移；
            else:
                j += 1
        # 未遇到重复元素：i移到j，j前进一位；
        else:
            i = j
            j += 1
    return j
```

#### 1.1.10. 合并两个有序数组

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        i, j, k = 0, 0, 0 # i、j为当前位置，k表示处理的原数组nums1第几个元素
        while k < m and j < n: #当指针在有效数字内
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
```

#### 1.1.11. 盛最多水的容器

- 题目分析：
  1. 最大值：min(左值，右值)\*左右间距
  2. 关键点：左右指针，移动哪个？移动两者中较小的。（无需考虑左右指分别移动，最大值的变化）
- 解题思路：
  1. 初始化左、右指针，分别指向首尾，水量最大值为：min(左值，右值)\*右间距；
  2. 重复以下操作，直到左右指针相遇：
     2.1 _移动指针_：移动左右值中较小的；
     2.2 更新水量最大值；
  3. 返回水量最大值；
- 证明此方法可得到最优解：
  1. 双指针代表的是：“可以作为容器边界的所有位置的范围”。
  2. 每次移动对应数值较小的指针，意味着 “这个指针不可能再作为容器的界了”。
  3. 证明：假设左右对应的值 x≤y，距离为 t，所以水量为 xt。
     - 若保持左指针不变，只移动右指针，则后续最大水量不可能超过 xt：
     - 若 y1>=y,则水量=x(t-1)<xt;
     - 若 y1<y，则水量=y1(t-1)<xt；
     - 因此可以丢掉左指针。

```python
def maxArea(height):
    mlen = len(height)
    if mlen < 2: return 0
    if mlen == 2: return min(height)
    left, right = 0, mlen - 1
    maxs = 0
    while left < right:
        # 关键：初始时，就要更新maxs
        maxs = max(maxs, min(height[left], height[right]) * (right - left))
        if (height[left] <= height[right]):
            left += 1
        else:
            right -= 1
    return maxs
```

#### 1.1.12. 167 两数之和 II - 输入有序数组 E

1. 思路：递归；哈希表记录 tar-当前值；

```python
class Solution:
    # 递归
    def twoSum1(self, numbers, target):
        res = []
        def helper(left,right):
            cur = numbers[left]+numbers[right]
            if cur==target:
                res.extend([left+1,right+1])
                return
            elif cur>target:
                helper(left,right-1)
            else:
                helper(left+1,right)
        helper(0,len(numbers)-1)
        return res
```

#### 1.1.13. 15 三数之和 中等

1. 解题思路：排序+双指针
   1. 边界情况：数组为空/长度小于 3，直接返回
   2. 对数组进行排序,arr
   3. 对 arr 进行遍历：i
      3.1 设置左右指针， left=i+1,right=n-1
      3.2 当 left<right 时，遍历：
      3.2.2 若三者和小于 0，则 left 右移；
      3.2.3 若三者和大于 0，则 right 左移；
      3.2.1 若三者和==0，是则添加到结果 res 中,并根据情况，更新 left,right;(遇到重复值，跳过)
2. 关键：如何去除重复解？
   1. 在最层遍历时，i 和 left 比较，去重；
   2. 在遇到符和条件的结果时：同时移动 left 和 right，去重。

```python
def threeSum(nums):
    mlen = len(nums)
    if mlen<3:return []
    res = []
    sort_nums = sorted(nums)
    for i in range(mlen):
        if sort_nums[i]>0:
            return res
        # 去除重复解1
        if i>0 and sort_nums[i]==sort_nums[i-1]:
            continue
        left,right = i+1,mlen-1
        while left<right:
            msum = sort_nums[i]+sort_nums[left]+sort_nums[right]
            if msum<0:
                left += 1
            elif msum>0:
                right -= 1
            else:
                res.append([sort_nums[i],sort_nums[left],sort_nums[right]])
                # 去除重复解2
                while left<right and sort_nums[left]==sort_nums[left+1]:
                    left+=1
                while left<right and sort_nums[right]==sort_nums[right-1]:
                    right-=1
                left+=1
                right-=1
    return res
```

#### 1.1.14. 16 最接近的三数之和 中等

1. 解题思路：排序+双指针
   1. 2 层遍历
   2. 若 cur_sum==tar:返回
   3. 若 cur_sum<tar:left 右移，酌情更新 res
   4. 若 cur_sum>tar:right 左移，酌情更新 res

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        n = len(nums)
        if n < 3: return 0
        nums.sort()
        res = float('inf')
        for i in range(n):
            if i > 0 and nums[i] == nums[i - 1]: continue
            start, end = i + 1, n - 1
            while start < end:
                cur = nums[i] + nums[start] + nums[end]
                if cur == target:
                    return cur
                elif cur < target:
                    start += 1
                    if abs(cur - target) < abs(res - target):
                        res = cur
                elif cur > target:
                    end -= 1
                    if abs(cur - target) < abs(res - target):
                        res = cur
        return res
```

#### 1.1.15. 349 两个数组的交集

1. 题目：无序数组且存在重复，返回交集（去重、值要唯一，顺序随意）
2. 类型：排序、哈希
3. 思路：用 list(set(a)&set(b))

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        a,b= set(nums1),set(nums2)
        return list(a&b)
```

#### 1.1.16. 350 两个数组的交集 II

1. 题目：无序数组且存在重复，返回交集（不去重、重复的值也返回，顺序随意）
2. 类型：排序、哈希
3. 思路：
   1. 慢：遍历较短的数组，构建哈希表(值：次数)；然后遍历较长数组，根据否存在于哈希表中，进行筛选。
   2. 快：2 数组进行排序，然后双指针分别遍历：遇到相等的则添加进来。

```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums1.sort()
        nums2.sort()
        i,j,k = 0,0,[]
        while i<len(nums1) and j<len(nums2):
            if nums1[i]<nums2[j]: i+=1
            elif nums1[i]>nums2[j]:j+=1
            else:
                k.append(nums1[i])
                i,j = i+1,j+1
        return k
```

#### 1.1.17. 1213 三个有序数组的交集(待做)

1. 思路 1：慢，遍历其中一个，另外 2 个用 i in arr2 来判断是否存在
2. 思路 2：快（答案），变成集合、再取交集
   - sorted(list(set(arr1)&set(arr2)&set(arr3)))
3. 集合运算：交&、并|、差-、对称差^（仅在其中一个中出现）

```python

```

### 1.2. 首尾指针：字符串（哈希）

#### 1.2.1. 习题列表

- 344 反转字符串 双指针字符串 70.60% 简单
- 345 反转字符串中的元音字母 双指针字符串 50.20% 简单
- 125 验证回文串 双指针字符串 45.70% 简单
- 28 实现 strStr() 双指针字符串 39.70% 简单
- 面试题 17.11 单词距离 双指针字符串 67.30% 中等

#### 1.2.2. 344 反转字符串

双指针字符串 70.60% 简单

1. 思路:首尾双指针+递归交换元素

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

#### 1.2.3. 345 反转字符串中的元音字母

双指针字符串 50.20% 简单

1. 题目：字符串中元音字母进行翻转/只翻转字母'he-lo-uat'

```python
def reverseVowels(mstr):
    s = list(mstr)
    p1, p2 = 0, len(s) - 1
    vowels = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U']
    while p1 < p2:  # 终止条件：两指针相遇
        if s[p1] not in vowels:  # if not s[p1].isalpha()
            p1 += 1
        elif s[p2] not in vowels:
            p2 -= 1
        else:
            s[p1], s[p2] = s[p2], s[p1]
            p1 += 1
            p2 -= 1
    s = ''.join(s)
    return s
```

#### 1.2.4. 125 验证回文串

双指针字符串 45.70% 简单

1. 题目：给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        slist = list(s)
        s, e = 0, len(s)-1
        while s<e:
            if (not slist[s].isalnum()):
                s += 1
            elif (not slist[e].isalnum()):
                e -= 1
            else:
                if slist[s].lower()!=slist[e].lower():
                    return False
                else:
                    s += 1
                    e -= 1
        return True
```

#### 1.2.5. 28 实现 strStr()

双指针字符串 39.70% 简单

1. 题目：判断字符串中，是否存在目标串；
2. 解体思路 1：模式匹配 "Sunday 算法"
   变量：目标字符串中：str; 待匹配字符串:str[id,id+len(pattern)]; 模式串:pattern;
   1. 每次匹配都会从 目标字符串中 提取 待匹配字符串与 模式串 进行匹配：
   2. 若匹配，则返回当前 idx
   3. 不匹配，则查看 待匹配字符串 的后一位字符 c：
      - 若 c 存在于 Pattern 中，则 idx = idx + 偏移表[c]
      - 否则，idx = idx + len(pattern)
3. 平均时间复杂度 O(n)
4. 偏移表：
   - 作用：存储每一个在 模式串 中出现的字符，在 模式串 中出现的"最右位置"到尾部的距离 +1+1，例如 aab：
   - a 的偏移位就是 len(pattern)-1 = 2
   - b 的偏移位就是 len(pattern)-2 = 1
   - 其他的均为 len(pattern)+1 = 4

```python
# me编码风格（被吐槽）
def strStr1(haystack, needle):
    if not needle: return 0 # needle为空，返回0（官方提示）
    if not haystack: return -1
    id = 0
    mlen = len(needle)
    while id <= len(haystack)-mlen:
        if haystack[id:id+mlen] == needle:
            return id
        else:
            if id+mlen<len(haystack) and needle.rfind(haystack[id+mlen])>=0:
                id += mlen-needle.rfind(haystack[id+mlen])
            else:
                id += mlen
    return -1
# 大佬编码风格
def strStr2(haystack, needle):
    # Func: 计算偏移表
    def calShiftMat(st):
        dic = {}
        for i in range(len(st)-1,-1,-1):
            if not dic.get(st[i]):
                dic[st[i]] = len(st)-i
        dic["ot"] = len(st)+1
        return dic
    # 其他情况判断
    if len(needle) > len(haystack):return -1
    if needle=="": return 0
    # 偏移表预处理
    dic = calShiftMat(needle)
    idx = 0
    while idx+len(needle) <= len(haystack):
        # 待匹配字符串
        str_cut = haystack[idx:idx+len(needle)]
        # 判断是否匹配
        if str_cut==needle:
            return idx
        else:
            # 边界处理
            if idx+len(needle) >= len(haystack):
                return -1
            # 不匹配情况下，根据下一个字符的偏移，移动idx
            cur_c = haystack[idx+len(needle)]
            if dic.get(cur_c):
                idx += dic[cur_c]
            else:
                idx += dic["ot"]
    return -1 if idx+len(needle) >= len(haystack) else idx
```

#### 1.2.6. 面试题 17.11 单词距离

双指针字符串 67.30% 中等

1. 题目：给定一个单词列表（含重复），问任意 2 个单词之间的距离；
2. 思路 1:
   1. 1 次遍历，存储所有单词出现的位置 defaultdict；
   2. 依次遍历 2 单词出现的位置，不断更新 2 者之间的距离；
3. 思路 2：直接遍历单词表，并更新两者直接的距离；

```python
class Solution:
    def findClosest(self, words: List[str], word1: str, word2: str) -> int:
        from collections import defaultdict
        i_dict = defaultdict(list)
        for i, v in enumerate(words):
            i_dict[v].append(i)
        s1, s2 = 0, 0
        res = len(words)
        while s1 < len(i_dict[word1]) and s2 < len(i_dict[word2]):
            res = min(res, abs(i_dict[word1][s1] - i_dict[word2][s2]))
            if i_dict[word1][s1] < i_dict[word2][s2]:
                s1 += 1
            else:
                s2 += 1
        return res
```

### 1.3. 快慢指针：链表、链表、哈希

#### 1.3.1. 习题列表

- 27 移除元素 数组双指针 58.40% 简单
- 234 回文链表 链表双指针 42.30% 简单
- 141 环形链表 简单
- 142 环形链表 II 链表双指针 50.70% 中等
- 19 删除链表的倒数第 N 个节点 链表双指针 38.90% 中等

#### 1.3.2. 27 移除元素

数组双指针 58.40% 简单

1. 要求：原地操作，前 n 项改为正确值，空 O(1)；
2. 思路：快慢指针，慢的指向新数组，快的指向原数组；

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        s = 0
        for i in nums:
            if i!=val:
                nums[s]=i
                i+=1
        return len(nums) and i

    def removeElement2(nums, val):
        i = 0
        for num in nums:
            if num != val:  # 若遍历的元素不为要去掉的val，则加入结果集
                nums[i] = num  # 将合法元素插入结果集
                i += 1  # 已归档序列元素数
        return len(nums) and i
```

#### 1.3.3. 234 回文链表

链表双指针 42.30% 简单
思路：将值复制到数组中后用双指针法

```python
def isPalindrome(self, head: ListNode) -> bool:
    vals = []
    current_node = head
    while current_node is not None:
        vals.append(current_node.val)
        current_node = current_node.next
    return vals == vals[::-1]
```

#### 1.3.4. 141 环形链表 E

1. 解题思路 1：（快慢指针） 空间复杂度 O(1)
   1. 快指针，每次步进 2；慢指针步进 1；
   2. 若存在环，则 2 指针一定有相遇的时候；

```python
def hasCycle(head):
    if head == None: return False
    # p2快，p1慢
    p1, p2 = head, head
    while True：
        if not(p2,p2.next):
            return False
        p1 = p1.next
        p2 = p2.next.next
        if p1 == p2:
            return True
```

#### 1.3.5. 142 环形链表 II M

链表双指针 50.70% 中等

1. 解题思路 1：（快慢指针）
   1. 先用快慢指针, 找到他们相遇点(如果存在环);
   2. 再让快指针从链表头开始,与慢指针共速，再次相遇就是环的入口;
2. 证明：(变量说明：a 链头到环入口距离，b 环的周长)
   1. 第 1 次相遇，2 指针步数关系：f = 2s; f = s+nb; 因此：f=2nb;s=nb;两者差 nb；
   2. 每个经过环入口的结点，走过的距离为：k = a+nb;又由 1 得 s=nb；
      - 所以：慢指针第一次相遇后，再走 a,即可到环换入口(第 1 次相遇点到环入口距离为 a)；
      - 第一次相遇点到环入口距离 = 链头到环入口的距离 = a；
   3. 由 2.1 得，让快指针从链头开始，慢指针从第一次相遇点开始，同速前进，则到入口相遇；（第 2 次相遇）
3. 解题思路 2：（哈希法）

- 把遍历过的节点记录,当发现遍历的节点下一个节点遍历过, 返回它；

```python
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
```

#### 1.3.6. 19 删除链表的倒数第 N 个节点

链表双指针 38.90% 中等

1. 题目要求：不知道链表总长度,一趟扫描实现
2. 解题思路：（双指针）
   - 变量说明：假设链表总长为 s
   1. 第 1 个指针先走 n 步；(不是 n-1)
   2. 第 2 个指针加入，从头开始走；
   3. 第 1 个到达链尾时，第 2 个指针走了 s-n,则“下一个结点”，刚好为倒数第 n 个结点的位置；
   4. 删除该位置结点；
3. 易错点：
   1. 该题原链不含头结点/头指针（每次首先打印下 head）
   2. 第 2 个指针开始前进的条件：第一个指针运行第 n 个数（而不是 n-1)

```python
class Solution19:
    # 添加头结点
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
    # 不添加头结点
    def getKthFromEnd2(self, head: ListNode, k: int) -> ListNode:
        if not head or k<0: return head
        pre,pro = head,head
        for i in range(k):
            pro = pro.next
        while pro:
            pre = pre.next
            pro = pro.next
        return pre
```

### 1.4. 滑动窗口

#### 1.4.1. 滑窗总结

1. 扩张/收缩目的
   1. 扩张窗口：为了找到一个可行解，找到了就不再扩张
   2. 持续收缩窗口：在长度上优化该可行解，直到条件被破坏
2. 解题关键：
   1. 如何判断是否(不)满足条件：
      1. 数值型上限：迭代时，直接更新上限值
      2. 字母无重复：
         1. defaultdict+len判重+cnts记录重复次数：可用于扩展到“重复x次；
         2. set+x in 判重：判断较简单；
         3. Counter+len()==k判重：较慢；
3. 问题分类
   1. 找最大/最小
   2. 窗口可变/固定，子序列限制条件
      1. 可变窗口，子序列有数值上限：如限制替换次数/花费，序列和：1208, 485+1004, 424, 209；
      2. 可变窗口，子序列有非数值上限：如无重复、字母均出现：159, 3, 567;
      3. 固定窗口，求和最大/小的子串长度：1100, 239 H, 1423, 1052 ;
   3. 特殊：数学、非连续子序列、仅比较左右：1498，978
4. 算法模板：

   1. 可变窗口，找最大
      1. 扩张窗口，end++
      2. *迭代，while不满足条件：持续收缩窗口
      3. *当前已满足条件：更新结果集
      4. 返回结果（res初始化为0）
   2. 可变窗口，找最小
      1. 扩张窗口，end++
      2. *迭代，while满足条件：持续收缩窗口、更新结果集
      3. 返回结果（res初始化为n+1）
   3. 固定窗口，求最大/小子串长度
      1. 先计算首个窗口的值
      2. 迭代：持续移动窗口，每次只更新窗口首尾元素，更新结果
      3. 返回结果

#### 1.4.2. 习题列表

- 1208.尽可能使字符串相等 M （找最大窗口）
- 485 最大连续 1 的个数 E （数组）（找最大窗口）
- 1004 最大连续 1 的个数 III M （类似 1208）（找最大窗口）
- 3 无重复字符的最长子串 哈希表-双指针-滑动窗口 （M）（找最大窗口）
- 159 至多包含两个不同字符的最长子串 中等会员（找最大窗口）
- 209 长度最小的子数组 M（找最“小”窗口）
- 239 滑动窗口最大值 (H) 滑动窗口
- 1100 长度为 K 的无重复字符子串 中等会员 （窗口固定）
- 438 找到字符串中所有字⺟异位词 （M）（窗口固定）
- 76 最⼩覆盖⼦串 （H） 待做
- 1456 定长子串中元音的最大数目 M（待补充）
- 1423 可获得的最大点数 M
- 1498 满足条件的子序列数目 M
- 424 替换后的最长重复字符 M

#### 1.4.3. 1208.尽可能使字符串相等 M

（找最大窗口，花费有上限cost）

1. 题目：问字符串 s 转化为 t 的最大子串长度，转化一个字符，花费 abs(ASCII 之差)，总 cost 一定；
2. 窗口和<=tar，求窗口最大长度;
3. 思路：
   1. 无限扩展窗口,更新 tar（！重要，窗口左闭右开）
   2. 当“不满足”条件时，持续收缩，更新 tar；
   3. 满足条件、更新结果 res；
4. 注意：不能根据 tar 大小，决定是否收缩窗口

```python
class Solution:
    def equalSubstring(self, s: str, t: str, maxCost: int) -> int:
        if not s or not t:
            return 0

        def get_cost(a, b):
            return abs(ord(a) - ord(b))

        start, end = 0, 0
        m = len(s)
        res = 0
        while end < m:
            maxCost -= get_cost(t[end], s[end])
            end += 1
            # 不满足时，修正
            while maxCost < 0:
                maxCost += get_cost(t[start], s[start])
                start += 1
            # 到此处，maxCost>=0
            res = max(res, end - start)
        return res
```

#### 1.4.4. 485 最大连续 1 的个数 E

（找最大窗口）

1. 类型：要找连续 1，求最大子串长度；（数组）
2. 思路：1）遇到 1，扩张；2）遇到 0，更新结果，重置窗口；

```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        cnt = 0
        temp = 0
        for i in nums:
            if i==1:
                temp+=1
            else:
                cnt = max(cnt,temp)
                temp = 0
        return max(cnt,temp)
```

#### 1.4.5. 1004 最大连续 1 的个数 III (M)

（找最大窗口）（类似 1208）

1. 题目:数组中仅含 0,1，可替换其中的 k 个 0，问最长连续 1 的个数；输入：A = [1,1,1,0,0,0,1,1,1,1,0], K = 2
2. 类型：窗口只能含有 1+最多 k 个 0，找最大窗口长度；
3. 思路：
   - 无限扩张窗口，更新 tar
   - 当不满足条件 tar<0，收缩窗口，更新 tar
   - 满足条件，更新结果集

```python
class Solution:
    def longestOnes(self, A: List[int], K: int) -> int:
        if not A:return 0
        left,right = 0,0
        res = 0
        n = len(A)
        while right<n:
            K-= 1-A[right]
            right+=1
            while K<0:
                K+=1-A[left]
                left+=1
            res = max(res,right-left)
        return res
```

#### 1.4.6. 3 无重复字符的最长子串 M

（找最大窗口）

1. 类型：窗口内元素不重复，求最大子串长度；
2. 思路 1：cnt 记录重复次数，map 记录元素上次出现的位置
   1. 扩张窗口：根据 cur 是否重复，并更新 map 和 cnt，扩张窗口；
   2. 若 cnt>0：不断收缩窗口，并更新 map 和 cnt;
   3. 更新结果 res
3. 思路 2：set 记录窗口中的元素
   1. while 当前元素重复：不断收缩窗口，更新 set。
   2. 不再重复了：加入 set，更新结果集。0

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        from collections import defaultdict
        if not s:
            return 0
        n = len(s)
        start, end, res = 0, 0, 0
        # 当前窗口中含重复数量
        cnt = 0
        voc = defaultdict(int)
        while end < n:
            if voc[s[end]] > 0: # 若重复，更新cnt
                cnt += 1
            voc[s[end]] += 1
            end += 1
            while cnt > 0:
                if voc[s[start]] > 1:# 若去掉重复，更新cnt
                    cnt -= 1
                voc[s[start]] -= 1
                start += 1
            res = max(res, end - start)
        return res
```

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0
        n = len(s)
        voc = set()
        start, end, res = 0, 0, 0
        while end < n:
            while s[end] in voc:
                voc.remove(s[start])
                start += 1
            # 注意，不同于常规模板
            voc.add(s[end]) 
            end += 1
            res = max(res, end - start)
        return res
```

#### 1.4.7. 159.至多包含两个不同字符的最长子串 M 会员

（找最大窗口）

1. 题目：如"eceba" 输出: 3
2. 类型：窗口内不同单词数量<=2,求最大窗口长度
3. 思路：1）无限扩张窗口》2）不满足条件，持续收缩，更新现状》3）更新结果集
4. 条件判断
   1. 用 set：`len(set(s[start:end]))`
   2. 优化：用`ct=Counter()`来计数，增`ct[x]+=1`，减`ct[x]-=1`(若计数为 0，则`del ct[x]`)，元素数量`len(ct)`；

```python
class Solution:
    def lengthOfLongestSubstringTwoDistinct(self, s: str) -> int:
        if not s:return 0
        left,right=0,0
        n = len(s)
        res = 0
        while right<n:
            right+=1
            while len(set(s[left:right]))>2:
                left+=1
            res = max(res,right-left)
        return res
    def lengthOfLongestSubstringTwoDistinct2(self, s: str) -> int:
        length = len(s)
        left = right = 0
        cnter = Counter()
        ret = 0
        while right < length:
            # 增加元素
            cnter[s[right]] += 1
            right += 1
            while len(cnter) > 2: # 判断元素数量
                cnter[s[left]] -= 1 # 减少元素
                # 注意要删除掉次数为 0 的字符
                if cnter[s[left]] == 0:
                    del cnter[s[left]]
                left += 1
            ret = max(ret, right - left)
        return ret
```

#### 1.4.8. 209 长度最小的子数组 M

（找最小窗口）

- 题目：给定数组、目标值，求符和条件的最短数组长度；输入：s = 7, nums = [2,3,1,2,4,3]
- 类型：窗口和>=tar,求窗口最小长度;
- 思路：
  1 无限扩张扩张窗口，更新 sum>>2“满足”条件时，持续进行：更新结果、收缩窗口、更新 sum 以优化结果;
- 注意：
  无符和的结果，需返回 0.(因为是求最小长度，res 开始设置的为 len(nums)+1，最终判断是否为初始值，是则返回 0)

```python
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        left,right = 0,0
        m_sum = 0
        n = len(nums)
        res = n+1 # 初始化结果，用于判断是否有解
        while right<len(nums):
            m_sum += nums[right]
            right += 1
            while m_sum>=s:#有可行解，先更新结果，再收缩窗口进行优化
                res = min(res,right-left)
                m_sum -= nums[left]
                left += 1
        return res if res!=n+1 else
```

#### 1.4.9. 239 滑动窗口最大值 H(待做)

窗口固定

```python

```

#### 1.4.10. 1100.长度为 K 的无重复字符子串 中等会员(待做)

窗口固定

1. 题目：S='havefunonleetcode', k=5;输出 6；分别为'havef', 'avefu', 'vefun', 'efuno', 'etcod', 'tcode;
2. 类型：窗口内元素无重复(字典记录出现位置)，窗口定长，求复合要求的窗口数；
3. 思路：固定窗口，逐步滑动、判断、更新结果；（可借助 Counter/dict）

```python
class Solution:
    def numKLenSubstrNoRepeats(self, S: str, K: int) -> int:
        n = len(S)
        if n == K:
            return 1 if set(S) == K else 0
        if n < K:
            return 0
        start, end = 0, K - 1
        res = 0
        cnts = Counter()
        for i in range(K):
            cnts[S[i]] += 1
        if len(cnts) == K:
            res += 1
        while end < n-1:
            end += 1
            cnts[S[start]] -= 1
            if cnts[S[start]] == 0:
                del cnts[S[start]]
            start += 1
            cnts[S[end]] += 1
            if len(cnts) == K:
                res += 1
        return res
```

#### 1.4.11. 438 找到字符串中所有字⺟异位词 M

窗口固定

1. 题目：给出 s，找到 s 中 p 的异位词(字母同，顺序不同)；如 s: "cbaebabacd" p: "abc"；返回[0, 6]；

2. 关键：异位词的判断 

3. 思路 1：

   1）开辟 26 个字母的空间，分别统计窗口、p 的字母频次；当二者相同，则证明找到；》2）窗口右移，继续判断； 

4. 思路 2：
   1) 利用字典记录字母出现次数，利用变量 valid 记录已匹配到的 p 中字符数量；
   当 valid==len(p):开始收缩优化，直到长度(right-left==len(p))、词频(valid==len(p))均满足,加入结果集；

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        # 记录p, s字母个数
        p_count = [0] * 26
        s_count = [0] * 26
        res = []
        for a in p:
            p_count[ord(a) - 97] += 1
        left = 0
        for right in range(len(s)):
            if right < len(p) - 1:
                s_count[ord(s[right]) - 97] += 1
                continue
            # 窗口加一个， 减一个，维护长度为len(p)的长度
            s_count[ord(s[right]) - 97] += 1
            if p_count == s_count:
                res.append(left)
            s_count[ord(s[left]) - 97] -= 1
            left += 1
        return res

    def findAnagrams2(self, s: str, p: str) -> List[int]:
        if not s:return []
        from collections import defaultdict
        m,n = len(s),len(p)
        left,right = 0,0
        window = defaultdict(int)
        needle = defaultdict(int)
        valid = 0
        res = []
        for i in p:
            needle[i] +=1
        while right<m:
            cur = s[right] # 扩张窗口，并更新
            if cur in p:
                window[cur]+=1
                if needle[cur]==window[cur]:
                    valid +=1
            right += 1
            while valid==len(needle):# 收缩窗口
                if (right-left) == n:# 判断是否需要加入结果集
                    res.append(left)
                # 收缩窗口并更新
                temp = s[left]
                if temp in p:
                    window[temp]-=1
                    if needle[temp]>window[temp]:
                        valid -=1
                left += 1
        return res
```

#### 1.4.12. 76 最⼩覆盖⼦串 （H） 待做

#### 1.4.13. 1456 定长子串中元音的最大数目 M

1. 关键：统计窗口中元音次数 cnt 时，实际只改变了窗口首尾元素，据此更新 cnt 值，更快；

#### 1.4.14. 1423 可获得的最大点数 M

- 题目：每次从左/右侧取值，共 k 次，问得到的和最大是多少？
- 总结：巧妙转化为常规题、举例辅助思考边界、保留中间结果节省时间开销；
- 思路：滑动窗口、“逆向思维”，反过来就是求中间连续子数组得和最小；
- 关键：窗口大小 n-k、窗口移动次数 k、窗口求和(只更改首尾；若每次取 sum，会超时)；

- 特例：k=len(arr)，直接返回 sum(arr)；

```python
class Solution:
    def maxScore(self, cardPoints: List[int], k: int) -> int:
        n = len(cardPoints)
        c_sum = sum(cardPoints[:n-k])#窗口大小n-k
        res = c_sum # 结果
        if k==n : return sum(cardPoints)
        left,right = 0,n-k-1
        for i in range(k): # 移动k次
            left += 1
            right += 1
            # 长度计算，不要每次重新取sum，
            c_sum = c_sum-cardPoints[left-1]+cardPoints[right]
            res = min(res,c_sum)
        return sum(cardPoints)-res
```

#### 1.4.15. 1498 满足条件的子序列数目 M

- 题目：无序数组，找子序列，使得最大、最小值之和，不大于 target；问子序列个数？
- 关键：子序可不连续、数学、排序简化问题、双指针；
- 思路：数学、排序;固定 min，再枚举 max(2 者和受限)，分别累加每组(min,max)对应的子序列数；
  1. 确定 min,max 范围：min 范围，遍历枚举，其中 min<target/2；max 范围[min,target-min];
  2. 确定 min,max 后，计算子序列数:长度为 n 的数组的子序列数为 2^n-1，先计算(min,max]子序列数 2^(n-1)-1，每个子序加上 min，再加上单独的 min 组成的子序列，一共 2^(n-1)个；

```python
class Solution:
    def numSubseq(self, nums: List[int], target: int) -> int:
        nums.sort()()
        if nums[0]>target//2:return 0
        left,right = 0,len(nums)-1
        res = 0
        while left<=right:
            if nums[left]+nums[right]<=target:
                # 长度为right-left+1
                res+=2**(right-left)
                left+=1
            else:
                right-=1
        return res%(10**9 + 7 )
```

#### 1.4.16. 424 替换后的最长重复字符 M

- 总结：求最长，不用缩小窗口；窗口滑动/扩张；
- 题目：字符串可替换 K 次，问最长重复字符的长度
- 思路：记录每个字母的出现次数、历史出现次数最大值；每次扩张/滑动窗口；扩张时，更新结果；
- 窗口滑动：若“窗口大小-历史上所有字符的最大重复次数>k”，则滑动;（为什么要用 max_val 呢，因为这样可以使替换的数量降到最低)
- 窗口扩张：不满足滑动条件，则扩张，扩张完再更新最大长度

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        from collections import defaultdict
        if not s:return 0
        max_cnt=0
        res=0
        left=0
        voc = defaultdict(int)
        for i in range(len(s)):
            # 指针右移，更新历史上字符出现的最大次数
            cur = s[i]
            voc[cur]+=1
            max_cnt=max(max_cnt,voc[cur])
            # 窗口滑动：k不足以替换，使得窗口内字符相等
            # 用while也对，因为每次只变化一个字母，所以用while，也是1次改变；
            if i-left+1-max_cnt>k:
                voc[s[left]]-=1
                left+=1
                # 满足要求，更新结果
            # 扩张窗口
            else:
                res = max(res,i-left+1)
        return res
```
