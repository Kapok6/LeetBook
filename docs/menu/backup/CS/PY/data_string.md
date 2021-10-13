[TOC]

## 总结

1. 简单：双指针、哈希表
2. 中级：哈希表、栈、双指针、滑动窗口、贪心、回溯、广搜、位运算、动态规划
3. 主要题型：
   1. 拼接、替换、比较、反转：
   2. 子串（前缀，后缀等）
   3. 匹配（KMP 算法、BM 算法等）

## 分类练习

【经典】

- 5.最长回文子串
- 93.复原 IP 地址
- 227.基本计算器 II
- 227.基本计算器 II 中等 字符串
- 1071.字符串的最大公因子 简单 工作级模拟试题一
- 面试题 01.03. URL 化 简单 工作级模拟试题二
- 165.比较版本号 中等 工作级模拟试题二
- 8.字符串转换整数 (atoi)

【前缀、后缀】

- 14.最长公共前缀 (E)
- 560.和为 K 的子数组：O(n)复杂度解决 中等 前缀和&HASH
- 974.和可被 K 整除的子数组 中等 前缀和&HASH

【匹配（朴素匹配/暴力 BF、KMP、BM、sunday、Horspool）】

- 28.实现 strStr (E)(sunday 算法)
- （选修） 字符串匹配算法: KMP(待做)

【回文】

- 5.最长回文子串 (M)(中心扩散、动态规划、Manachaer)
- 1143.最长公共子序列 (M)(待做)
- 125.验证回文 (E)
- 560.和为 K 的子数组 (M)

【重复】

- 459.重复的子字符串 (E)
- 686.重复叠加字符串匹配 (E)
- 316.去除重复字母 （难）

【反转】

- 541 反转字符串 II (E):语言特性，步长 k/2k,[::-1]
- 练习:反转字符串中的单词 (E)
- 练习:反转字符串中的单词 2 (E)
- 557 反转字符串中的单词 2

【栈、队列】

- 20 有效的括号 E （见栈&队列专题）

【双指针】

- 151 翻转字符串里的单词 (M)
- 344 反转字符串 双指针字符串 70.60% 简单
- 345 反转字符串中的元音字母 双指针字符串 50.20% 简单
- 125 验证回文串 双指针字符串 45.70% 简单
- 28 实现 strStr() 双指针字符串 39.70% 简单
- 面试题 17.11 单词距离 双指针字符串 67.30% 中等
- 438 找到字符串中所有字⺟异位词 (M)
- 最大连续 1 的个数 III（ 1004 M）
- 3 无重复字符的最长子串 哈希表-双指针-滑动窗口 34.90% 中等
- 433 压缩字符串 E

【特殊】

- 6.Z 字形变换 (M)

【数学】

- 13 罗马数字转整数 E
- 8.字符串转为整数 (M)
- 67.二进制求和 (E)
- 43.字符串相乘 M

【回溯】

- 17.电话号码的字母组合 M：回溯
- 93.复原 IP 地址 M 真题

#### 8.字符串转换整数 (atoi)

1. 题目:大意，首尾空格可忽略，除此外，只能是+-开头，尾部非数字可忽略；非数字，都返回 0；
2. 思路：
   1. 普通遍历
   2. 有限状态机
   3. 正则：截取中间的数字
      - res = re.match('[ ]\*[+-]?[0-9]+',str);
      - res = re.match('[ ]\*([+-]?\d+)',str)
      - res.group()匹配结果；
3. 关键：
   1. 去掉首尾无限空格 strip(' ')
   2. 截取一个字符串中间的连续整数部分:正则/普通遍历
   3. 对于溢出的处理方式通常可以转换为 INT_MAX 的逆操作。比如判断某数乘 10 是否会溢出，那么就把该数和 INT_MAX 除 10 进行比较。

```python
class Solution8:
    # 普通遍历
    def myAtoi1(self,str):
        str = str.strip(' ')
        if not str:return 0
        if not str[0] in '+-' and not str[0].isdigit():return 0
        end = len(str)
        for i in range(1,len(str)):
            if not str[i].isdigit():
                end = i
                break
        sign = 1
        if str[0] in '+-':
            if not str[1:end].isdigit():return 0
            else:
                sign = 1 if str[0]=='+' else -1
        res = int(str[:end])
        max_int = 2**31-1
        min_int = -1*2**31
        if sign and sign<0:
            return max(res,min_int)
        else:
            return min(res,max_int)

    # 正则
    def myAtoi(self,str):
        import re
        #
        pattern = r'[ ]*[+-]?[0-9]+'
        res = re.match(pattern,str)
        if not res:return 0
        res = int(res.group())
        max_int = 2**31-1
        min_int = -1*2**31
        if res>0:
            return min(res,max_int)
        else:
            return max(res,min_int)
```

#### 14. 最长公共前缀

解体思路 1:横向扫描 1. 求出字符串数组中，最短的长度 mlen，作为边界值; 2. 下标从 0 到 mlen 进行遍历，每次将所有字符串的该下标值拼接为 mstr，然后判断 len(set(mstr))==1
解体思路 2:纵向比较--经典 1. 使用 zip(*s)得到每个字符中相同下标组成的数组; 2. 遍历 zip(*s),如果 len(set(i))==1,则归入结果集；
解题思路 3:二分--经典 1. 通过二分法，选前缀的下标； 2. 批量判断字符串是否相等:all；--经典

总结：关于 zip(), zip(*) 1. zip(a,b)：打包为元组的列表， a = [1,2,3], b = [4,5,6], 则 zip(a,b)=[(1,4),(2,5),(3,6)] 2. zip(*s):解压，返回二维矩阵式, zip(\*[(1,4),(2,5),(3,6)]) = [(1,2,3), b = (4,5,6)]

```python
def longestCommonPrefix1(strs):
    res = ''
    index = 0
    if len(strs) == 0: return ''  # 注意考虑空的边界值
    mlen = len(strs[0])
    for i in strs:  # 拿到最小字符串的长度，作为边界值
        mlen = min(mlen, len(i))
    while index < mlen:
        pre = ''
        for i in strs:  # 相同位置字符进行拼接
            pre += i[index]
        if len(set(pre)) == 1:  # 通过len(set(mstr))==1判断是否为前缀
            res += pre[0]
            index += 1
        else:
            break
    return res

def longestCommonPrefix2(strs):
    s = ""
    for i in zip(*strs):# [('f', 'f', 'f'),('l', 'l', 'l'),('o', 'o', 'i')]
        if len(set(i)) == 1:
            s += i[0]
        else:
            break
    return s

def longestCommonPrefix3(self, strs: List[str]) -> str:
    def isCommonPrefix(length):
        # 判断字符串集strs，前缀是否为[0:length]
        str0, count = strs[0][:length], len(strs)
        # 经典-简洁
        # all():判断给定的可迭代参数 iterable 中的所有元素是否都为 TRUE,返回True/False
        return all(strs[i][:length] == str0 for i in range(1, count))
    if not strs:
        return ""
    minLength = min(len(s) for s in strs)
    low, high = 0, minLength
    while low < high:
        mid = (high - low + 1) // 2 + low
        if isCommonPrefix(mid):
            low = mid
        else:
            high = mid - 1
    return strs[0][:low]
```

#### 5. 最长回文子串 (M)

题目:找到字符串中最长的回文串，已知所给串最长 1000；
思路 1: 中心扩散法(双指针) 1. 遍历字符串下标：
1.1 以当前下标为入参，调用“获取最长回文子串”；
1.2 以当前和下一个下标为中心，“获取最长回文子串”；
1.3 比较 1.2 和 1.3，哪个长，赋给结果； 2. 以 xx 为中心，获取最长回文子串；
2.1 从中心点，开始向两端扩展，直到两指针不再相同；
思路 2: 动态规划(不如“思路 1”，空建复杂度更高)

```python
class Solution5:
    def longestPalindrome(self, s: str) -> str:
        def helper(s,i,j):
            while i>=0 and j<=len(s)-1 and s[i]==s[j]:
                i,j = i-1,j+1
            # 当前的i,j各回退以为，才是要返回的子串
            return s[i+1:j]
        if s==s[::-1]:return s
        res = s[:1]
        for i in range(len(s)):
            s1 = helper(s,i,i)
            s2 = helper(s,i,i+1)
            res = max(s1,s2,res,key=len)
        return res
```

#### 28. 实现 strStr

题目：判断字符串中，是否存在目标串；
解体思路 1:模式匹配 "Sunday 算法"--经典
变量：目标字符串中：str; 待匹配字符串:str[id,id+len(pattern)]; 模式串:pattern; 1. 每次匹配都会从 目标字符串中 提取 待匹配字符串与 模式串 进行匹配： 2. 若匹配，则返回当前 idx; 3. 不匹配，则查看 待匹配字符串 的后一位字符 c：
3.1 若 c 存在于 Pattern 中，则 idx = idx + 偏移表[c];
3.2 否则，idx = idx + len(pattern)
平均时间复杂度 O(n)
偏移表：
作用：存储每一个在 模式串 中出现的字符，在 模式串 中出现的"最右位置"到尾部的距离，例如 aab：
a 的偏移位就是 len(pattern)-1 = 2
b 的偏移位就是 len(pattern)-2 = 1
其他的均为 len(pattern)+1 = 4
思路 2:双指针(分别指向待匹配串、模式串) 1. 顺序遍历，匹配到模式串首字母时，才进行进一步匹配； 2. 完全匹配时，若未完全匹配，则从开始的下一位，重新执行；

```python
# me编码风格（被吐槽）
def strStr1(haystack, needle):
    len1,len2 = len(haystack),len(needle)
    if len2 == 0 :return 0
    if len1 == 0:return -1
    # 设置偏移量
    m_dict = {}
    for i in range(len2):
        m_dict[needle[i]] = len2-i
    # 匹配字符串
    i = 0
    while i<= len1-len2:
        if haystack[i:i+len2]==needle:
            return i
        # 使用下面的方式，减少圈复杂度
        # else:
        #     if i+len2<len1 and haystack[i+len2] in needle:
        #         i += m_dict[haystack[i+len2]]
        #     else:
        #         i += len2+1
        # 匹配不成功：1.下一个字符在“模式串”中
        elif i+len2<len1 and haystack[i+len2] in needle:
            i += m_dict[haystack[i+len2]]
        # 匹配不成功：2.下一个字符不在“模式串”中，或下标超长
        else:
            i += len2+1
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

#### 125. 验证回文

只考虑字母和数字字符，可以忽略字母的大小写。

```python
def isPalindrome(s):
    slist = list(s)
    s, e = 0, len(s)-1
    while s<e:
        if (not slist[s].isdigit()) and (not slist[s].isalpha()):
            s += 1
        elif (not slist[e].isdigit()) and (not slist[e].isalpha()):
            e -= 1
        else:
            if slist[s].lower()!=slist[e].lower():
                return False
            else:
                s += 1
                e -= 1
    return True
```

#### 67. 二进制求和 (易)

解题思路：
将 a 和 b 转换为十进制整数。
求和。
将求和结果转换为二进制整数。

```python
def addBinary(a, b):
    a1 = int(a,2)
    b1 = int(b,2)
    return bin(a1+b1)[2:]
```

#### 8.字符串转为整数（中）

1. 解体思路：
   1. 判断首字母
   2. 依次遍历后面字符，遇到非数字，终止
   3. 字符转数字（int）后，判断得到的数字是否大于边界值
   4. 注意：int 转换字符串时，字符串不能含有非数
2. 容易遗漏点：
   1. 首尾空字符可以不考虑，直接去掉，处理后：
   2. 首尾的情况：首位只能是+/-/数字，否则返回 0；
   3. 第二位的情况：首位为+/-，则第二位必须为数字，否则返回 0
3. 可能的用例： +，-， +3，+-3，+1.3，1.345
   '''

```python
def myAtoi(str):
    import math
    str = str.strip()
    if len(str) == 0: return 0
    end = len(str)
    # 首位只能是+，-，数字
    if (str[0] not in ['+', '-']) and (not str[0].isdigit()):
        return 0
    INT_MAX = int(math.pow(2, 31) - 1)
    INT_MIN = int((-1) * (INT_MAX + 1))
    for i in range(1, len(str)):
        if not str[i].isdigit():
            end = i
            break
    # 若首位含+/-，则要考虑第二位只能为数字
    if str[0] in ['+', '-'] and (not str[1:end].isdigit()): return 0
    numb = int(str[:end])
    if numb < INT_MIN: return INT_MIN
    elif numb > INT_MAX: return INT_MAX
    else: return numb
```

#### 438 找到字符串中所有字⺟异位词(M)

思路： 1. 初始化:起始指针,窗口计数器(字符次数),目标串计数器(字符次数),计数器(窗口满足目标串的次数) 2. 窗口未到达边界时：
2.1 扩大窗口(right>1)
2.2 更新操作：“当前元素”在目标串中出现，更新“窗口”；若其出现次数=目标串中次数，更新“计数器”；
2.2 若窗口要收缩(窗口长度>目标串)：若找到(计数器==目标串长度)，加入结果集；否则不断“收缩窗口”，并做更新操作；
关键： 1. 收缩条件:目标串中所有字符，均包含在窗口中； 2. 计入结果条件：1)目标串中所有字符，均包含在窗口中；2)窗口长度=目标串长度；

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        if not s:return []
        from collections import defaultdict
        m,n = len(s),len(p)
        left,right = 0,0
        # 只记录目标值中出现的字符
        window = defaultdict(int)
        needle = defaultdict(int)
        valid = 0
        res = []
        for i in p:
            needle[i] +=1
        while right<m:
            # 扩张窗口，并更新
            cur = s[right]
            # 只记录目标值中出现的字符
            if cur in p:
                window[cur]+=1
                if needle[cur]==window[cur]:
                    valid +=1
            right += 1
            # 收缩条件：目标值中每个字符的出现次数均满足了要求(要考虑到字符重复的情况)
            while valid==len(needle):
                # 判断是否需要加入结果集
                if (right-left) == n:
                    res.append(left)
                # 收缩窗口并更新
                temp = s[left]
                # 只去掉记录目标值中出现的字符
                if temp in p:
                    window[temp]-=1
                    if window[temp]<needle[temp]:
                        valid -=1
                left += 1
        return res
```

#### 最大连续 1 的个数 III（1004 M）

```python
class Solution:
    def longestOnes(self, A, K) :
        i = res = 0
        for j in range(len(A)):
            K -= 1 - A[j]
            if K < 0:
                while i < j and A[i] == 1:
                    i += 1
                i += 1
                K += 1
            res = max(res, j - i + 1)
        return res
    def longestOnes2(self, A, K):
        i = 0
        for j in range(len(A)):
            K -= 1 - A[j]
            if K < 0:
                K += 1 - A[i]
                i += 1
        return j - i + 1
```

#### 3. 无重复字符的最长子串(中)

解体思路 1：“滑动窗口法” （慢） 1. 变量：窗口(数组)，最大长度，当前长度； 2. 全局 1 次遍历，针对每个元素，做以下判断： 3. 若“当前字符”与“字符串窗口”不重复，则当前长度加 1，并将当前字符加入“窗口”； 4. 若重复，则找到重复位置，更新窗口为“从重复位置的下一位，到当前字符”，更新当前长度；
解题思路 2： 1. 变量：1 窗口(left,right),2 窗口内各字符数量 window，3 结果集； 2. 窗口未到达边界时：
2.1 扩大窗口(right>1)，并更新 window；
2.2 当 新加元素 出现重复，不断“收缩窗口”，并更新 window；
2.3 更新结果集； 3. 关键点：
3.1 如何判断“新加元素，出现重复？”：window 中记录的次数>1，即重复；
3.2 无需用数组/字符串记录窗口内容，因为这样判重时，还需要遍历窗口；不如直接记录次数，进行判重；

```python
class Solution3:
    def lengthOfLongestSubstring1(s):
        if not s: return 0
        lookup = list()  # 当前字符串（窗口）
        n = len(s)
        max_len = 0
        cur_len = 0
        # 仅1次遍历，判断当前元素是否与当前字符串重复；1）不重复则直接添加，且长度+1；2）重复则更新当前字符串，更新长度；
        for i in range(n):
            val = s[i]  # 当前指针指向的元素
            if not val in lookup:  # 若跟当前字符串无重复
                lookup.append(val)
                cur_len += 1
            else:  # 跟当前字符串重复
                index = lookup.index(val)  # 找到重复的“相对下标
                lookup = lookup[index + 1:]  # 更新当前字符串为 从“重复下标的下一位”，到“当前字符”
                lookup.append(val)
                cur_len = len(lookup)
            if cur_len > max_len:
                max_len = cur_len  #比max_len=max(max_len,cur_len)快
        return max_len
    def lengthOfLongestSubstring2(self, s: str) -> int:
        from collections import defaultdict
        if not s: return 0
        start,end = 0,0
        n = len(s)
        # 记录窗口中字符的出现次数,不用按顺序记录
        window = defaultdict(int)
        res = 0
        while end<n:
            # 扩大窗口(不考虑有无重复)
            cur = s[end]
            end += 1
            window[cur]+=1
            # 当 新加元素 出现重复，不断“收缩窗口”
            while window[cur]>1:
                temp = s[start]
                start+=1
                window[temp]-=1
            res = max(res,end-start)
        return res
```

#### 433 压缩字符串 E

思路：双指针 1. write:写入位置；anchor：连续字符串起点；read：当前元素位置 2. 遍历：read 达到尾部/read 和其后元素不重复，写入结果；(注意更新 write 的值) 3. write 即为最终结果；

```python
class Solution:
    def compress(self, chars: List[str]) -> int:
        if len(chars)==1:return 1
        # 写入位置，连续区间起点
        write,anchor = 0,0
        for i in range(len(chars)):
            #若到达末尾/当前元素不再与之前元素重复：计入结果
            if i+1==len(chars) or chars[i+1]!=chars[i]:
                chars[write] = chars[anchor]
                write+=1
                cnt = i-anchor+1
                if cnt>1:
                    # 数字长度len(str(cnt))
                    chars[write:write+len(str(cnt))]=list(str(cnt))
                    # 写入位置前移len(str(cnt))个
                    write += len(str(cnt))
                anchor=i+1
        return write
```

#### 6. Z 字形变换 (中)

题意理解： 1. 目标：将竖着打印的“之”字形字符串，逐行打印；
2：规律：竖着打印的“之”字形字符串，行坐标变化为 0>numRows>0，变大>变小>变大，如此循环;
解题思路： 1. 初始化 numRows 个行字符串； 2. 模拟“竖着打印”的过程，并将数字填到对应的“行字符串”中； 3. 过程 2 实现的技巧：利用 flag=1/-1，让 index+=flag，flag 在一个循环后，改变一次正负，完成行坐标的变大/小的转换；

```python
def convert(s, numRows):
    if len(s) <= 2 or numRows==1:
        return s
    # 定义numRows个字符串形成的列表['','','',...]
    res = ['' for _ in range(numRows)]
    index, flag = 0, -1
    for i in s:
        res[index] += i
        # 行下标，在[0,numRows)之间，变大>变小>变大，如此循环；
        if index == 0 or index == numRows - 1:
            # flag妙用，完成 大>小>大>小的循环
            flag = -flag
        index += flag
    return ''.join(res)
```

#### 560. 和为 K 的子数组 （中）

原理推导：
注：pre[i]表示从[0,i]的连续子序列的和；mdict 存储：{前缀和：出现次数}；

1. 要找到以第 i 个元素结尾的连续子序列满足和为 k，需存在一个 j，使得 pre[i]-pre[j] = k；
2. 由 1 可得：pre[j]=pre[i]-k，若存在 pre[j]，则可找到以 i 为结尾，和为 k 的连续子序列为；

```python
def subarraySum(nums, k):
    from collections import defaultdict
    mp = defaultdict(int)
    count = 0 # 结果
    pre = 0 # 当前前缀和
    mp[0] = 1 # 初始化：前缀和为0时，次数为1；
    for i in nums:
        # 更新当前前缀和
        pre += i
        # 若hash中存在，则结果更新
        if pre-k in mp:
            count += mp[pre-k]
        # 将当前前缀和加入hash表，次数加1
        # 对应两种情况：1) cur_sum 之前出现过; 2) cur_sum 没出现过;(因为开始为0，可以直接加1)
        mp[pre]+=1
    return count
```

#### 316. 去除重复字母

```python
def fun():
    s1 = ['0']
    for k,v in enumerate(s):
        if v not in s1:
            while v < s1[-1] and s1[-1] in s[k+1:]:
                s1.pop()
            s1.append(v)
    return ''.join(s1[1:])
```

#### 151. 翻转字符串里的单词

注意：字符串分隔，不加参数，则忽略字符串中间的空格
思路 1:语言特性（内置 api)；
思路 2:双指针(1.单词头部 2.单词尾部)，均从末尾出发；

```python
def reverseWords( s) :
    return ' '.join(s.strip().split()[::-1])
```

#### 344 反转字符串

题目：["h","e","l","l","o"]变成["o","l","l","e","h"]，原地反转，空间 O(1)；
思路:首尾双指针+递归交换元素；

```python
# 官网递归法：简洁，首、尾双指针，且无需区分是否“奇偶”
class Solution:
    def reverseString(self, s):
        def helper(left, right):
            if left < right:
                s[left], s[right] = s[right], s[left]
                helper(left + 1, right - 1)
        helper(0, len(s) - 1)
```

#### 541 反转字符串 II (E)

题目：每 2k 个字符串，反转前 k 个；不足 2k 的，仍反转前 k 个；不足 k 的，全部反转；
思路 1:语言特性，遍历，步长 2k，只反转前 k 个 s[i:i+k][::-1]；
思路 1:语言特性，遍历，步长 k，设置反转标识 flag，标识 True 时才反转 s[i:i+k][::-1]；

```python
class Solution541:
    def reverseStr1(s, k):
        a = list(s)
        for i in range(0, len(s), 2 * k):
            a[i:i + k] = reversed(a[i:i + k])
        return ''.join(a)
    def reverseStr1(s, k):
        res,flag = '',True
        # 设置步长k,并设置“是否反转”的标志
        for i in range(0,len(s),k):
            res += s[i:i+k][::-1] if flag else s[i:i+k]
            flag = not flag
        return res
```

#### 练习:反转字符串中的单词

题目:将字符串中的单词(空格分隔)，进行逐个反转，如'hello boy'->'olleh yob'；
思路 1:语言特性，2 次反转；

```python
def reverseWords(mstr):
    return ' '.join(mstr.split(' ')[::-1])[::-1]
```

#### 练习:反转字符串中的单词 2

将字符串(只包含大小写字母，单词首字母大写)，按单词进行逐个反转,如'ABoyIsGood'->'AyoBsIdooG'
'''

```python
def caseConverse(mstr):
    for i in mstr:
        if i.isupper():
            mstr = mstr.replace(i, ' ' + i)
    return ''.join(mstr.split(' ')[::-1])[::-1]
```

#### 345 反转字符串中的元音字母

思路：首尾指针，同为元音，交换，同时前进；

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

#### 459. 重复的子字符串 (E)

题目:给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过 10000。
思路 1:数学规律，S+S 去掉首尾后，形成新串 A，判断 S 是否在 A 中即可；
思路 2:

```python
def repeatedSubstringPattern(s):
    #2倍后，去首尾
    s1 = (s + s)[1:-1]
    return (s in s1)
```

#### 686. 重复叠加字符串匹配 (E)

题目:判断能否通过重复叠加字符串 A，使得 B 成为其子串；能则输出次数，否则返回-1；

```python
def repeatedStringMatch(A, B):
    # 取整
    base_num = len(B) // len(A)
    A_new = A*base_num
    if B in A_new:
        return base_num
    if B in A_new + A:
        return base_num + 1
    if B in A_new + A + A:
        return base_num + 2
    return -1
```

#### 字符串去重，保持相对位置不变

def duplicate1(s):
mlist = list(set(s))
mlist.sort(key=s.index) #妙,以原有顺序重排
return ''.join(mlist)

#### 字符串去重，使字典序最小

```python
def duplicate2(s):
    stack = ['0'] #最终的队列
    for k, v in enumerate(s):
        if v not in stack:
            #如果当前元素比栈顶小，且栈顶元素在后面有出现，则删掉栈顶
            while v < stack[-1] and stack[-1] in s[i + 1:]:
                stack.pop()
            stack.append(_)
    return ''.join(stack[1:])
```

#### 字符串中的第一个唯一字符

```python
def firstUniqChar(s):
    for i in enumerate(s):
        if s.count(i[1]) == 1:
            return i[0]
    return -1
```

#### 罗马数字转整数

I1， V5， X10， L50，C100，D500 和 M1000。通常情况下，小的在大的数字右边。例外：IV4,IX9,XL40,XC90,CD400,CM900.XXVII
1.\*构建一个字典记录所有罗马数字子串，注意长度为 2 的子串记录的值是（实际值 - 子串内左边罗马数字代表的数值） 2.这样一来，遍历整个 ss 的时候判断当前位置和前一个位置的两个字符组成的字符串是否在字典内，如果在就记录值，不在就说明当前位置不存在小数字在前面的情况，直接记录当前位置字符对应值
举个例子，遍历经过 IVIV 的时候先记录 II 的对应值 11 再往前移动一步记录 IVIV 的值 33，加起来正好是 IVIV 的真实值 44。max 函数在这里是为了防止遍历第一个字符的时候出现 [-1:0][−1:0] 的情况

```python
def romanToInt(s):
    d = {
        'I': 1,
        'IV': 3,
        'V': 5,
        'IX': 8,
        'X': 10,
        'XL': 30,
        'L': 50,
        'XC': 80,
        'C': 100,
        'CD': 300,
        'D': 500,
        'CM': 800,
        'M': 1000
    }
    # enumerate将list转为{(index,value),...}
    # d.get(key,value),若指定的key不存在，则返回默认值value.
    # s[max(i-1, 0):i+1]表示当前位+前一位的字符组，d[n]表示当前位字符代表的数字大小；
    # 举例：s[max(i-1, 0):i+1], d[n]的值为：I,1; IV,5; VI,1; IV,5;d[]
    return sum(d.get(s[max(i - 1, 0):i + 1], d[n]) for i, n in enumerate(s))
```

#### 43.字符串相乘 M

思路：模拟乘法运算，双层循环； 1. 固定大数做被乘数，逆序遍历小数，依次与大数相乘； 2. 注意扩张 10 的倍数，以及是否有进位；(如下面的 563*1*10,563*2 时，最后进 1 位)；
563*12 = 563*2 + 563*1\*10；

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        if num1 == '0' or num2 == '0': return '0'
        n1, n2 = len(num1), len(num2)
        res = []
        if n1 < n2:
            min_v, max_v = num1, num2
        else:
            min_v, max_v = num2, num1
        cnt = 0 # 乘积扩大几个10倍
        for i in range(len(min_v)-1,-1,-1):# 逆序遍历较小值，作为乘数
            cur = ''
            carry = 0
            p = len(max_v) - 1 # 指向 被乘数
            while p >= 0:
                product = int(max_v[p]) * int(min_v[i]) + carry
                cur = str(product % 10) + cur
                carry = product // 10
                p -= 1
            if carry:cur=str(carry)+cur #遍历完被乘数，判断是否有进位
            cur+=cnt*'0' # 乘数进位，乘积扩大10倍
            cnt+=1
            res.append(cur)
        m_sum = 0
        for i in res:
            m_sum+=int(i)
        return (str(m_sum))
```

#### 17. 电话号码的字母组合 M：回溯

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:return []
        res = []
        s_map = {
            '2':'abc',
            '3':'def',
            '4':'ghi',
            '5':'jkl',
            '6':'mno',
            '7':'pqrs',
            '8':'tuv',
            '9':'wxyz'
        }
        n = len(digits)
        # 选择列表、路径
        def dfs(path,index):
            if len(path)==n:
                res.append(path)
                return
            cur = s_map[digits[index]]
            for i in cur:
                dfs(path+i,index+1)
        dfs('',0)
        return res
```

#### 93.复原 IP 地址 M

ip 规则：[0,255]；001,010 这种，是非法的；

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        if not s or len(s)>12:return []
        res = []
        n = len(s)
        def helper(path,index): #路径、选择列表
            if len(path)==4 and index==n:# 找到结果
                res.append('.'.join(path))
                return
            for i in range(index,index+3):  # ip最多3位
                if not s[index:i+1]:return # 可能为''
                if s[index:i+1]!='0' and s[index]=='0':return #排除01,001这种
                if int(s[index:i+1])<256 and s[index:i+1]>='0' :#正确ip，递归
                    helper(path+[s[index:i+1]],i+1)
        helper([],0)
        return res
```

#### 将字符串中字符向右进行循环移位

样例：fun2("abcd123",3)；Return "123abcd"；
思路 1：前后交换
思路 2：2 部分单独翻转，然后对整个字符串进行翻转；

```python
def fun2(s,k):
  if not s:return ''
  n = len(s)
  k = k%n
  return s[n-k:]+s[:n-k]
```

#### 字符串中单词的翻转

样例：s = "I am a student"；Return "student a am I"

```python
def fun3(s):
  if not s:return ''
  s_list = s.split(" ")
  return  ' '.join(s_list[::-1])
```

#### 409 计算一组字符集合可以组成的回文字符串的最大长度

举例：'aabbcccddddd'，返回 4+3+4
思路：偶数直接加入；遇到第一个奇数，计入结果；再遇到奇数，只计入其最大的偶数部分；

```python
def longestPalindrome(s):
  ans = 0
  from collections import Counter
  count = Counter(s)
  for v in count.values():
      ans += v // 2 * 2
      # 遇到第一个奇数，计入结果；再遇到奇数，只计入其最大的偶数部分；
      if ans % 2 == 0 and v % 2 == 1:
          ans += 1
  return ans
```

#### 205 判断两个字符串是否同构

思路 1：记录每个元素最近出现的下标，若一直相同，则同构；
思路 2：

```python
class Solution205:
  def fun1(s,t):
     from collections import defaultdict
      map1, map2 = defaultdict(int), defaultdict(int)
      i1, i2 = 0, 0
      n1, n2 = len(s), len(t)
      while i1 < n1 and i2 < n2:
          if s[i1] not in map1: map1[s[i1]] = i1
          if t[i2] not in map2: map2[t[i2]] = i2
          cur1, cur2 = map1[s[i1]], map2[t[i2]]
          if cur1 != cur2:
              return False
          i1 += 1
          i2 += 1
      return True
  def fun2(s,t):
    if not s:
      return True
    dic={}
    for i in range(len(s)):
      if s[i] not in dic:
        if t[i] in dic.values():
          return False
        else:
          dic[s[i]]=t[i]
      else:
          if dic[s[i]]!=t[i]:
            return False
    return True
```

#### 647. 回文子字符串个数 M

Input: "aaa"; Output: 6 ("a", "a", "a", "aa", "aa", "aaa")
给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被计为是不同的子串。

#### 9. 判断一个整数是否是回文数 E

要求：不能使用额外空间，也就不能将整数转换为字符串进行判断。
题目：将整数分成左右两部分，右边那部分需要转置，然后判断这两部分是否相等。
思路：

1. 负数/个位数，直接返回；
2. 将整数转字符串，逆序，判等；或将整数拆成数组，判断是否对称；

```python
def countSubstrings(s):
    if x<0:return False
    elif x<10:return True
    else:
        res = []
        while x//10:
            x,cur = x//10, x%10
            res.append(cur)
        res.append(x)
        return res[::-1]==res
        # 慢
        # return str(x)[::-1]==str(x)
```

#### 696. 统计二进制字符串中连续 1 和连续 0 数量相同的子字符串个数 E

10101: “10”，“01”，“10”，“01”
00110011:“0011”，“01”，“1100”，“10”，“0011” 和 “01”

```python
# 优化：记录每个数字出现的次数的方法；
# 记录每个数字出现的次数；最后再计算结果；

def countBinarySubstrings1(s):
    if not s:return 0
    res = [1]
    ans = 0
    for i in range(1,len(s)):
        if s[i]==s[i-1]:
            res[-1]+=1
        else:
            res.append(1)
    for j in range(len(res)-1):
        ans+= min(res[j],res[j+1])
    return ans

# 优化存储空间，不记录每个字符串的出现次数；
# 只用pre,cur记录相邻两个数字的次数；每次遇到pre>=cur时，更新结果；
def countBinarySubstrings2(s):
    if not s:return 0
    pre,cur = 0,1
    res = 0
    pre_val = 0
    for i in range(len(s)-1):
        if s[i]==s[i+1]:
            cur += 1
        else:
            pre,cur = cur,1
        # 每当pre>=cur，证明遇到了s[i]!=s[i+1]的情况，且满足成串条件；
        if pre>=cur:
            res+=1
    return (res)
```
